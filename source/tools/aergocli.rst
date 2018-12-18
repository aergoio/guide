Aergocli
========

**aergocli** is a command line tool that interfaces with the GRPC exposed by **aergosvr**.

In order to use all features of aergocli you will need to have the end point (IP address and port number) to a aergosvr instance.

For a list of all commands known to :code:`aergocli`, simply run it with no arguments.

.. code-block:: text

    Usage:
    aergocli [command]

    Available Commands:
    account     account command
    blockchain  Print current blockchain status
    committx    Send transaction
    contract    contract command
    getblock    Get block information
    getpeers    Get Peer list
    getstate    Get account state
    gettx       Get transaction information
    help        Help about any command
    keygen      Generate private key
    listblocks  Get block headers list
    node        Show internal metric
    receipt     receipt command
    sendtx      Send transaction
    signtx      Sign transaction
    verifytx    Verify transaction
    version     Print the version number of Aergocli
    
    Flags:
        --config string   config file (default is config.toml) (default "config.toml")
    -h, --help            help for aergocli
        --home string     aergo home path (default ".")
    -H, --host string     Host address to aergo server (default "localhost")
    -p, --port int32      Port number to aergo server (default 7845)

    Use "aergocli [command] --help" for more information about a command.

Examples
--------

Create account
~~~~~~~~~~~~~~

.. code-block:: shell

    $ aergocli account new --password yourpasswordhere
    AmP96xd6WYRe1jrWixC3VyFXRCzwZxAJtU3Uc54vn182xG9Yn9Cs

or 

.. code-block:: shell

    $ aergocli account new
    Enter Password:
    Repeat Password:
    AmP96xd6WYRe1jrWixC3VyFXRCzwZxAJtU3Uc54vn182xG9Yn9Cs

Now we can use the account in aergosvr.
Please remember the password carefully because there is no way to retrieve the password again.

Send transaction
~~~~~~~~~~~~~~~~

Before request send transaction you must unlock your account first

.. code-block:: shell

    $ aergocli account unlock --address AmQFgm1gCvoRw2RfBXnipRmeCLEc6tTQ1kBMmLEzHjp91xYnXK78
    Enter Password:
    AmQFgm1gCvoRw2RfBXnipRmeCLEc6tTQ1kBMmLEzHjp91xYnXK78

or

.. code-block:: shell

    $ aergocli account unlock --address AmQFgm1gCvoRw2RfBXnipRmeCLEc6tTQ1kBMmLEzHjp91xYnXK78 --password yourpasswordhere
    AmQFgm1gCvoRw2RfBXnipRmeCLEc6tTQ1kBMmLEzHjp91xYnXK78

and then

.. code-block:: shell

    $ ./aergocli sendtx --from AmQFgm1gCvoRw2RfBXnipRmeCLEc6tTQ1kBMmLEzHjp91xYnXK78 --to AmLnVfGwq49etaa7dnzfGJTbaZWV7aVmrxFes4KmWukXwtooVZPJ --amount 1000
    75yMzPTS2oFJYvfj6QDxPsuYXVnwgQvKh1xaYgfuqGhJ TX_OK

Look up transaction
~~~~~~~~~~~~~~~~~~~

.. code-block:: shell

    $ aergocli gettx 75yMzPTS2oFJYvfj6QDxPsuYXVnwgQvKh1xaYgfuqGhJ

    Confirm:  {
    "TxIdx": {
    "BlockHash": "GGT9wahqcKKGKUncMuhRLLL3JaCs2MEBx7V8UdrK9JNi",
    "Idx": 0
    },
    "Tx": {
    "Hash": "75yMzPTS2oFJYvfj6QDxPsuYXVnwgQvKh1xaYgfuqGhJ",
    "Body": {
    "Nonce": 1,
    "Account": "AmQFgm1gCvoRw2RfBXnipRmeCLEc6tTQ1kBMmLEzHjp91xYnXK78",
    "Recipient": "AmLnVfGwq49etaa7dnzfGJTbaZWV7aVmrxFes4KmWukXwtooVZPJ",
    "Amount": 1000,
    "Payload": "",
    "Limit": 0,
    "Price": 0,
    "Type": 0,
    "Sign": "AN1rKvt8ZQ3Dg7UU86NqmTPmwgmQTkp7WqMDGmqvYBXyYoDiLTx6WyCmhqTcYUMXBW5NYvCeDviYPWyMniEsnYsYz2AdXFCno"
    }
    }
    }

