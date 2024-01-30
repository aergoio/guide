Aergo Web3 0.1.0
================

.. toctree::
    :maxdepth: 3


Description
~~~~~~~~~~~

Aergo Web3 API





ACCOUNT
~~~~~~~


Blockchain related operations





GET ``/getBalance``
-------------------


Summary
+++++++

Get account balance


Parameters
++++++++++

.. csv-table::
    :delim: |
    :header: "Name", "Located in", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 15, 10, 10, 10, 20, 30

        account | query | Yes | :ref:`account <i_346cd666f1509ac3d9df73bb80271146>` |  |  | 


Request
+++++++


Responses
+++++++++

**200**
^^^^^^^

Successful response


.. _i_fbe68c238895783dfd28c1312f3ba3a0:

**Response Schema:**

.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        balance | No | string |  |  | 


**Example:**

.. code-block:: javascript

    {
        "balance": "1000000000000000000"
    }





GET ``/getNameInfo``
--------------------


Summary
+++++++

Owner of account name


Parameters
++++++++++

.. csv-table::
    :delim: |
    :header: "Name", "Located in", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 15, 10, 10, 10, 20, 30

        name | query | No | :ref:`account <i_346cd666f1509ac3d9df73bb80271146>` |  |  | 
        number | query | No | :ref:`number <i_2a09f513670971c9f439b4179dd21393>` |  |  | 


Request
+++++++


Responses
+++++++++

**200**
^^^^^^^

Successful response


.. _i_cf96977c1fb6f22c794dd848317cca81:

**Response Schema:**

.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        destination | No | string |  |  | 
        name | No | string |  |  | 
        owner | No | string |  |  | 


**Example:**

.. code-block:: javascript

    {
        "destination": "",
        "name": "AmNdzAYv3dYKFtPRgfUMGppGwBJS2JvZLRTF9gRruF49vppEepgj",
        "owner": ""
    }





GET ``/getState``
-----------------


Summary
+++++++

Get account state


Parameters
++++++++++

.. csv-table::
    :delim: |
    :header: "Name", "Located in", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 15, 10, 10, 10, 20, 30

        account | query | Yes | :ref:`account <i_346cd666f1509ac3d9df73bb80271146>` |  |  | 


Request
+++++++


Responses
+++++++++

**200**
^^^^^^^

Successful response


.. _i_f3362093bd7bb82eba3fed49f9781122:

**Response Schema:**

.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        account | No | string |  |  | 
        balance | No | string |  |  | 
        isContract | No | boolean |  |  | 
        nextaction | No | integer |  |  | 
        nonce | No | integer |  |  | 
        stake | No | string |  |  | 
        total | No | string |  |  | 
        votingpower | No | string |  |  | 
        when | No | integer |  |  | 


**Example:**

.. code-block:: javascript

    {
        "account": "AmNpHWATDRqrK6YrF9shuPYpFwucX8dVGhW7X8ypmwBLSAoM6xDG",
        "balance": "28085280000000000000000",
        "isContract": false,
        "nextaction": 341839,
        "nonce": 7,
        "stake": "10000000000000000000000",
        "total": "38085280000000000000000",
        "votingpower": "50000000000000000000000",
        "when": 255439
    }





GET ``/getStateAndProof``
-------------------------


Summary
+++++++

Get account state and proof


Parameters
++++++++++

.. csv-table::
    :delim: |
    :header: "Name", "Located in", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 15, 10, 10, 10, 20, 30

        account | query | Yes | :ref:`account <i_346cd666f1509ac3d9df73bb80271146>` |  |  | 
        compressed | query | Yes | :ref:`compressed <i_ab678747d1d8d0bcf93576dfe6bfeede>` |  |  | 


Request
+++++++


Responses
+++++++++

**200**
^^^^^^^

Successful response


.. _i_2dcfaed7115fe31a9ab9c64dfbc3a2bc:

**Response Schema:**

.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        account | No | string |  |  | 
        balance | No | string |  |  | 
        merkle proof length | No | integer |  |  | 
        nonce | No | integer |  |  | 


**Example:**

.. code-block:: javascript

    {
        "account": "AmNdzAYv3dYKFtPRgfUMGppGwBJS2JvZLRTF9gRruF49vppEepgj",
        "balance": "100000000 aergo",
        "merkle proof length": 4,
        "nonce": 0
    }



  
BLOCK
~~~~~


Block related operations





GET ``/blockchain``
-------------------


Summary
+++++++

Get current blockchain status



Request
+++++++


Responses
+++++++++

**200**
^^^^^^^

Successful response


.. _i_165e3c4c3f6ef869103353cabd909c43:

**Response Schema:**

.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        best_block_hash | No | string |  |  | 
        best_chain_id_hash | No | string |  |  | 
        best_height | No | integer |  |  | 
        chain_info | No | :ref:`chain_info <i_908bc26e9012b1477a73d45c3e7d4b76>` |  |  | 
        consensus_info | No | :ref:`consensus_info <i_61615ad2c6ef1bb860728d5cc7515b30>` |  |  | 
        isContract | No | boolean |  |  | 
        nextaction | No | integer |  |  | 
        votingpower | No | string |  |  | 
        when | No | integer |  |  | 

.. _i_908bc26e9012b1477a73d45c3e7d4b76:

**Chain_info schema:**


.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        bpNumber | No | integer |  |  | 
        gasprice | No | string |  |  | 
        id | No | :ref:`id <i_5a82517bb862fb2e8f6148fe737f99a5>` |  |  | 
        maxblocksize | No | integer |  |  | 
        maxtokens | No | string |  |  | 
        nameprice | No | string |  |  | 

.. _i_5a82517bb862fb2e8f6148fe737f99a5:

**Id schema:**


.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        consensus | No | string |  |  | 
        magic | No | string |  |  | 
        version | No | integer |  |  | 

.. _i_61615ad2c6ef1bb860728d5cc7515b30:

**Consensus_info schema:**


.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        Status | No | :ref:`Status <i_ce8c9da0a5a5ac10021775e9d0b76526>` |  |  | 
        Type | No | string |  |  | 

.. _i_ce8c9da0a5a5ac10021775e9d0b76526:

**Status schema:**


.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        Leader | No | string |  |  | 
        Name | No | string |  |  | 
        RaftId | No | string |  |  | 
        Status | No | string |  |  | 
        Total | No | integer |  |  | 


**Example:**

.. code-block:: javascript

    {
        "best_block_hash": "OxnydGsA8B0VFcKOM09cq3qOlyZjkr10Zz9yUYR0eWg=",
        "best_chain_id_hash": "mhY5TnCy9Uv4X9mQCk0ad0jdUqGMrX05oixwJBvQnH0=",
        "best_height": 491,
        "chain_info": {
            "bpNumber": 3,
            "consensus": "raft",
            "gasprice": "C6Q7dAA=",
            "id": null,
            "magic": "bmt.aergo.io",
            "maxblocksize": 1000400,
            "maxtokens": "AkMGxAl4WcQ8AAAA",
            "nameprice": "DeC2s6dkAAA=",
            "version": 3
        },
        "consensus_info": {
            "Leader": "local1",
            "Name": "local1",
            "RaftId": "6d3862958785a554",
            "Status": null,
            "Total": 3,
            "Type": "raft"
        },
        "isContract": false,
        "nextaction": 5,
        "votingpower": "value",
        "when": 5
    }





