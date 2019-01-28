============
Peer Connect
============

NOTE: Some document is not translated yet.

Node Discovery
==============
Aergo server need to know where other server nodes are.
There are several ways to do.

Query polaris
-------------
Polaris keep list of aergo server nodes, such like DNS. Aergo server connect and query to official Aergo Polaris automitically, if the chain is Official AergoMainNet or AergoTestNet.

You can build and run custom Polaris for your private chain or custom public chain. You should configure to use custom Polaris by modifying config file.

Designate Peer
--------------
You can add list of peers you know to connect at boot time in configuration file. use options 'npaddpeers'.

Dynamic peer discovery
----------------------
Aergo server ask peers list to other connected peers and Polaris if it connect not enough peers, and try to connect received peers.

Choosing remote peers
---------------------
[below will be added to Aergo server soon]
TODO transalate from Korean text

Peer connect process
====================
TODO transalate from Korean text

Peer Handshake
--------------
TODO transalate from Korean text

Keep Alive
----------
TODO transalate from Korean text

Peer blacklist
--------------
[below will be added to Aergo server soon]
TODO transalate from Korean text
