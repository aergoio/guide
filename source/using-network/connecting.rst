Connecting to Well-known Nodes
==============================

Mainnet
-------

Use `mainnet-api.aergo.io` and the default port 7845 to connect to a public GRPC server for the mainnet.

.. code-block:: shell

    docker run --rm aergo/tools aergocli -H mainnet-api.aergo.io blockchain

A public blockchain explorer is available at `mainnet.aergoscan.io <https://mainnet.aergoscan.io>`_.

.. testnet:

Testnet
-------

Use `testnet-api.aergo.io` and the default port 7845 to connect to a public GRPC server for the testnet.

.. code-block:: shell

    docker run --rm aergo/tools aergocli -H testnet-api.aergo.io blockchain

A public blockchain explorer is available at `testnet.aergoscan.io <https://testnet.aergoscan.io>`_.

Alpha
-----

Use `alpha-api.aergo.io` and the default port 7845 to connect to a public GRPC server for the alpha network.

.. code-block:: shell

    docker run --rm aergo/tools aergocli -H alpha-api.aergo.io blockchain

A public blockchain explorer is available at `alpha.aergoscan.io <https://alpha.aergoscan.io>`_.