Check block
~~~~~~~~~~~

.. code-block:: shell

    $ aergocli getblock --hash GGT9wahqcKKGKUncMuhRLLL3JaCs2MEBx7V8UdrK9JNi

    {
    "Hash": "GGT9wahqcKKGKUncMuhRLLL3JaCs2MEBx7V8UdrK9JNi",
    "Header": {
    "PrevBlockHash": "49E5xQxnuDnhSa2qZNb59qDRd48d8vsWFQyxuc33DijV",
    "BlockNo": 7454,
    "Timestamp": 1540968429100585000,
    "BlockRootHash": "",
    "TxRootHash": "75yMzPTS2oFJYvfj6QDxPsuYXVnwgQvKh1xaYgfuqGhJ",
    "Confirms": 0,
    "PubKey": "",
    "Sign": ""
    },
    "Body": {
    "Txs": [
    {
        "Hash": "75yMzPTS2oFJYvfj6QDxPsuYXVnwgQvKh1xaYgfuqGhJ",
        "Body": {
        "Nonce": 1,
        "Account": "AmQFgm1gCvoRw2RfBXnipRmeCLEc6tTQ1kBMmLEzHjp91xYnXK78",
        "Recipient": "AmLnVfGwq49etaa7dnzfGJTbaZWV7aVmrxFes4KmWukXwtooVZPJ",
        "Amount": 1000,
        "Payload": "",
        "Limit": 0,
        "Price": 0,
        "Type": 0,
        "Sign": "AN1rKvt8ZQ3Dg7UU86NqmTPmwgmQTkp7WqMDGmqvYBXyYoDiLTx6WyCmhqTcYUMXBW5NYvCeDviYPWyMniEsnYsYz2AdXFCno"
        }
    }
    ]
    }
    }

Sign transaction
~~~~~~~~~~~~~~~~

After unlock the account

.. code-block:: shell

    $ aergocli signtx --jsontx \
                            "{\"account\":\"AmNBZ8WQKP8DbuP9Q9W9vGFhiT8vQNcuSZ2SbBbVvbJWGV3Wh1mn\", \
                            \"nonce\": 2 , \
                            \"price\": 1 , \
                            \"limit\": 100 , \
                            \"recipient\":\"AmLnVfGwq49etaa7dnzfGJTbaZWV7aVmrxFes4KmWukXwtooVZPJ\", \
                            \"type\": 0, \
                            \"amount\": 25000 }"
    {
    "Hash": "HB44gJvHhVoEfgiGq3VZmV9VUXfBXhHjcEvroBMkJGnY",
    "Body": {
    "Nonce": 2,
    "Account": "AmNBZ8WQKP8DbuP9Q9W9vGFhiT8vQNcuSZ2SbBbVvbJWGV3Wh1mn",
    "Recipient": "AmLnVfGwq49etaa7dnzfGJTbaZWV7aVmrxFes4KmWukXwtooVZPJ",
    "Amount": 25000,
    "Payload": "",
    "Limit": 100,
    "Price": 1,
    "Type": 0,
    "Sign": "381yXYxTtq2tRPRQPF7tHH6Cq3y8PvcsFWztPwCRmmYfqnK83Z3a6Yj9fyy8Rpvrrw76Y52SNAP6Th3BYQjX1Bcmf6NQrDHQ"
    }
    }

Commit Transaction
~~~~~~~~~~~~~~~~~~

Send given transactions to **aergosvr**

.. code-block:: shell

    $ aergocli committx --jsontx "{ \
    \"Hash\": \"HB44gJvHhVoEfgiGq3VZmV9VUXfBXhHjcEvroBMkJGnY\", \
    \"Body\": { \
    \"Nonce\": 2, \
    \"Account\": \"AmNBZ8WQKP8DbuP9Q9W9vGFhiT8vQNcuSZ2SbBbVvbJWGV3Wh1mn\", \
    \"Recipient\": \"AmLnVfGwq49etaa7dnzfGJTbaZWV7aVmrxFes4KmWukXwtooVZPJ\", \
    \"Amount\": 25000, \
    \"Payload\": \"\", \
    \"Limit\": 100, \
    \"Price\": 1, \
    \"Type\": 0, \
    \"Sign\": \"381yXYxTtq2tRPRQPF7tHH6Cq3y8PvcsFWztPwCRmmYfqnK83Z3a6Yj9fyy8Rpvrrw76Y52SNAP6Th3BYQjX1Bcmf6NQrDHQ\" \
    } \
    }"
    1 : HB44gJvHhVoEfgiGq3VZmV9VUXfBXhHjcEvroBMkJGnY TX_OK