GET ``/getBlock``
-----------------


Summary
+++++++

Get block information


Parameters
++++++++++

.. csv-table::
    :delim: |
    :header: "Name", "Located in", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 15, 10, 10, 10, 20, 30

        hash | query | No | :ref:`account <i_346cd666f1509ac3d9df73bb80271146>` |  |  | 
        number | query | No | :ref:`number <i_71063bc7a76e3760c678a32453e127bc>` |  |  | 


Request
+++++++


Responses
+++++++++

**200**
^^^^^^^

Successful response


.. _i_a720349adb097cba03aed108664ad455:

**Response Schema:**

.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        body | No | :ref:`body <i_4d863967ef9a9d9efdadd1b250c76bd6>` |  |  | 
        hash | No | string |  |  | 
        header | No | :ref:`header <i_a116828d306a125c8cf4174b0f299a54>` |  |  | 

.. _i_4d863967ef9a9d9efdadd1b250c76bd6:

**Body schema:**




.. _i_a116828d306a125c8cf4174b0f299a54:

**Header schema:**


.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        blockNo | No | integer |  |  | 
        blocksRootHash | No | string |  |  | 
        chainID | No | string |  |  | 
        prevBlockHash | No | string |  |  | 
        pubKey | No | string |  |  | 
        receiptsRootHash | No | string |  |  | 
        sign | No | string |  |  | 
        timestamp | No | integer |  |  | 
        txsRootHash | No | string |  |  | 


**Example:**

.. code-block:: javascript

    {
        "body": {},
        "hash": "C0pxUo0dljwDELyLG9lijEdmBjOOH9XNsfPfGMQ0pmA=",
        "header": {
            "blockNo": 1,
            "blocksRootHash": "4YQSYp0dkqUHYzTLAciMW9ZDc3RU89VMYuFBpfyHxaQ=",
            "chainID": "AwAAAAAAYm10LmFlcmdvLmlvL3JhZnQ=",
            "prevBlockHash": "lK4dq/zpT/Y8cSSdebJY9j8FmGJHjKG4SJF4r4++a8E=",
            "pubKey": "CAISIQNmmKv+inhXk+dWY2XdX8MxkZk7SCxlBoXSrAaYKsweFQ==",
            "receiptsRootHash": "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=",
            "sign": "MEQCIAu7xj6QIBKM+IfSLQho+b1l3fHXhxWYbpxC9ZnfLT2pAiAEbSmG7oeRGSfkWW96xAuaAZri9tlx4LD/eZNS9mQzlw==",
            "timestamp": 1692154145436345000,
            "txsRootHash": "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA="
        }
    }





GET ``/getBlockBody``
---------------------


Summary
+++++++

Get block body


Parameters
++++++++++

.. csv-table::
    :delim: |
    :header: "Name", "Located in", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 15, 10, 10, 10, 20, 30

        hash | query | No | :ref:`account <i_346cd666f1509ac3d9df73bb80271146>` |  |  | 
        number | query | No | :ref:`number <i_71063bc7a76e3760c678a32453e127bc>` |  |  | 
        offset | query | No | :ref:`number <i_71063bc7a76e3760c678a32453e127bc>` |  |  | 
        size | query | No | :ref:`number <i_71063bc7a76e3760c678a32453e127bc>` |  |  | 


Request
+++++++


Responses
+++++++++

**200**
^^^^^^^

Successful response


.. _i_5b3341f73d8b3229ba209c72c0b72d97:

**Response Schema:**

.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        body | No | :ref:`body <i_6d516d35ab1217925d65597f82964563>` |  |  | 
        offset | No | integer |  |  | 
        size | No | integer |  |  | 

.. _i_6d516d35ab1217925d65597f82964563:

**Body schema:**


.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        txs | No | array of :ref:`txs <i_d091c46eb1749f90637f3af37f6d8d3f>` |  |  | 

.. _i_d091c46eb1749f90637f3af37f6d8d3f:

**Txs schema:**


.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        body | No | :ref:`body <i_906d1d0743311e7e04cd566395f71f0d>` |  |  | 
        hash | No | string |  |  | 

.. _i_906d1d0743311e7e04cd566395f71f0d:

**Body schema:**


.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        account | No | string |  |  | 
        amount | No | string |  |  | 
        chainIdHash | No | string |  |  | 
        nonce | No | integer |  |  | 
        recipient | No | string |  |  | 
        sign | No | string |  |  | 
        type | No | integer |  |  | 


**Example:**

.. code-block:: javascript

    {
        "body": {
            "txs": [
                {
                    "body": {
                        "account": "AmPpcKvToDCUkhT1FJjdbNvR4kNDhLFJGHkSqfjWe3QmHm96qv4R",
                        "amount": "1000000000000000000",
                        "chainIdHash": "BNVSYKKqSR78hTSrxkaroFK2S3mgsoocpWg9HYtb1Zhn",
                        "nonce": 6,
                        "recipient": "AmM1M4jAxeTxfh9CHUmKXJwTLxGJ6Qe5urEeFXqbqkKnZ73Uvx4y",
                        "sign": "AN1rKvtYAKYdFiDJP68RGcUxS3YYeMD7ExgpZzMMgzvKLoce4SJx5YBAQBLSPTf1kzT5aH4qqj6BbjyurzqRbjyG6wXJfBdwo",
                        "type": 4
                    },
                    "hash": "7dYEViBVX3Qs1BTTPntT8fvi1zsBUFGbFtUscioWxQWJ"
                }
            ]
        },
        "offset": 0,
        "size": 5
    }





GET ``/getBlockMetadata``
-------------------------


Summary
+++++++

Get block meta


Parameters
++++++++++

.. csv-table::
    :delim: |
    :header: "Name", "Located in", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 15, 10, 10, 10, 20, 30

        hash | query | No | :ref:`account <i_346cd666f1509ac3d9df73bb80271146>` |  |  | 
        number | query | No | :ref:`number <i_71063bc7a76e3760c678a32453e127bc>` |  |  | 


Request
+++++++


Responses
+++++++++

**200**
^^^^^^^

Successful response


.. _i_753a600e30db2d222b07cf5130ab06e6:

**Response Schema:**

.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        hash | No | string |  |  | 
        header | No | :ref:`header <i_a116828d306a125c8cf4174b0f299a54>` |  |  | 
        size | No | integer |  |  | 

.. _i_a116828d306a125c8cf4174b0f299a54:

**Header schema:**


.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        blockNo | No | integer |  |  | 
        blocksRootHash | No | string |  |  | 
        chainID | No | string |  |  | 
        prevBlockHash | No | string |  |  | 
        pubKey | No | string |  |  | 
        receiptsRootHash | No | string |  |  | 
        sign | No | string |  |  | 
        timestamp | No | integer |  |  | 
        txsRootHash | No | string |  |  | 


**Example:**

