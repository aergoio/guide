Syncing
=======

This setup assumes some basic knowledge about using Docker.

Initializing
------------

To access the testnet, you must first have the same genesis block. For this, you create a genesis block.

.. code-block:: shell

    docker run -v $(pwd)/data:/aergo/data aergo/node aergosvr init /aergo/testnet-genesis.json

testnet-genesis.json is the specification of the testnet genesis block.
This file is `included <https://github.com/aergoio/aergo-docker/blob/8dbb2eeec271e2b6371587512614fc57e2dd7360/node/testnet-genesis.json>`_ in the Docker image,
so you can use the above command without any additional setup.

.. note::
   
   When you run the Docker image named `aergo/node` for the first time, Docker downloads the latest image automatically.
   To update the image to a new version, run :code:`docker pull aergo/node`.
   You can also specify a specific version by replacing :code:`aergo/node` with :code:`aergo/node:0.9.9`.
   Refer to `Docker Hub <https://hub.docker.com/r/aergo/node/>`_ to see a list of available tags.

Running
-------

When you run the server after creating the genesis block, it automatically starts synchronizing. 

.. code-block:: shell

    docker run -v $(pwd)/data:/aergo/data -p 7846:7846 aergo/node

If you want to connect to the RPC API, you also need to specify :code:`-p 7845:7845` to map the Docker container's internal port to the host machine.

.. note::
   If your machine is behind a NAT (such as a router), you need to setup manual forwarding of the port 7846 to allow other peers to sync with your node.
