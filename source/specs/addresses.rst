Addresses
=========

Client-side
-----------

Account and contract addresses are Base58-check encoded strings that look like this:

.. code-block:: text

    AmQA7dHJFiA4mXXxTV5sAniLpxMrankdW4Cow3ykj1UM5G14QKL5

- `Technical explanation of Base58-check encoding <https://en.bitcoin.it/wiki/Base58Check_encoding>`_

The prefix used for encoding Aergo addresses in base58-check is :code:`0x42`.

Internal
--------

Internally (in the server and over the wire, i.e. in GRPC requests) addresses are byte arrays with a length of 33.
They represent the compressed public key of an account.

- `Technical explanation of public keys <http://learnmeabitcoin.com/glossary/public-key>`_

In the case of smart contracts, the address is generated from a hash of the creator's account and the creating transaction's nonce, prefixed with the byte 0x0C to arrive at a compatible length of 33 bytes.
As a developer, you don't have to worry about this: smart contract addresses can be used just the same as account addresses.