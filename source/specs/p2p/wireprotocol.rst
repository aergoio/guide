P2P wire protocol
=================

This page describe the p2p wire protocol for protocol version 0.3.3

Note: Some contents are work in progress and will be implemented before the launch of the mainnet.

Messages
--------

Handshake message
^^^^^^^^^^^^^^^^^

A handshake message is used once at starting handshake. The outbound peer sends HSReqHeader and the inbound peer send HSResp to select perper handshake protocol version.
As of Aergo v1.2.0, the protocol version is 0.3.2 and can accept 0.3.1 and 0.3.0

HSReq is variable length byte stream which contains accepted protocol versions

* 4 : Magic
* 4 : count of accepted protocol versions
* 4*n : list of accepted versions. The former is perfered version (actually the newest version is the first)  

HSResp is 8 bytes length byte stream 

* 4 : The same Magic as HSReq request or zero if handshaking is not possible
* 4 : The selected protocol version or error code if first 4 byte value is zero 

Handshake response error codes
""""""""""""""""""""""""""""""

+------------------------+------+------------------------------------------------------------------------------------------------------+
|Name                    |Code  |Remark                                                                                                |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|HSCodeWrongHSReq        |  0001|Request header is wrong                                                                               |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|HSCodeNoMatchedVersion  |  0002|The inbound server have no prefer protocol version                                                    |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|HSCodeAuthFail          |  0003|Wrong peer identiry                                                                                   |
+------------------------+------+------------------------------------------------------------------------------------------------------+

P2P version notation 
""""""""""""""""""""

* 2 : Major verion
* 1 : Minor version
* 1 : Patch version

Normal message
^^^^^^^^^^^^^^

Header (48bytes) + Payload (variable size)

Message header
^^^^^^^^^^^^^^

* 4 : code number of subprotocol . Big endian number
* 4 : payload size. Big endian number.
* 8 : creation time of this message. unix timestamp with precision of nanosecond . Big endian number
* 16 : message id. binary form of uuid.
* 16 : original request id. only meaningful if message is response message. binary form of uuid.


Message payload
^^^^^^^^^^^^^^^

Payload is serialized form of protobuf struct. The size and struct type is differ by subprotocol.


List of Subprotocols
--------------------

Code is hexadecimal number.
Refer to `Subprotocols <subprotocols.html>`_ for detailed information of each subprotocol.

+------------------------+------+------------------------------------------------------------------------------------------------------+
|Name                    |Code  |Summary                                                                                               |
+========================+======+======================================================================================================+
|StatusRequest           |  0001|Used in handshake                                                                                     |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|PingRequest             |  0002|Ping including last block hash and number                                                             |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|PingResponse            |  0003|Response ping                                                                                         |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|GoAway                  |  0004|Disconnect notice with reason                                                                         |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|AddressesRequest        |  0005|Query request to get list of connected peers                                                          |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|AddressesResponse       |  0006|Response for AddressesRequest.                                                                        |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|GetBlocksRequest        |  0010|request for getting datas of blocks                                                                   |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|GetBlocksResponse       |  0011|response of GetBlocksRequest. Multiple responses for a single request if size of blocks is too big    |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|GetBlockHeadersRequest  |  0012|request list of headers of consecutive blocks.                                                        |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|GetBlockHeadersResponse |  0013|response of GetBlockHeadersRequest                                                                    |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|NewBlockNotice          |  0016|notice of block which the sender was not produced; i.e. it is relay of block notice.                  |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|GetAncestorRequest      |  0017|request for finding common ancestor. used for peer sync                                               |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|GetAncestorResponse     |  0018|response of GetAncestorRequest                                                                        |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|GetHashesRequest        |  0019|request for get hashes of consecutive blocks.                                                         |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|GetHashesResponse       |  001A|response of GetHashesRequest                                                                          |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|GetHashByNoRequest      |  001B|request for get hash of single block by number                                                        |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|GetHashByNoResponse     |  001C|response of GetHashByNoRequest                                                                        |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|GetTXsRequest           |  0020|request for getting datas of txs                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|GetTxsResponse          |  0021|response of GetTXsRequest. Multiple responses for a single request if size of blocks is too big       |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|NewTxNotice             |  0022|notice of valid tx                                                                                    |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|BlockProducedNotice     |  0030|block notice from block producer                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|BlockProducedNotice     |  0030|A block is produced. This notice is created only in BP and sent to other trusted BP or FULL nodes.    |
+------------------------+------+------------------------------------------------------------------------------------------------------+

List of Response Status
-----------------------

Some subprotocols for responsing other message have ResultStatus property.

