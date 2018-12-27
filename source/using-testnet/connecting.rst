Connecting to Well-known Nodes
==============================

Without any specific settings, the server connect to Polaris for the testnet, registers itself, obtains addresses of other nodes, and automatically attempts to connect those nodes.
If the server is in the NAT environment, or has multiple NICs, additional settings are required for the external node to access the server.
Set up outside address for other nodes in the external network connection, and set internal address for binding address.

.. code-block:: toml

    netprotocoladdr = "211.12.34.56" # external address to which other peer can connect
    netprotocolport = 7846
    npbindaddr = "192.168.0.2" # no config element or empty string means using same address as external 
    npbindport = 17846 # negative number means it is same as external port, in this case 7846

