Reference
=========

## Overview

Aergo uses Lua, a lightweight scripting language, as its smart contract language.
The following is an example of a simple coinstack smart contract written in Lua that stores key-value pairs in a blockchain state store and reads the values:

```lua
-- Storing key-values in the state store
function set(key, value)
    system.setItem(key, value)
end

-- Returns the value corresponding to the key in the state store
function get(key)
    return system.getItem(key)
end

-- Output the hash value of the previous block. The output is printed on the server log.
function printPrevBlock()
    system.print(system.getPrevBlockHash())
end

-- Functions to be called for contract declare as abi
abi.register(set, get, printBlock)
```

To read and write data on a blockchain within a contract, you must use the system package.
In the above example, the smart contract provides the function to access the key-value repository through the setItem and getItem functions of the system package and store the data permanently. 
In addition, a simple debug message can be output to the node's log file via the print function.


## system package
This packages provides blockchain information and store/get state

### getBlockheight() 
This function returns the block number that contains the current contract transaction.
### getPrevBlockHash()
This function returns the hash of the previous block
### getTimestamp()
This function returns the creation start time of the block that contains current contract transaction.
### getTxhash()
This function returns the id of the current contract transaction.
### getAmount()
This function returns number of AER sent with contract call. Return type is string.
### isFeeDelegation()
This function returns whether the transaction is using fee delegation.
### getContractID()
This function returns the current contract address.
### getCreator()
This function returns creator address of current contract
### getOrigin()
This function returns sender address of current transaction
### getSender()
This function returns caller address of current contract 
### isContract(address)
This function return if address is contract then true else false. when address is invalid then raise error   
### setItem(key, value) 
This function sets the value corresponding to key to the storage belonging to current contract

Restrictions:
* key type can only be string (number type is implicitly converted to string)
* value type can be: number, string, table
### getItem(key)
This function returns the value corresponding to key in storage belonging to current contract
* If there is no value corresponding to key, it returns nil.
### print(args...)
This function print args with json format at console log in node running current contract


## contract package
This packages provides contract operation

### send(address, amount)
This function transfers an amount of coins from this contract to the given address.
The Amount can be in string, number or bignum formats. The default unit is AER.

```lua
contract.send("Amh4S9pZgoJpxdCoMGg6SXEpAstTaTQNfQdZFsE26NpkqPwmaWod", 1)
contract.send("Amh4S9pZgoJpxdCoMGg6SXEpAstTaTQNfQdZFsE26NpkqPwmaWod", "1 aergo 10 gaer")
contract.send("Amh4S9pZgoJpxdCoMGg6SXEpAstTaTQNfQdZFsE26NpkqPwmaWod", bignum.number("999999999999999"))
```

### deploy(code, args...)
The deploy function creates a contract account using `code` and `args`, and returns the corresponding address and the returned value(s) from the constructor function.

If you use an existing address instead of code, it will be deployed with the code from the given address.

In addition, you can call the `value` function to send coins to the new contract.
The value function accepts a string, a number or a bignum as the argument like the send function.

```lua
    src = [[
        function hello(say)
            return "Hello " .. say .. system.getItem("made")
        end
        function constructor(check)
            system.setItem("made", check)
        end
        abi.register(hello)
        abi.payable(constructor)
    ]]
    addr = contract.deploy.value("1 aergo")(src, system.getContractID())
```

### call(address, function_name, args...)

The call function executes a function on another contract and returns the result. The call is executed in the context of the target contract's state.

We can also call the `value` function to send an amount of native aergo tokens to the contract.

The value function accepts a string, a number or a bignum as the argument like the send function.

```lua
contract.call("Amh4S9pZgoJpxdCoMGg6SXEpAstTaTQNfQdZFsE26NpkqPwmaWod", "inc", 1)
contract.call.value(10)("Amh4S9pZgoJpxdCoMGg6SXEpAstTaTQNfQdZFsE26NpkqPwmaWod", "inc", 2)
```

If an error occurs on the called contract (or another one called by it) then the execution is halted and the transaction is marked as failed.

To avoid that, use the `pcall` command, like this:

