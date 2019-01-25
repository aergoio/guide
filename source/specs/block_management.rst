Block management
==========================

Block의 생성
------------------
Block은 consensus engine의 Block Factory에서 생성된다.

Consensus engine은 Block이 언제 어떤 노드에서 생성하는지를 결정하고 관리하는 역할을 하는 모듈이다.
AERGO는 디폴트 consensus로 DPOS를 사용하기 때문에 1초마다  한 BP노드가 생성 권한을 가지고 블럭을 생성한다.

Block Factory에서 블럭이 생성되는 과정은 다음과 같다.

#. mempool에서 수행가능한 Transaction candidates를 가져온다.
#. transaction을 하나씩 수행하여 성공한 transaction은 블럭에 포함한다.
#. 블럭 크기, 시간 제한을 넘어서면 더이상 transaction을 포함하지 않는다.
#. 포함된 transaction으로 Block을 생성하고 Block에 서명을 추가 한다.

Block Factory는 생성된 Block을 영구적인 저장을 위해 ChainService에 전달한다.

ChainService는 내부 storage에 block과 추가 적인 meta정보를 저장한다.

Block의 전파
-----------------
Chain Service에 전달된 블럭은 DB에 저장되기 전 다른 노드에 Notify Block Message를 통해 연결된 Peer Node들에 전파된다.

해당 Block을 전달 받은 Node의 ChainService는 Block을 검증하고 Transaction들을 실행한후 Block을 자신의 Chain에 추가한다.

Block의 저장
------------------
생성된 Block은 Chain에 연결하기 위해 추가적인 meta정보와 함께 DB에 저장된다. Block의 저장은 ChainService에서 이루어진다.

AERGO는 permanent storage로 Badger Database를 사용한다.

Badger는 Key/Value 형태로 data를 저장하며 일반적인 database와 유사하게 Transaction 개념을 지원한다.

DB에 저장되는 정보는 다음과 같다.

DB는 별도로 유지되는 ChainDB와 StateDB 두개로 나뉘어 관리된다.
이 글에서는 Chain정보를 저장하기 위해 사용되는 ChainDB에 관해 설명한다.

TODO:  StateDB에 관한 정보는 architecture/State를 참고한다.


Block info
^^^^^^^^^^


해당 정보는 chain에서 Hash를 이용해 Block을 검색할 때 사용된다.
대표적으로 다른 노드에서 Block을 Hash로 요청시 요청받은 Hash를 Key로 Block을 검색하여 응답한다.

=================  ==========================
Key					Value
=================  ==========================
Hash of Block       serialized data of Block
=================  ==========================



Chain info
^^^^^^^^^^
Block이 main branch에 연결되는 경우, 이를 나타내는 추가적인 meta 정보가 필요하다.

main branch에 연결되지 않는 경우, 즉 sub branch인 경우는 Block info만이 저장되고 Chain info는 저장되지 않는다.

Chain info를 이용해 main branch의 모든 높이의 block들을 검색 할수 있고 chain의 top block의 높이를 알수 있다.

chain info는 block이 main branch에 속하는지 sub branch에 속하는지를 판단하기 위해서도 사용된다.

=================  ==================================
Key					Value
=================  ==================================
Number of Block     block hash
Latest block Key    latest block number of main branch
=================  ==================================


Transaction Info
^^^^^^^^^^^^^^^^^^^^
main branch에 포함되어 수행된 transaction의 meta정보를 저장한다.

transaction hash로 해당 transacion이 어떤 Block에서 몇번째로 저장되어 있는지를 알수 있다.

transaction의 내용은 별도 저장하지 않기 때문에 실제 transaction의 내용을 알기 위해서는 Block hash와 index를 이용해 Block info을 검색해보아야 한다.


=================   ==============================================
Key					Value
=================   ==============================================
Transaction hash    (Block Hash, index of transaction in block)
=================   ==============================================
