Recovery
========
Cluster 의 과반수 이상 노드가 회생 불가능이 된 경우, Cluster에는 더이상 노드 삭제 및 추가를 할 수 없게 된다. 문제가 된 노드를 삭제하고자 하여
도 과반수 이상의 합의가 있어야 하기 때문이다. 

이 경우에 BlockChain의 data를 그대로 유지 하고자 하는 경우에는 기존 노드 data의 backup 을 이용해 새로운 Cluster를 구성 할 수 있다. 단 Raft Cluster 구성 정보는 재활용 할 수 없다.

일반적인 Cluster 생성시와는 몇가지 차이점이 있다.

1. Recovery를 이용해 Cluster를 생성할 경우 initial member 노드는 1개이어야 한다.
2. BlockChain data에는 이미 genesis block이 생성되어 있으므로, genesis block 대신 config 파일에 멤버 노드를 별도로 설정해 주도록 한다.
3. N개 node cluster를 구성하고자 하는 경우 2번째 멤버 부터는 동적 멤버 추가 기능을 이용한다.

backup files에서 Cluster 관련 정보는 새 Cluster를 생성할 때 모두 지워진다. 따라서 datafile을 복사하더라도 기존 Cluster에 포함된 노드들과 관련
 없는 새로운 Cluster가 만들어 진다. 새로 생성된 Cluster는 backup에 속한 blockchain의 best block에 이어 block을 생성한다.


Prepare backup datafiles
------------------------

기존 노드의 backup datafiles을 준비한다. backup datafiles는 Old cluster에 속해 있던 노드의 datadir 디렉토리를 통채로 복사해서 새로 Cluster를 구성할 노드의 Storage에 복사해둔다.

Write Configuration file
------------------------

.. code-block:: shell
    datadir="[copied data directory]"

    [p2p]
    ...
    npaddpeers = [
    ]
    ...

    [consensus.raft]
    newcluster=true
    usebackup=true
    name="[NICKNAME OF NEW BP]"
    recoverbp={name="[NICKNAME OF NEW BP]",address="/ip4/[IP ADDRESS FROM BP 01]/tcp/7846",peerid="[PEER ID FROM NEW BP 01]"}

- 새로 cluster를 생성하는 과정이므로 newcluster=true 설정을한다.
- backup을 이용해 cluster를 재생성하고자 하므로 usebackup=true 설정을 한다.
- 새로운 cluster의 member를 recoverbp에 기술해 준다. 이 때 member는 반드시 1개 노드 이어야 한다.

Running a node
--------------
새로 추가한 노드를 시작한다. 이 후 노드를 추가하고자 하는 경우 Dynamic membership change 기능을 이용하여 하나씩 추가한다.

