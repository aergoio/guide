# Using fee delegation
The user who requests for a transaction has to perform it by providing the transaction fee. However, if the user enables the feature of smart contract then transaction would automatically be done. Enabling the smart contract minimizes the burden of the user. 
Fee delegation can only be used through the support of contract. The transaction should be performed under the fee_delegation type.


## abi(abi.fee_delegation)
The function being performed should be specified in abi as fee_delegation.

``` lua
    abi.fee_delegation(work)
```

## check_delegation function
The definition of delegation is considered by the condition of check_delegation function. Check_delegation function works internally.  The function name (fname) and argument (arg0) are arguments of the transaction which are called by check_delegation. If check_delegation is true, fee_delegation is performed. (note: the check_delegation function is allowed read only)

## contract example

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

## receipt

``` bash
aergocli receipt get HtyNZdJzJNYkeCQCrz8xozPvGLn1xhde9ExNEEiEKPy4
{
 "BlokNo": 9673,
 "BlockHash": "Az8pJvDso44nP9Lq5Wj7QoU6n9yZcFkCcmJaiRKyq9Qz",
 "contractAddress": "Amh6aHxfoMrCmXd2GrV3Yem1zmcqgAiPaJAhsux3wedpEVUCGowx",
 "status": "SUCCESS",
 "ret": ""
 "txHash": "HtyNZdJzJNYkeCQCrz8xozPvGLn1xhde9ExNEEiEKPy4",
 "txIndex": 0,
 "from": "AmPbWrQbtQrCaJqLWdMtfk2KiN83m2HFpBbQQSTxqqchVv58o82i",
 "to": "Amh6aHxfoMrCmXd2GrV3Yem1zmcqgAiPaJAhsux3wedpEVUCGowx",
 "usedFee": 2000000000000000,
 "feeDelegation": true,
 "events": []
}
```
