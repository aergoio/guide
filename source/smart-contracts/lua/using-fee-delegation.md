# Fee delegation

Generally the user who sends a transaction needs to pay the transaction execution fee.
The fee delegation is a feature that allows the contract to pay the execution, so the user does not need to pay for it.

In order to use this feature, the contract needs to support fee delegation and the transaction must be built with the FeeDelegation type.
The contract also needs to have sufficient balance to pay the execution.

## abi.fee_delegation()

The functions to be executed using fee delegation must be specified with `abi.fee_delegation()`

``` lua
    abi.fee_delegation(work, work2)
```

## check_delegation function

The `check_delegation` is a special function used to check whether the transaction can be processed using fee delegation.
In another words: if the contract will pay for this specific transaction execution or not.

It is called by the engine when a transaction with fee delegation type arrives (a transaction asking to be paid by the contract).
The arguments to this function are:

* the name of the function that the transaction wants to call/execute
* the arguments to that function

You can use any logic to determine if the transaction will be paid by the contract.
It is common to check who is sending the transaction with `system.getOrigin()` or `system.getSender()`.

This function should return a boolean (true=accept or false=reject) and it is read-only (it should not write to the state).

## Contract example

``` lua
state.var{
    whitelist = state.map(),
    item = state.value()
}

function reg(user)
    if (k == nil) then
        whitelist[system.getSender()] = true
    else
        whitelist[user] = true
    end
end

function work(arg0)
    if (system.isFeeDelegation() == true) then
        whitelist[system.getSender()] = false
    end
    item:set(arg0)
end

function check_delegation(fname,arg0)
    if (fname == "work") then
        return whitelist[system.getSender()]
    end
    return false
end

function default()
end

abi.register(reg, work)
abi.payable(default)
abi.fee_delegation(work)
```

## aergocli example

``` bash
aergocli contract call --delegation AmPbWrQbtQrCaJqLWdMtfk2KiN83m2HFpBbQQSTxqqchVv58o82i Amh6aHxfoMrCmXd2GrV3Yem1zmcqgAiPaJAhsux3wedpEVUCGowx work
1 : HtyNZdJzJNYkeCQCrz8xozPvGLn1xhde9ExNEEiEKPy4 TX_OK
```

## Receipt

``` bash
aergocli receipt get HtyNZdJzJNYkeCQCrz8xozPvGLn1xhde9ExNEEiEKPy4
{
 "BlokNo": 9673,
 "BlockHash": "Az8pJvDso44nP9Lq5Wj7QoU6n9yZcFkCcmJaiRKyq9Qz",
 "contractAddress": "Amh6aHxfoMrCmXd2GrV3Yem1zmcqgAiPaJAhsux3wedpEVUCGowx",
 "status": "SUCCESS",
 "ret": "",
 "txHash": "HtyNZdJzJNYkeCQCrz8xozPvGLn1xhde9ExNEEiEKPy4",
 "txIndex": 0,
 "from": "AmPbWrQbtQrCaJqLWdMtfk2KiN83m2HFpBbQQSTxqqchVv58o82i",
 "to": "Amh6aHxfoMrCmXd2GrV3Yem1zmcqgAiPaJAhsux3wedpEVUCGowx",
 "usedFee": 2000000000000000,
 "feeDelegation": true,
 "events": []
}
```
