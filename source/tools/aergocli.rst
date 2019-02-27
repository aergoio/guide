Aergocli
========

**aergocli** is a command line tool that interfaces with the GRPC exposed by **aergosvr**.

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

.. code-block:: text

    Usage:
    aergocli [command]

    Available Commands:
    account     account command
    blockchain  Print current blockchain status
    bp          show bp voting stat
    chaininfo   Print current blockchain information
    committx    Send transaction
    contract    contract command
    event       Get event
    getblock    Get block information
    getpeers    Get Peer list
    getstate    Get account state
    gettx       Get transaction information
    help        Help about any command
    keygen      Generate private key
    listblocks  Get block headers list
    metric      Show metric informations
    name        Name command
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

    $ ./aergocli sendtx --from AmPLWBzx4tAYt91JM3jKWFs3aYWHSvKpYYzdUQuQMNa7jAw5t65q --to AmLnVfGwq49etaa7dnzfGJTbaZWV7aVmrxFes4KmWukXwtooVZPJ --amount 1aergo
    {
     "hash": "AkjqFcayutenhonnZPU4X5QB1fNBTZv3o2fNzMLNQR3q"
    }

Look up transaction
~~~~~~~~~~~~~~~~~~~

Use gettx command. If transaction is in block, return transaction with index that represent where it's included.

When transaction is in mempool, gettx result is like below

.. code-block:: shell

    $ aergocli gettx 9cAphBMD2zJCD13QfCn7rmxh5iDfrj6M9Wmo54TPNPCg
    {
     "Hash": "9cAphBMD2zJCD13QfCn7rmxh5iDfrj6M9Wmo54TPNPCg",
     "Body": {
      "Nonce": 2,
      "Account": "AmPLWBzx4tAYt91JM3jKWFs3aYWHSvKpYYzdUQuQMNa7jAw5t65q",
      "Recipient": "AmLnVfGwq49etaa7dnzfGJTbaZWV7aVmrxFes4KmWukXwtooVZPJ",
      "Amount": "1230000000000000000",
      "Payload": "",
      "Limit": 0,
      "Price": "0",
      "Type": 0,
      "Sign": "AN1rKvt8EZHKEE2wNPXAhGcDA4pMo7yRRjTWCcpyrW9QCMv6nMhvhqriWujHdaDJgJ6ft6VLDActEFtUFA2pRnRJFVFSWxPSR"
     }
    }
    
When transaction is in block, gettx result is like below

.. code-block:: shell

    $ aergocli gettx 9cAphBMD2zJCD13QfCn7rmxh5iDfrj6M9Wmo54TPNPCg
    {
     "TxIdx": {
      "BlockHash": "ECVG696Jc7FvUL86sDSxT28akh4Hf1RXFRzmGqtAv9zU",
      "Idx": 1
     },
     "Tx": {
      "Hash": "9cAphBMD2zJCD13QfCn7rmxh5iDfrj6M9Wmo54TPNPCg",
      "Body": {
       "Nonce": 2,
       "Account": "AmPLWBzx4tAYt91JM3jKWFs3aYWHSvKpYYzdUQuQMNa7jAw5t65q",
       "Recipient": "AmLnVfGwq49etaa7dnzfGJTbaZWV7aVmrxFes4KmWukXwtooVZPJ",
       "Amount": "1230000000000000000",
       "Payload": "",
       "Limit": 0,
       "Price": "0",
       "Type": 0,
       "Sign": "AN1rKvt8EZHKEE2wNPXAhGcDA4pMo7yRRjTWCcpyrW9QCMv6nMhvhqriWujHdaDJgJ6ft6VLDActEFtUFA2pRnRJFVFSWxPSR"
      }
     }
    }

Check block
~~~~~~~~~~~

