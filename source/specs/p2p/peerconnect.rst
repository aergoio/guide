============
Peer Connect
============

NOTE: Some document is not translated yet.

Node Discovery
==============

When the Aergo server starts running, you need a way to connect to the network.
To do this, you need to know and connect to other Aergo server nodes already connected to the chain.
Aergo does this using several methods.

Query polaris
-------------
Polaris keep list of aergo server nodes, such like DNS. Aergo server connect and query to official Aergo Polaris automitically, if the chain is Official AergoMainNet or AergoTestNet.

You can build and run custom Polaris for your private chain or custom public chain. You should configure to use custom Polaris by modifying config file.

Designate Peer
--------------
You can add a list of designated known peers to connect to at boot time in configuration file using the option 'npaddpeers'.

Dynamic peer discovery
----------------------
Aergo server requests peer list from other connected peers as well as Polaris if it cannot find enough peers.

Peer connect process
====================
The network communication of Aergo server operates on libp2p basis, and libp2p is responsible for encryption and node distinction at transmission level.
After a tcp session is created, both peers start the handshake operation.

Peer Handshake
--------------
In the Handshake phase, peers exchanges each other's version, chain ID and state to determine whether to connect.
If the other node version is not supported by the current server or the operating chain ID is different, the connection is stopped.
Once the handshake is successful, the difference in block height is compared and synchronization started.

Keep Alive
----------
Aergo server maintains a connection-based communication.
Both peers disconnect when the internally defined retention score increases beyond a certain level.
This score decreases to a low level when a query is requested, and increases when a bad block or TX notification is sent.


Ongoing Development Features
============================

Peer blacklist
--------------

When an internally defined ban score exceeds a certain level, the Aergo server blocks the peer's connection.
This score increases due to the connection being disconnected due to exceeding the connection maintenance score, etc., and the blocking period is also changed by calculating the score or the number of blocking times.
You can also permanently block by specifying a block address in the configuration file.

Choosing remote peers
---------------------

The list of peers to connect to is managed in Kademlia-like manner.
The number of peers that are closer to the peer is larger and the number of peers that are connected to the peer is smaller.
The peer's distance is based on the difference based on the hash value, not the physical distance.