Brick
=====

Brick is a tool to test smart contracts in Lua.

It can run operations directly in the integrated Aergo Virtual Machine.
This is way faster than using a real blockchain network, as there is no need to wait for blocks to be produced.

It comes with an interactive shell that is very useful for learning.

It also provides a batch function to process multiple operations from a script file.

It is the main tool to test smart contracts, with support for:

* Unit tests
* Functional tests
* Packing smart contracts with `import` statements

See the `Brick reference <https://github.com/aergoio/aergo/tree/master/cmd/brick>`_
for details.

For integration tests, use `aergocli <aergocli.html>`_
