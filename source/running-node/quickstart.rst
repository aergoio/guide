Quickstart
==========

Using Docker
------------

The easiest way to run a local Aergo server is using Docker. An up-to-date Docker image is always available on `Docker Hub <https://hub.docker.com/r/aergo/node/>`_.

.. code-block:: shell

    docker run -p 7845:7845 aergo/node

You can pass arguments to the server like this:

.. code-block:: shell

    docker run -p 7845:7845 aergo/node aergosvr --testmode

To supply your own config file, use:

.. code-block:: shell

    docker run -p 7845:7845 -v $(pwd)/config.toml:/aergo/config.toml aergo/node aergosvr --config /aergo/config.toml

**Syncing with public networks:** Please refer to the `syncing guide <../using-network/syncing.html>`_ for further instructions.

Using binaries
--------------

You can download the most recent binary releases from `Github <https://github.com/aergoio/aergo/releases>`_.

Building from source
--------------------

You can also build the binaries yourself from source. `See here for details <../contribution/building-from-source.html>`_.
