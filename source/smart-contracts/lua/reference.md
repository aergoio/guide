Reference
=========

## Overview
aergo smart contract uses Lua, a lightweight scripting language, as a smart contract language.
The following is an example of a simple coin stack smart contract written in Lua that stores key-value values ​​in a block-chain state store and reads the values.
```lua
-- Storing key-values ​​in the state store
function set(key, value)
    system.setItem(key, value);
end
​
-- Returns the value corresponding to the key in the state store
function get(key)
    return system.getItem(key);
end
​
-- Output the hash value of the current block. The output is printed on the server log.
function printBlock()
    system.print(system.getBlockhash());
end

-- Functions to be called for contract declare as abi
abi.register(set, get, printBlock)
```
To read and write data on a block chain within a contract, you must use a system package. In the above example, smart contract  provides the function to access the key-value repository through the setItem and getItem functions of the system package and store the data permanently. In addition, a simple debug message can be output to the log file of the node via the print function.

## system package
This packages provides blockchain information and store/get state
### getSender()
This function returns current contract transaction caller address
### getBlockheight() 
This function returns the block number that contains the current contract transaction.
### getTxhash()
This function returns the id of the current contract transaction.
### getTimestamp()
This function returns the creation start time of the block that contains current contract transaction.
### getContractID()
This function returns the current contract address.
### getAmount()
This function returns a transaction containing balance
### setItem(key, value) 
This function sets the value corresponding to key to the storage belonging to current contract
* restriction
  * key type string only
  * value type available : number, string, table
### getItem(key)
This function returns the value corresponding to key in storage belonging to current contract
* If there is no value corresponding to key, it returns nil.
## contract package
This packages provides contract operation
### send(address, amount)
This function transfers the coins in this contract by address and amount
### call(address, function_name, args...)
The call function returns the result of the function of the contract being executed in the state of the corresponding address.
* In addition, can call the value function to send a coin.
```lua
contract.call("Amh4S9pZgoJpxdCoMGg6SXEpAstTaTQNfQdZFsE26NpkqPwmaWod", "inc", '[1]')
contract.call.value(10)("Amh4S9pZgoJpxdCoMGg6SXEpAstTaTQNfQdZFsE26NpkqPwmaWod", "inc", '[2]')
```
### delegatecall(address, function_name, args...)
The delegatecall function returns the result of the function of the calling process, executed in the state in the current address.

### pcall(fn, args...)
It is an error handling function that works just like pcall in lua. The difference is that when the error occurs, the modified state,table or balance of the function executed rollback

```lua
contract.pcall(inc, '[1]')
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
If the smart contract is handling simple types of data, it would not be difficult to implement using only the basic APIs (getItem (), setItem ()). However, complex data structures, data association, scope queries, filtering, sorting, and other features require the complexity and size of the data logic so developers can not focus on critical business logic. To solve this problem, Aergo supports SQL. This section details the types and usage of SQL APIs available in smart contracts
### exec(sql)
This function perform DDL or DML statements
### query(sql)
This function perform SELECT statements and return result set object
### prepare(sql)
This function create prepared statement and return statement object
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
​
function insert(id, passwd, name, birth, mobile)
  db.exec("insert into customer values ('" .. id .. "', '"
      .. passwd .. "', '"
      .. name .. "', '"
      .. birth .. "', '"
      .. mobile .. "')")
end
```
### functions of result set object
Object functions must be called with the: operator
#### next
This function prepare the next result row. Returns false if there is a row, false if it is not.
#### get
This function return result row
```lua
function query(id)
  local rt = {}
  local rs = db.query("select * from customer where id like '%'" .. id .. "'%'")
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
### functions of prepared statement object
equivalent to the prepareStatement object in JDBC. You can use the parameters in SELECT or DML to view, add, modify, or delete information.
#### query(bind1 , bind2, ....)
This function execute SELECT statement by specifying argument value corresponding to bind parameter and return result set object
#### exec(bind1, bind2, ....)
This function execute DML statement by specifying argument value corresponding to bind parameter

### SQL Restrictions
Smart Contract's SQL processing engine is built on SQLite. Therefore, detailed SQL usage grammar can be found at https://sqlite.org/lang.html and https://sqlite.org/lang_corefunc.html. However, because of the stability and security of the Aergo, not all SQL is allowed.

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
* Default: https://www.sqlite.org/lang_corefunc.html
* Date and time: https://www.sqlite.org/lang_datefunc.html
* Set: https://www.sqlite.org/lang_aggfunc.html
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
    db.exec("create table if not exists customer (cid integer PRIMARY KEY ASC AUTOINCREMENT , passwd text , cname text, birthdate date, rgdate date)")
    db.exec("insert into customer (cid, passwd, cname, birthdate, rgdate) values (100 ,'passwd1','홍길동', date('1988-01-03'),date('2018-05-30'))")
    db.exec("insert into customer (passwd, cname, birthdate, rgdate) values ('passwd2','김철수', date('1978-11-03'),date('2018-05-30'))")
    db.exec("insert into customer (passwd, cname, birthdate, rgdate) values ('passwd3','이영미', date('1938-04-23'),date('2018-05-30'))")
​
    db.exec("drop table if exists product")
    db.exec("create table if not exists product (pid integer PRIMARY KEY ASC AUTOINCREMENT, pname text, price real, rgdate date)")
    db.exec("insert into product (pid, pname, price, rgdate) values (1000 ,'사과',1000, date('2018-05-30'))")
    db.exec("insert into product (pname, price, rgdate) values ('수박',10000, date('2018-05-30'))")
    db.exec("insert into product (pname, price, rgdate) values ('포도',3500, date('2018-05-30'))")
​
    db.exec("drop table if exists order_hist")
    db.exec("create table order_hist (oid integer PRIMARY KEY ASC AUTOINCREMENT, cid integer, pid integer, p_cnt integer, total_price real, rgtime datetime, FOREIGN KEY(cid) REFERENCES customer(cid), FOREIGN KEY(pid) REFERENCES product(pid) )")
    db.exec("insert into order_hist(oid, cid, pid,  p_cnt, total_price,rgtime) values(10000,100,1000,3,3000, datetime('2018-05-30 16:00:00'))")
    db.exec("insert into order_hist(cid, pid, p_cnt, total_price, rgtime) values(100,1000,3,3000, datetime('2018-05-30 17:00:00'))")
    db.exec("insert into order_hist(cid, pid, p_cnt, total_price, rgtime) values(100,1000,3,3000, datetime('2018-05-30 18:00:00'))")
end
```
## json package
Json package is provided for user convenience in input and output. This package allows automatic conversion between Json format strings and Lua Table structures.
### encode(arg)
This function returns a JSON-formatted string with the given lua value.
### decode(string)
This function converts a string in JSON format to the corresponding Lua structure and returns it
