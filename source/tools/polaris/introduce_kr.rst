Introduction
============

Polaris는 aergoserver노드가 다른 노드에 접속하기 위한 노드 정보를 제공하는 서버이다.
하나의 Polaris는 하나의 체인만 담당한다. 
Polaris는 aergosvr와 마찬가지로 운영을 위해서 고유 개인키가 필요하다. aergosvr와 달리 자동생성을 하지 않으므로 직접 생성해서 설정에 지정해야한다.

동작 프로세스
-------------

1. aergo 노드가 Polaris에 접속하여 다른 aergo 노드 목록을 요청한다. (선택적으로 자신의 노드 등록도 요청한다.)
2. Polaris는 aergo 노드의 체인이 동일한 것임을 확인한 후 목록을 반환한다. aergo 노드가 등록을 요청한 경우면 반대방향 접속이 가능한지 확인한 후 노드 목록에 추가한다.

Building Polaris
================

Docker를 사용하지 않고 소스에서 Polaris를 빌드하는 방법 설명
Polaris는 0.11버전 현재 aergo 프로젝트 내의 하위 모듈 형태로 소스가 존재한다.

1. github.com/aergoio/aergo 에서 소스를 받는다.
2. make polaris 로 polaris 실행파일을 빌드한다.

Runnig Polaris
==============

Configuration
-------------

Polaris의 동작을 설정하는 것은 크게 네 개의 파일이 사용된다.

1. Polaris 설정파일 : 전반적인 Polaris의 동작방식을 결정한다. 다른 설정 관련 파일의 경로도 지정한다.
2. 개인키 파일 : Polaris의 통신에 사용할 PK파일이다. aergosvr의 개인키 파일과 동일한 포맷을 사용한다.
3. genesis 파일 : Polaris가 제공할 노드의 체인 정보를 담고 있다. aergosvr를 초기화 할 때 사용하는 genesis파일과 동일한 포맷을 사용한다.
4. 로그 설정 파일 : 파일명은 arglog.toml 로 aergosvr 에서 사용하는 파일과 동일한 포맷을 사용한다.

설정파일 생성 및 예제
^^^^^^^^^^^^^^^^^^^^^

Polaris 설정파일
""""""""""""""""

::

	[p2p]
	netprotocoladdr = "192.168.0.2"  # 외부에서 접근 가능한 IP주소나 domain name 
	netprotocolport = 8915           # 기본 포트는 8915이다.
	npbindaddr = ""                  
	npkey = "mychain-polaris.key"    # 개인키 파일 위치

	[rpc]
	netserviceaddr = "127.0.0.1"     # RPC접근 주소. 기본 설정인 127.0.0.1로, 이 경우 로컬 머신에서만 RPC접근이 가능하다. 
	netserviceport = 9915

	[polaris]
	allowprivate = true              # aergo 노드의 접근 주소에 사설망 주소를 허용할지 여부. 테스트나 사설망 안에서 운영하는 비공개체인용 Polaris를 구축할 경우 사용 
	genesisfile = "mychain-genesis.json" # genesis 파일 위치

개인키 파일
"""""""""""
aergocli에서 keygen 명령을 이용해 생성할 수 있다.

genesis 파일
""""""""""""
aergosvr의 genesis 파일을 참조하라.

로그 설정 파일
""""""""""""""
aergosvr의 arglog.toml 파일을 참조하라.


Polaris 서버 실행
-----------------

Docker를 이용해 실행하기
^^^^^^^^^^^^^^^^^^^^^^^^
TODO

수동으로 실행하기
^^^^^^^^^^^^^^^^^

수동으로 빌드해서 생생한 polaris 실행파일을 아래와 같은 형식으로 실행한다.

::

	./polaris --config mychain-polaris.toml


Colaris
=======

Polaris RPC 접속을 위한 클라이언트이다. 

Building colaris
----------------
Polaris와 마찬가지로 aergo의 하위 모듈로 빌드한다.

1. github.com/aergoio/aergo 에서 소스를 받는다.
2. make colaris 로 실행파일을 빌드한다.

Colaris CLI 실행
----------------

aergocli와 동일한 인터페이스이다.

::

	./colaris [flags] <command> [[arg1]...]


명령
^^^^

node
""""
Polaris의 actor 상태를 반환한다.

current
"""""""
Polaris에 등록된 노드 목록을 반환한다.


