Aergoluac
=========

**aergoluac** is a command line tool that compile lua script to binary code for contract.

For a list of all commands known to aergoluac, simply run it with no arguments.

.. code-block:: text

    Usage:
    aergoluac [flags] srcfile bcfile

    Flags:
    -a, --abi string   abi filename
    -h, --help         help for aergoluac
        --payload      print the compilation result consisting of bytecode and abi


Examples
---------

out to file
~~~~~~~~~~~

.. code-block:: shell

    aergoluac test.lua test.bc -a test.abi

out to console
~~~~~~~~~~~~~~

.. code-block:: shell

    aergoluac test.lua --payload