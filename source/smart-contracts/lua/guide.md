Programming Guide
=================

Lua provides useful functions and libraries. So you can easily create smart contracts using these functions.

Please refer to the [Lua Reference Manual](http://www.lua.org/manual/5.1/) for detailed syntax, explanation, basic built-in functions and libraries.

Aergo currently uses Lua version 5.1.

## Restrictions

Because Lua smart contract is performed by the aergo system, OS related functions including input/output are not provided for stability and security.

These are the base functions that are not available:

> print, dofile, loadfile, module, require

You can replace `print` with `system.print`.

And you can use `import` instead of `require`. `import` is not a Lua syntax.

Use [SHIP](https://github.com/aergoio/ship/wiki) to build and deploy smart contracts using multiple files.

These are the default libraries that are not available:

> coroutine, io, os, debug, jit

The `string`, `math`, `bit` and `table` packages are available. All functions from these packages are available,
except for the `math` package. These are the functions available in the `math` package:

> abs, ceil, floor, pow, max, min

There are no restrictions on literals, expressions and statements.

After the version 2.0, the Lua virtual machine can internally check the block execution timeout (about 0.25s ~ 0.75s per block) and return an error upon block execution timeout. The transaction which causes timeout is not included into the block like the pre-2.0 version. Additionally, if it is the only transaction in the block, it will be evicted from the mempool for the stable operation of the block producer.

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

### import

This replaces the `require` function.

It allows you to divide and develop one smart contract into multiple modules (files).

This is not a Lua feature. You should use [SHIP](https://github.com/aergoio/ship/wiki) to build and deploy smart contracts using multiple files.

### state variable

The `state.var` function defines global state variables.

Three types of state variables can be defined:

#### value

This type stores any Lua value.

You can define a state value with the syntax `var_name = state.value()`.
It has `get` and `set` methods for reading and writing data.

```lua
var_name:set("data")
local data = var_name:get()
```

#### map

The type map implements associative arrays.

It can be indexed only with `string`, but the value of a map element can be of any type.

You can define a state map with the syntax `var_name = state.map()`.
The index operator is used for reading and inserting elements.

You can delete an element of a map by using the `delete` method, with the syntax `var_name:delete(key)`

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

function delete_elem(key)
  Map_var:delete(key)
end
```

The state map supports multiple dimensions, with syntax `var_name = state.map(dimension)`

The multi index operator is used for reading and inserting elements.

```lua
state.var {
  Map_var = state.map(2)
}

function contract_func()
  Map_var["kslee"]["age"] = 38
  Map_var["kslee"]["birth"] = "1999/09/09"
  -- ...
  local kslee = Map_var["kslee"]
  local age = kslee["age"]
  local birth = kslee["birth"]
  
  kslee["birth"] = "1970/10/9"
end
```

##### Restrictions

* max dimensions: 5
* it does not support setting intermediate dimension element, like this:

```lua
state.var {
  Map_var = state.map(2)
  person_var = state.map()
}

function contract_func()
  person_var["age"] = 38
  person_var["birth"] = "1999/09/09"
  Map_var["kslee"] = person_var   -- NOT SUPPORTED!
end
```

#### array

The type array is a fixed-length ordinary array.

It can be indexed only with `integer`, but the value of an array element can be of any type. The index starts at 1.

You can define a state array with the syntax `var_name = state.array(size)`

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
  for i, v in Arr_var:ipairs() do
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

The state array supports multiple dimensions, with syntax `var_name = state.array(1st_ncount, 2nd_ncount,....)`

The multi index operator is used for reading and inserting elements.

```lua
state.var {
  -- declare 2 dimensional array 
  Arr_var = state.array(2, 3)
}

function contract_func()
  Arr_var[1][1] = 1
  Arr_var[1][2] = 2
  Arr_var[1][3] = 3

  -- ...
  local Arr2 = Arr_var[2]
  Arr2[1] = 4
  Arr2[2] = 5
  Arr2[3] = 6

  local sum1 = 0
  for i, v in Arr_var:ipairs() do
   for j, k in v:ipairs() do
    if k ~= nil then
      sum1 = sum1 + k
    end
   end
  end

  local sum2 = 0
  for i = 1, #Arr_var do
   for j = 1, #Arr_var[i] do
    if Arr_var[i][j] ~= nil then
      sum2 = sum2 + Arr_var[i][j]
    end
   end
  end

  if sum1 == sum2 then
  -- ...
  end
end
```

##### Restrictions

* max dimensions: 5
* it does not support setting intermediate dimension element, like this:

```lua
state.var {
  Marr_var = state.map(2,3)
  Arr_var = state.map(3)
}

function contract_func()
  Arr_var[1] = 1
  Arr_var[2] = 2
  Arr_var[3] = 3

  Marr_var[1] = Arr_var    -- NOT SUPPORTED!
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

### Private functions

These functions cannot be accessed from outside the smart contract.
You can use them as helper functions.

### Exported functions

These are the functions that can be called from contract call and query commands.
They should be registered using `abi.register()`.

You can restrict who can call these functions by checking the caller using `system.getOrigin()` or `system.getSender()`.
Example:

```lua
function my_restricted_function()
  assert(system.getOrigin() == system.getCreator(), "permission denied")
  ...
end

abi.register(my_restricted_function)
```

### Special functions

#### constructor

The `constructor` function is executed only once during the contract deployment.
It does not need to be exported with `abi.register()`.

It can have arguments, that should be passed at the contract deployment.

It can also write to the contract state, generally initializing it. Example:

```lua
state.var {
  my_number = state.value()
}

function constructor(value)
  my_number:set(value)
end

...
```

#### default

The `default` function is called when the function name cannot be found or when the transaction
has no call information. It does not need to be exported through `abi.register()`. `default` is used internally
by the VM. You should not use `default` for any other purpose.

You can define a default function as follows:

```lua
...
function default()
  ...
end
...
```

You can call this default function. This happens when a transaction of type `call` is sent to the contract
without information about which function to call.

```shell
./aergocli contract call <sender> <contract>
```

#### payable

The `payable` is a property of a function. Only payable functions can receive Aergos sent from a sender.
We can make a payable function using `abi.payable()`. `payable` functions are automatically exported. Therefore, you do not need to register them using `abi.register()`. `constructor` and `default` are not payable functions by default. They can be made payable functions by using `abi.payable()`.

On the example below we can call the ReceiveAergo function with aergo, but we cannot call the NotReceiveAergo function with aergo:

```lua
...
function ReceiveAergo()
  ...
end

function NotReceiveAergo()
  ...
end

abi.register(NotReceiveAergo)
abi.payable(ReceiveAergo)
```

```shell
./aergocli contract call --amount=10 <sender> <contract> ReceiveAergo     # success
./aergocli contract call --amount=10 <sender> <contract> NotReceiveAergo  # fail
```

#### view

The `view` is a property of a function. Functions can be declared view in which case they promise not to modify the state (send aergo, emit event, set state, etc...).
We can make a view function using `abi.register_view()`. `register_view` functions are automatically exported. Therefore, you do not need to register them using `abi.register()`. 

On the example below we cannot call the sendAergo function:

```lua
...
function sendAergo()
  contract.send(addr, "1 aergo")
end

abi.register_view(sendAergo)
```

```shell
./aergocli contract call <sender> <contract> sendAergo  # fail
```


## SQL

Aergo smart contract has `db` library that supports SQL features.

> Note: The db package is currently only available on private networks and publicly on [SQL TestNet](https://sqltestnet.aergoscan.io/).

The code below is an example of creating a table and inserting a row using `db.exec()`

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
  db.exec("insert into customer values (?, ?, ?, ?, ?)",
          id, passwd, name, birth, mobile)
end
```

The `db.query()` function returns a result set. You can fetch rows from the result set. 

```lua
function query(name)
  local rt = {}
  local rs = db.query("select * from customer where name like '%' || ? || '%'", name)
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

You can also use **prepared statements**:

```lua
function insert(id , passwd, name, birth, mobile)
  stmt = db.prepare("insert into customer values (?, ?, ?, ?, ?)")
  stmt:exec(id, passwd, name, birth, mobile)
end

function insert_contacts(contacts)
  local stmt = db.prepare("insert into contacts (name, email) values (?, ?)")
  for _,contact in ipairs(contacts) do
    stmt:exec(contact.name, contact.email)
  end
end

function query_names(ids)
  local names = {}
  local stmt = db.prepare("select name from customer where id=?")
  for _,id in ipairs(ids) do
    local rs = stmt:query(id)
    if rs:next() then
      table.insert(names, rs:get())
    else
      table.insert(names, "")
    end
  end
  return names
end
```

### Security

DO NOT concatenate values when building SQL commands!

This would make your smart contract vulnerable to `SQL injection` attacks.

These are bad examples that should *NOT* be used:

```lua
  -- WRONG! AVOID THIS:
  db.exec("insert into customer values ('" .. id .. "', '"
      .. passwd .. "', '"
      .. name .. "', '"
      .. birth .. "', '"
      .. mobile .. "')")

  -- WRONG! AVOID THIS:
  local rs = db.query("select * from customer where id like '%'" .. id .. "'%'")
```

### Restrictions

[LiteTree](https://github.com/aergoio/litetree) is used as the SQL processing engine for Aergo smart contracts.
LiteTree is implemented based on SQLite.

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

You can use the following contraints:

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
* contract deploy/call/query/abi/statequery
* receipt get
* event list/stream

* [Reference](../../tools/aergocli.html)

### brick

Toy for Contract Developers. You can use it to test smart contracts.

* [Reference](https://github.com/aergoio/aergo/tree/master/cmd/brick)


## Style conventions

It is good to adopt a consistent coding style for readability.
We recommend the [Lua style guide](https://github.com/luarocks/lua-style-guide).
