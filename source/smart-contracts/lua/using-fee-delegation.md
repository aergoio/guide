# Using fee delegation
In order to perform a transaction, the user who sends the transaction is basically required to perform the transaction fee. Providing fees from the contract to enable users without fee to perform the contact will reduce the user's burden on using the contact.

In order to use the fee delegation, the contract need to support fee delegation and the transaction must be performed with the FeeDelegation type.

## abi(abi.fee_delegation)
The function to be performed must be specified in abi as fee_delegation.

``` lua
    abi.fee_delegation(work)
```

## check_delegation function
the function check_delegation function to check condition to delegation need to be defined.
The check_delegation function is performed internally and the call function name and arguments of the transaction are used as arguments.
The feedelegation type transaction is performed when the check_delegation function is true.
(note: the check_delegation function is allowed read only)

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
