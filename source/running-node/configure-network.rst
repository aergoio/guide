Configuring a Network
=====================

This article explains the steps needed to configure a network of multiple block producers.
You can follow this guide to setup a private Aergo blockchain network.

This guide specifically requires no prior setup, so it should be easy to follow along with new bare machines running Ubuntu.
It has been tested with AWS EC2 instances. We will setup three block producers, but you can adjust the procedure to any number of BPs.

Per-machine setup
-----------------

**Install Docker**

You will need Docker for this setup, so let's install that now.

.. code-block:: shell

    sudo apt-get update && sudo apt-get install docker.io
    sudo usermod -aG docker ubuntu

**NTP**

Timing is critical in a blockchain, so it's better to configure the machines' time setup manually:

.. code-block:: shell

    sudo bash
    apt-get install chrony
    vi /etc/chrony/chrony.conf

.. code-block:: text

    server time1.google.com iburst
    server time2.google.com iburst
    server time3.google.com iburst
    server time4.google.com iburst

.. code-block:: shell

    systemctl restart chronyd    
    chronyc makestep

Generate BP accounts and keys
-----------------------------

You can use any machine (e.g. your own local machine) for this.

Start by installing `aergocli <../tools/aergocli.html>`__ if you haven't already.

**Generate accounts**

.. code-block:: shell

    aergocli account new --password yourpassword --path genesis
    (displays generated address)

    aergocli account new --password yourpassword --path bp01
    (displays generated address)

    aergocli account new --password yourpassword --path bp02
    (displays generated address)

    aergocli account new --password yourpassword --path bp03
    (displays generated address)

**Export accounts**

.. code-block:: shell

    aergocli account export --address [insert address from genesis]  --password yourpassword --path genesis
    (displays exported private key)

    aergocli account export --address [insert address from bp01]  --password yourpassword --path bp01
    (displays exported private key)

    aergocli account export --address [insert address from bp02]  --password yourpassword --path bp02
    (displays exported private key)

    aergocli account export --address [insert address from bp03]  --password yourpassword --path bp03
    (displays exported private key)

**Generate peer keys**

.. code-block:: shell

    aergocli keygen bp01
    Wrote files bp01.{key,pub,id}.

    aergocli keygen bp02
    Wrote files bp02.{key,pub,id}.

    aergocli keygen bp03
    Wrote files bp03.{key,pub,id}.

Write configuration files
-------------------------

**genesis.json** (same for all machines)

.. code-block:: json

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
            "[insert address from genesis]": "470000000000000000000000000",
            "[insert address from bp01]": "10000000000000000000000000",
            "[insert address from bp02]": "10000000000000000000000000",
            "[insert address from bp03]": "10000000000000000000000000"
        },
        "bps": [
            "[insert text from bp01.id]",
            "[insert text from bp02.id]",
            "[insert text from bp03.id]"
        ]
    }

**config.toml** (one per machine)

.. code-block:: toml

    # aergo TOML Configration File (https://github.com/toml-lang/toml)
    # base configurations
    datadir = "./data"
    enableprofile = true
    profileport = 6060
    enablerest = true
    personal = false
    
    [rpc]
    netserviceaddr = "0.0.0.0"
    netserviceport = 7845
    nstls = false
    nscert = ""
    nskey = ""
    nsallowcors = false
    
    [p2p]
    netprotocoladdr = "{LOCAL_IP}"  # Insert IP address from this machine
    netprotocolport = 7846
    npbindaddr = "0.0.0.0"
    npbindport = 7846
    nptls = false
    npcert = ""
    npkey = "bp{01,02,03}.key"  # Name of key file of node
    npaddpeers = [
        "/ip4/[IP ADDRESS FROM BP 01]/tcp/7846/p2p/[PEER ID FROM BP 01]",
        "/ip4/[IP ADDRESS FROM BP 02]/tcp/7846/p2p/[PEER ID FROM BP 02]",
        "/ip4/[IP ADDRESS FROM BP 03]/tcp/7846/p2p/[PEER ID FROM BP 03]"
    ]
    
    npexposeself = false
    npusepolaris = false
    nphiddenpeers= [
        "[PEER ID FROM BP 01]",
        "[PEER ID FROM BP 02]",
        "[PEER ID FROM BP 03]"
    ]
    
    [blockchain]
    usefastsyncer = true
    blockchainplaceholder = false
    coinbaseaccount = "[ADDRESS FROM THIS PEER]"
    
    [mempool]
    showmetrics = true
    dumpfilepath = "./data/mempool.dump"
    
    [consensus]
    enablebp = true

Running
-------

We are going to use the Docker image `aergo/node <https://hub.docker.com/r/aergo/node/>`__ to run the server.
Please refer to the `Docker documentation <https://docs.docker.com/engine/reference/run/>`__ for learn about the available run options.

**Check the directory contents**

After following the above steps, you should now have these files in one directory for each machine. In this example we use the path /blockchain, but you can use any directory
as long as you substitute its local path in the Docker run commands below.

.. code-block:: shell

    /blockchain/
        bp{01,02,03}.key
        genesis.json
        config.toml

**Create genesis block**

.. code-block:: shell

    docker run --rm \
        -v /blockchain:/aergo \
        aergo/node \
        aergosvr init /aergo/genesis.json --dir /aergo/data --config /aergo/config.toml

**Start the node**

.. code-block:: shell

    docker run -d --log-driver json-file --log-opt max-size=1000m --log-opt max-file=7 \
        -v /blockchain:/aergo \
        -p 7845:7845 -p 7846:7846 -p 6060:6060 \
        --restart="always" --name aergo-node \
        aergo/node \
        aergosvr --home /aergo --config /aergo/config.toml

Further reading
---------------

You may now want to setup further `full nodes <configuration.html>`__ as well as `Polaris <../tools/polaris.html>`__ for automatic node discovery.