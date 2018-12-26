Syncing
=======

To access the Testnet, you must first have the same genesis-block as the public Testnet. For this, you create a genesis-block.

.. code-block:: shell

    docker run --rm -v $(pwd)/:/aergo/ aergo/node aergosvr init /aergo/genesis-testnet.json --dir /aergo/data --config /aergo/config.toml

When you run a server after creating genesis-block, the server automatically starts synchronizing after accessing the network. 

.. code-block:: shell

    docker run --rm -v $(pwd)/:/aergo/ aergo/node aergosvr --config /aergo/config.toml