.. code-block:: javascript

    {
        "hash": "A7L0SFNlkIAGivGOMKyzQIeaIbIZzUN0KiB/XZzzyRU=",
        "header": {
            "blockNo": 1,
            "blocksRootHash": "4YQSYp0dkqUHYzTLAciMW9ZDc3RU89VMYuFBpfyHxaQ=",
            "chainID": "AwAAAAAAYm10LmFlcmdvLmlvL3JhZnQ=",
            "prevBlockHash": "lK4dq/zpT/Y8cSSdebJY9j8FmGJHjKG4SJF4r4++a8E=",
            "pubKey": "CAISIQNmmKv+inhXk+dWY2XdX8MxkZk7SCxlBoXSrAaYKsweFQ==",
            "receiptsRootHash": "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=",
            "sign": "MEUCIQC1ZOHY2Yi1tMl6zi4uvM5Rd2Mo+JAmlzYBNFd9hC7aRQIgbR+b5bhz06OBElZaiMt60gQK6KakgQjDZvomSRPIWEM=",
            "timestamp": 1692161210463122000,
            "txsRootHash": "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA="
        },
        "size": 324
    }





GET ``/listBlockHeaders``
-------------------------


Summary
+++++++

Get block headers list


Parameters
++++++++++

.. csv-table::
    :delim: |
    :header: "Name", "Located in", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 15, 10, 10, 10, 20, 30

        height | query | No | :ref:`number <i_71063bc7a76e3760c678a32453e127bc>` |  |  | 
        size | query | No | :ref:`number <i_71063bc7a76e3760c678a32453e127bc>` |  |  | 
        offset | query | No | :ref:`number <i_71063bc7a76e3760c678a32453e127bc>` |  |  | 
        asc | query | No | :ref:`asc <i_bf3ced9be752bb3e756b3074256e6139>` |  |  | 


Request
+++++++


Responses
+++++++++

**200**
^^^^^^^

Successful response


.. _i_b1dcb1fcf27f161742bb7cbb19bf6741:

**Response Schema:**

.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        hash | No | string |  |  | 
        header | No | :ref:`header <i_bf219d024297c130687982329cc491b7>` |  |  | 

.. _i_bf219d024297c130687982329cc491b7:

**Header schema:**


.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        blocksRootHash | No | string |  |  | 
        chainID | No | string |  |  | 
        receiptsRootHash | No | string |  |  | 
        timestamp | No | integer |  |  | 
        txsRootHash | No | string |  |  | 


**Example:**

.. code-block:: javascript

    {
        "hash": "value",
        "header": {
            "blocksRootHash": "value",
            "chainID": "value",
            "receiptsRootHash": "value",
            "timestamp": 5,
            "txsRootHash": "value"
        }
    }



  
ETC
~~~


Etc related operations





GET ``/chainStat``
------------------


Summary
+++++++

Get chain statistics



Request
+++++++


Responses
+++++++++

**200**
^^^^^^^

Successful response


.. _i_f43b9028bbd62d694ed777af0d58d02b:

**Response Schema:**

.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        report | No | :ref:`report <i_50dc71006854eb5581562ea6b6dee932>` |  |  | 

.. _i_50dc71006854eb5581562ea6b6dee932:

**Report schema:**





**Example:**

.. code-block:: javascript

    {
        "report": {
            "ReorgStat": {
                "Count": 0
            }
        }
    }





GET ``/getAccountVotes``
------------------------


Summary
+++++++

Show voting stat


Parameters
++++++++++

.. csv-table::
    :delim: |
    :header: "Name", "Located in", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 15, 10, 10, 10, 20, 30

        account | query | No | :ref:`account <i_346cd666f1509ac3d9df73bb80271146>` |  |  | 


Request
+++++++


Responses
+++++++++

**200**
^^^^^^^

Successful response


.. _i_28ef1e350b62223af29ca6197a75f06a:

**Response Schema:**

.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        voting | No | array of :ref:`voting <i_99666fbd4ef94680554c67e3de3a896b>` |  |  | 

.. _i_99666fbd4ef94680554c67e3de3a896b:

**Voting schema:**


.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        id | No | string |  |  | 


**Example:**

.. code-block:: javascript

    {
        "voting": [
            {
                "id": "voteBP"
            },
            {
                "id": "BPCOUNT"
            },
            {
                "id": "STAKINGMIN"
            },
            {
                "id": "GASPRICE"
            },
            {
                "id": "NAMEPRICE"
            }
        ]
    }





GET ``/getChainId``
-------------------


Summary
+++++++

Get ChainID



Request
+++++++


Responses
+++++++++

**200**
^^^^^^^

Successful response






GET ``/getChainInfo``
---------------------


Summary
+++++++

Get current blockchain information



Request
+++++++


Responses
+++++++++

**200**
^^^^^^^

Successful response


.. _i_d16f225064ec4fd85c721d2995a83c0e:

**Response Schema:**

.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        bpNumber | No | integer |  |  | 
        gasprice | No | string |  |  | 
        id | No | :ref:`id <i_e29441ccfdbb5ff6c233d5d3780174eb>` |  |  | 
        maxblocksize | No | integer |  |  | 
        maxtokens | No | string |  |  | 
        nameprice | No | string |  |  | 
        totalvotingpower | No | string |  |  | 
        votingreward | No | string |  |  | 

.. _i_e29441ccfdbb5ff6c233d5d3780174eb:

**Id schema:**


.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        consensus | No | string |  |  | 
        magic | No | string |  |  | 
        version | No | string |  |  | 


**Example:**

.. code-block:: javascript

    {
        "bpNumber": 3,
        "gasprice": "50000000000",
        "id": {
            "consensus": "raft",
            "magic": "bmt.aergo.io",
            "version": 3
        },
        "maxblocksize": 1000400,
        "maxtokens": "700000000000000000000000000",
        "nameprice": "1000000000000000000",
        "totalvotingpower": "0",
        "votingreward": "0"
    }





GET ``/getConsensusInfo``
-------------------------


Summary
+++++++

Get consensus information



Request
+++++++


Responses
+++++++++

**200**
^^^^^^^

Successful response


.. _i_d5dc14706d9c664d27afa97307e019a4:

**Response Schema:**

.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        bps | No | array of :ref:`body <i_4d863967ef9a9d9efdadd1b250c76bd6>` |  |  | 
        info | No | :ref:`body <i_4d863967ef9a9d9efdadd1b250c76bd6>` |  |  | 
        type | No | string |  |  | 

.. _i_4d863967ef9a9d9efdadd1b250c76bd6:

**Body schema:**





**Example:**

