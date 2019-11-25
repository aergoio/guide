Transaction Fees
================

The Aergo protocol includes transaction fees that need to be paid
according to the configuration of the network.

Versions prior to 2.0 (`see 1.3 docs <https://docs.aergo.io/en/1.3/specs/fees.html>`_) 
included a simplified transaction fee scheme, taking into account payload and state database usage size.

This is not fair because it does not consider the actual execution time and resource usage of the transaction.
In version 2.0, a gas system was introduced to correct this problem.

Gas is a numerical representation of execution and storage costs in the transaction and contract.
But gas is not a fee. To calculate the fee, we need the gas price.

Gas Price
---------

Gas price is the AER value corresponding to 1 gas. All transactions in a block have the same gas price.
Gas price is determined by a DAO vote.

Gas Limit
---------

The gas limit allows the user to specify the maximum amount of gas used for transactions and contracts.
For backward compatibility with previous versions that did not specify a gas limit, 
a gas limit of 0 automatically calculates the maximum amount of gas available from an account's balance.

Gas Table
---------

Transaction
'''''''''''

Transactions other than governance transactions consume ``100,000 gas`` by default.
If you have a payload, additional gas will be used depending on the size.

**Payload gas**: (*Bytes of a payload* - 200) * 5

Instructions
''''''''''''

The table below shows gas usage for Lua bytecode.

.. list-table::
    :widths: 30 30
    :header-rows: 1

    * - Name
      - GAS
    * - ISLT, ISGE, ISLE, ISGT, ISEQV, ISNEV, ISEQS, ISNES, ISEQN, ISNEN, ISEQP, ISNEP
      - 2
    * - ISTC, ISFC, IST, ISF, ISTYPE, ISNUM
      - 1
    * - MOV
      - 2
    * - NOT, UNM
      - 1
    * - LEN
      - 3
    * - ADDVN, SUBVN
      - 2
    * - MULVN, DIVVN, MODVN
      - 3
    * - ADDNV, SUBVN
      - 2
    * - MULNV, DIVNV, MODNV
      - 3
    * - ADDVV, SUBVV
      - 2
    * - MULVV, DIVVV, MODVV
      - 3
    * - POW
      - 3
    * - CAT
      - 3
    * - KSTR, KCDATGA, KSHORT, KNUM, KPRI, KNIL
      - 1
    * - UGET, USETV, USETS, USETN, USETP, UCLO, FNEW
      - 2
    * - TNEW
      - 2
    * - TDUP
      - 5
    * - GGET, GSET
      - 3
    * - TGETV, TGETS, TGETB, TGETR, TSETV, TSETS, TSETB, TSETM, TSETR
      - 2
    * - CALLM, CALL, CALLMT, CALLT, ITERC, ITERN
      - 10
    * - VARG
      - 5
    * - ISNEXT
      - 2
    * - RETM
      - 5
    * - RET, RET0, RET1
      - 3
    * - FORI, FORL, ITERL, LOOP, JMP
      - 2

Built-in Functions
''''''''''''''''''

Gas usage for built-in functions provided by the Lua VM

.. list-table::
    :widths: 30 30
    :header-rows: 1

    * - Name
      - GAS
    * - assert
      - 3
    * - error
      - 5
    * - getfenv
      - 5
    * - getmetable
      - 3
    * - ipairs
      - 3
    * - next
      - 3
    * - pairs
      - 3
    * - pcall
      - 10
    * - rawequal
      - 5
    * - rawget
      - 3
    * - rawset
      - 5
    * - select
      - 3 (length), 5 (get an element)
    * - setfenv
      - 5
    * - setmetatable
      - 3
    * - tonumber
      - 5
    * - tostring
      - 5
    * - type
      - 2
    * - unpack
      - 5 + (2 * *number of arguments*)
    * - xpcall
      - 10
    * - string.byte, string.char
      - 3 + (1 * *number of string units (arguments)*)
    * - string.dump
      - 10
    * - string.find, string.gmatch, string.gsub, string.match
      - 3 + (5 * *number of matching strings*)
    * - string.format
      - 3 + (2 * *number of format modifiers*)
    * - string.lower
      - 3 + (1 * *number of string units (argument)*)
    * - string.rep
      - 3 + (2 * *number of concatenation*)
    * - string.reverse
      - 3 + (1 * *number of string units (argument)*)
    * - string.sub
      - 3 + (1 * *number of string units (argument)*)
    * - string.upper
      - 3 + (1 * *number of string units (argument)*)
    * - table.concat
      - 3 + (3 * *number of arguments*)
    * - table.insert
      - 3 + (3 * *number of cells to move*)
    * - table.maxn, table.minn
      - 3 + (3 * *comparison count*)
    * - table.sort
      - 3 + (5 * *comparison count*)
    * - math.abs, math.ceil, math.floor, math.pow
      - 3
    * - math.max, math.min
      - 3 + (1 * *number of arguments*)
    * - bit.tobit
      - 3
    * - bit.tohex
      - 5
    * - bit.bnot
      - 2
    * - bit.bor, bit.band, bit.xor
      - 3 + (2 * *number of arguments*)
    * - bit.lshift, bit.rshift, bit.ashift, bit.rol, bit.ror
      - 3
    * - bit.bswap
      - 2

Aergo-extension Functions
'''''''''''''''''''''''''

Gas usage for aergo-extension functions

.. list-table::
    :widths: 30 30
    :header-rows: 1

    * - Name
      - GAS
    * - system.getSender
      - 1000
    * - system.getBlockheight
      - 300
    * - system.getTxhash
      - 500
    * - system.getTimestamp
      - 300
    * - system.getContractID
      - 1000
    * - system.setItem
      - 100 + (5 * *bytes of data*)
    * - system.getItem
      - 100
    * - system.getAmount
      - 300
    * - system.getCreator
      - 500
    * - system.getOrigin
      - 1000
    * - system.getPrevBlockHash
      - 500
    * - contract.balance, contract.send, contract.pcall
      - 300
    * - contract.event
      - 500
    * - contract.deploy
      - 5000
    * - contract.call
      - 2000
    * - contract.delegatecall
      - 2000
    * - contract.stake, contract.unstack. contract.vote, contract.voteDao
      - 500
    * - json.encode, json.decode
      - 50 + (50 * *number of objects*)
    * - crypto.sha256
      - 500
    * - crypto.ecverify
      - 5000
    * - bignum.number
      - 50
    * - bignum.isneg, bignum.iszero
      - 10
    * - bignum.tonumber, bignum.tostring
      - 50
    * - bignum.neg
      - 100
    * - bignum.sqrt
      - 300
    * - bignum.compare
      - 50
    * - bignum.add, bignum.sub
      - 100
    * - bignum.mul, bignum.div, bignum.mod
      - 300
    * - bignum.pow, bignum.divmod, bignum.powmd
      - 500

Gas estimation using Brick
--------------------------

Although there is a gas usage tables, it is not easy to calculate the exact gas limit.
The simplest way to estimate is to use the `Brick <https://github.com/aergoio/aergo/tree/master/cmd/brick>`_.
The `Brick <https://github.com/aergoio/aergo/tree/master/cmd/brick>`_ allows you to deploy and perform contracts without running an actual blockchain.
Using Brick, you can check contract call  results, gas usage, etc. It also has functions for debugging the contract.
