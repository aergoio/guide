Composable Transactions
========================

Normal transactions can make only a single call to a smart contract.

This means that when we need to call a function many times, or interact with different smart contracts,
we need a separate transaction for each smart contract interaction.

Composable Transactions bring the ability to **make many calls in a single transaction**, either to the same smart contract or to different ones.


## Characteristics

1. Multiple calls
2. Atomicity
3. Sequential execution
4. Act on current state
5. Scripting
6. No blind signing


### Atomicity

If any of the calls fail, the entire transaction is aborted.

This means that either all calls succeed or none of them.


### Sequential Execution

Execute a series of contract calls with the guarantee that they happen in sequence order with no external action occurring between them.


### Act on current state

Do actions using the current values from contract and account states
(by making queries to contracts and using the returned value on the same transaction)
instead of a previously hardcoded value acquired at transaction building time.

This is useful when another transaction arrives first and changes the contract state.


### Scripting

Instead of just a list of calls, it is possible to process the returned value and act according to them.

The language supports data conversion and manipulation, assertion, conditional execution, loops, and more.


### No Blind Signing

The transaction can be reviewed even on hardware wallets before approval or rejection



## Use Cases

**1)** Purchase airplane tickets and hotel reservation from different contracts, only if both succeed. If some of the purchases fail, the other is reverted (the user does not want one of them if the other is not acquired).

**2)** Swap token and buy something with it. If the purchase fails, the user does not want that token.

**3)** Swap tokens using a split route. If one of the routes fail, revert the other(s). Also check the minimum output in the transaction itself (by adding individual outputs) and revert if not satisfied.

**4)** Swap to an exact amount of the output token when using a multi-hop route, by either: a. doing a reverse swap of the surplus amount, or b. querying a contract for the right amount to send (between approved range) in the same transaction.

**5)** Add liquidity to a pool (2 tokens) in a single transaction

**6)** Trustless swap or purchase - check if the purchased token was received, otherwise revert the transaction

**7)** Transferring tokens (fungible and/or non-fungible) to multiple recipients at the same time

**8)** Transferring different tokens (fungible and/or non-fungible) to one recipient in a single transaction

**9)** Mint many non-fungible tokens (NFTs) in a single transaction

**10)** Approve contract B to use contract A (token) and then call a function on contract B that handles the resources on contract A (eg: approve and swap, approve and add liquidity) on a single transaction

**11)** Approve, use resource, and remove approval on a single transaction, with the guarantee that no other operation would happen in between while the resource/token is approved to contract B


## Implementation

The call information is stored in the transaction payload in JSON format.

You can view it as a list of calls processed in order:

```
[
  <call 1>
  <call 2>
  <call 3>
]
```

Each call containing the contract address, function to call, and arguments:

```
[
  [call, contract, function, arg1, arg2, arg3...],
  [call, contract, function, arg1, arg2, arg3...],
  [call, contract, function, arg1, arg2, arg3...]
]
```

### Some Examples

Buy 2 products or services on a single transaction:

```
[
  ["call","<token 1>","transfer","<amount 1>","<shop 1>","buy","product 1"],
  ["call","<token 2>","transfer","<amount 2>","<shop 2>","buy","product 2"],
]
```

Acquire token2 via swap and buy a product/service with it:

```
[
  ["call","<token 1>","transfer","<amount 1>","<swap pair>","swap",{"exact_output":"<amount 2>"}],
  ["call","<token 2>","transfer","<amount 2>","<shop>","buy","product"],
]
```

Add liquidity to a swap pair:

```
[
  ["call","<token 1>","transfer","<amount 1>","<swap_pair>","store_token"],
  ["call","<token 2>","transfer","<amount 2>","<swap_pair>","add_liquidity"],
]
```


### Variables

Another feature is the use of variables, in the format `%name%`

When a string in the above format is found, it is replaced by the corresponding value.

One hard-coded variable is the returned value from the last operation: `%last result%`

Example usage:

```
[
  ["call","<address 1>","function1","arg"],
  ["call","<address 2>","function2","%last result%"]
]
```


### Scripting

This feature uses a new simple scripting language based on JSON.

The first element of each operation is a command. Here are some of them:

* call
* store (result)
* assert

The `call` is a normal smart contract call

The `store` command is used to store the returned value in a temporary variable

The `assert` is used to ensure that a value is within expected bounds, otherwise the whole transaction is reverted

Example 1:

```
[
  ["call","<address>","function","arg"],
  ["assert","%last result%",">=","250"]
]
```

Example 2:

```
[
  ["call","<address 1>","function1","arg"],
  ["call","<address 2>","function2","arg"],
  ["assert","%last result%",">=","250"]
]
```