Get Account state
~~~~~~~~~~~~~~~~~

Check account's state (nonce, balance) 

.. code-block:: shell

    $ aergocli getstate --address "AmNvFyqKFGVWvQ3MTi3eMFiNB9zvL9cK43B9c9bzcA732YZjZgfn"

Get state with a compressed merkle proof.

.. code-block:: shell

    $ aergocli getstate --address "AmNvFyqKFGVWvQ3MTi3eMFiNB9zvL9cK43B9c9bzcA732YZjZgfn" --proof --compressed


By default, the returned state is the one at the latest block, but you may specify any past block's state root.

.. code-block:: shell

    $ aergocli getstate --address "AmNvFyqKFGVWvQ3MTi3eMFiNB9zvL9cK43B9c9bzcA732YZjZgfn" --root "9NBSjkcNTdE5ciBxfb52RmsVW7vgX5voRsv6KcosiNjE"


Example without aergosvr
------------------------

There are some feature working on **aergocli** itself without **aergosvr**.

Create, Export, Import account
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

With :code:`--path` option, aergocli creates an account in the given path and not in **aergosvr**.

.. code-block:: shell

    $ aergocli account new --password yourpasswordhere --path path/to/save/account
    AmNFcocofUvmyLtXA6WgpANbjiF7RScGvQ4memNyNzS4ARJox3yq

Private key of account is store in the given path.

Of course this account can be exported and imported to aergosvr or another path.

.. code-block:: shell

    $ aergocli account export --address AmNFcocofUvmyLtXA6WgpANbjiF7RScGvQ4memNyNzS4ARJox3yq --password yourpasswordhere --path path/to/save/account
    47rsdfckuUCcjY3SmzCtmthhQm336Cpz9341xQHq6sr5Wm3md9FaTZDj6Gkqtff3WBPoqtzVV

.. code-block:: shell

    $ aergocli account import --if 47rsdfckuUCcjY3SmzCtmthhQm336Cpz9341xQHq6sr5Wm3md9FaTZDj6Gkqtff3WBPoqtzVV --password yourpasswordhere --path other/path/to/save/account
    AmNFcocofUvmyLtXA6WgpANbjiF7RScGvQ4memNyNzS4ARJox3yq

Sign transaction
~~~~~~~~~~~~~~~~

With :code:`--path` option, aergocli can sign the transaction using private key of account in given path.

Unlike using aergosrv, parameter :code:`--address` and :code:`--password` are needed instead of unlock.

.. code-block:: shell

    $ aergocli signtx --address AmNFcocofUvmyLtXA6WgpANbjiF7RScGvQ4memNyNzS4ARJox3yq --jsontx \
                        "{\"account\":\"AmNBZ8WQKP8DbuP9Q9W9vGFhiT8vQNcuSZ2SbBbVvbJWGV3Wh1mn\", \
                        \"nonce\": 2 , \
                        \"price\": 1 , \
                        \"limit\": 100 , \
                        \"recipient\":\"AmLnVfGwq49etaa7dnzfGJTbaZWV7aVmrxFes4KmWukXwtooVZPJ\", \
                        \"type\": 0, \
                        \"amount\": 25000 }" --path path/to/save/account --password yourpasswordhere
    {
        "Hash": "HB44gJvHhVoEfgiGq3VZmV9VUXfBXhHjcEvroBMkJGnY",
        "Body": {
            "Nonce": 2,
            "Account": "AmNBZ8WQKP8DbuP9Q9W9vGFhiT8vQNcuSZ2SbBbVvbJWGV3Wh1mn",
            "Recipient": "AmLnVfGwq49etaa7dnzfGJTbaZWV7aVmrxFes4KmWukXwtooVZPJ",
            "Amount": 25000,
            "Payload": "",
            "Limit": 100,
            "Price": 1,
            "Type": 0,
            "Sign": "381yXYxTtq2tRPRQPF7tHH6Cq3y8PvcsFWztPwCRmmYfqnK83Z3a6Yj9fyy8Rpvrrw76Y52SNAP6Th3BYQjX1Bcmf6NQrDHQ"
        }
    }

