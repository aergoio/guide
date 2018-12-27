Creating Accounts
=================

Accounts are identified by `addresses <../specs/addresses.html>`_ that belong to private keys. To own an account, all you need is access to its private key.
Always make sure that nobody except you gains access to your keys!

There are several methods to create accounts.

1. If you are running your own node, you can create a local account:

   .. code-block:: shell
   
        docker run --rm --net=host aergo/node aergocli account new

2. To create an account without a node (aka offline):

   .. code-block:: shell
   
        docker run --rm aergo/tools aergocli keygen --json --password yourPassword

   Now you can see your new private key (shown in encrypted form) and the corresponding address.
   Using the encrypted private key and password, you can import this account to other nodes or wallets.
