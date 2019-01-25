chain management
==================
chain을 manage하는 일은 ChainServcie module에서 담당한다.

ChainServcie는 크게 다음과 같은 일을 수행한다.

1. block insert to Chain
2. reorganization
3. synchronize chain

chain insertion process
---------------------------
Chainservcie가 블럭을 추가하는 경우는 다음의 두가지이다.

1. Block Factory에서 생성한 블럭
  BP 노드가 자신이 생성할 round가 되면 Block Factory에서 블럭을 생성한다.

  Block Factory는 transaction을 모두 수행하고 block을 ChainService에 전달 하기 때문에 실행 결과를 함께 포함하게 된다.

  실행 결과는 transaction 수행결과인 receipts와 Account에 대한 변경사항을 나타내는 State changed entiry들로 이루어 진다.

  Block과 관련된 정보는 ChainDB에 저장하고 Account에 관한 정보는 StateDB에 저장한다.

2. 다른 Peer node에서 전달 받은 블럭
  다른 Peer node에서 전달 받은 블럭은 크게 다음 3가지 경우이다.

  - orphan block
      parent block이 아직 DB에 저장 되지 않은 경우에 해당 된다. orphan block은 orphan block cache에 저장된다.

      이 후 parent가 전송되면 orphan block cache에서 제거되고 다시 처리된다.

  - side branch block
      parent block이 저장되어 있지만 main branch에 속하지 않는 블럭인 경우이다. 이 경우 block은 수행되지 않고, Block info{hash, block} 만 저장된다.

  - main branch block
      main branch의 head의 다음 블럭인 경우이다.

main branch에 블럭이 저장되는 과정은 다음과 같다.

1. validation before execution
2. execute transactions
3. apply changed entries of Account to State SMT
4. validate after execution
5. save State SMT to StateDB
6. save chain meta data to Chain DB

Block validation
---------------------
network로 받은 블럭은 valid하지 않을 수 있기 때문에 여러가지 검증을 거치게 된다.

실행 전 검증
^^^^^^^^^^^^
- consensus validation
    valid한 BP가 생성한 블럭이 맞는지 block생성시간과 sign을 통해 검증한다.
- transaction merkel 검증
    transaction들이 위변조 되지 않았는지 검증한다.
    transaction들로 merkel tree를 생성하여 Block header에 저장된 transaction merkel root와 동일한지 검사한다.
  실행전 검증이 완료되면 Block이 valid한 BP에서 생성되었고 블럭에 포함된 transaction이 위변조 되지 않았음을 보장한다.

실행 후 검증
^^^^^^^^^^^^
- State root validation
  실행후 변경된 State의 SMT root node hash가 Block header에 저장된 blocksRootHash와 동일한지 검사한다.
- Receipt merkle 검증
  Transaction들의 수행결과로 생성된 receipt들로 merkel tree를 생성하여 Block Header에 저장된 recipts merkel root와 동일한지 검사한다.

  이 검증들을 통해 Block에 포함된 transaction을 수행한 결과가 Block을 생성한 BP node와 동일한 결과를 가짐을 보장한다.


reorgnize process
--------------------
ChainService module은 longest chain을 main branch로 선택하여 유지한다.

side branch들은 실행되지 않은 상태로 DB에 Block info만 저장된다.

이 후 다른 peer를 통해서 받은 side branch가 Node에서 유지하고 있는 main branch보다 더 길어진 경우 side branch를 main branch로 변경 하는 절차를 거치게 된다. 이를 reorgnize process라고 한다.

reorgnize 는 다음과 같은 절차로 수행된다.

1. find common ancestor between main branch and side branch
2. rollback master branch
3. rollforward side branch
4. swap chain meta

- find common ancestor
    main branch와 side branch의 가장 마지막 common ancestor block을 찾는다.

- rollback master branch
    State DB의 상태를 common ancestor block까지만 추가한 상태로 되돌린다.

- rollforward side branch
    common ancestor block 다음 높이 부터 side branch의 head block 까지 실행한다.
    이 때 StateDB만을 변경하고 Chain info, Tx info는 변경하지 않는다.

- swap chain meta
    atomic하게 chain을 교체하게 하기 위해 rollback, rollforward중에 chain info를 변경하지 않고 마지막에 변경된 chain 정보를 한번에 변경한다.
    이 때 rollback된 블럭에 대해 chain info와 transaction info가 삭제 되고, rollforward된 블럭에 대해 chain info와 transaction info를 새로 추가한다.

    단 기존 main branch에만 속해있고 side branch에 포함되지 않은 transaction들은 다시 mempool에 반환 된다. 이는 transaction 유실을 막기 위함이다


Synchronize process
--------------------------
새 노드를 추가하거나 일시적으로 중지되었던 노드를 다시 시작한 경우 최신의 Chain정보를 기존 노드들에게서 받아야 한다. 이를 Synchonize 과정이라고 한다.
Syncer module이 Synchro nize를 담당한다.

sync를 유발하는 경우는 다음과 같다
 - peer가 connect하기 위해 handshake 과정을 거칠때 remote peer가 현재 node 보다 height가 더 높은 경우
 - peer에서 notify 받은 block이 현재 main branch 보다 더 높은 경우

sync를 유발한 블럭을 보낸 노드를 target node로 지정하여 해당 node의 chain과 synchronize를 수행한다.

긴 chain을 동기화 하기 위해서는 대량의 block 정보를 peer node에서 받아야 한다.

이는 Peer node에 performance degrade를 유발 시킬 가능성이 있다.

따라서 load를 분산 시키기 위해 최대한 여러 peer에게서 정보를 나누어서 받아오게 한다.

현재 sync를 유발한 target node로 부터 동기화할 대상 블럭의 Hash를 가져오고, Block은 연결된 모든 valid Peer를 통해 나누어 받아오게 처리된다.


Synchronize step
^^^^^^^^^^^^^^^^^^^
1. find common ancestor
현재 node의 chain과 target node의 last common ancestor를 찾는다.

2. Get Hashes
target node로 부터 common ancestor 이후 block의 hash를 받아온다.

3. Get Blocks
현재 node에 연결된 모든 valid peer로 부터 N개씩 block을 요청하여 받아온다.

4. Insert blocks to chain
받아온 block은 ChainService module에 insert 요청을 하여 chain에 추가한다.

2, 3, 4는 parallel 하게 수행된다. 대부분의 시간은 chain에 insert하는 부분에서 소요되므로 insert하는 동안
network에서 hash와 block을 받는 작업이 overlap 된다.
