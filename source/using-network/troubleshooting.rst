Troubleshooting
===============

Unavailable personal feature 
----------------------------

.. code-block:: text

   Failed: rpc error: code = Unavailable desc = Unavailable personal feature 

This means that on the node you connected to, personal account features are
`disabled <../running-node/configuration.html>`_ using the setting :code:`personal = False`.
This setting is usually used on public nodes, so you cannot use personal account features when `connecting to public nodes <./connecting.html>`_.
Always create and use accounts using local nodes or offline, e.g. by generating private keys using one of Aergo's SDKs.
*See also:* `Creating Accounts <./accounts.html>`_