+------------------------+------+------------------------------------------------------------------------------------------------------+
|Name                    | Code | Remark                                                                                               |
+========================+======+======================================================================================================+
|OK                      |    0 | OK is returned on success.                                                                           |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|CANCELED                |    1 | when operation was canceled                                                                          |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|UNKNOWN                 |    2 |                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|INVALID_ARGUMENT        |    3 | INVALID_ARGUMENT is missing or wrong value of argument                                               |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|DEADLINE_EXCEEDED       |    4 | timeout                                                                                              |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|NOT_FOUND               |    5 | Resource is not found                                                                                |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|ALREADY_EXISTS          |    6 |                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|PERMISSION_DENIED       |    7 |                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|RESOURCE_EXHAUSTED      |    8 |                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|FAILED_PRECONDITION     |    9 |                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|ABORTED                 |   10 |                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|OUT_OF_RANGE            |   11 |                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|UNIMPLEMENTED           |   12 | indicates operation is not implemented or not supported/enabled in this service.                     |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|INTERNAL                |   13 |                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|UNAVAILABLE             |   14 | Unavailable indicates the service is currently unavailable.                                          |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|DATA_LOSS               |   15 |                                                                                                      |
+------------------------+------+------------------------------------------------------------------------------------------------------+
|UNAUTHENTICATED         |   16 | indicates the request does not have valid authentication credentials for the operation.              |
+------------------------+------+------------------------------------------------------------------------------------------------------+

Payload of Subprotocols
-----------------------

StatusRequest
^^^^^^^^^^^^^

* sender: information of sender (address, port, peerID or etc)
* bestBlockHash: current best block of sender
* bestHeight: current best block height of sender
* chainID: ChainID which sender is storing
* genesis: hash of genesis block, added since protocol version v0.3.2

PingRequest
^^^^^^^^^^^

* bestBlockHash: current best block of sender
* bestHeight: current best block height of sender

GoAway
^^^^^^

* reason: description text
  
AddressesRequest
^^^^^^^^^^^^^^^^

* sender: address information of requester
* maxSize: limit of response size
  
AddressesResponse
^^^^^^^^^^^^^^^^^

* status: response status code
* peers: list of peers

GetBlocksRequest
^^^^^^^^^^^^^^^^

* hashes: array of block hashes 
  
GetBlocksResponse
^^^^^^^^^^^^^^^^^

* status: response status code
* blocks: list of block data
* hasNext: boolean flag indicating there are more response(s) for the request

GetBlockHeadersRequest
^^^^^^^^^^^^^^^^^^^^^^

* hash: starting hash to get. 
* height: starting height to get. height is ignored if hash is not empty.
* size: maximum header count to get.
  
GetBlockHeadersResponse
^^^^^^^^^^^^^^^^^^^^^^^

* status: response status code
* hashes: array of block hashes which the response contains.  
* headers: list of block headers. the order of hashes and headers is matching
* hasNext: boolean flag indicating there are more response(s) for the request

NewBlockNotice
^^^^^^^^^^^^^^
* blockHash: hash of new block
* blockNo: block number

GetAncestorRequest
^^^^^^^^^^^^^^^^^^

* hashes: list of block hashes

GetAncestorResponse
^^^^^^^^^^^^^^^^^^^

* status: response status code
* ancestorHash: block hash of common ancestor 
* ancestorNo: block number of common ancestor

GetHashesRequest
^^^^^^^^^^^^^^^^

* prevHash: block hash of starting point. the hash and number must match to actual block
* prevNumber: block number of starting point
* size: maximum hash count to get.

GetHashesResponse
^^^^^^^^^^^^^^^^^

* status: response status code
* hashes: array of block hashes which the response contains.  

GetHashByNoRequest
^^^^^^^^^^^^^^^^^^

* blockNo: block number 

GetHashByNoResponse
^^^^^^^^^^^^^^^^^^^

* status: response status code
* blockHash: hash of requested block

GetTXsRequest
^^^^^^^^^^^^^

* hashes: array of tx hashes 
  
GetTXsResponse
^^^^^^^^^^^^^^

* status: response status code
* hashes: array of tx hashes which the response contains. 
* txs: list of tx data. the order of hashes and txs is matching
* hasNext: boolean flag indicating there are more response(s) for the request

 
Legacy version infomation
=========================

v0.3.0
------

Handshake message
^^^^^^^^^^^^^^^^^

-A handshake message is used once at starting handshake. It contains two 4-byte number. Both outbound peer send HSReq

+HSReq is 8 byte stream which p2p protocol version

+4 : Magic
+4 : p2p protocol version of outbound peer. The inbound peer accept handshake if version is matching or close connection if not.
