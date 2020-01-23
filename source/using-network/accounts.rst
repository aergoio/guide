Creating Accounts
=================

Accounts are identified by `addresses <../specs/addresses.html>`_ that belong to private keys. To own an account, all you need is access to its private key.
Always make sure that nobody except you gains access to your keys!

There are several methods to create accounts.

1. If you are **running your own node** on your machine, you can create a local account:

   .. code-block:: shell
   
        aergocli account new
        
        # Or using Docker:
        docker run --rm --net=host aergo/tools aergocli account new

2. To create an account **without a node** (aka offline):

   .. code-block:: shell
   
        aergocli account new --path ./wallet
        
        # Or using Docker:
        docker run --rm -it -v $(pwd)/wallet:/wallet aergo/tools aergocli account new --path /wallet

   The output shows the address of your new account. It is saved in account/data.db in the given path.

   You can export this new account:

   .. code-block:: shell
   
        aergocli account export --address your_address --password your_password --path ./wallet
        
        # Or using Docker:
        docker run --rm -it -v $(pwd)/wallet:/wallet aergo/tools aergocli account export --address your_address --path /wallet

   The output shows the encrypted private key belonging to your address which can be imported in other wallets.

.. note::

   When `connecting to public nodes <./connecting.html>`_, creating accounts on that node is impossible.
   On public nodes, personal account features are `disabled <../running-node/configuration.html>`_ using :code:`personal = False`.
   Always create accounts using local nodes or offline by one of the methods above.