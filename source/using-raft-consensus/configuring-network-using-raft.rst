Configuring a Network Using Raft
================================

이 글은 Raft Consensus를 사용하여 여러 block producers의 network를 설정하기 위한 과정을 설명한다.
Raft consensus는 오직 private network를 위해서만 사용 될 수 있다.
Raft consensus를 이용하여 구성된 block chain network를 Raft Cluster라고 부른다.

아래의 모든 설정은 3 node로 구성된 blockchain network가 Raft consensus를 사용하기 위한 경우를 가정하고 설명한다.

Raft consensus를 이용한 blockchain network를 생성하기 위해서는 genesis block과 config file에 Raft를 위한 설정을 해야 한다.
그 이외 설정은 DPoS를 사용하는 private network 설정과 동일하다.

Common Configuration (per machines)
-----------------------------------
Aergo node를 block producer로 설정하기 위해서는 `Configuring a Network <../running-node/configure-network.html#configuring-a-network>`__ 페이지에 설명된 공통의 설정을 필요로 한다.

단 genesis block과 config 파일에 추가적인 설정을 해주면 된다.


Write configuration files for Raft
-----------------------------------
Raft cluster생성시 포함될 initial 노드들은 genesis block에 추가되어야 한다. network에 포함되는 모든 노드는 block producer가 될수 있는 후보가 된다. 이 노드들 >중 한 노드만이 Leader가 되어 BP로서 블럭을 생성한다. 나머지 노드는 leader로 부터 지시를 받는 follower가 된다. 
Leader가 crash되면 follower노드들중 하나가 새롭게 leader가 된다. 


BP가 될수 없는 노드들은 Fullnode로 추가하여 사용 할 수 있다. 이 노드들은 P2P 프로토콜을 통해 블럭은 전파받지만 Raft Cluster에는 포함되지 않는
다.

**genesis.json** (same for all machines)

- Raft는 public network에서는 지원하지 않으므로 chain_id는 public과 maninet을 반드시 false로 지정하여야 한다. 
- Network에 포함될 노드를 enterprise_bps 항목에 추가해 준다. enterprise_bps 항목에는 원하는 bp의 name, address, peerid를 추가해 준다.  enterprise_bps의 address 항목은 config 파일의 p2p peer format을 사용한다.
- consensus 항목은 "raft"로 설정한다.
- bps 항목은 빈 list로 설정한다.

.. code-block:: json

    {
        "chain_id":{
            "magic": "[insert an identifier string for your network]",
            "public": false,
            "mainnet": false,
            "consensus": "raft"
        },
        "timestamp": 1548918000000000000,         
        "balance": {
            "[insert address from genesis]": "470000000000000000000000000",
            "[insert address from bp01]": "10000000000000000000000000",
            "[insert address from bp02]": "10000000000000000000000000",
            "[insert address from bp03]": "10000000000000000000000000"        },
        "bps": [
        ],
        "enterprise_bps": [
          {
            "name": "[NICKNAME OF BP 01]",
            "address": "/ip4/[IP ADDRESS FROM BP 01]/tcp/7846",
            "peerid": "[PEER ID FROM BP 01]",
          },
          {
            "name": "[NICKNAME OF BP 02]",
            "address": "/ip4/[IP ADDRESS FROM BP 02]/tcp/7846",
            "peerid": "[PEER ID FROM BP 02]",

          },
          {
            "name": "[NICKNAME OF BP 03]",
            "address": "/ip4/[IP ADDRESS FROM BP 03]/tcp/7846"
            "peerid": "[PEER ID FROM BP 03]"
          }
        ]
    }

**config.toml** (one per machine)

.. code-block:: toml

    # aergo TOML Configration File (https://github.com/toml-lang/toml)
    ...
    [ insert common configuration for block producer ]

    [p2p]
    ...
    npaddpeers = [
    ]
    ...
    ...
    # configuration for block producer using RAFT
    [consensus.raft]
    newcluster=true
    name="[NICKNAME OF THIS BP]"


기본적으로 DPoS를 사용할 때의 config는 동일하게 적용해야 한다. 하지만 [p2p].npaddpeers 설정은 하지 않아도 된다. 노드들 사이의 p2p connection은 enterprise_bps에 설정된 `address` 값을 이용해서 연결된다. 

RAFT를 사용하기 위해서는 [consensus.raft] section이 추가되어야 한다.
`name`에는 현재 node의 nickname을 적는다. name은 genesis block의 enterprise_bps에 포함되어 있어야 한다.
network를 최초로 생성할 때는 newcluster=true 로 설정해야 한다.

그외 설정
---------
** block 생성주기를 변경 하고자 하는 경우 **

.. code-block:: toml
    
    [consensus]
    blockinterval=[insert value in seconds]

** Heartbeat 주기를 변경 하고자 하는 경우 **

.. code-block:: toml

  [consensus.raft]
  heartbeattick=[insert value in milliseconds ]

** 빈 block은 생성하지 않고자 하는 경우 **

.. code-block:: toml

  [consensus.raft]
  skipempty=true
k