```lua
local success, result = pcall(function()
  return contract.call("Amh4S9pZgoJpxdCoMGg6SXEpAstTaTQNfQdZFsE26NpkqPwmaWod", "do_something", 123, "id")
end)
if success == false then
  -- the call failed
  ...
end
return result
```

On this case when the called function fails, no error is raised and the contract execution continues.
It just returns `false` to indicate the failure.

#### Restrictions

- maximum call depth of 64 calls in one transaction


### delegatecall(address, function_name, args...)
The delegatecall function loads the code from the target address and executes it in the context of the calling contract.

Storage, current address and balance still refer to the calling contract, only the code is taken from the called address.

Notice that the call can change the state of the current contract.

This is used to implement the "library" feature, with reusable library code.

```lua
contract.delegatecall("Amh4S9pZgoJpxdCoMGg6SXEpAstTaTQNfQdZFsE26NpkqPwmaWod", "inc", 1)
```

#### Restrictions

- maximum call depth of 64 calls in one transaction


### balance(address)
This function returns the balance of the given address in AER. The return type is string.
If address is nil then it returns the balance of the current address.

```lua
contract.balance()  -- get balance of current contract address 
contract.balance("Amh4S9pZgoJpxdCoMGg6SXEpAstTaTQNfQdZFsE26NpkqPwmaWod")
```

### event(eventName, args...)
This function fires an event containing `eventName` and `args`.
The event is recorded in the blockchain in the contract result receipt.
It is also broadcasted to all the listening applications connected to the blockchain network that have subscribed to this event.

The user can search for events that happened in the past and also receive notifications of new events by using RPC.

```lua
contract.event("send", 1, "toaddress") 
```

#### Restrictions

- maximum length of event name: 64
- maximum size of event argument: 4k
- maximum number of events in one transaction: 50

#### Examples: searching for event and receiving notification with aergocli

Searching for events with name "send" from the contract address AmhbdCEg4TUFm6Hpdoz8d81eSdzRncsekBLN3mYgLCbAVdPnu1MZ

``` bash
./aergocli event list --address AmhbdCEg4TUFm6Hpdoz8d81eSdzRncsekBLN3mYgLCbAVdPnu1MZ --event send
```

Searching for events where argument 0 is 1 and argument 1 is "toaddress" in the contract address AmhbdCEg4TUFm6Hpdoz8d81eSdzRncsekBLN3mYgLCbAVdPnu1MZ

``` bash
./aergocli event list --address AmhbdCEg4TUFm6Hpdoz8d81eSdzRncsekBLN3mYgLCbAVdPnu1MZ --argfilter '{"0":1, "1":"toaddress"}'
```

Getting notifications for events with name "send" for contract AmhbdCEg4TUFm6Hpdoz8d81eSdzRncsekBLN3mYgLCbAVdPnu1MZ

``` bash
./aergocli event stream --address AmhbdCEg4TUFm6Hpdoz8d81eSdzRncsekBLN3mYgLCbAVdPnu1MZ --event send
```

### stake(amount), unstake(amount), vote([bps,...]), voteDao(arg1, arg2,...)
You can do governance (stake, unstake, vote, voteDao) in the smart contract, using the contract address

```lua
contract.stake("1 aergo") 
contract.vote(["<bp1 address>", "<bp2 address>"....)
contract.voteDao("gasprice", "5")
contract.unstake("1 aergo")
```

## Built-in Functions
Lua provides the language itself as a useful function and basic package. It provides useful functions such as string management functions, so you can easily create smart contracts using these functions. Please refer to the Lua Reference Manual for detailed syntax, explanation, basic built-in functions and packages.

Since Lua Smart Contract is performed in Blockchain, OS related functions including input / output are not provided for stability and security.

Here is a list of the main functions that are not available.
```
print, loadstring, dofile, loadfile and module
```
Here is a list of the default packages that are not available.
```
coroutine, io, os and debug
```
The string, math, and table packages are available. However, you can not use the random, randomseed functions in the math package.


## DB package

