Transactions
============

Tx
--

+------------------+--------+-----------------------------------------------------+
|       Field      | Type   | Description                                         |
+==================+========+=====================================================+
| Hash             | bytes  | Hash of body                                        |
+------+-----------+--------+-----------------------------------------------------+
| Body | Nonce     | uint64 | Increasing number used only once per sender account |
+      +-----------+--------+-----------------------------------------------------+
|      | Account   | bytes  | Decoded sender account address                      |
+      +-----------+--------+-----------------------------------------------------+
|      | Recipient | bytes  | Decoded receiver account address                    |
+      +-----------+--------+-----------------------------------------------------+
|      | Amount    |  bytes | Amount of transfer                                  |
+      +-----------+--------+-----------------------------------------------------+
|      | Payload   |  bytes | Smart contract data                                 |
+      +-----------+--------+-----------------------------------------------------+
|      | Limit     | uint64 | Reserved                                            |
+      +-----------+--------+-----------------------------------------------------+
|      | Price     | bytes  | Reserved                                            |
+      +-----------+--------+-----------------------------------------------------+
|      | Type      | int    | 0 is normal type, 1 is governance type              |
+      +-----------+--------+-----------------------------------------------------+
|      | Sign      | bytes  | ECDSA signature with secp256k1                      |
+------+-----------+--------+-----------------------------------------------------+

Transaction types
-----------------

There are two kinds of transactions.

Normal type
^^^^^^^^^^^

Normal transactions are used to transfer tokens and calling smart contracts.

Governance type
^^^^^^^^^^^^^^^

Governance transactions are used for calling system contracts, such as staking and voting.
Transactions of this type have a special payload format and recipient.

The following table shows the specification for each field of the transaction body.

===========  ====================  =================  ==========================================
Action       Recipient             Amount             Payload                                   
===========  ====================  =================  ==========================================
staking      :code:`aergo.system`  amount to stake    :code:`s`                                 
unstaking    :code:`aergo.system`  amount to unstake  :code:`u`                                 
voting       :code:`aergo.system`  0                  :code:`v<peer ids bytes, no separator>`   
create name  :code:`aergo.name`    1 aergo            :code:`c<name string>`                    
update name  :code:`aergo.name`    1 aergo            :code:`u<name string>,<new owner address>`
===========  ====================  =================  ==========================================