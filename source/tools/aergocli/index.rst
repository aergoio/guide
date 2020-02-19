Aergocli
========

**aergocli** is a command line tool that interfaces with the GRPC exposed by **aergosvr**.

.. contents:: :local:

.. toctree::
   :maxdepth: 2

   examples

Installation
------------

**Using the binary**

You can find the latest binary release on `Github <https://github.com/aergoio/aergo/releases/latest>`__.
Just download and extract the archive.

**Using Docker**

You can also use the Docker image `aergo/tools <https://hub.docker.com/r/aergo/tools>`__.
Example: :code:`docker run --rm aergo/tools aergocli version`

Usage
-----

In order to use all features of aergocli you will need to have the end point (IP address and port number) to a aergosvr instance.

For a list of all commands known to :code:`aergocli`, simply run it with no arguments.
To see the usage of sub commands, run those without arguments, e.g. :code:`aergocli account`.

.. code-block:: text

    Usage:
      aergocli [command]

    Available Commands:
      account          Account command
      blockchain       Print current blockchain status
      bp               show BP list
      chaininfo        Print current blockchain information
      chainstat        Print chain statistics
      cluster          Cluster command for raft consensus
      committx         commit transaction to aergo server
      contract         Contract command
      enterprise       Enterprise command
      event            Get event
      getblock         Get block information
      getconsensusinfo Print consensus info
      getpeers         Get Peer list
      getstate         Get account state
      gettx            Get transaction information
      help             Help about any command
      keygen           Generate private key
      listblocks       Get block headers list
      metric           Show metric informations
      name             Name command
      node             Show internal metric
      receipt          Receipt command
      sendtx           Send transaction
      serverinfo       Show configs and status of server
      signtx           Sign transaction
      verifytx         Verify transaction
      version          Print the version number of Aergocli
      votestat         show voting stat

    Flags:
          --config string          config file (default is cliconfig.toml)
      -h, --help                   help for aergocli
          --home string            aergo cli home path
      -H, --host string            Host address to aergo server (default "localhost")
          --keystore string        Path to keystore (default "$HOME/.aergo")
          --node-keystore          use node keystore
      -p, --port int32             Port number to aergo server (default 7845)
          --tlscacert string       aergosvr CA certification file for TLS
          --tlscert string         client certification file for TLS
          --tlskey string          client key file for TLS
          --tlsservername string   aergosvr name for TLS

    Use "aergocli [command] --help" for more information about a command.

Local or remote keystore?
-------------------------

.. versionchanged:: 2.2.0

Since version 2.2.0, aergocli uses a local keystore by default.
When you create or import accounts, they are saved on the local machine in an encrypted keystore file format.
The default storage directory is $HOME/.aergo, but you can change it by passing the :code:`--keystore` flag.
Whenever you sign or send a transaction, these locally saved accounts are used.

In aergocli versions before 2.2.0, account actions would use an account that is stored on the remote server (full node)
you are connected to. This is fine if you are running your own full node, but for the vast majority of users who want to
connect to publicly available nodes, this is not useful. That is why the default behavior was changed to use local keystores.

Continue using the old method (server-based accounts)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To use the pre-2.2 method of selecting accounts from the connected server,
pass the :code:`--node-keystore` flag to aergocli.
Remember you then have to unlock accounts before using them on the server. Check out the `examples <./examples.html#using-remote-node-based-accounts>`__.

Migration from remote to local keystore
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To migrate from the old to the new storage format,
first export an account from your node as a keystore file, and then import it into your local keystore:

.. code-block:: text

    # Export from remote
    $ aergocli -H YOUR_NODE_IP --node-keystore account export --address YOUR_ADDRESS > YOUR_ADDRESS__keystore.txt

    # Import locally
    $ aergocli account import --path YOUR_ADDRESS__keystore.txt

.. seealso:: In version 2.2.0, the internal keystore method was also changed. `See here <../../running-node/configuration.html#account-storage>`__ for details.
