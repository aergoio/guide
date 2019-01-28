=======================
Tutorial - Custom Chain
=======================

개요
====

이 문서는 aergo 프로젝트로 사용자 전용 체인을 구축하는 방법을 소개한다.

노드 구성
---------

아래와 같은 형태의 노드 구성을 가정한다.

.. image:: aergo_network.png


1. BP는 블록을 생성하며 자신의 담당 FullNode 와 신뢰할 수 있는 다른 BP와 연결된다.
2. FullNode는 담당 BP에 연결하며 다른 노드와 동기화를 수행하거나 API를 제공한다.
3. Polaris는 FullNode가 다른 노드의 정보를 받아와 접속할 수 있도록 노드 목록을 관리한다.
   

체인 구축 순서
==============

전체 노드 구성 정보
-------------------
1. Polaris 노드 1
2. BP 노드 7대 : BP노드는 다른 BP 및 내부 FullNode 두 대와 연결됨
3. FullNode 2대 : 

공통 파일 사전 준비
-------------------

key파일 생성
^^^^^^^^^^^^
Polaris 및 서버 노드용 

genesis용 json생성
^^^^^^^^^^^^^^^^^^
chainID와 최초 BP 목록 등을 저장한 genesis 파일을 만든다. 

예) mychain-genesis.json 
::  
	
	{
	    "chain_id":{
	        "magic":"mychain.net",
	        "public":false,
	        "mainnet":true,
	        "coinbasefee":"1000000000",
	        "consensus":"dpos"
	    },
	    "timestamp": 1545195494000000000,
	    "balance": {
	        "AmMK3LZiR1oEf66xzXir7mA5SUVVHSinWUYmh5FwueoVmciH3CuJ": "498700000000000000000000000",
	        "AmLsSfxo9aQRZJBvMBoLFb9QZABQK2RiG3Uq1JBhyAfbDYPf31J2": "100000000000000000000000",
	        "AmPJRLHDKtzLpsaC8ubmPuRkxnMCyBSq5wBwYNDD6DJdgiRhAhYR": "100000000000000000000000",
	        "AmNSmkDtwwSSrGHk4EP8yLP7YaC629fjnPg977JJfLpTe9zXtpsd": "100000000000000000000000",
	        "AmMR1CLHNnBCq7itY5XRX4LqLs5Ve1nZKxAcBRVZtwcsRdrytFqk": "100000000000000000000000",
	        "AmMvmu6HxRhYMjVgih4sXSYyDiM8HpuvZFnP9VHUtUd95qTUENR6": "100000000000000000000000",
	        "AmP5vYXeHBjGuvqDaGrZBoHyTHEJcUztUDvTam2HzqvA9jseqekd": "100000000000000000000000",
	        "AmNEfsKt1v2gosbu6eMg7fGBchkJzvEcmbG8A4EeMyusAU3M38mk": "100000000000000000000000",
	        "AmNJRh4ntQWT145jQwDHQGuHYdATmMWCEbENW2DN7mztfr5pqPbP": "100000000000000000000000",
	        "AmLvTAXSTVAs7tgASWnmZwgtyq7cLdtaoNqxrm11K91Jpc3XrHex": "100000000000000000000000",
	        "AmLqZFnwMLqLg5fMshgzmfvwBP8uiYGgfV3tBZAm36Tv7jFYcs4f": "100000000000000000000000",
	        "AmMLFNs1f5kFxxoG2Pq8wkFi5mJATQve7ZPPBZngHRjo2myU41k6": "100000000000000000000000",
	        "AmPxDZYmc7f6cTEzv8rTkbDxYtziTkRQEBbCW9r5GgVntpSmgXWb": "100000000000000000000000"
	    },
	    "bps": [
	        "16Uiu2HAmQn3nFBGhJM7TnZRguLhgUx1HnpNL2easdt2JrxdbFjtb",
	        "16Uiu2HAmAnQ5jjk7huhepfFtDFFCreuJ21nHYBApVpg8G7EBdwme",
	        "16Uiu2HAkvbHmK1Ke1hqAHmahwTGE4ndkdMdXJeXFE3kgBs17k2oQ",
	        "16Uiu2HAkw9ZZ61iq8uWbrQrmNEXFbrbkWupdqiHSKkCuCFLTM6gF",
	        "16Uiu2HAmUkoPDPHrYYC8J4sVvaVRho8UxfWPLDgZS8gu5bsGSRSA",
	        "16Uiu2HAmNxKsrFQ4Wez4DYHW6o72y2Jpy6RMv5TuqAvjcQ5QPZWw",
	        "16Uiu2HAmDFV41vku39rsMtXBaFT1MFUDyHxXiDJrUDt7gJycSKnX"
	    ]
	}


genesis block 생성
^^^^^^^^^^^^^^^^^^
각 노드마다 이 동작을 반복한다. 앞서 생성한 genesis 파일을 노드에 복사해서 genesis block을 초기화한다.

::

	aergosvr init mychain-genesis.json --dir data

이 결과로 data디렉토리 내부에 genesis block 및 기본 데이터 파일들이 생성된다.

Polaris 구동
------------
앞서 생성한 genesis 파일을 polaris 노드에 복사해 사용한다.

Polaris 설정파일
^^^^^^^^^^^^^^^^
key파일 및 genesis파일을 설정하고, 사설망 내부 체인/공개 체인 여부에 따른 설정을 한다.

예) mychain-polaris.toml
::

	[rpc]
	netserviceaddr = "127.0.0.1"
	netserviceport = 9915
	[p2p]
	netprotocoladdr = "192.168.0.2"
	netprotocolport = 8915
	npbindaddr = ""
	npkey = "mychain-polaris.key"

	[polaris]
	allowprivate = true
	genesisfile = "mychain-genesis.json"

Polaris 서버 실행
^^^^^^^^^^^^^^^^^
::

	./polaris --config mychain-polaris.toml


aergosvr 구동
-------------

aergosvr 설정파일
^^^^^^^^^^^^^^^^^
예) mychain-bp01.toml
::

	# aergo TOML Configration File (https://github.com/toml-lang/toml)
	# base configurations
	datadir = "/blockchain/aergo/data"
	dbtype = "badgerdb"
	enableprofile = false

	[rpc]
	netserviceaddr = "127.0.0.1"
	netserviceport = 7845
	netservicetrace = false

	[p2p]
	netprotocoladdr = "192.168.0.11"
	netprotocolport = 7846
	# Set file path of key file
	npkey = "/blockchain/aergo/auth/mychain-bp01.key"
	npusepolaris= true
	npaddpolarises = [
	    "/ip4/192.168.0.2/tcp/8915/p2p/16Uiu2HAmJCmxe7CrgTbJBgzyG8rx5Z5vybXPWQHHGQ7aRJfBsoFs"
	]

aergosvr 서버 실행
^^^^^^^^^^^^^^^^^^
::

	./aergosvr --config mychain-bp01.toml


