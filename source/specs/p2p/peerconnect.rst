============
Peer Connect
============

NOTE: Some document is not translated yet.

Node Discovery
==============
Aergo server need to know where other server nodes are.
There are several ways to do.

Query polaris
-------------
Polaris keep list of aergo server nodes, such like DNS. Aergo server connect and query to official Aergo Polaris automitically, if the chain is Official AergoMainNet or AergoTestNet.

You can build and run custom Polaris for your private chain or custom public chain. You should configure to use custom Polaris by modifying config file.

Designate Peer
--------------
You can add list of peers you know to connect at boot time in configuration file. use options 'npaddpeers'.

Dynamic peer discovery
----------------------
Aergo server ask peers list to other connected peers and Polaris if it connect not enough peers, and try to connect received peers.

접속할 피어의 선택
------------------
[below will be added to Aergo server soon]
접속할 피어 목록은 Kademlia-like 방식으로 관리가 된다. 자신과 거리가 가까운 피어에 더 많이 접속하고 먼 피어와는 접속 유지 숫자가 작다. 피어의 거리는 물리적인 거리가 아닌 해시값에 기반한 차이를 기준으로 한다.


노드 접속 순서
==============
아르고 서버의 네트워크 통신은 libp2p 기반 위에서 동작하며, 전송단 수준에서의 암호화와 노드 구분은 libp2p가 담당한다. tcp 세션이 접속이 되고 나면 두 피어가 handshake 작업을 시작한다.

Peer Handshake
--------------
Handshake 단계에서는 서버는 서로의 버전과 체인아이디 및 상태를 교환하여 접속 여부를 결정한다. 상대방 노드 버전이 내 서버가 지원하지 않거나, 운영하는 체인 아이디가 다른 경우 접속을 중단한다. handshake가 성공하고 나면 블록 높이 차이를 비교해 동기화를 시작한다.

Keep Alive
----------
Aergo server는 connection기반의 접속을 유지한다. 두 피어는 내부적으로 정의된 접속유지 점수가 일정 수준을 넘어가면 접속을 해제한다. 이 점수는 query요청시 낮은 수준으로 증가하고, 잘못된 블록이나 TX 알림을 보낼 경우 큰 폭으로 증가한다.

Peer blacklist
--------------
[below will be added to Aergo server soon]
내부적으로 정의된 밴 점수가 일정 수준을 넘어가면 Aergo server는 해당 피어의 접속을 차단한다. 이 점수는 접속유지 점수 초과로 접속이 끊긴 경우 등에 의해 증가하고, 점수나 차단 횟수 등의 계산을 통해 차단 기간도 달라진다. 또한 설정 파일에서 차단 주소를 지정하여 영구적으로 차단을 할 수도 있다.