.. code-block:: javascript

    {
        "bps": [
            {
                "Addr": "/ip4/192.168.0.21/tcp/7846",
                "Name": "local1",
                "PeerID": "16Uiu2HAmKZUxcDNLo6BbhugZ8XfgdC4utmJ5M6HTjvFmYZ2tqTac",
                "RaftID": "6d3862958785a554"
            },
            {
                "Addr": "/ip4/192.168.0.21/tcp/27846",
                "Name": "local2",
                "PeerID": "16Uiu2HAm82vRXcL187jRhFW9jnNjueMc7bSJLdcydNWUwxeUCC2U",
                "RaftID": "57779d37986cc00"
            },
            {
                "Addr": "/ip4/192.168.0.21/tcp/37846",
                "Name": "local3",
                "PeerID": "16Uiu2HAkyDXhsCj63R3ufsrYaK72nFT1RLVg5upabi6WAQtJzGoM",
                "RaftID": "cf615af53daf2c6b"
            }
        ],
        "info": {
            "Leader": "id=0",
            "Name": "local1",
            "RaftId": "6d3862958785a554",
            "Status": {
                "applied": 3,
                "commit": 3,
                "id": "6d3862958785a554",
                "lead": "0",
                "leadtransferee": "0",
                "progress": {},
                "raftState": "StateCandidate",
                "term": 6,
                "vote": "6d3862958785a554"
            },
            "Total": 3
        },
        "type": "raft"
    }





GET ``/getEnterpriseConfig``
----------------------------


Summary
+++++++

Get config values of enterprise


Parameters
++++++++++

.. csv-table::
    :delim: |
    :header: "Name", "Located in", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 15, 10, 10, 10, 20, 30

        key | query | No | :ref:`account <i_346cd666f1509ac3d9df73bb80271146>` |  |  | 


Request
+++++++


Responses
+++++++++

**200**
^^^^^^^

Successful response


.. _i_3f1b8779b7538a5df58502c360e26499:

**Response Schema:**

.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        Key | No | string |  |  | 
        On | No | boolean |  |  | 
        Values | No | string |  |  | 


**Example:**

.. code-block:: javascript

    {
        "Key": "",
        "On": false,
        "Values": ""
    }





GET ``/getNodeInfo``
--------------------


Summary
+++++++

Show internal metric


Parameters
++++++++++

.. csv-table::
    :delim: |
    :header: "Name", "Located in", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 15, 10, 10, 10, 20, 30

        timeout | query | No | :ref:`number <i_71063bc7a76e3760c678a32453e127bc>` |  |  | 
        component | query | No | :ref:`account <i_346cd666f1509ac3d9df73bb80271146>` |  |  | 


Request
+++++++


Responses
+++++++++

**200**
^^^^^^^

Successful response


.. _i_871fd5191bf4385926260458dff03681:

**Response Schema:**

.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        AccountsSvc | No | :ref:`body <i_4d863967ef9a9d9efdadd1b250c76bd6>` |  |  | 
        ChainSvc | No | :ref:`body <i_4d863967ef9a9d9efdadd1b250c76bd6>` |  |  | 
        MemPoolSvc | No | :ref:`body <i_4d863967ef9a9d9efdadd1b250c76bd6>` |  |  | 
        RPCSvc | No | :ref:`body <i_4d863967ef9a9d9efdadd1b250c76bd6>` |  |  | 
        SyncerSvc | No | :ref:`body <i_4d863967ef9a9d9efdadd1b250c76bd6>` |  |  | 
        mapSvc | No | :ref:`body <i_4d863967ef9a9d9efdadd1b250c76bd6>` |  |  | 
        p2pSvc | No | :ref:`body <i_4d863967ef9a9d9efdadd1b250c76bd6>` |  |  | 

.. _i_4d863967ef9a9d9efdadd1b250c76bd6:

**Body schema:**





**Example:**

.. code-block:: javascript

    {
        "AccountsSvc": {
            "acc_processed_msg": 1,
            "actor": {
                "config": {
                    "UnlockTimeout": 60
                },
                "personal": true,
                "totalaccounts": 0
            },
            "error": "",
            "msg_latency": "46.1\u00b5s",
            "msg_queue_len": 0,
            "status": "started"
        },
        "ChainSvc": {
            "acc_processed_msg": 34,
            "actor": {
                "config": {
                    "CloseLimit": 100,
                    "CoinbaseAccount": "",
                    "ForceResetHeight": 0,
                    "MaxAnchorCount": 20,
                    "MaxBlockSize": 1000000,
                    "NumLStateClosers": 2,
                    "NumWorkers": 16,
                    "StateTrace": 0,
                    "VerifierCount": 8,
                    "VerifyBlock": 0,
                    "VerifyOnly": false,
                    "ZeroFee": true
                },
                "orphan": 0,
                "testmode": false,
                "testnet": false
            },
            "error": "",
            "msg_latency": "60.242\u00b5s",
            "msg_queue_len": 0,
            "status": "started"
        },
        "MemPoolSvc": {
            "acc_processed_msg": 16,
            "actor": {
                "config": {
                    "DumpFilePath": "./data/mempool.dump",
                    "EnableFadeout": false,
                    "FadeoutPeriod": 12,
                    "ShowMetrics": false,
                    "VerifierNumber": 32
                },
                "dead": 0,
                "orphan": 0,
                "total": 0,
                "whitelist": [],
                "whitelist_on": false
            },
            "error": "",
            "msg_latency": "36.045\u00b5s",
            "msg_queue_len": 0,
            "status": "started"
        },
        "RPCSvc": {
            "acc_processed_msg": 14,
            "actor": {
                "config": {
                    "NSAllowCORS": false,
                    "NSCACert": "",
                    "NSCert": "",
                    "NSEnableTLS": false,
                    "NSKey": "",
                    "NetServiceAddr": "0.0.0.0",
                    "NetServicePath": "",
                    "NetServicePort": 7845,
                    "NetServiceTrace": false
                },
                "streams": {
                    "block": 0,
                    "blockMeta": 0,
                    "event": 0
                },
                "version": "v2.4.4-134-gaa1d7d30"
            },
            "error": "",
            "msg_latency": "27.18\u00b5s",
            "msg_queue_len": 0,
            "status": "started"
        },
        "SyncerSvc": {
            "acc_processed_msg": 1,
            "actor": {
                "block_added": 0,
                "block_fetched": 0,
                "end": 0,
                "running": false,
                "start": 0,
                "total": 0
            },
            "error": "",
            "msg_latency": "34.66\u00b5s",
            "msg_queue_len": 0,
            "status": "started"
        },
        "mapSvc": {
            "acc_processed_msg": 1,
            "actor": null,
            "error": "",
            "msg_latency": "58.423\u00b5s",
            "msg_queue_len": 0,
            "status": "started"
        },
        "p2pSvc": {
            "acc_processed_msg": 18,
            "actor": {
                "config": {
                    "Agent": "",
                    "InternalZones": null,
                    "LogFullPeerID": false,
                    "NPAddPeers": null,
                    "NPAddPolarises": null,
                    "NPBindAddr": "0.0.0.0",
                    "NPBindPort": 7846,
                    "NPCert": "",
                    "NPDiscoverPeers": false,
                    "NPEnableTLS": false,
                    "NPExposeSelf": false,
                    "NPHiddenPeers": null,
                    "NPKey": "bmt1.key",
                    "NPMaxPeers": 100,
                    "NPPeerPool": 100,
                    "NPUsePolaris": false,
                    "NetProtocolAddr": "192.168.0.21",
                    "NetProtocolPort": 7846,
                    "PeerRole": "producer",
                    "Producers": null
                },
                "netstat": {
                    "in": 35885,
                    "out": 26500,
                    "since": "2023-08-25T17:58:05.12366+09:00"
                },
                "status": {
                    "Addresses": [
                        "/ip4/192.168.0.21/tcp/7846"
                    ],
                    "Hidden": true,
                    "ID": "16Uiu2HAmKZUxcDNLo6BbhugZ8XfgdC4utmJ5M6HTjvFmYZ2tqTac",
                    "ProducerIDs": [
                        "16Uiu2HAmKZUxcDNLo6BbhugZ8XfgdC4utmJ5M6HTjvFmYZ2tqTac"
                    ],
                    "Role": "Producer",
                    "Version": "v2.4.4-134-gaa1d7d30"
                },
                "syncman": {
                    "blocks": 0,
                    "transactions": {
                        "backCache": 0,
                        "frontCache": 0,
                        "queryQueue": 0
                    }
                },
                "whitelist": [],
                "whitelist_on": false
            },
            "error": "",
            "msg_latency": "66.815\u00b5s",
            "msg_queue_len": 0,
            "status": "started"
        }
    }