.. code-block:: shell

    $ aergocli getblock --hash GGT9wahqcKKGKUncMuhRLLL3JaCs2MEBx7V8UdrK9JNi
    {
     "Hash": "FwBq14HiBPMPoGV6jYxW4AaGHsgoD9UJjmYyWwnQR6xU",
     "Header": {
       "ChainID": "1111117eaxEoT4pDHzFTCyKv93acfDHbsytUPK3oqU6",
       "PrevBlockHash": "GocCiGXUVV4ygsbEi1VaJKkBwyvRsV4W4Qj72J5ZciZK",
       "BlockNo": 17655,
       "Timestamp": 1548314097754698000,
       "BlockRootHash": "5etqxP9HTtzgN8a3tN5Ev8ka4aG3aQM5GYNikwmsV41q",
       "TxRootHash": "AkjqFcayutenhonnZPU4X5QB1fNBTZv3o2fNzMLNQR3q",
       "ReceiptsRootHash": "7ZpYoMA2feXoXAit5J2FFuX16cwz9hp8twGwu7rUHCRZ",
       "Confirms": 2,
       "PubKey": "GZsJqUTtFVJTJ5SbwuFac4NxZFWgTRjpioPD76UL1DHZLhxmWg",
       "Sign": "AN1rKvtSPyBqB34TGmEQ6FQ8Lz61dvQ4zVAHodAnPGTnXD7gihmSkgrWcyeEoXNeS6zLTtgjukcTiwSKTHmMATGe8PXgwcJJK",
       "CoinbaseAccount": ""
      },
      "Body": {
       "Txs": [
        {
         "Hash": "AkjqFcayutenhonnZPU4X5QB1fNBTZv3o2fNzMLNQR3q",
         "Body": {
          "Nonce": 3,
          "Account": "AmPLWBzx4tAYt91JM3jKWFs3aYWHSvKpYYzdUQuQMNa7jAw5t65q",
          "Recipient": "AmLnVfGwq49etaa7dnzfGJTbaZWV7aVmrxFes4KmWukXwtooVZPJ",
          "Amount": "1000000000000000000",
          "Payload": "",
          "Limit": 0,
          "Price": "0",
          "Type": 0,
          "Sign": "381yXZAWNW8ZiST7PCLmywUsNVQvzgwhiyu9pxVDSvUhwyLHmoL7BQGXQhCNp7QZQycvbwT2nQJ7etscArfRbHu98Qi5MSmY"
         }
        }
       ]
      }
     }

Sign transaction
~~~~~~~~~~~~~~~~

After unlock the account

.. code-block:: shell

    $ aergocli signtx --jsontx "{ \
    \"Nonce\": 2, \
    \"Account\": \"AmPLWBzx4tAYt91JM3jKWFs3aYWHSvKpYYzdUQuQMNa7jAw5t65q\", \
    \"Recipient\": \"AmLnVfGwq49etaa7dnzfGJTbaZWV7aVmrxFes4KmWukXwtooVZPJ\", \
    \"Amount\": \"1.23aergo\", \
    \"Payload\": \"\", \
    \"Limit\": 0, \
    \"Price\": \"0\", \
    \"Type\": 0 }"
    {
     "Hash": "9cAphBMD2zJCD13QfCn7rmxh5iDfrj6M9Wmo54TPNPCg",
     "Body": {
      "Nonce": 2,
      "Account": "AmPLWBzx4tAYt91JM3jKWFs3aYWHSvKpYYzdUQuQMNa7jAw5t65q",
      "Recipient": "AmLnVfGwq49etaa7dnzfGJTbaZWV7aVmrxFes4KmWukXwtooVZPJ",
      "Amount": "1230000000000000000",
      "Payload": "",
      "Limit": 0,
      "Price": "0",
      "Type": 0,
      "Sign": "AN1rKvt8EZHKEE2wNPXAhGcDA4pMo7yRRjTWCcpyrW9QCMv6nMhvhqriWujHdaDJgJ6ft6VLDActEFtUFA2pRnRJFVFSWxPSR"
     }
    }

Commit Transaction
~~~~~~~~~~~~~~~~~~

Send given transactions to **aergosvr**

.. code-block:: shell

    $ aergocli -p 27845 committx --jsontx "{
    \"Hash\": \"9cAphBMD2zJCD13QfCn7rmxh5iDfrj6M9Wmo54TPNPCg\",
    \"Body\": {
      \"Nonce\": 2,
      \"Account\": \"AmPLWBzx4tAYt91JM3jKWFs3aYWHSvKpYYzdUQuQMNa7jAw5t65q\",
      \"Recipient\": \"AmLnVfGwq49etaa7dnzfGJTbaZWV7aVmrxFes4KmWukXwtooVZPJ\",
      \"Amount\": \"1230000000000000000\",
      \"Payload\": \"\",
      \"Limit\": 0,
      \"Price\": \"0\",
      \"Type\": 0,
      \"Sign\": \"AN1rKvt8EZHKEE2wNPXAhGcDA4pMo7yRRjTWCcpyrW9QCMv6nMhvhqriWujHdaDJgJ6ft6VLDActEFtUFA2pRnRJFVFSWxPSR\"
    }
    }"
    {
     "results": [
      {
       "hash": "9cAphBMD2zJCD13QfCn7rmxh5iDfrj6M9Wmo54TPNPCg"
      }
     ]
    }



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

