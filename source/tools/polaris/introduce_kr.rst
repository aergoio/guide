Introduction
============

Polaris는 aergoserver노드가 다른 노드에 접속하기 위한 노드 정보를 제공하는 서버이다.

0.11 버전 이전에는 아르고 풀 노드 기동 시, 설정파일 내 npaddpeers에 연결할 노드 리스트 정보를 작성하여 지정된 블록체인 소속 노드 그룹들과 연결하였다. 

하지만 0.11 버젼 이후부터는 새로운 Polarais 서버를 통해 노드 리스트 정보를 작성하지 않아도 지정된 블록체인 소속 노드 그룹들과 자동으로 연결할 수 있는 기능이 추가되었다.

기능
----

* 하나의 Polaris는 하나의 지정된 블록체인만 담당할 수 있다. 
* aergo 노드는 다른 aergo노드를 조회할 수 있다. 이 때 aergo 노드의 체인과 Polaris가 담당하는 체인이 동일해야 조회가 성공한다.
* aergo 노드는 자신을 Polaris에 등록할 수 있다. Polaris는 해당 aergo 노드로 접속이 가능한지 확인한 후 노드 목록에 추가한다.

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
개인키 파일 생성
""""""""""""""""
aergocli에서 keygen 명령을 이용해 생성할 수 있다.

::

	aergocli keygen mychain-polaris
	Wrote files mychain-polaris.{key,pub,id}.

genesis 파일 생성
"""""""""""""""""
aergosvr의 genesis 파일을 참조하라.

::

	{
	    "chain_id":{
	        "magic": "[insert an identifier string for your network]",
	        "public": false,
	        "mainnet": false,
	        "coinbasefee": "1000000000",
	        "consensus": "dpos"
	    },
	    "timestamp": 1548918000000000000,
	    "balance": {
	    },
	    "bps": [
	    ]
	}

Polaris 설정파일
""""""""""""""""

::

	[rpc]
	netserviceaddr = "127.0.0.1"               # RPC접근 주소. 기본 설정인 127.0.0.1로, 이 경우 로컬 머신에서만 RPC접근이 가능하며, 원격에서 RPC접속을 차단한다.
	netserviceport = 8915

	[p2p]
	netprotocoladdr = "[real IP address]"      # 외부에서 접근 가능한 IP주소나 domain name 
	netprotocolport = 8916                     
	npbindaddr = ""                  
	npkey = "mychain-polaris.key"              # 개인키 파일 위치

	[polaris]
	allowprivate = true                        # aergo 노드의 접근 주소에 사설망 주소를 허용할지 여부. 테스트나 사설망 안에서 운영하는 비공개체인용 Polaris를 구축할 경우 사용 
	genesisfile = "[location of genesis file]" # genesis 파일 위치


로그 설정 파일
""""""""""""""
aergosvr의 arglog.toml 파일을 참조하라.


Polaris 서버 실행
-----------------

Docker를 이용해 실행하기
^^^^^^^^^^^^^^^^^^^^^^^^
::

	docker run -d -w /tools -v /blockchain/polaris:/tools -p 9915:9915 -p 8915:8915 --restart="always" --name polaris-node aergo/polaris:latest polaris --home /tools --config /tools/polaris-conf.toml

수동으로 실행하기
^^^^^^^^^^^^^^^^^

수동으로 빌드해서 생생한 polaris 실행파일을 아래와 같은 형식으로 실행한다.

::

	./polaris --config polaris-conf.toml


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


flag
^^^^

1. -H <hostname> 원격 서버에 요청하는 경우 주소. 기본값은 localhost (127.0.0.1) 
2. -p <portnumber> RPC 포트 번호, 기본값은 8915

command
^^^^^^^

node
""""
Polaris의 actor 상태를 반환한다.

current
"""""""
Polaris에 등록된 노드 목록을 반환한다.

예:

:: 

	ubuntu@mypolaris:/blockchain/polaris$ ./colaris -p 8915 current
	{
	 "total": 1,
	 "peers": [
	  {
	   "address": {
	    "address": "52.231.31.38",
	    "port": 7846,
	    "peerID": "16Uiu2HAmBfFABqQ2eWwNMv1A2WJCqVykgPS2sz72jrYTHeZgyors"
	   },
	   "connected": 1549526282,
	   "lastCheck": 1549526463
	  }
	 ]
	}