GET ``/getPeers``
-----------------


Summary
+++++++

Get Peer list


Parameters
++++++++++

.. csv-table::
    :delim: |
    :header: "Name", "Located in", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 15, 10, 10, 10, 20, 30

        noHidden | query | No | :ref:`compressed <i_ab678747d1d8d0bcf93576dfe6bfeede>` |  |  | 
        showSelf | query | No | :ref:`compressed <i_ab678747d1d8d0bcf93576dfe6bfeede>` |  |  | 


Request
+++++++


Responses
+++++++++

**200**
^^^^^^^

Successful response


.. _i_ccc3858dd573a9f67fb384219caf817b:

**Response Schema:**

.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        peers | No | array of :ref:`peers <i_920422c901bca8dbd94d71ead3a62aaa>` |  |  | 

.. _i_920422c901bca8dbd94d71ead3a62aaa:

**Peers schema:**


.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        acceptedRole | No | string |  |  | 
        address | No | :ref:`body <i_4d863967ef9a9d9efdadd1b250c76bd6>` |  |  | 
        bestblock | No | :ref:`body <i_4d863967ef9a9d9efdadd1b250c76bd6>` |  |  | 
        hidden | No | boolean |  |  | 
        lashCheck | No | integer |  |  | 
        state | No | integer |  |  | 
        version | No | string |  |  | 

.. _i_4d863967ef9a9d9efdadd1b250c76bd6:

**Body schema:**





**Example:**

.. code-block:: javascript

    {
        "peers": [
            {
                "acceptedRole": "Producer",
                "address": {
                    "address": "192.168.0.21",
                    "addresses": [
                        "/ip4/192.168.0.21/tcp/37846"
                    ],
                    "peerID": "ACUIAhIhAjhSa/+nVg43tNhkJnaVx+xPjarZhVzd2PITk7uuJ8C0",
                    "port": 37846,
                    "producerIDs": [
                        "ACUIAhIhAjhSa/+nVg43tNhkJnaVx+xPjarZhVzd2PITk7uuJ8C0"
                    ],
                    "role": "Producer",
                    "version": "v2.4.1"
                },
                "bestblock": {
                    "blockHash": "2Sw66KMI9ACsHxdekMnzgO422nA+3M7wlq7LGAuxDD0=",
                    "blockNo": 166
                },
                "hidden": true,
                "lashCheck": 1692954055949754000,
                "state": 2,
                "version": "v2.4.1"
            },
            {
                "acceptedRole": "Producer",
                "address": {
                    "address": "192.168.0.21",
                    "addresses": [
                        "/ip4/192.168.0.21/tcp/27846"
                    ],
                    "peerID": "ACUIAhIhArtT5yoZccZO1OKI5stQRZf+h6dMC+KSg2UvdS1YY0Ep",
                    "port": 27846,
                    "producerIDs": [
                        "ACUIAhIhArtT5yoZccZO1OKI5stQRZf+h6dMC+KSg2UvdS1YY0Ep"
                    ],
                    "role": "Producer",
                    "version": "v2.4.1"
                },
                "bestblock": {
                    "blockHash": "zr34lRpn8ZKLMaM5wBP0NVM5B8Bm+/bZo4AXUtosvWA=",
                    "blockNo": 118
                },
                "hidden": true,
                "lashCheck": 1692954008028108000,
                "state": 2,
                "version": "v2.4.1"
            }
        ]
    }





GET ``/getServerInfo``
----------------------


Summary
+++++++

Show configs and status of server


Parameters
++++++++++

.. csv-table::
    :delim: |
    :header: "Name", "Located in", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 15, 10, 10, 10, 20, 30

        key | query | No | array of string |  |  | 


Request
+++++++


Responses
+++++++++

**200**
^^^^^^^

Successful response


.. _i_41e3bc66a85d2f8fa43e35795f5280d9:

**Response Schema:**

.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        config | No | :ref:`config <i_2e1f6a135f91caa863847cc0fbad639c>` |  |  | 
        status | No | :ref:`status <i_36aee0afab8e5c098084a8191aa9cbdc>` |  |  | 

.. _i_2e1f6a135f91caa863847cc0fbad639c:

**Config schema:**


.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        account | No | :ref:`body <i_4d863967ef9a9d9efdadd1b250c76bd6>` |  |  | 
        base | No | :ref:`body <i_4d863967ef9a9d9efdadd1b250c76bd6>` |  |  | 

.. _i_4d863967ef9a9d9efdadd1b250c76bd6:

**Body schema:**




.. _i_36aee0afab8e5c098084a8191aa9cbdc:

**Status schema:**


.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        addr | No | string |  |  | 
        id | No | string |  |  | 
        port | No | string |  |  | 
        version | No | string |  |  | 


**Example:**

.. code-block:: javascript

    {
        "config": {
            "account": {
                "props": {
                    "unlocktimeout": "60"
                }
            },
            "base": {
                "props": {
                    "personal": "true"
                }
            }
        },
        "status": {
            "addr": "192.168.0.21",
            "id": "16Uiu2HAmKZUxcDNLo6BbhugZ8XfgdC4utmJ5M6HTjvFmYZ2tqTac",
            "port": "7846",
            "version": "v2.4.4-134-gaa1d7d30"
        }
    }





GET ``/getStaking``
-------------------


Summary
+++++++

Get the staking info from the address


Parameters
++++++++++

.. csv-table::
    :delim: |
    :header: "Name", "Located in", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 15, 10, 10, 10, 20, 30

        address | query | No | :ref:`account <i_346cd666f1509ac3d9df73bb80271146>` |  |  | 


Request
+++++++


Responses
+++++++++

**200**
^^^^^^^

Successful response


.. _i_9e934c658f738846064c89d522095a88:

**Response Schema:**

.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        amount | No | string |  |  | 
        when | No | integer |  |  | 


**Example:**

.. code-block:: javascript

    {
        "amount": "10000000000000000000000",
        "when": 255439
    }





GET ``/getVotes``
-----------------


Summary
+++++++

Show voting stat


Parameters
++++++++++

.. csv-table::
    :delim: |
    :header: "Name", "Located in", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 15, 10, 10, 10, 20, 30

        id | query | No | :ref:`account <i_346cd666f1509ac3d9df73bb80271146>` |  |  | 
        count | query | No | :ref:`number <i_71063bc7a76e3760c678a32453e127bc>` |  |  | 


