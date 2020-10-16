# Hello World

This is the most basic lua smart contract to store and retrieve states in aergo.
You can save a name on the blockchain with the contract call function. And you can print 'hello ...' with the query function.

``` lua
-- Define global state variables
state.var {
  Name = state.value(),
  My_map = state.map()
}

-- Initialize the state variables
function constructor()
  -- a constructor is called only once at the contract deployment
  Name:set("world")
  My_map["key"] = "value"
end

-- Update the name
-- @call
-- @param name          string: new name
function set_name(name)
  Name:set(name)
end

-- Say hello
-- @query
-- @return              string: 'hello ' + name
function hello()
  return "hello " .. Name:get()
end

-- register functions to expose
abi.register(set_name, hello)
```

## Tutorial
This is explained based on using cli. Variables used in this example are
* Account to deploy and execute a contract: `AmPbWrQbtQrCaJqLWdMtfk2KiN83m2HFpBbQQSTxqqchVv58o82i`
* cli commands in this page need a aergosvr with enable personal feature

### Check Account and Balance
First, you need an account with enough balance to deploy and execute smart contracts.
(If you don't) Import or Unlock Account to aergo server.

### Compile Contract
Copy above code and save it to a file (e.g. helloword.lua). And Compile using the `aergoluac` compiler
``` bash
./aergoluac --payload helloworld.lua
37mGLDoCPNDQw7HbCG5WPfcM3E3cLhqhgE2V2UJKwQp9QZ5nJhT14nkCdGFcmN91fewB2ZuMZ5NWJUPyD4G4G2beaTeE1cigLzyNdGuuU4Y7cY2A6MUMq5weoAGGJdyf6PUfzgQ7k1cwjjDRe7P8bN5tNR8xxiibYk1hoeV2fuG4ZGVUwosutqFYePonLAtvK57N2MphJWVkTVxkySkXBnKywCiKB2pqv93VVQbBepYrUwURAeaj5aGhuFa8sxbSjuZfrvdy3i7dAEesf1jPHyeoN6CHWEB3teQJHUUh5K89p8vZ7nXKYPSa7MbzEg3YCUx8uLYmkJPjqA5YT1dxcKWCmHV4M2nYxmv3h9v3viPFkkFuMdpJboCTV2iqkMr3robxT6okSJtDdVUk2PprZKuiipS6tbKmDmxuicJqFvtP653qdWpu4tvWQDBb5k3UJcMTcEDdrjqqtNcE8dujFxr4TWfDu9Nwb4FedLrb5z7CoRWvXi5eZBkujJdDBm5MowamWLy9Pu6UmjAmBHfkDpQiagocrFzZrDPvNbaon8r5Rph8gFHAzwaUdM3dpt3dghePsiH1jbz8j93aMhMxgE6Lgap
```

### Deploy Contract
With the payload generated above, Deploy contract
``` bash
./aergocli contract deploy AmPbWrQbtQrCaJqLWdMtfk2KiN83m2HFpBbQQSTxqqchVv58o82i --payload 37mGLDoCPNDQw7HbCG5WPfcM3E3cLhqhgE2V2UJK
wQp9QZ5nJhT14nkCdGFcmN91fewB2ZuMZ5NWJUPyD4G4G2beaTeE1cigLzyNdGuuU4Y7cY2A6MUMq5weoAGGJdyf6PUfzgQ7k1cwjjDRe7P8bN5tNR8xxiibYk1hoeV2fuG4ZGVUwosutqFYePonLAtvK57N2MphJWVkTVxkySkXBnKywCiKB2pqv93VVQbBepYrUwURAeaj5aGhuFa8sxbSjuZfrvdy3i7dAEesf1jPHyeoN6CHWEB3teQJHUUh5K89p8vZ7nXKYPSa7MbzEg3YCUx8uLYmkJPjqA5YT1dxcKWCmHV4M2nYxmv3h9v3viPFkkFuMdpJboCTV2iqkMr3robxT6okSJtDdVUk2PprZKuiipS6tbKmDmxuicJqFvtP653qdWpu4tvWQDBb5k3UJcMTcEDdrjqqtNcE8dujFxr4TWfDu9Nwb4FedLrb5z7CoRWvXi5eZBkujJdDBm5MowamWLy9Pu6UmjAmBHfkDpQiagocrFzZrDPvNbaon8r5Rph8gFHAzwaUdM3dpt3dghePsiH1jbz8j93aMhMxgE6Lgap
1 : FPqA3kNQHoVXqKJv8JNpUSsh8F8id87yvRr5UzQFoCcH TX_OK
``` 

### Get receipt of contract
Look up the actual contract address with the transaction ID above. 

``` bash
./aergocli receipt get FPqA3kNQHoVXqKJv8JNpUSsh8F8id87yvRr5UzQFoCcH
{
 "BlokNo": 317,
 "BlockHash": "48zceVwBZt5dpzuEFMtJB9icPXUu7YG1Xkxvw5N92yFW",
 "contractAddress": "AmfzX3SHXVTBU9NSEWXaLxxjN11KsUpm1Gb3YjF7kmsHrgmL41WU",
 "status": "CREATED",
 "ret": {},
 "txHash": "AWeaoCTpohuQpBMTFaW3qFpZqWwuTehXA8ZkAX59UjMV",
 "txIndex": 0,
 "from": "AmPbWrQbtQrCaJqLWdMtfk2KiN83m2HFpBbQQSTxqqchVv58o82i",
 "to": "",
 "usedFee": 10,
 "events": []
}
```

If the status is not 'CREATED', it may not be included in the block yet, or there may be an error. Wait a while until the transaction is included in the block. Or check the server's error log.

### Get ABI of contract
Look up ABI of contract with the contract address above.

``` bash
./aergocli contract abi AmfzX3SHXVTBU9NSEWXaLxxjN11KsUpm1Gb3YjF7kmsHrgmL41WU
{
 "version": "0.2",
 "language": "lua",
 "functions": [
  {
   "name": "hello"
  },
  {
   "name": "set_name",
   "arguments": [
    {
     "name": "name"
    }
   ]
  },
  {
   "name": "constructor"
  }
 ],
 "state_variables": [
  {
   "name": "Name",
   "type": "value"
  },
  {
   "name": "My_map",
   "type": "map"
  }
 ]
}
```

### Query Initial State
You can query the generated contract in the following way.

``` bash
./aergocli contract query AmfzX3SHXVTBU9NSEWXaLxxjN11KsUpm1Gb3YjF7kmsHrgmL41WU hello
value:"\"hello world\""
```

You can see that the name 'world' assigned by the constructor is output.

### Call Contract
You can change the name recorded in the block chain as follows:

``` bash
./aergocli contract call AmPbWrQbtQrCaJqLWdMtfk2KiN83m2HFpBbQQSTxqqchVv58o82i AmfzX3SHXVTBU9NSEWXaLxxjN11KsUpm1Gb3YjF7kmsHrgmL41WU
 set_name '["aergo"]'
1 : 8mcuEFNxNCF6h4Q3FJk3mGN356R1AmWgGptAgJHNfaKs TX_OK
```

### Query Changed State
If you look at the results again, it has changed.

``` bash
./aergocli contract query AmfzX3SHXVTBU9NSEWXaLxxjN11KsUpm1Gb3YjF7kmsHrgmL41WU hello
value:"\"hello aergo\""
```

### Query contract variable with merkle proof
Value
``` bash
./aergocli contract statequery AmfzX3SHXVTBU9NSEWXaLxxjN11KsUpm1Gb3YjF7kmsHrgmL41WU Name --compressed
```
Map
``` bash
./aergocli contract statequery AmfzX3SHXVTBU9NSEWXaLxxjN11KsUpm1Gb3YjF7kmsHrgmL41WU My_map key --compressed
```
Array
``` bash
./aergocli contract statequery AmfzX3SHXVTBU9NSEWXaLxxjN11KsUpm1Gb3YjF7kmsHrgmL41WU array_name array_index --compressed
```
By default, the returned state is the one at the latest block, but you may specify any past block's state root.
``` bash
./aergocli contract statequery AmhbdCEg4TUFm6Hpdoz8d81eSdzRncsekBLN3mYgLCbAVdPnu1MZ var_name --root "9NBSjkcNTdE5ciBxfb52RmsVW7vgX5voRsv6KcosiNjE"
```
