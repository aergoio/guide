Transactions
============

Tx
--

+--------------------+--------+-----------------------------------------------------+
|       Field        | Type   | Description                                         |
+====================+========+=====================================================+
| Hash               | bytes  | Hash of body                                        |
+------+-------------+--------+-----------------------------------------------------+
| Body | Nonce       | uint64 | Increasing number used only once per sender account |
+      +-------------+--------+-----------------------------------------------------+
|      | Account     | bytes  | Decoded sender account address                      |
+      +-------------+--------+-----------------------------------------------------+
|      | Recipient   | bytes  | Decoded receiver account address                    |
+      +-------------+--------+-----------------------------------------------------+
|      | Amount      | bytes  | Amount of transfer                                  |
+      +-------------+--------+-----------------------------------------------------+
|      | Payload     | bytes  | Smart contract data                                 |
+      +-------------+--------+-----------------------------------------------------+
|      | Limit       | uint64 | Reserved                                            |
+      +-------------+--------+-----------------------------------------------------+
|      | Price       | bytes  | Reserved                                            |
+      +-------------+--------+-----------------------------------------------------+
|      | Type        | int    | 0 is normal type, 1 is governance type              |
+      +-------------+--------+-----------------------------------------------------+
|      | ChainIDHash | bytes  | Hash of chain ID                                    |
+      +-------------+--------+-----------------------------------------------------+
|      | Sign        | bytes  | ECDSA signature with secp256k1                      |
+------+-------------+--------+-----------------------------------------------------+

Payload
-------

A payload can be any kind of binary data, but is most often used with JSON strings for
`smart contract calls <contracts.html>`__.

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

FeeDelegation type
^^^^^^^^^^^^^^^

FeeDelegation transactions are used for calling smart contract to charge fees to contract that support fee delegation.

The following table shows the specification for each field of the transaction body.

===================  =========================  =================  =========================================================================================================================================
Action               Recipient                  Amount             Payload
===================  =========================  =================  =========================================================================================================================================
staking              :code:`aergo.system`       amount to stake    :code:`{"Name":"v1stake"}`
unstaking            :code:`aergo.system`       amount to unstake  :code:`{"Name":"v1unstake"}`
voting               :code:`aergo.system`       0                  :code:`{"Name":"v1voteBP","Args":[<peer IDs>]}`
voting DAO           :code:`aergo.system`       0                  :code:`{"Name":"v1voteDAO","Args":[<DAO ID>,<candidate>]}`
create name          :code:`aergo.name`         1 aergo            :code:`{"Name":"v1createName","Args":[<a name string>]}`
update name          :code:`aergo.name`         1 aergo            :code:`{"Name":"v1updateName","Args":[<a name string>, <new owner address>]}`
add admin            :code:`aergo.enterprise`   0 aergo            :code:`{"Name":"appendAdmin","Args":[<new admin address>]}`
remove admin         :code:`aergo.enterprise`   0 aergo            :code:`{"Name":"removeAdmin","Args":[<admin address>]}`
change raft cluster  :code:`aergo.enterprise`   0 aergo            :code:`{"Name":"changeCluster","Args":[{"command":"add","name":"[node name]","address":"[peer address]","peerid":"[peer id]"}]}`
add config           :code:`aergo.enterprise`   0 aergo            :code:`{"Name":"appendConf","Args":[<config key>,<config value>]}`
enable config        :code:`aergo.enterprise`   0 aergo            :code:`{"Name":"enableConf","Args":[<config key>,<true|false>]}`
remove config        :code:`aergo.enterprise`   0 aergo            :code:`{"Name":"removeConf","Args":[<config key>,<config value>]}`
===================  =========================  =================  =========================================================================================================================================

The aergo.system transactions, including staking, unstaking and voting, can be sent about once per day per account. The only exception is when you first vote.
For staking and unstaking, there is a limit to the amount of requests. It must be over 10000 aergo based on the amount of staked. Therefore, the first staking request should exceed 10000 aergo, and in the case of the unstaking request, more than 10000 must be left or withdrawn altogether.

When voting for DAO, there are IDs by paramter. It will be changed to the new value when the first place gets 2/3 of the staking total.

===================  ================================  =========================================================================================================================================
DAO ID               Value                             Description
===================  ================================  =========================================================================================================================================
BPCOUNT              1 to 100                          The number of block producer
STAKINGMIN           1 to 500000000000000000000000000  The amount of staking minimum
GAPPRICE             1 to 500000000000000000000000000  The price of gas
NAMEPRICE            1 to 500000000000000000000000000  The price of name
===================  ================================  =========================================================================================================================================

The aergo.enterprise transactions are only for the private blockchain network. if you want to enable aergo.enterprise, you should make the genesis block with :code:`"public":"false"`


Transaction receipts
--------------------

.. seealso:: See `API → Receipt <../api/rpc-autogenerated.html#receipt>`__ for a detailed explanation of all the receipt data.

Every transaction generates a receipt upon succesful execution.
The :code:`status` can be one of three values:

SUCCESS
    Simple value transfer transactions and succesful contract executions.
    For contract calls, the result is available in :code:`result`.

ERROR
    Failed contract execution. The error message can be found in :code:`result`.

CREATED
    Succesful contract deployment transaction. The created address can be found in :code:`contractAddress`.