Request
+++++++


Responses
+++++++++

**200**
^^^^^^^

Successful response


.. _i_808fedaad3c4d86b32f91bd77e51815c:

**Response Schema:**

.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        votes | No | array of :ref:`votes <i_4f5e9bf1e90425020fd42497b968c4b4>` |  |  | 

.. _i_4f5e9bf1e90425020fd42497b968c4b4:

**Votes schema:**


.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        amount | No | string |  |  | 
        candidate | No | string |  |  | 


**Example:**

.. code-block:: javascript

    {
        "votes": [
            {
                "amount": "162179999999999990562816",
                "candidate": "16Uiu2HAkyDXhsCj63R3ufsrYaK72nFT1RLVg5upabi6WAQtJzGoM"
            },
            {
                "amount": "162179999999999990562816",
                "candidate": "16Uiu2HAmKZUxcDNLo6BbhugZ8XfgdC4utmJ5M6HTjvFmYZ2tqTac"
            },
            {
                "amount": "162179999999999990562816",
                "candidate": "16Uiu2HAm82vRXcL187jRhFW9jnNjueMc7bSJLdcydNWUwxeUCC2U"
            }
        ]
    }





GET ``/metric``
---------------


Summary
+++++++

Show metric informations


Parameters
++++++++++

.. csv-table::
    :delim: |
    :header: "Name", "Located in", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 15, 10, 10, 10, 20, 30

        type | query | No | :ref:`type <i_95872d145b57683b793a80759e9bf4c4>` |  |  | 


Request
+++++++


Responses
+++++++++

**200**
^^^^^^^

Successful response


.. _i_450e30f1d26afab93b3c23f78bdf664d:

**Response Schema:**

.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        peers | No | array of :ref:`peers <i_3a07f4722ae3c769f5350ebecdc076ce>` |  |  | 

.. _i_3a07f4722ae3c769f5350ebecdc076ce:

**Peers schema:**


.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        avrIn | No | integer |  |  | 
        avrOut | No | integer |  |  | 
        peerID | No | string |  |  | 
        sumIn | No | integer |  |  | 
        sumOut | No | integer |  |  | 


**Example:**

.. code-block:: javascript

    {
        "peers": [
            {
                "avrIn": 19497,
                "avrOut": 26261,
                "peerID": "ACUIAhIhArtT5yoZccZO1OKI5stQRZf+h6dMC+KSg2UvdS1YY0Ep",
                "sumIn": 163708,
                "sumOut": 220664
            },
            {
                "avrIn": 19651,
                "avrOut": 26367,
                "peerID": "ACUIAhIhAjhSa/+nVg43tNhkJnaVx+xPjarZhVzd2PITk7uuJ8C0",
                "sumIn": 165021,
                "sumOut": 221601
            }
        ]
    }



  
TRANSACTION
~~~~~~~~~~~


Transaction related operations





GET ``/getABI``
---------------


Summary
+++++++

Get ABI of the contract


Parameters
++++++++++

.. csv-table::
    :delim: |
    :header: "Name", "Located in", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 15, 10, 10, 10, 20, 30

        address | query | No | :ref:`account <i_346cd666f1509ac3d9df73bb80271146>` |  |  | 


Request
+++++++


Responses
+++++++++

**200**
^^^^^^^

Successful response






GET ``/getBlockTx``
-------------------


Summary
+++++++

Get transaction is in block


Parameters
++++++++++

.. csv-table::
    :delim: |
    :header: "Name", "Located in", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 15, 10, 10, 10, 20, 30

        hash | query | No | :ref:`account <i_346cd666f1509ac3d9df73bb80271146>` |  |  | 


Request
+++++++


Responses
+++++++++

**200**
^^^^^^^

Successful response


.. _i_b8d1cd95c844764285de1c58f6c150fa:

**Response Schema:**

.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        tx | No | :ref:`txs <i_d091c46eb1749f90637f3af37f6d8d3f>` |  |  | 
        txIdx | No | :ref:`txIdx <i_48508353cb51d5490c49314cad9212a2>` |  |  | 

.. _i_48508353cb51d5490c49314cad9212a2:

**Txidx schema:**


.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        blockHash | No | string |  |  | 

.. _i_d091c46eb1749f90637f3af37f6d8d3f:

**Txs schema:**


.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        body | No | :ref:`body <i_906d1d0743311e7e04cd566395f71f0d>` |  |  | 
        hash | No | string |  |  | 

.. _i_906d1d0743311e7e04cd566395f71f0d:

**Body schema:**


.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        account | No | string |  |  | 
        amount | No | string |  |  | 
        chainIdHash | No | string |  |  | 
        nonce | No | integer |  |  | 
        recipient | No | string |  |  | 
        sign | No | string |  |  | 
        type | No | integer |  |  | 


**Example:**

.. code-block:: javascript

    {
        "tx": {
            "body": {
                "account": "AmPpcKvToDCUkhT1FJjdbNvR4kNDhLFJGHkSqfjWe3QmHm96qv4R",
                "amount": "1000000000000000000",
                "chainIdHash": "BNVSYKKqSR78hTSrxkaroFK2S3mgsoocpWg9HYtb1Zhn",
                "nonce": 1,
                "recipient": "AmM1M4jAxeTxfh9CHUmKXJwTLxGJ6Qe5urEeFXqbqkKnZ73Uvx4y",
                "sign": "381yXZ1ekFJxDAPRTrNmZMoBWgBNRUsF9d755v1rkukZ2gZL5GFyrq1zo3ib9MY4TkpASdSwxsSci6a6n7at4nu1H97rNyhn",
                "type": 4
            },
            "hash": "C5r1VqkYWnBAHtEqJBHUjXPzbKG9f7QYXNEd5sD2KdQb"
        },
        "txIdx": {
            "blockHash": "HRdfbN8Pp9iUuknJ7SWaRbKdy23x5zruLL7dBdVLFdxS"
        }
    }





GET ``/getReceipt``
-------------------


Summary
+++++++

Get a receipt


Parameters
++++++++++

.. csv-table::
    :delim: |
    :header: "Name", "Located in", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 15, 10, 10, 10, 20, 30

        hash | query | No | :ref:`account <i_346cd666f1509ac3d9df73bb80271146>` |  |  | 


Request
+++++++


Responses
+++++++++

**200**
^^^^^^^

Successful response


.. _i_9cc27f8d209e631e3f4cfff5e64b1b5e:

**Response Schema:**

.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        blockHash | No | string |  |  | 
        blockNo | No | integer |  |  | 
        contractAddress | No | string |  |  | 
        cumulativeFeeUsed | No | string |  |  | 
        feeUsed | No | string |  |  | 
        from | No | string |  |  | 
        ret | No | string |  |  | 
        status | No | string |  |  | 
        to | No | string |  |  | 
        txHash | No | string |  |  | 


**Example:**