Example 3:

```
[
  ["call","<address 1>","function","arg"],
  ["store result as","result1"],
  ["call","<address 2>","function","arg"],
  ["assert","%last result%",">=","%result1%"]
]
```

Example 4:

```
[
  ["call","<address 1>","function1","arg"],
  ["store result as","result1"],
  ["call","<address 2>","function2","arg"],
  ["store result as","result2"],
  ["call","<address 3>","function3","%result1%","%result2%"]
]
```


### Commands

To support a variety of use cases, composable transactions contain many different commands:

* Smart contract call
* Sending Aergo tokens
* Checking Aergo balance
* Mathematical operations
* String operations
* Table operations (lists and dictionaries)
* Variables
* Conversions
* Assertion
* Conditional execution
* Loops
* Returning a result



## List of Commands

(click on a command to expand)


### contract call

<details>
<summary>call</summary><blockquote>

```javascript
["call",address,function,args...]
```

Calls a function on a smart contract

Examples:

```javascript
["call","%contract%","transfer","%to%","10.5"]
["call","Ammlaklkfld","transfer","Amafpoajf","10.5"]
```

If the call fails, the entire transaction is reverted
</blockquote></details>

<details>
<summary>call + send</summary><blockquote>

```javascript
["call + send",amount,address,function,args...]
```

Calls a function on a smart contract, also sending an amount of native AERGO tokens

Examples:

```javascript
["call + send","1.5 aergo","%contract%","buy_something","%arg%"]
```

If the call fails, the entire transaction is reverted
</blockquote></details>

<details>
<summary>try call</summary><blockquote>

```javascript
["try call",address,function,args...]
```

Makes a protected call to a function on a smart contract.

A "protected call" means that if the call fails, it continues execution on the next command.

It returns 2 values: `%call succeeded%` and `%last result%`

You can use that information to conditionally execute commands

Examples:

```javascript
["try call","%contract%","transfer","%to%","10.5"]
["if","%call succeeded%"]
["...","%last result%"]
...
["end if"]
```
</blockquote></details>

<details>
<summary>try call + send</summary><blockquote>

```javascript
["try call + send",amount,address,function,args...]
```

Makes a protected call to a function on a smart contract,
also sending an amount of native AERGO tokens.

A "protected call" means that if the call fails, it continues execution on the next command.

It returns 2 values: `%call succeeded%` and `%last result%`

You can use that information to conditionally execute commands

Examples:

```javascript
["try call + send","1.5 aergo","%contract%","buy_something","%arg%"]
["if","%call succeeded%"]
["...","%last result%"]
...
...
["end if"]
```
</blockquote></details>


### aergo balance and transfer

<details>
<summary>get aergo balance</summary><blockquote>

```javascript
["get aergo balance",address]
```

Returns the current balance in AERGO tokens from the given account address.

It is also possible to direclty use the `%my aergo balance%` variable
</blockquote></details>

<details>
<summary>send</summary><blockquote>

```javascript
["send",address,amount]
```

Transfer an amount of native AERGO tokens to the informed address

The amount can be a string or a bignumber
</blockquote></details>


### variables

<details>
<summary>let</summary><blockquote>

```javascript
["let",variable_name,value]
["let",variable_name,value,token_address]
```

Assign the value to the variable

The value can be a number, bignumber, string, boolean, list or object

It is also possible to convert an amount in decimal format to bignumber,
by supplying the address of the token.

Here are some examples:

```javascript
["let","min_amount","0.05","%token2%"]
["let","min_amount","1.5","%token2%"]
["let","min_amount","100","Am..."]
```
</blockquote></details>

<details>
<summary>store result as</summary><blockquote>

```javascript
["store result as",variable_name]
```

Store the result of the last operation in a variable with the given name

Example:

```javascript
["store result as","amount"]
```
</blockquote></details>


### tables

<details>
<summary>get</summary><blockquote>

```javascript
["get",table,key_or_pos]
```

Retrieve an element from a table (list or object)

For lists we inform the position:

```javascript
["get","%list%",3]
```

For dictionaries we inform the key in string format:

```javascript
["get","%result%","price"]
```
</blockquote></details>

<details>
<summary>set</summary><blockquote>

```javascript
["set",table,key_or_pos,value]
```

Set a value in a table (list or object)

For lists we inform the position:

```javascript
["set","%list%",2,"%value%"]
```

For dictionaries we inform the key in string format:

```javascript
["set","%obj%","price","%last result%"]
```
</blockquote></details>

<details>
<summary>insert</summary><blockquote>

```javascript
["insert",table,item]
["insert",table,pos,item]
```

Inserts an element on a list

