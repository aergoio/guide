Name System
===========

Aergo contains a name service to translate addresses into easy to remember short names.

One name maps to exactly one address. Names are currently fixed to a length of 12 characters.

Names include an :code:`owner` and a :code:`destination`.
The owner is used to determine who can change the name, the destination is the actual adress the name should resolve to.
In case of normal accounts, these two values will be identical, but they differ in case of smart contracts.

Creating and updating names
---------------------------

The easiest way to create and update names is the `aergocli <../tools/aergocli.html>`_. 

Registering and updating names currently requires spending 1 aergo.

To register a new name:

.. code-block:: text

    aergocli name new --from my_unlocked_account_address --name my12charname

To change the destination of the name:

.. code-block:: text

    aergocli name update --from my_unlocked_account_address --to account_address_or_contract_address --name my12charname

.. warning::
   The update command affects both the destination and the owner of a name.
   If the new destination is a regular account, both owner and destination are set to that address.
   If the new destination is a smart contract address, the owner is set to the address of that contract's creator.

Lookups
-------

To retrieve the owner and destination of a registered name:

.. code-block:: text

   aergocli name owner --name my12charname

The :code:`destination` field contains the resolved address.

This method is also available over `RPC <../api/index.html>`__ and in the various `SDKs <../sdks/index.html>`__.

Reverse lookups
---------------

Aergo server currently supports no way to do reverse lookups, i.e. finding all names associated with an address.
On public networks, you can use block explorers like Aergoscan to find that information.

Technical details
-----------------

Names are stored in the state of the special account :code:`aergo.name`. They are created and updated using special governance transactions.
Refer to `transaction types <transaction-types.html>`_ for the technical specification of these actions.
Governance transactions currently don't require any fee, but the name system requires aergo determined by DAO voting to be included in the transaction. It

Transactions that create or update names are effective after they are included in a block.
That means that you can only refer to new or updated names in the following block.