Show connected peers
~~~~~~~~~~~~~~~~~~~~

Use getpeers to show list of peers connected to a aergosvr.

It shows remote peers by default, but with :code:`--self` options, local aergosvr itself is shown in list. You can find it by checking :code:`Self` property.
If you show only opened peers, use with :code:`--nohidden` options.

.. code-block:: shell

    [
     {
      "Address": {
       "Address": "13.124.83.51",
       "Port": "7846",
       "PeerId": "16Uiu2HAmQn3nFBGhJM7TnZRguLhgUx1HnpNL2easdt2JrxdbFjtb"
      },
      "BestBlock": {
       "BlockHash": "BXAFbTbwEuywukzBqRKdCsuUpinnNQsVJCAXNVJAR6F4",
       "BlockNo": 1251651
      },
      "LastCheck": "2019-02-27T11:26:38.481451+09:00",
      "State": "RUNNING",
      "Hidden": false,
      "Self": true
     },
     {
      "Address": {
       "Address": "13.211.156.203",
       "Port": "7846",
       "PeerId": "16Uiu2HAkvbHmK1Ke1hqAHmahwTGE4ndkdMdXJeXFE3kgBs17k2oQ"
      },
      "BestBlock": {
       "BlockHash": "AF68dtMMHd1h5LjxSyYY9AomMve6Qk2kBRSoQuuuSkhM",
       "BlockNo": 1251650
      },
      "LastCheck": "2019-02-27T11:26:38.349938+09:00",
      "State": "RUNNING",
      "Hidden": false,
      "Self": false
     },
     {
      "Address": {
       "Address": "203.109.12.23",
       "Port": "7846",
       "PeerId": "16Uiu2HAmAnQ5jjk7huhepfFtDFFCreuJ21nHYBApVpg8G7EBdwme"
      },
      "BestBlock": {
       "BlockHash": "BXAFbTbwEuywukzBqRKdCsuUpinnNQsVJCAXNVJAR6F4",
       "BlockNo": 1251651
      },
      "LastCheck": "2019-02-27T11:26:38.364262+09:00",
      "State": "RUNNING",
      "Hidden": false,
      "Self": false
     }
    ]


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

    $ aergocli -p 17845 signtx --address AmPLWBzx4tAYt91JM3jKWFs3aYWHSvKpYYzdUQuQMNa7jAw5t65q --jsontx "{ \
    \"Nonce\": 2, \
    \"Account\": \"AmPLWBzx4tAYt91JM3jKWFs3aYWHSvKpYYzdUQuQMNa7jAw5t65q\", \
    \"Recipient\": \"AmLnVfGwq49etaa7dnzfGJTbaZWV7aVmrxFes4KmWukXwtooVZPJ\", \
    \"Amount\": \"1.23aergo\", \
    \"Payload\": \"\", \
    \"Limit\": 0, \
    \"Price\": \"0\", \
    \"Type\": 0 }" --path path/to/save/account --password yourpasswordhere 
    {
     "Hash": "9cAphBMD2zJCD13QfCn7rmxh5iDfrj6M9Wmo54TPNPCg",
     "Body": {
      "Nonce": 2,
      "Account": "AmPLWBzx4tAYt91JM3jKWFs3aYWHSvKpYYzdUQuQMNa7jAw5t65q",
      "Recipient": "AmLnVfGwq49etaa7dnzfGJTbaZWV7aVmrxFes4KmWukXwtooVZPJ",
      "Amount": "1230000000000000000",
      "Payload": "",
      "Limit": 0,
      "Price": "0",
      "Type": 0,
      "Sign": "AN1rKvt8EZHKEE2wNPXAhGcDA4pMo7yRRjTWCcpyrW9QCMv6nMhvhqriWujHdaDJgJ6ft6VLDActEFtUFA2pRnRJFVFSWxPSR"
     }
    }