Other elements are moved up

If no position is informed, the element is inserted at the end
</blockquote></details>

<details>
<summary>remove</summary><blockquote>

```javascript
["remove",table,position]
```

Removes an element from a list, by position

It returns the removed item
</blockquote></details>

<details>
<summary>get size</summary><blockquote>

```javascript
["get size",table_or_string]
```

Retrieve the number of elements on a table (array) or the length of a string
</blockquote></details>


### math

<details>
<summary>add</summary><blockquote>

```javascript
["add",value1,value2]
```

Adds 2 values and return the result

The values must be of the same type

They can be number or bignumber
</blockquote></details>

<details>
<summary>subtract</summary><blockquote>

```javascript
["subtract",value1,value2]
```

Subtract one value from the other and return the result

The values must be of the same type

They can be number or bignumber
</blockquote></details>

<details>
<summary>multiply</summary><blockquote>

```javascript
["multiply",value1,value2]
```

Multiplies 2 values and return the result

The values must be of the same type

They can be number or bignumber
</blockquote></details>

<details>
<summary>divide</summary><blockquote>

```javascript
["divide",value1,value2]
```

Divides the first value by the second and returns the result

The values must be of the same type

They can be number or bignumber
</blockquote></details>

<details>
<summary>remainder</summary><blockquote>

```javascript
["remainder",value1,value2]
```

Returns the modulo of the division of the first value by the second

The values must be of the same type

They can be number or bignumber
</blockquote></details>


### strings

<details>
<summary>combine</summary><blockquote>

```javascript
["combine",str1,str2,...]
```

Concatenates all the given strings into one
</blockquote></details>

<details>
<summary>format</summary><blockquote>

```javascript
["format",formatstring,...]
```

