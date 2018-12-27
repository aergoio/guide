Syncing
=======

To access the Testnet, you must first have the same genesis-block as the public Testnet. For this, you create a genesis-block.

.. code-block:: shell

    docker run -v $(pwd)/data:/aergo/data aergo/node aergosvr init /aergo/testnet-genesis.json

When you run a server after creating genesis-block, the server automatically starts synchronizing after accessing the network. 

.. code-block:: shell

    docker run -v $(pwd)/data:/aergo/data aergo/node
