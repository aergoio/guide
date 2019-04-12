Transaction Fees
================

The Aergo protocol includes transaction fees that need to be paid according to the configuration of the network.

Currently, the public mainnet has a minimum fee of 0.002 aergo.

And additional fees is required to use a contract.

* 5 gaer per byte of the tx's payload
* 5 gaer per byte requested to update the state DB

Because Aergo does not have gas prime and gas limit, during the early period, execution of the contract has some limitations.

* To send a coin transfer TX, the sender must have a balance of 1.026 aergo or more.
* And the sender who to execute a contract must have a balance of 2.05 aergo or more.
* The maximum size of the state DB that can be modified per contract is 200 KB.

This fee system may change in the future.

Sidechains can configure a custom fee. For private chains, free transactions are also possible.
