Syncing
=======

This setup assumes some basic knowledge about using Docker.

.. note::
   
   When you run the Docker image named `aergo/node` for the first time, Docker downloads the latest image automatically.
   To update the image to a new version, run :code:`docker pull aergo/node`.
   You can also specify a specific version by replacing :code:`aergo/node` with :code:`aergo/node:0.12.0`.
   Refer to `Docker Hub <https://hub.docker.com/r/aergo/node/>`_ to see a list of available tags.

Running
-------

When you run the server after creating the genesis block, it automatically starts synchronizing. 

.. code-block:: shell

    docker run -v $(pwd)/data:/aergo/data -p 7845:7845 -p 7846:7846 aergo/node

The :code:`-p` argument maps ports from the aergo server inside the container to your host machine.
7846 is for the peer to peer protocol.
7845 is for the RPC API; you can remove this port binding to disallow access to the API.

.. note::
   If your machine is behind a NAT (such as a router), you need to setup manual forwarding of the port 7846 to allow other peers to sync with your node.

Configuration
-------------

The above commands use a default configuration file included in the Docker image that is suitable to connect to the testnet.
If you want to override configuration parameters, you can create a local copy of `testnet.toml <https://github.com/aergoio/aergo-docker/blob/1ad16cf7881d9ba8f2efc350cf609c9416e76666/node/testnet.toml>`_
and override the included version using a Docker volume, for example:

.. code-block:: shell

    docker run -v $(pwd)/data:/aergo/data -v $(pwd)/testnet.toml:/aergo/testnet.toml -p 7846:7846 aergo/node

Please refer to `Configuration <../running-node/configuration.html>`_ for a detailed explanation of the available settings.

.. note::

    To customize the log format, place an `arglog.toml <../running-node/configuration.html#logging-options>`_ file in the container's working directory: :code:`-v $(pwd)/arglog.toml:/aergo/arglog.toml`.
