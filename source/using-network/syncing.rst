Syncing
=======

This setup assumes some basic knowledge about using Docker.
Using Docker is the recommended way to run an Aergo node as it requires minimal configuration and
enables you to use Docker features such as logging and restart policies.

.. note::
   
   When you run the Docker image named `aergo/node` for the first time, Docker downloads the latest image automatically.
   To update the image to the latest version, run :code:`docker pull aergo/node`.
   You can also specify a specific version by replacing :code:`aergo/node` with, for example, :code:`aergo/node:1.0`.
   Refer to `Docker Hub <https://hub.docker.com/r/aergo/node/>`_ to see a list of available tags.

Running
-------

This is the main command to run a node syncing with the main Aergo network:

.. code-block:: shell

    docker run -v $(pwd)/data:/aergo/data -p 7845:7845 -p 7846:7846 aergo/node

The :code:`-p` argument maps ports from the aergo server inside the container to your host machine.
7846 is for the peer to peer protocol.
7845 is for the RPC API for connecting clients to the server; you can remove this port binding to disallow access to the API.

When running this command for the first time, it will create the default genesis block (block number 0).
Afterwards it automatically starts synchronizing. 

.. note::
   If your machine is behind a NAT (such as a router), you need to setup manual forwarding of the port 7846 to allow other peers to sync with your node.

Testnet
"""""""

To sync with the testnet instead, use the testnet flag:

.. code-block:: shell

    docker run -v $(pwd)/data:/aergo/data -p 7845:7845 -p 7846:7846 aergo/node aergosvr --home /aergo --testnet

Configuration
-------------

The above commands use default configuration files suitable to connect to officially supported networks.
If you want to override configuration parameters, you can supply a custom config file using a Docker volume, for example:

.. code-block:: shell

    docker run -v $(pwd)/data:/aergo/data -v $(pwd)/config.toml:/aergo/config.toml -p 7846:7846 aergo/node aergosvr --home /aergo --config /aergo/config.toml

Please refer to `Configuration <../running-node/configuration.html>`_ for a detailed explanation of the available settings.

.. note::

    To customize the log format, place an `arglog.toml <../running-node/configuration.html#logging-options>`_ file in the container's working directory: :code:`-v $(pwd)/arglog.toml:/aergo/arglog.toml`.

Server key file
"""""""""""""""

Every node is identified by a key and peer id. In the default configuration, a key is generated every time you launch the docker container.
This may not be what you want.
You can persist the generated key by adding a volume :code:`-v $(pwd)/auth:/aergo/auth`.
You can also supply the key manually by placing `aergo-peer.key` and `aergo-peer.id` files in that volume.

Refer to `Configuration <../running-node/configuration.html>`_ for details about server keys.

.. tip::

    Instead of binding several volumes, you can use one combined volume, e.g. :code:`-v $(pwd)/aergo:/aergo`.

Node discovery
""""""""""""""

Without any specific settings, the server connects to Polaris, registers itself, obtains addresses of other nodes, and automatically attempts to connect to those nodes.
If the server is in a NAT environment or has multiple NICs, additional settings are required for external nodes to access the server.
You have to setup the external address in the external network connection and set the internal address for binding address.

.. code-block:: toml

    netprotocoladdr = "211.12.34.56" # external address to which other peer can connect
    netprotocolport = 7846
    npbindaddr = "192.168.0.2" # no config element or empty string means using same address as external 
    npbindport = 17846 # negative number means it is same as external port, in this case 7846

