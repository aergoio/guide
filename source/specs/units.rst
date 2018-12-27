Token Units
===========

1 aergo = 1 * 10^18 aer = 1 * 10^9 gaer

The Aergo CLI and client libraries have support for these units,
i.e. you can specify transaction amounts as :code:`1 aer` instead of :code:`1000000000000000000`.

Note that amounts in the base unit aer exceed the range of 64-bit integers.
You need some implemention of Big Integer to deal with these numbers.
Aergo SDKs come bundled with a recommended way to do that.
In most cases, you can just use strings instead of numbers. For example, when creating a JSON transaction, set

.. code-block:: json

    {
        "amount": "1000000000000000000"
    }
