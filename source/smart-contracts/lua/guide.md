Programming Guide
=================

Lua provides useful functions and libraries. So you can easily create smart contracts using these functions.

Please refer to the [Lua Reference Manual]((http://www.lua.org/manual/5.1/)) for detailed syntax, explanation, basic built-in functions and libraries.

## Restrictions

Because Lua smart contract is performed in the aergo, OS related functions including Input/Output are not provided for stability and security.

Here is a list of the base functions that are not available.

> print, dofile, loadfile, module, and require

You can replace `print` with `system.print`.

And you can use `import` instead of `require`. `import` is not a Lua syntax.

Use [SHIP](https://github.com/aergoio/ship/wiki) to build and deploy smart contracts using multiple files.

Here is a list of the default libraries that are not available.

> coroutine, io, os, debug

The `string`, `math`, `bit`, and `table` packages are available. However, you can not use the `random`, `randomseed` functions in the `math` package.

There are no restrictions on literals, expressions, and statements.

## Libraries

We provide libraries for smart contract as follows:

* Blockchain API
  * system module
  * contract module
  * state module
  * db module
  * abi module
* Utils
  * json module
  * crypto module
  * bignum module

You can find detailed descriptions for libraries on this [page](reference.html)

## Smart Contract

### Layout
```lua
import "./path/to/library"

state.var {
  Value = state.value(),
  Map = state.map(),
  Array = state.array(10)
}

function constructor(init_value)
  Value:set(init_value)
end

function contract1(name, id)
  Map["name"] = name
  Map["ID"] = id
end

function contract2()
  local sum = 0
  for i, v in state.array_pairs(Array) do
    if v ~= nil then
      sum = sum + v
    end
  end
  return sum
end

abi.register(contract1, contract2) -- , contract3, ...
```

#### import
This replaces the `require` function.

It allows you to divide and develop one smart contract into multiple modules(files).

This is not a Lua feature. You should use [SHIP](https://github.com/aergoio/ship/wiki) to build and deploy smart contracts using multiple files.

#### state variable
The `state.var` function defines global state variables.

Three types of state variables can be defined.

##### value
This type store any Lua values.

You can define a state value with the syntax `var_name = state.value()`.
It has `get` and `set` methods for reading and writing data.

```lua
Value:set("data")
local data = Value:get()
```

##### map
The type map implements associative arrays.

It can be indexed only with `string`, but the value of a map element can be of any type.

You can define a state map with the syntax `var_name = state.map()`.
The index operator is used for reading and inserting elements.

```lua
state.var {
  Map_var = state.map()
}

function contract_func()
  Map_var["name"] = "kslee"
  Map_var["age"] = 38
  -- ...
  local age = Map_var["age"]
end
```

##### array
The type array is a fixed-length ordinary array.

It can be indexed only with `integer`, but the value of an array element can be of any type. The index starts at 1.

You can define a state array with the syntax `var_name = state.array(size)`.
The index operator is used for reading and inserting elements.
```lua
state.var {
  Arr_var = state.array(3)
}

function contract()
  Arr_var[1] = 1
  Arr_var[2] = 2
  Arr_var[3] = 3

  local sum1 = 0
  for i, v in state.array_pairs(Array) do
    if v ~= nil then
      sum1 = sum1 + v
    end
  end

  local sum2 = 0
  for i = 1, #Arr_var do
    if Arr_var[i] ~= nil then
      sum2 = sum2 + Arr_var[i]
    end
  end

  if sum1 == sum2 then
  -- ...
  end

end
```

Note: The state variables are just syntax sugar that replace `system.getItem(), system.setItem()` functions.

The fields of state variables that are directly modified cannot update the state db.

See `InvalidUpdateAge()` and `ValidUpdateAge()` functions in the example.

```lua
state.var{
   Person = state.value()
}

function constructor()
  Person:set({ name = "kslee", age = 38, address = "blahblah..." })
end

function InvalidUpdateAge(age)
  Person:get().age = age
end

function ValidUpdateAge(age)
  local p = Person:get()
  p.age = age
  Person:set(p)
end

function GetPerson()
  return Person:get()
end

abi.register(InvalidUpdateAge, ValidUpdateAge, GetPerson)

```

#### constructor
The `constructor` is executed only once during deployment. It can has arguments. It does not need to register into the `abi.register()` function because it is handled automatically.

#### functions
Write business logic and help functions.

#### export contract function(s)
You should add global functions that must be called from contract call/query commands to the `abi.register()`.

#### special functions

##### default
`default` is a special function. It is called when the function name can not be found or when the transaction has no a call information. It does not need to export through `abi.register()`. `default` is the name of this function. `default` is used internally by the VM. You should not use `default` for any other purpose.

You can define a default function as follows:

```lua
...
function default()
  ...
end
...
```

You can call this default function. There is no call information for the contract function.

```shell
./aergocli contract call <sender> <contract>
```

##### payable
The `payable` is a property of a function. Only payable function can receives Aergo(s) sent from a sender.
We can make a payable function using `abi.payable()`. `payable` functions are automatically exported. Therefore, you do not have to register using the `abi.register` function. `constructor` and `default` are not payable functions by default. They can be payable functions using `abi.payable()`.

You can call the ReceiveAergo with aergo, But you can not call the NotReceiveAergo:

```lua
...
function ReceiveAergo()
  ...
end

function NotReceiveAergo()
  ...
end

abi.register(NotReceiveAergo)
abi.payable(RecieveAergo)
```

```shell
./aergocli contract call --amount=10 <sender> <contract> ReceiveAergo     # success
./aergocli contract call --amount=10 <sender> <contract> NotReceiveAergo  # fail
```

## SQL

Aergo smart contract has `db` library that supports SQL features.

> Note: The db package is only available on private networks ([SQL TestNet](https://sqltestnet.aergoscan.io/)).

The below code is a example of creating table and insert a row using `db.exec()`

```lua
-- creates a customer table
function createTable()
  db.exec([[create table if not exists customer(
        id text,
        passwd text,
        name text,
        birth text,
        mobile text
    )]])
end

-- insert a row to the customer table
function insert(id, passwd, name, birth, mobile)
  db.exec("insert into customer values ('" .. id .. "', '"
      .. passwd .. "', '"
      .. name .. "', '"
      .. birth .. "', '"
      .. mobile .. "')")
end
```
The `db.query()` function returns a result set. You can fetch rows from the result set. 

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

You can also use prepared statements. The following examples is rewrite `insert` and `query` contract functions  using prepared statements.

```lua
function insert(id , passwd, name, birth, mobile)
  stmt = db.prepare("insert into customer values (?, ?, ?, ?, ?)")
  stmt.exec(id, passwd, name, birth, mobile)
end

function query(id)
  local rt = {}
  local stmt = db.query("select * from customer where id like '%' || ? || '%'")
  local rs = stmt:query(id)
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

### Restrictions

[Litetree](https://github.com/aergoio/litetree) is used as the SQL processing engine for the aergo smart contract. Litetree is implemented based on SQLite.

Detailed SQL usage can be found at https://sqlite.org/lang.html and https://sqlite.org/lang_corefunc.html

However, we do not provide full SQL functionality. There are some limitations due to stability and security.

**Data types**

Allow only SQL datatypes corresponding to Lua strings and numbers(int, float).

* text
* integer
* real
* null
* date, datetime

**SQL statements**

You can execute the following SQL statements. However, DDL and DML can not be run on smart contract queries.

* DDL
  * TABLE: ALTER, CREATE, DROP
  * VIEW: CREATE, DROP
  * INDEX: CREATE, DROP
* DML
  * INSERT, UPDATE, DELETE, REPLACE
* Query
  * SELECT

**functions**

Here is a list of functions that are not available:

* `load_XXX` functions
* `random` function
* `sqlite_XXX` functions
* data and time related functions can be used, except `now` _timestring_ and `localtime` _modifier_.

A list of other functions and descriptions is available via the links below.

* basic : https://www.sqlite.org/lang_corefunc.html
* data and time : https://www.sqlite.org/lang_datefunc.html
* aggregation : https://www.sqlite.org/lang_aggfunc.html

**contraints**

You can use the following contraints.

* NOT NULL
* DEFAULT
* UNIQUE
* PRIMARY KEY (FOREIGN KEY)
* CHECK

## Tools

### aergoluac
`aergoluac` is a compiler for Lua smart contracts.

* [Reference](../../tools/aergoluac.html)

### aergocli
`aergocli` is a command line tool that interfaces with the GRPC exposed by `aergosvr`.

It provides smart contract-related commands as follows:
* contract deploy/call/query/abi
* receipt get

* [Reference](../../tools/aergocli.html)

### brick
Toy for Contract Developers. You can use it to test smart contracts.

https://github.com/aergoio/aergo/tree/master/cmd/brick

## Style conventions

It is good adopt a consistent coding style for readability.
We recommend the [Lua style guide](https://github.com/luarocks/lua-style-guide).