The same as [string.format](https://www.lua.org/manual/5.2/manual.html#pdf-string.format) in Lua

It can also be used for concatenation:

```javascript
["format","%s %s","hello","world"]
```
</blockquote></details>
<details>

<summary>extract</summary><blockquote>

```javascript
["extract",string,start]
["extract",string,start,end]
```

The same as [string.sub](https://www.lua.org/manual/5.2/manual.html#pdf-string.sub) in Lua
</blockquote></details>

<details>
<summary>find</summary><blockquote>

```javascript
["find",string,pattern]
["find",string,pattern,init]
```

The same as [string.match](https://www.lua.org/manual/5.2/manual.html#pdf-string.match) in Lua
</blockquote></details>

<details>
<summary>replace</summary><blockquote>

```javascript
["replace",string,pattern,replace]
["replace",string,pattern,replace,n]
```

The same as [string.gsub](https://www.lua.org/manual/5.2/manual.html#pdf-string.gsub) in Lua
</blockquote></details>


### conversions

<details>
<summary>to big number</summary><blockquote>

```javascript
["to big number"]
["to big number",value]
```

Converts a value to a bignumber

If no argument is given, it converts the last result
</blockquote></details>

<details>
<summary>to number</summary><blockquote>

```javascript
["to number"]
["to number",value]
```

Converts a value to a number

If no argument is given, it converts the last result
</blockquote></details>

<details>
<summary>to string</summary><blockquote>

```javascript
["to string"]
["to string",value]
```

Converts a value to a string

If no argument is given, it converts the last result
</blockquote></details>

<details>
<summary>to json</summary><blockquote>

```javascript
["to json"]
["to json",list_or_object]
```

Converts a value to a json string

If no argument is given, it converts the last result
</blockquote></details>

<details>
<summary>from json</summary><blockquote>

```javascript
["from json"]
["from json",json_string]
```

Converts a json string to a table

If no argument is given, it converts the last result
</blockquote></details>


### assertion

<details>
<summary>assert</summary><blockquote>

```javascript
["assert",<expression>]
```

If the assertion fails, the whole transaction is reverted and marked as failed

If the first value is a big number, the second value can be a string representation of an amount

The expression can be a single boolean value

Examples:

```javascript
["assert","%call succeeded%"]
["assert","%variable_name%",">","10.5 aergo"]
["assert","%var1%","<=","%var2%"]
["assert","%var1%","<","%var2%","and","%var1%",">=",1500]
["assert","%var1%","=","text1","or","%var1%","=","text2","or","%var1%","=","text3"]
```

The operators can be: `=` `!=` `>` `>=` `<` `<=` `match`

The logic operators: `and` `or`
</blockquote></details>


### conditional execution

<details>
<summary>if</summary><blockquote>

```javascript
["if",<expression>]
```

Executes the following commands only if the expression is true

The expression can be a single boolean value

Example expressions:

```javascript
["if","%call succeeded%"]
["if","%variable_name%",">",10]
["if","%var1%","<=","%var2%"]
["if","%var1%","<","%var2%","and","%var1%",">=",1500]
["if","%var1%","=","text1","or","%var1%","=","text2","or","%var1%","=","text3"]
```

The operators can be: `=` `!=` `>` `>=` `<` `<=` `match`

The logic operators: `and` `or`

Example:

```javascript
["if","%amount%",">",20],
...
["end if"]
```

**:warning: Important:** `if` statements can **NOT** be nested!
</blockquote></details>

<details>
<summary>else if</summary><blockquote>

```javascript
["else if",<expression>]
```

Executes the following commands only if the expression is true

Example expressions:

```javascript
["else if","%variable_name%",">",10]
["else if","%var1%","<=","%var2%"]
["else if","%var1%","<","%var2%","and","%var1%",">=",1500]
["else if","%var1%","=","text1","or","%var1%","=","text2","or","%var1%","=","text3"]
```

The operators can be: `=` `!=` `>` `>=` `<` `<=` `match`

The logic operators: `and` `or`

Example:

```javascript
["if","%amount%",">",20],
...
["else if","%amount%",">",10],
...
["end if"]
```
</blockquote></details>

<details>
<summary>else</summary><blockquote>

```javascript
["else"]
```

Example:

```javascript
["if","%amount%",">",20],
...
["else"],
...
["end if"]
```
</blockquote></details>

<details>
<summary>end if</summary><blockquote>

```javascript
["end if"]
```

Used to close an `if` statement. Check examples above
</blockquote></details>


### loops

<details>
<summary>for</summary><blockquote>

```javascript
["for",variable,start,end,increment]
```

Example:

```javascript
["for","n",1,10],
...
[...,"%n%"],
...
["loop"]
```

With a decrement:

```javascript
["for","n",50,10,-10],
...
[...,"%n%"],
...
["loop"]
```
</blockquote></details>

<details>
<summary>for each</summary><blockquote>

Iterate on the items from a list or a dictionary

To iterate on a list:

```javascript
["for each",item,"in",list]
```

Example:

```javascript
["let","list",[11,22,33]],
["for each","item","in","%list%"],
...
[...,"%item%"],
...
["loop"]
```

To iterate on a dictionary:

```javascript
["for each",key,value,"in",table]
```

Example:

```javascript
["let","payments",{"Am..":"10.5","Am..":"12.3"}],
["for each","address","amount","in","%payments%"],
["send","%address%","%amount%"],
["loop"]
```
</blockquote></details>

<details>
<summary>loop</summary><blockquote>

```javascript
["loop"]
```

This command can be used in 3 ways:

1. With a `for` command
2. With a `for each` command
3. Alone as the last command

When used alone, it will loop to the first command on the list.

We can exit the loop using the `break` command or a `return` inside of an `if` statement.
</blockquote></details>

<details>
<summary>break</summary><blockquote>

```javascript
["break"]
["break","if",<expression>]
```

Used to exit from a loop

Example:

```javascript
["for each","item","in","%list%"],
...
["break","if",...],
...
["loop"]
```

Or if already using an if statement:

```javascript
["for each","item","in","%list%"],
...
["if",...],
["break"],
["end if"],
...
["loop"]
```
</blockquote></details>


### return result

<details>
<summary>return</summary><blockquote>

```javascript
["return"]
["return",value]
```

Example:

```javascript
["return","%last result%"]
```
</blockquote></details>



### variables

There is also these predefined variables:

`%my account address%`

`%my aergo balance%`

Example:

```javascript
["assert","%my aergo balance%",">","10 aergo"],
["call","%token address%","balanceOf","%my account address%"],
```


## Example Scripts

Here is an example script (payload) that would be used to check the returned amount of tokens on a token swap:

```
["call","<tokenB>","balanceOf"],                         <-- get the previous balance of token B
["store result as","balance_before"],                    <-- store it in a variable
["call","<tokenA>","transfer","<to>","<amount>","swap"], <-- swap token A for token B
["call","<tokenB>","balanceOf"],                         <-- get the new balance of token B
["subtract","%last result%","%balance_before%"],         <-- subtract one balance by the other
["assert","%last result%",">=","<minimum_amount>"]       <-- assert that we got the minimum amount
```

This feature allows real **TRUSTLESS TRANSACTIONS**, where the user does not trust even the swap contract!

It can allow for direct token swaps between users A and B, in a complete trustless way




## Using Composable Transactions

To build a transaction with composable transactions we:

* Set the type to `MULTICALL` (7)
* Set the recipient to null/none
* Set the amount to 0
* Put the JSON script in the payload