.. code-block:: javascript

    {
        "blockHash": "HRdfbN8Pp9iUuknJ7SWaRbKdy23x5zruLL7dBdVLFdxS",
        "blockNo": 8,
        "contractAddress": "AmM1M4jAxeTxfh9CHUmKXJwTLxGJ6Qe5urEeFXqbqkKnZ73Uvx4y",
        "cumulativeFeeUsed": "0",
        "feeUsed": "0",
        "from": "AmPpcKvToDCUkhT1FJjdbNvR4kNDhLFJGHkSqfjWe3QmHm96qv4R",
        "ret": "",
        "status": "SUCCESS",
        "to": "AmM1M4jAxeTxfh9CHUmKXJwTLxGJ6Qe5urEeFXqbqkKnZ73Uvx4y",
        "txHash": "C5r1VqkYWnBAHtEqJBHUjXPzbKG9f7QYXNEd5sD2KdQb"
    }





GET ``/getReceipts``
--------------------


Summary
+++++++

Get receipt list in block


Parameters
++++++++++

.. csv-table::
    :delim: |
    :header: "Name", "Located in", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 15, 10, 10, 10, 20, 30

        hash | query | No | :ref:`account <i_346cd666f1509ac3d9df73bb80271146>` |  |  | 
        number | query | No | :ref:`number <i_71063bc7a76e3760c678a32453e127bc>` |  |  | 


Request
+++++++


Responses
+++++++++

**200**
^^^^^^^

Successful response


.. _i_7fd4667809a7a0cdae794dcddf3b338f:

**Response Schema:**

.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        blockNo | No | integer |  |  | 
        receipts | No | array of :ref:`200 <i_9cc27f8d209e631e3f4cfff5e64b1b5e>` |  |  | 

.. _i_9cc27f8d209e631e3f4cfff5e64b1b5e:

**200 schema:**


.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        blockHash | No | string |  |  | 
        blockNo | No | integer |  |  | 
        contractAddress | No | string |  |  | 
        cumulativeFeeUsed | No | string |  |  | 
        feeUsed | No | string |  |  | 
        from | No | string |  |  | 
        ret | No | string |  |  | 
        status | No | string |  |  | 
        to | No | string |  |  | 
        txHash | No | string |  |  | 


**Example:**

.. code-block:: javascript

    {
        "blockNo": 8,
        "receipts": [
            {
                "blockHash": "value",
                "blockNo": 5,
                "contractAddress": "value",
                "cumulativeFeeUsed": "value",
                "feeUsed": "value",
                "from": "value",
                "ret": "value",
                "status": "value",
                "to": "value",
                "txHash": "value"
            },
            {
                "blockHash": "value",
                "blockNo": 5,
                "contractAddress": "value",
                "cumulativeFeeUsed": "value",
                "feeUsed": "value",
                "from": "value",
                "ret": "value",
                "status": "value",
                "to": "value",
                "txHash": "value"
            },
            {
                "blockHash": "value",
                "blockNo": 5,
                "contractAddress": "value",
                "cumulativeFeeUsed": "value",
                "feeUsed": "value",
                "from": "value",
                "ret": "value",
                "status": "value",
                "to": "value",
                "txHash": "value"
            }
        ]
    }





GET ``/getTx``
--------------


Summary
+++++++

Get transaction information


Parameters
++++++++++

.. csv-table::
    :delim: |
    :header: "Name", "Located in", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 15, 10, 10, 10, 20, 30

        hash | query | No | :ref:`account <i_346cd666f1509ac3d9df73bb80271146>` |  |  | 


Request
+++++++


Responses
+++++++++

**200**
^^^^^^^

Successful response


.. _i_d33d46efb2d9e250461784c2467dd5f7:

**Response Schema:**

.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        tx | No | :ref:`txs <i_d091c46eb1749f90637f3af37f6d8d3f>` |  |  | 
        txIdx | No | :ref:`txIdx <i_39414f3c49d845cffc3017563fb74f34>` |  |  | 

.. _i_39414f3c49d845cffc3017563fb74f34:

**Txidx schema:**


.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        blockHash | No | string |  |  | 
        idx | No | integer |  |  | 

.. _i_d091c46eb1749f90637f3af37f6d8d3f:

**Txs schema:**


.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        body | No | :ref:`body <i_906d1d0743311e7e04cd566395f71f0d>` |  |  | 
        hash | No | string |  |  | 

.. _i_906d1d0743311e7e04cd566395f71f0d:

**Body schema:**


.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        account | No | string |  |  | 
        amount | No | string |  |  | 
        chainIdHash | No | string |  |  | 
        nonce | No | integer |  |  | 
        recipient | No | string |  |  | 
        sign | No | string |  |  | 
        type | No | integer |  |  | 


**Example:**

.. code-block:: javascript

    {
        "tx": {
            "body": {
                "account": "AmNdzAYv3dYKFtPRgfUMGppGwBJS2JvZLRTF9gRruF49vppEepgj",
                "amount": "1000000000000000000",
                "chainIdHash": "BNVSYKKqSR78hTSrxkaroFK2S3mgsoocpWg9HYtb1Zhn",
                "nonce": 1,
                "recipient": "AmQ2tS46doLfqsH4vy7PXmvGn7paec8wgqPWmGrcJKiLZADrJcME",
                "sign": "AN1rKvtRfKap2yDrBAp3gefs2YyS362oTCWPQk1oEYVwsyRm4MfMDtvbAPazr5NPBBP9TJHda8yzCXQXz2hnzrc5WRoaWBxqd",
                "type": 4
            },
            "hash": "8EiUrFKE2ivrmbP21iAusiuHUViVdJPwqLgX2byZuRN1"
        },
        "txIdx": {
            "blockHash": "Dss9zEetcSJy8tr7uLxUr9C8LLjLj5XQYZsfkm7jp1sa",
            "idx": 0
        }
    }





GET ``/getTxCount``
-------------------


Summary
+++++++

Transaction number in block


Parameters
++++++++++

.. csv-table::
    :delim: |
    :header: "Name", "Located in", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 15, 10, 10, 10, 20, 30

        hash | query | No | :ref:`account <i_346cd666f1509ac3d9df73bb80271146>` |  |  | 
        number | query | No | :ref:`number <i_71063bc7a76e3760c678a32453e127bc>` |  |  | 


Request
+++++++


Responses
+++++++++

**200**
^^^^^^^

Successful response






GET ``/listEvents``
-------------------


Summary
+++++++

List event


Parameters
++++++++++

.. csv-table::
    :delim: |
    :header: "Name", "Located in", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 15, 10, 10, 10, 20, 30

        address | query | No | :ref:`account <i_346cd666f1509ac3d9df73bb80271146>` |  |  | 
        eventName | query | No | :ref:`account <i_346cd666f1509ac3d9df73bb80271146>` |  |  | 
        blockfrom | query | No | :ref:`number <i_71063bc7a76e3760c678a32453e127bc>` |  |  | 
        blockto | query | No | :ref:`number <i_71063bc7a76e3760c678a32453e127bc>` |  |  | 
        desc | query | No | :ref:`compressed <i_ab678747d1d8d0bcf93576dfe6bfeede>` |  |  | 
        argFilter | query | No | :ref:`account <i_346cd666f1509ac3d9df73bb80271146>` |  |  | 
        recentBlockCnt | query | No | :ref:`number <i_71063bc7a76e3760c678a32453e127bc>` |  |  | 


