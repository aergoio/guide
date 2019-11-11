Java Database Connectivity (JDBC)
=================================

The Aergo smart contract engine has rich SQL support and is compatible with JDBC adapters.
With this you can connect to a smart contract just like any other traditional SQL database.

The JDBC adapter removes the need for creating and sending blockchain transactions manually.
Instead, you can run SQL queries directly.

How to
------

1. Download the required JDBC adapter and Lua smart contract from `github.com/aergoio/aergojdbc <https://github.com/aergoio/aergojdbc>`_
2. `Compile and deploy <../lua/hello-world.html#compile-contract>`_ the smart contract on a Aergo network that supports SQL operations (private networks or the public `Alpha <https://alpha.aergoscan.io/>`_ network at alpha-api.aergo.io).
   The deployed smart contract acts as your database instance on the blockchain.
3. Use a JDBC compatible software, such as the database connection in Eclipse or SQuirrelSQL, and connect using the provided adapter, an encrypted private key, and password.

Connection
----------

- Driver: aergo-jdbc-version.jar, org.aergojdbc.JDBC
- URL: :code:`jdbc:aergo:<aergo node ip>:<port>@<jdbc contract address>`
- User: encrypted private key
- Password: passphrase used to encrypt private key

Limitations
-----------

- Maximum response size: 4mb. If the resulting amount of data exceeds this size, adjust the fetch size using resultSet.setFetchSize (default: 10 rows).
- Sqlite: Since the SQL engine in Aergo is Sqlite, only operations supported by Sqlite are possible.
- Unsupported features:

  - Transaction commit, rollback: transactions always auto commit
  - Scrollable cursor
  - CallableStatement