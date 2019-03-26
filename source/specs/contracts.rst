Smart Contracts
===============

Contracts are special kinds of accounts that have code, an ABI, and state.

This is a technical description of the protocol and execution.
It is recommended to interact with contracts using higher level API supported by
aergocli and the various SDKs.

Deployment
----------

Contracts are deployed by sending a transaction with the contract code as payload to 
the null receiver (empty receiver address). Upon execution, the contract state is
created at an address calculated from the sender and the nonce of the deployment transaction.
You can find out the created contract address in the transaction receipt.

`See here <addresses.html>`__ for details about address generation. 

Calling Contracts
-----------------

To call contract methods, send a transaction to the contract's address specifying the
function name and arguments as JSON payload.

Events
------

Contracts can log events during execution. This is the preferred way to notify the outside world of important state changes.

Logged events can be found as part of the transaction receipt and also easily searched for using the GetEvents API.