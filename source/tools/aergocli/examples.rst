Examples
========

With all account actions, you will be prompted for a password.
For automation, you can also pass it as the :code:`--password` flag, but that is less secure depending on your terminal environment.

.. versionchanged:: 2.2.0
    Account actions now use a local keystore. The path can be configured using :code:`--keystore` (default: $HOME/.aergo).
    In previous versions, this used a remote keystore on the node you connected to. You can restore the previous behavior
    using the :code:`--node-keystore` option. See below for examples.

Create account
--------------

.. code-block:: shell

    $ aergocli account new
    Enter Password:
    Repeat Password:
    AmP96xd6WYRe1jrWixC3VyFXRCzwZxAJtU3Uc54vn182xG9Yn9Cs

Please remember the password carefully because there is no way to retrieve the password again.

Send transaction
----------------

.. code-block:: shell

    $ ./aergocli sendtx --from AmPLWBzx4tAYt91JM3jKWFs3aYWHSvKpYYzdUQuQMNa7jAw5t65q --to AmLnVfGwq49etaa7dnzfGJTbaZWV7aVmrxFes4KmWukXwtooVZPJ --amount 1aergo
    {
     "hash": "AkjqFcayutenhonnZPU4X5QB1fNBTZv3o2fNzMLNQR3q"
    }

Look up transaction
-------------------

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
-----------

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
----------------

.. code-block:: shell

    $ aergocli signtx --address AmPLWBzx4tAYt91JM3jKWFs3aYWHSvKpYYzdUQuQMNa7jAw5t65q --jsontx "{ \
    \"Nonce\": 2, \
    \"Account\": \"AmPLWBzx4tAYt91JM3jKWFs3aYWHSvKpYYzdUQuQMNa7jAw5t65q\", \
    \"Recipient\": \"AmLnVfGwq49etaa7dnzfGJTbaZWV7aVmrxFes4KmWukXwtooVZPJ\", \
    \"Amount\": \"1.23aergo\", \
    \"Payload\": \"\", \
    \"Limit\": 0, \
    \"Price\": \"0\", \
    \"Type\": 0 }"
    Enter Password:
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
------------------

Send given transactions to **aergosvr**

.. code-block:: shell

    $ aergocli committx --jsontx "{
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
-----------------

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
--------------------

Use getpeers to show list of peers connected to a aergosvr.

It shows remote peers by default, but with :code:`--self` options, local aergosvr itself is shown in list. You can find it by checking :code:`Self` property.
If you want to see only opened peers, use with :code:`--nohidden` options.
The option :code:`--detail` is added in v2, which change output information level. :code:`--detail=0` or no option shows same as prvious version, :code:`--detail=-1` shows shortened version, and :code:`--detail=1` shows more information about peers 

normal output

.. code-block:: json

    [
     {
      "Role": "Producer",
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
      "Version": "v2.0.0-dev1-14-g138d4f04"
     },
     {
      "Role": "Producer",
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
      "Version": "v2.0.0-dev1-14-g138d4f04"
     },
     {
      "Role": "Producer",
      "Address": {
       "Address": "192.168.2.21",
       "Port": "7846",
       "PeerId": "16Uiu2HAmAnQ5jjk7huhepfFtDFFCreuJ21nHYBApVpg8G7EBdwme"
      },
      "BestBlock": {
       "BlockHash": "BXAFbTbwEuywukzBqRKdCsuUpinnNQsVJCAXNVJAR6F4",
       "BlockNo": 1251651
      },
      "LastCheck": "2019-02-27T11:26:38.364262+09:00",
      "State": "RUNNING",
      "Hidden": true,
      "Self": false
      "Version": "v2.0.0-dev1-14-g138d4f04"
     },
     {
      "Role": "Agent",
      "Address": {
       "Address": "192.168.2.11",
       "Port": "7846",
       "PeerId": "16Uiu2HAm7e2TBhLDrsEyRVTnqqeyZznLxfpHcWQTdCZd8r4DhF9Q"
      },
      "BestBlock": {
       "BlockHash": "BXAFbTbwEuywukzBqRKdCsuUpinnNQsVJCAXNVJAR6F4",
       "BlockNo": 1251651
      },
      "LastCheck": "2019-02-27T11:26:38.342138+09:00",
      "State": "RUNNING",
      "Hidden": false,
      "Self": false,
      "Version": "v2.0.0-dev1-14-g138d4f04"
     }
    ]

shortened output

.. code-block:: json

    [
     "16*dbFjtb;13.124.83.51/7846;Producer;1251651",
     "16*17k2oQ;13.211.156.203/7846;Producer;1251650",
     "16*EBdwme;192.168.2.21/7846;Producer;1251651",
     "16*4DhF9Q;192.168.2.11/7846;Agent;1251651"
    ]

Using remote node-based accounts
--------------------------------

.. deprecated:: 2.2.0
    To be removed in future versions, use local keystores (the new default) instead.

You can also store accounts on a remote node.
When using remote account management, pass the :code:`--node-keystore` flag to all commands.
In versions prior to 2.2.0, this was the default behavior.

Create, Export, Import account
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: shell

    $ aergocli account new --node-keystore
    Enter Password:
    Repeat Password:
    AmNFcocofUvmyLtXA6WgpANbjiF7RScGvQ4memNyNzS4ARJox3yq

This account can be exported and imported.

.. code-block:: shell

    $ aergocli account export --address AmNFcocofUvmyLtXA6WgpANbjiF7RScGvQ4memNyNzS4ARJox3yq --node-keystore > AmNFcocofUvmyLtXA6WgpANbjiF7RScGvQ4memNyNzS4ARJox3yq__keystore.txt
    Enter Password:

.. code-block:: shell

    $ aergocli account import --node-keystore --path AmNFcocofUvmyLtXA6WgpANbjiF7RScGvQ4memNyNzS4ARJox3yq__keystore
    AmNFcocofUvmyLtXA6WgpANbjiF7RScGvQ4memNyNzS4ARJox3yq

Unlock
~~~~~~

Before request send transaction you must unlock your account first

.. code-block:: shell

    $ aergocli account unlock --node-keystore --address AmQFgm1gCvoRw2RfBXnipRmeCLEc6tTQ1kBMmLEzHjp91xYnXK78
    Enter Password:
    AmQFgm1gCvoRw2RfBXnipRmeCLEc6tTQ1kBMmLEzHjp91xYnXK78

Sign transaction
~~~~~~~~~~~~~~~~

With :code:`--node-keystore` option, aergocli can sign the transaction using private key of account on the remote node.

.. code-block:: shell

    $ aergocli -p 17845 signtx --address AmPLWBzx4tAYt91JM3jKWFs3aYWHSvKpYYzdUQuQMNa7jAw5t65q --jsontx "{ \
    \"Nonce\": 2, \
    \"Account\": \"AmPLWBzx4tAYt91JM3jKWFs3aYWHSvKpYYzdUQuQMNa7jAw5t65q\", \
    \"Recipient\": \"AmLnVfGwq49etaa7dnzfGJTbaZWV7aVmrxFes4KmWukXwtooVZPJ\", \
    \"Amount\": \"1.23aergo\", \
    \"Payload\": \"\", \
    \"Limit\": 0, \
    \"Price\": \"0\", \
    \"Type\": 0 }" --node-keystore
    Enter Password:
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
