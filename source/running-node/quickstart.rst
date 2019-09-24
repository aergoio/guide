Quickstart
==========

The easiest way to run a local Aergo server is using Docker.

.. code-block:: shell

    docker run -p 7845:7845 aergo/node

You can pass arguments to the server like this:

.. code-block:: shell

    docker run -p 7845:7845 aergo/node aergosvr --testmode

To supply your own config file, use:

.. code-block:: shell

    docker run -p 7845:7845 -v $(pwd)/config.toml:/aergo/config.toml aergo/node aergosvr --config /aergo/config.toml

**Building from source:** You can also build the binaries yourself from source. `See here for details <../contribution/building-from-source.html>`_.

**Syncing with public networks:** Please refer to the `syncing guide <../using-network/syncing.html>`_ for more instructions.