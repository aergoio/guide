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

    docker run -p 7845:7845 -v $(pwd)/config.toml:/aergo/config.toml aergo/node

If you want to override other command line options as well, you need to pass :code:`--config` as well:

.. code-block:: shell

    docker run -p 7845:7845 -v $(pwd)/config.toml:/aergo/config.toml aergo/node aergosvr --config /aergo/config.toml --testmode