Request
+++++++


Responses
+++++++++

**200**
^^^^^^^

Successful response


.. _i_5e1cb5bcfb7c2e8aa39be2ca4bff6043:

**Response Schema:**

.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        events | No | array of :ref:`events <i_642ad77412b5f2f2d7c004a76e22d04c>` |  |  | 

.. _i_642ad77412b5f2f2d7c004a76e22d04c:

**Events schema:**


.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        Args | No | array of string |  |  | 
        BlockHash | No | string |  |  | 
        BlockNo | No | integer |  |  | 
        EventIdx | No | integer |  |  | 
        TxIndex | No | integer |  |  | 
        contractAddress | No | string |  |  | 
        eventName | No | string |  |  | 
        txHash | No | string |  |  | 


**Example:**

.. code-block:: javascript

    {
        "events": [
            {
                "Args": [
                    "name",
                    "hong"
                ],
                "BlockHash": "AahyLGkriDwqCGR4xBfgxUPo95r4ySqbiVDXUq5HJETS",
                "BlockNo": 9981,
                "EventIdx": 0,
                "TxIndex": 0,
                "contractAddress": "AmggBF4etmnuX2rHAtKszY2GJyLXrhN3Md7tL2Y3s8JKd99xLsak",
                "eventName": "save",
                "txHash": "6bJ7RBRAE6wFPRRQVH2TUGzQt93mHEZjSVbsPYnGxpve"
            }
        ]
    }





GET ``/queryContract``
----------------------


Summary
+++++++

Call to execute a smart contract


Parameters
++++++++++

.. csv-table::
    :delim: |
    :header: "Name", "Located in", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 15, 10, 10, 10, 20, 30

        address | query | No | :ref:`account <i_346cd666f1509ac3d9df73bb80271146>` |  |  | 
        name | query | No | :ref:`account <i_346cd666f1509ac3d9df73bb80271146>` |  |  | 
        query | query | No | :ref:`account <i_346cd666f1509ac3d9df73bb80271146>` |  |  | 


Request
+++++++


Responses
+++++++++

**200**
^^^^^^^

Successful response


.. _i_b8d1cd95c844764285de1c58f6c150fa:

**Response Schema:**

.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        tx | No | :ref:`txs <i_d091c46eb1749f90637f3af37f6d8d3f>` |  |  | 
        txIdx | No | :ref:`txIdx <i_48508353cb51d5490c49314cad9212a2>` |  |  | 

.. _i_48508353cb51d5490c49314cad9212a2:

**Txidx schema:**


.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        blockHash | No | string |  |  | 

.. _i_d091c46eb1749f90637f3af37f6d8d3f:

**Txs schema:**


.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        body | No | :ref:`body <i_906d1d0743311e7e04cd566395f71f0d>` |  |  | 
        hash | No | string |  |  | 

.. _i_906d1d0743311e7e04cd566395f71f0d:

**Body schema:**


.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        account | No | string |  |  | 
        amount | No | string |  |  | 
        chainIdHash | No | string |  |  | 
        nonce | No | integer |  |  | 
        recipient | No | string |  |  | 
        sign | No | string |  |  | 
        type | No | integer |  |  | 


**Example:**

.. code-block:: javascript

    {
        "tx": {
            "body": {
                "account": "value",
                "amount": "value",
                "chainIdHash": "value",
                "nonce": 5,
                "recipient": "value",
                "sign": "value",
                "type": 5
            },
            "hash": "value"
        },
        "txIdx": {
            "blockHash": "value"
        }
    }





GET ``/queryContractStateProof``
--------------------------------


Summary
+++++++

Query the state of a contract with variable name and optional index


Parameters
++++++++++

.. csv-table::
    :delim: |
    :header: "Name", "Located in", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 15, 10, 10, 10, 20, 30

        address | query | No | :ref:`account <i_346cd666f1509ac3d9df73bb80271146>` |  |  | 
        varname1 | query | No | :ref:`account <i_346cd666f1509ac3d9df73bb80271146>` |  |  | 
        varname2 | query | No | :ref:`account <i_346cd666f1509ac3d9df73bb80271146>` |  |  | 
        compressed | query | No | :ref:`compressed <i_ab678747d1d8d0bcf93576dfe6bfeede>` |  |  | 


Request
+++++++


Responses
+++++++++

**200**
^^^^^^^

Successful response


.. _i_7b4400998111e1b030d4caa018840ada:

**Response Schema:**

.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        auditPath | No | array of string |  |  | 
        key | No | string |  |  | 
        proofKey | No | integer |  |  | 
        proofVal | No | string |  |  | 


**Example:**

.. code-block:: javascript

    {
        "auditPath": [
            "value",
            "value",
            "value"
        ],
        "key": "value",
        "proofKey": 5,
        "proofVal": "value"
    }





POST ``/sendSignedTransaction``
-------------------------------


Summary
+++++++

Commit transaction to aergo server



Request
+++++++



.. _i_b37ac9865ff51082359a7c7fb947e49a:

Body
^^^^

.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        body | No | :ref:`body <i_196cde5a3e3d70faaea7528b5daca421>` |  |  | 
        hash | No | string |  |  | 

.. _i_196cde5a3e3d70faaea7528b5daca421:

**Body schema:**


.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        account | No | string |  |  | 
        amount | No | string |  |  | 
        chainIdHash | No | string |  |  | 
        nonce | No | integer |  |  | 
        payloadJson | No | :ref:`payloadJson <i_36bdc1d393de47b05b9c34a56428654d>` |  |  | 
        recipient | No | string |  |  | 
        sign | No | string |  |  | 
        type | No | integer |  |  | 

.. _i_36bdc1d393de47b05b9c34a56428654d:

**Payloadjson schema:**


.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        args | No | array of string |  |  | 
        name | No | string |  |  | 

.. code-block:: javascript

    {
        "body": {
            "account": "value",
            "amount": "value",
            "chainIdHash": "value",
            "nonce": 5,
            "payloadJson": {
                "args": [
                    "value",
                    "value",
                    "value"
                ],
                "name": "value"
            },
            "recipient": "value",
            "sign": "value",
            "type": 5
        },
        "hash": "value"
    }

Responses
+++++++++

**200**
^^^^^^^

Successful response


.. _i_a16d9438392a3f022ec81fdaeff4d966:

**Response Schema:**

.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        results | No | array of :ref:`results <i_489d1ad284ab0cbe1edddf81b3bf1598>` |  |  | 

.. _i_489d1ad284ab0cbe1edddf81b3bf1598:

**Results schema:**


.. csv-table::
    :delim: |
    :header: "Name", "Required", "Type", "Format", "Properties", "Description"
    :widths: 20, 10, 15, 15, 30, 25

        hash | No | string |  |  | 


**Example:**

.. code-block:: javascript

    {
        "results": [
            {
                "hash": "pK5LKSZ1mKIJyGJSkM4T496pCTu1xQhXv8ZBdjXTkkg="
            }
        ]
    }



  
Data Structures
~~~~~~~~~~~~~~~