If the smart contract is handling simple types of data, it would not be difficult to implement using only the basic APIs (getItem(), setItem()). However, complex data structures, data association, scope queries, filtering, sorting, and other features require the complexity and size of the data logic so developers can not focus on critical business logic. To solve this problem, Aergo supports SQL. This section details the types and usage of SQL APIs available in smart contracts

> Note: The db package is currently only available on private networks and publicly on [SQL TestNet](https://sqltestnet.aergoscan.io/) (currently unavailable).

### exec(sql)
This function performs DDL or DML statements

### query(sql)
This function performs SELECT statements and returns a result set object

### prepare(sql)
This function creates a prepared statement and returns a statement object

```lua
-- create customer table 
function createTable()
  db.exec([[create table if not exists customer(
        id text,
        passwd text,
        name text,
        birth text,
        mobile text
    )]])
end

function insert(id, passwd, name, birth, mobile)
  db.exec("insert into customer values (?, ?, ?, ?, ?)",
      id, passwd, name, birth, mobile)
end
```

### Functions of result set object
Object functions must be called with the `:` operator

#### next
This function prepares the next result row. Returns `true` if there is a row, otherwise `false`

#### get
This function returns a result row

```lua
function query(id)
  local rt = {}
  local rs = db.query("select * from customer where id like '%' || ? || '%'", id)
  while rs:next() do 
    local col1, col2, col3, col4, col5 = rs:get()
    local item = {
        id = col1,
        passwd = col2,
        name = col3,
        birth = col4,
        mobile = col5
    }
    table.insert(rt, item)
  end
  return rt
end
```

### Functions of prepared statement object
You can use the parameters in the SELECT or DML statement to view, add, modify, or delete information.

#### query(bind1, bind2, ....)
This function executes a SELECT statement using the specified argument values corresponding to bind parameters and returns a result set object

#### exec(bind1, bind2, ....)
This function executes a DML statement using the specified argument values corresponding to bind parameters

### SQL Restrictions
Smart Contract's SQL processing engine is built on SQLite. Therefore, detailed SQL usage grammar can be found at <https://sqlite.org/lang.html> and <https://sqlite.org/lang_corefunc.html>. However, because of the stability and security of the Aergo, not all SQL is allowed.

#### types
Allow only SQL datatypes corresponding to Lua strings and numbers (int, float).
* text
* integer
* real
* null
* date, datetime

#### SQL statement
The allowed SQL statements are listed below. However, DDL and DML are only allowed in smart contract transactions.
* DDL
    * TABLE: ALTER, CREATE, DROP
    * VIEW: CREATE, DROP
    * INDEX: CREATE, DROP
* DML
    * INSERT, UPDATE, DELETE, REPLACE
* Query
    * SELECT

#### Function
The following is an unavailable list
* Data and time related functions can be used except 'now' timestring and 'localtime' modifier
* load_XXX function
* random function
* sqlite_XXX function

For a list of other functions and descriptions, please refer to the links below.
* Core: <https://www.sqlite.org/lang_corefunc.html>
* Date and time: <https://www.sqlite.org/lang_datefunc.html>
* Aggregation: <https://www.sqlite.org/lang_aggfunc.html>

#### Constraint
The following constraints can be used.
* NOT NULL
* DEFAULT
* UNIQUE
* PRIMARY KEY(FOREIGN KEY)
* CHECK

```lua
function init_database()
    db.exec("drop table if exists customer")
    db.exec("create table customer (cid integer PRIMARY KEY ASC AUTOINCREMENT , passwd text , cname text, birthdate date, rgdate date)")
    db.exec("insert into customer (cid, passwd, cname, birthdate, rgdate) values (100 ,'passwd1','홍길동', date('1988-01-03'),date('2018-05-30'))")
    db.exec("insert into customer (passwd, cname, birthdate, rgdate) values ('passwd2','김철수', date('1978-11-03'),date('2018-05-30'))")
    db.exec("insert into customer (passwd, cname, birthdate, rgdate) values ('passwd3','이영미', date('1938-04-23'),date('2018-05-30'))")

    db.exec("drop table if exists product")
    db.exec("create table product (pid integer PRIMARY KEY ASC AUTOINCREMENT, pname text, price real, rgdate date)")
    db.exec("insert into product (pid, pname, price, rgdate) values (1000 ,'사과',1000, date('2018-05-30'))")
    db.exec("insert into product (pname, price, rgdate) values ('수박',10000, date('2018-05-30'))")
    db.exec("insert into product (pname, price, rgdate) values ('포도',3500, date('2018-05-30'))")

    db.exec("drop table if exists order_hist")
    db.exec("create table order_hist (oid integer PRIMARY KEY ASC AUTOINCREMENT, cid integer, pid integer, p_cnt integer, total_price real, rgtime datetime, FOREIGN KEY(cid) REFERENCES customer(cid), FOREIGN KEY(pid) REFERENCES product(pid) )")
    db.exec("insert into order_hist(oid, cid, pid,  p_cnt, total_price,rgtime) values(10000,100,1000,3,3000, datetime('2018-05-30 16:00:00'))")
    db.exec("insert into order_hist(cid, pid, p_cnt, total_price, rgtime) values(100,1000,3,3000, datetime('2018-05-30 17:00:00'))")
    db.exec("insert into order_hist(cid, pid, p_cnt, total_price, rgtime) values(100,1000,3,3000, datetime('2018-05-30 18:00:00'))")
end
```


## json package
Json package is provided for user convenience in input and output. This package allows automatic conversion between JSON format strings and Lua Table structures.

### encode(arg)
This function returns a JSON-formatted string with the given lua value.

### decode(string)
This function converts a string in JSON format to the corresponding Lua structure and returns it


## crypto package

### sha256(arg)
This function computes the SHA-256 hash of the argument.

### ecverify(message, signature, address)
This function verifies the address associated with the public key from elliptic curve signature.
```lua
function validate_sig(data, signature, address)
    msg = crypto.sha256(data)
    return crypto.ecverify(msg, signature, address)
end
```


## bignum package
Since the lua number type has a limit on the range that can be represented by an integer (less than 2 ^ 53), the bignum module is used to provide an exact operation for larger numbers.
  * <b>Notice</b>
    * <b>== Operations on bignum and other types always return false.</b>
    * <b>Bignum does not allow a decimal point.</b>
    * <b>Bignum value range : -(2^256 - 1) ~ (2^256 -1) </b>

### number(x)
This function make bignum object with argument x(string or number)
### isneg(x)
Check bignum x if negative than return true else false 
### iszero(x)
Check bignum x if zero than return true else false
### tonumber(x)
Convert bignum x to lua number
### tostring(x) (bignum.tostring(x) same as tostring(x))
Convert bignum x to lua string
### neg(x) (same as -x)
Negate bignum x and return as bignum
### sqrt(x)
Returns the square root of a positive number as bignum. not permitted negative value to x
### compare(x, y)
Compare two big numbers.  Return value is 0 if equal, -1 if x is less than y and +1 if x is greater than y.
### add(x, y) (same as x + y)
Add two big numbers and return bignum
### sub(x, y) (same as x - y)
Subtract two big numbers and return bignum
### mul(x, y) (same as x * y)
Multiply two big numbers and return bignum
### mod(x, y) (same as x % y)
Returns the bignum remainder after bignum x is divided by bignum y
### div(x, y) (same as x / y)
Divide two big numbers and return bignum
### pow(x, y) (same as x ^ y)
Power of two big numbers and return bignum. not permitted negative value to y
### divmod(x, y)
Returns a pair of big numbers consisting of their quotient and remainder
### powmod(x, y, m)
Return the bignum remainder after pow(bignum x,bignum y) is divided by bignum m. not permitted negative value to y
### isbignum(x)
Return true if x is bignum else false
### tobyte(x)
Return byte string of bignum x. not permitted negative value to x
### frombyte(x)
Return bignum from byte string x

``` lua
function factorial(n,f)
 for i=2,n do f=f*i end
 return f
end
for i=1,30 do
  local b=factorial(i,bignum.number(1))
  return bignum.mod(b, 10)
end
```
