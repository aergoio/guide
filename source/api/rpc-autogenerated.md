# Protocol Documentation
<a name="top"></a>

This reference is auto-generated from [aergoio/aergo-protobuf](https://github.com/aergoio/aergo-protobuf).


- [rpc.proto](#rpc.proto)
    - [AergoRPCService](#types.AergoRPCService)
        - [NodeState](#NodeState)
        - [Metric](#Metric)
        - [Blockchain](#Blockchain)
        - [GetChainInfo](#GetChainInfo)
        - [ChainStat](#ChainStat)
        - [ListBlockHeaders](#ListBlockHeaders)
        - [ListBlockMetadata](#ListBlockMetadata)
        - [ListBlockStream](#ListBlockStream)
        - [ListBlockMetadataStream](#ListBlockMetadataStream)
        - [GetBlock](#GetBlock)
        - [GetBlockMetadata](#GetBlockMetadata)
        - [GetBlockBody](#GetBlockBody)
        - [GetTX](#GetTX)
        - [GetBlockTX](#GetBlockTX)
        - [GetReceipt](#GetReceipt)
        - [GetABI](#GetABI)
        - [SendTX](#SendTX)
        - [SignTX](#SignTX)
        - [VerifyTX](#VerifyTX)
        - [CommitTX](#CommitTX)
        - [GetState](#GetState)
        - [GetStateAndProof](#GetStateAndProof)
        - [CreateAccount](#CreateAccount)
        - [GetAccounts](#GetAccounts)
        - [LockAccount](#LockAccount)
        - [UnlockAccount](#UnlockAccount)
        - [ImportAccount](#ImportAccount)
        - [ExportAccount](#ExportAccount)
        - [QueryContract](#QueryContract)
        - [QueryContractState](#QueryContractState)
        - [GetPeers](#GetPeers)
        - [GetVotes](#GetVotes)
        - [GetAccountVotes](#GetAccountVotes)
        - [GetStaking](#GetStaking)
        - [GetNameInfo](#GetNameInfo)
        - [ListEventStream](#ListEventStream)
        - [ListEvents](#ListEvents)
        - [GetServerInfo](#GetServerInfo)
        - [GetConsensusInfo](#GetConsensusInfo)
        - [ChangeMembership](#ChangeMembership)
  
  
    - [AccountAddress](#types.AccountAddress)
    - [AccountAndRoot](#types.AccountAndRoot)
    - [AccountVoteInfo](#types.AccountVoteInfo)
    - [BlockBodyPaged](#types.BlockBodyPaged)
    - [BlockBodyParams](#types.BlockBodyParams)
    - [BlockHeaderList](#types.BlockHeaderList)
    - [BlockMetadata](#types.BlockMetadata)
    - [BlockMetadataList](#types.BlockMetadataList)
    - [BlockchainStatus](#types.BlockchainStatus)
    - [ChainId](#types.ChainId)
    - [ChainInfo](#types.ChainInfo)
    - [ChainStats](#types.ChainStats)
    - [CommitResult](#types.CommitResult)
    - [CommitResultList](#types.CommitResultList)
    - [ConfigItem](#types.ConfigItem)
    - [ConfigItem.PropsEntry](#types.ConfigItem.PropsEntry)
    - [ConsensusInfo](#types.ConsensusInfo)
    - [Empty](#types.Empty)
    - [EventList](#types.EventList)
    - [ImportFormat](#types.ImportFormat)
    - [Input](#types.Input)
    - [KeyParams](#types.KeyParams)
    - [ListParams](#types.ListParams)
    - [Name](#types.Name)
    - [NameInfo](#types.NameInfo)
    - [NodeReq](#types.NodeReq)
    - [Output](#types.Output)
    - [PageParams](#types.PageParams)
    - [Peer](#types.Peer)
    - [PeerList](#types.PeerList)
    - [PeersParams](#types.PeersParams)
    - [Personal](#types.Personal)
    - [ServerInfo](#types.ServerInfo)
    - [ServerInfo.ConfigEntry](#types.ServerInfo.ConfigEntry)
    - [ServerInfo.StatusEntry](#types.ServerInfo.StatusEntry)
    - [SingleBytes](#types.SingleBytes)
    - [Staking](#types.Staking)
    - [VerifyResult](#types.VerifyResult)
    - [Vote](#types.Vote)
    - [VoteInfo](#types.VoteInfo)
    - [VoteList](#types.VoteList)
    - [VoteParams](#types.VoteParams)
  
    - [CommitStatus](#types.CommitStatus)
    - [VerifyStatus](#types.VerifyStatus)
  
  

- [blockchain.proto](#blockchain.proto)
  
    - [ABI](#types.ABI)
    - [AccountProof](#types.AccountProof)
    - [Block](#types.Block)
    - [BlockBody](#types.BlockBody)
    - [BlockHeader](#types.BlockHeader)
    - [ContractVarProof](#types.ContractVarProof)
    - [Event](#types.Event)
    - [FilterInfo](#types.FilterInfo)
    - [FnArgument](#types.FnArgument)
    - [Function](#types.Function)
    - [Query](#types.Query)
    - [Receipt](#types.Receipt)
    - [State](#types.State)
    - [StateQuery](#types.StateQuery)
    - [StateQueryProof](#types.StateQueryProof)
    - [StateVar](#types.StateVar)
    - [Tx](#types.Tx)
    - [TxBody](#types.TxBody)
    - [TxIdx](#types.TxIdx)
    - [TxInBlock](#types.TxInBlock)
    - [TxList](#types.TxList)
  
    - [TxType](#types.TxType)
  
  

- [metric.proto](#metric.proto)
  
    - [Metrics](#types.Metrics)
    - [MetricsRequest](#types.MetricsRequest)
    - [PeerMetric](#types.PeerMetric)
  
    - [MetricType](#types.MetricType)
  
  

- [Scalar Value Types](#scalar-value-types)



<a name="rpc.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## rpc.proto



<a name="types.AergoRPCService"></a>

### AergoRPCService
AergoRPCService is the main RPC service providing endpoints to interact 
with the node and blockchain. If not otherwise noted, methods are unary requests.

<a name="NodeState"></a>
#### NodeState

*Request Type:* [NodeReq](#types.NodeReq)<br>
*Response Type:* [SingleBytes](#types.SingleBytes)

Returns the current state of this node
<a name="Metric"></a>
#### Metric

*Request Type:* [MetricsRequest](#types.MetricsRequest)<br>
*Response Type:* [Metrics](#types.Metrics)

Returns node metrics according to request
<a name="Blockchain"></a>
#### Blockchain

*Request Type:* [Empty](#types.Empty)<br>
*Response Type:* [BlockchainStatus](#types.BlockchainStatus)

Returns current blockchain status (best block's height and hash)
<a name="GetChainInfo"></a>
#### GetChainInfo

*Request Type:* [Empty](#types.Empty)<br>
*Response Type:* [ChainInfo](#types.ChainInfo)

Returns current blockchain's basic information
<a name="ChainStat"></a>
#### ChainStat

*Request Type:* [Empty](#types.Empty)<br>
*Response Type:* [ChainStats](#types.ChainStats)

Returns current chain statistics
<a name="ListBlockHeaders"></a>
#### ListBlockHeaders

*Request Type:* [ListParams](#types.ListParams)<br>
*Response Type:* [BlockHeaderList](#types.BlockHeaderList)

Returns list of Blocks without body according to request
<a name="ListBlockMetadata"></a>
#### ListBlockMetadata

*Request Type:* [ListParams](#types.ListParams)<br>
*Response Type:* [BlockMetadataList](#types.BlockMetadataList)

Returns list of block metadata (hash, header, and number of transactions) according to request
<a name="ListBlockStream"></a>
#### ListBlockStream

*Request Type:* [Empty](#types.Empty)<br>
*Response Type:* [Block](#types.Block)

Returns a stream of new blocks as they get added to the blockchain
<a name="ListBlockMetadataStream"></a>
#### ListBlockMetadataStream

*Request Type:* [Empty](#types.Empty)<br>
*Response Type:* [BlockMetadata](#types.BlockMetadata)

Returns a stream of new block's metadata as they get added to the blockchain
<a name="GetBlock"></a>
#### GetBlock

*Request Type:* [SingleBytes](#types.SingleBytes)<br>
*Response Type:* [Block](#types.Block)

Return a single block incl. header and body, queried by hash or number
<a name="GetBlockMetadata"></a>
#### GetBlockMetadata

*Request Type:* [SingleBytes](#types.SingleBytes)<br>
*Response Type:* [BlockMetadata](#types.BlockMetadata)

Return a single block's metdata (hash, header, and number of transactions), queried by hash or number
<a name="GetBlockBody"></a>
#### GetBlockBody

*Request Type:* [BlockBodyParams](#types.BlockBodyParams)<br>
*Response Type:* [BlockBodyPaged](#types.BlockBodyPaged)

Return a single block's body, queried by hash or number and list parameters
<a name="GetTX"></a>
#### GetTX

*Request Type:* [SingleBytes](#types.SingleBytes)<br>
*Response Type:* [Tx](#types.Tx)

Return a single transaction, queried by transaction hash
<a name="GetBlockTX"></a>
#### GetBlockTX

*Request Type:* [SingleBytes](#types.SingleBytes)<br>
*Response Type:* [TxInBlock](#types.TxInBlock)

Return information about transaction in block, queried by transaction hash
<a name="GetReceipt"></a>
#### GetReceipt

*Request Type:* [SingleBytes](#types.SingleBytes)<br>
*Response Type:* [Receipt](#types.Receipt)

Return transaction receipt, queried by transaction hash
<a name="GetABI"></a>
#### GetABI

*Request Type:* [SingleBytes](#types.SingleBytes)<br>
*Response Type:* [ABI](#types.ABI)

Return ABI stored at contract address
<a name="SendTX"></a>
#### SendTX

*Request Type:* [Tx](#types.Tx)<br>
*Response Type:* [CommitResult](#types.CommitResult)

Sign and send a transaction from an unlocked account
<a name="SignTX"></a>
#### SignTX

*Request Type:* [Tx](#types.Tx)<br>
*Response Type:* [Tx](#types.Tx)

Sign transaction with unlocked account
<a name="VerifyTX"></a>
#### VerifyTX

*Request Type:* [Tx](#types.Tx)<br>
*Response Type:* [VerifyResult](#types.VerifyResult)

Verify validity of transaction
<a name="CommitTX"></a>
#### CommitTX

*Request Type:* [TxList](#types.TxList)<br>
*Response Type:* [CommitResultList](#types.CommitResultList)

Commit a signed transaction
<a name="GetState"></a>
#### GetState

*Request Type:* [SingleBytes](#types.SingleBytes)<br>
*Response Type:* [State](#types.State)

Return state of account
<a name="GetStateAndProof"></a>
#### GetStateAndProof

*Request Type:* [AccountAndRoot](#types.AccountAndRoot)<br>
*Response Type:* [AccountProof](#types.AccountProof)

Return state of account, including merkle proof
<a name="CreateAccount"></a>
#### CreateAccount

*Request Type:* [Personal](#types.Personal)<br>
*Response Type:* [Account](#types.Account)

Create a new account in this node
<a name="GetAccounts"></a>
#### GetAccounts

*Request Type:* [Empty](#types.Empty)<br>
*Response Type:* [AccountList](#types.AccountList)

Return list of accounts in this node
<a name="LockAccount"></a>
#### LockAccount

*Request Type:* [Personal](#types.Personal)<br>
*Response Type:* [Account](#types.Account)

Lock account in this node
<a name="UnlockAccount"></a>
#### UnlockAccount

*Request Type:* [Personal](#types.Personal)<br>
*Response Type:* [Account](#types.Account)

Unlock account in this node
<a name="ImportAccount"></a>
#### ImportAccount

*Request Type:* [ImportFormat](#types.ImportFormat)<br>
*Response Type:* [Account](#types.Account)

Import account to this node
<a name="ExportAccount"></a>
#### ExportAccount

*Request Type:* [Personal](#types.Personal)<br>
*Response Type:* [SingleBytes](#types.SingleBytes)

Export account stored in this node
<a name="QueryContract"></a>
#### QueryContract

*Request Type:* [Query](#types.Query)<br>
*Response Type:* [SingleBytes](#types.SingleBytes)

Query a contract method
<a name="QueryContractState"></a>
#### QueryContractState

*Request Type:* [StateQuery](#types.StateQuery)<br>
*Response Type:* [StateQueryProof](#types.StateQueryProof)

Query contract state
<a name="GetPeers"></a>
#### GetPeers

*Request Type:* [PeersParams](#types.PeersParams)<br>
*Response Type:* [PeerList](#types.PeerList)

Return list of peers of this node and their state
<a name="GetVotes"></a>
#### GetVotes

*Request Type:* [VoteParams](#types.VoteParams)<br>
*Response Type:* [VoteList](#types.VoteList)

Return result of vote
<a name="GetAccountVotes"></a>
#### GetAccountVotes

*Request Type:* [AccountAddress](#types.AccountAddress)<br>
*Response Type:* [AccountVoteInfo](#types.AccountVoteInfo)

Return staking, voting info for account
<a name="GetStaking"></a>
#### GetStaking

*Request Type:* [AccountAddress](#types.AccountAddress)<br>
*Response Type:* [Staking](#types.Staking)

Return staking information
<a name="GetNameInfo"></a>
#### GetNameInfo

*Request Type:* [Name](#types.Name)<br>
*Response Type:* [NameInfo](#types.NameInfo)

Return name information
<a name="ListEventStream"></a>
#### ListEventStream

*Request Type:* [FilterInfo](#types.FilterInfo)<br>
*Response Type:* [Event](#types.Event)

Returns a stream of event as they get added to the blockchain
<a name="ListEvents"></a>
#### ListEvents

*Request Type:* [FilterInfo](#types.FilterInfo)<br>
*Response Type:* [EventList](#types.EventList)

Returns list of event
<a name="GetServerInfo"></a>
#### GetServerInfo

*Request Type:* [KeyParams](#types.KeyParams)<br>
*Response Type:* [ServerInfo](#types.ServerInfo)

Returns configs and statuses of server
<a name="GetConsensusInfo"></a>
#### GetConsensusInfo

*Request Type:* [Empty](#types.Empty)<br>
*Response Type:* [ConsensusInfo](#types.ConsensusInfo)

Returns status of consensus and bps
<a name="ChangeMembership"></a>
#### ChangeMembership

*Request Type:* [MembershipChange](#types.MembershipChange)<br>
*Response Type:* [MembershipChangeReply](#types.MembershipChangeReply)

Add & remove member of raft cluster

 <!-- end services -->


<a name="types.AccountAddress"></a>

### AccountAddress



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| value | [bytes](#bytes) |  |  |






<a name="types.AccountAndRoot"></a>

### AccountAndRoot



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| Account | [bytes](#bytes) |  |  |
| Root | [bytes](#bytes) |  |  |
| Compressed | [bool](#bool) |  |  |






<a name="types.AccountVoteInfo"></a>

### AccountVoteInfo



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| staking | [Staking](#types.Staking) |  |  |
| voting | [VoteInfo](#types.VoteInfo) | repeated |  |






<a name="types.BlockBodyPaged"></a>

### BlockBodyPaged



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| total | [uint32](#uint32) |  |  |
| offset | [uint32](#uint32) |  |  |
| size | [uint32](#uint32) |  |  |
| body | [BlockBody](#types.BlockBody) |  |  |






<a name="types.BlockBodyParams"></a>

### BlockBodyParams



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| hashornumber | [bytes](#bytes) |  |  |
| paging | [PageParams](#types.PageParams) |  |  |






<a name="types.BlockHeaderList"></a>

### BlockHeaderList



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| blocks | [Block](#types.Block) | repeated |  |






<a name="types.BlockMetadata"></a>

### BlockMetadata



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| hash | [bytes](#bytes) |  |  |
| header | [BlockHeader](#types.BlockHeader) |  |  |
| txcount | [int32](#int32) |  |  |
| size | [int64](#int64) |  | blocksize in bytes |






<a name="types.BlockMetadataList"></a>

### BlockMetadataList



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| blocks | [BlockMetadata](#types.BlockMetadata) | repeated |  |






<a name="types.BlockchainStatus"></a>

### BlockchainStatus
BlockchainStatus is current status of blockchain


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| best_block_hash | [bytes](#bytes) |  |  |
| best_height | [uint64](#uint64) |  |  |
| consensus_info | [string](#string) |  |  |
| best_chain_id_hash | [bytes](#bytes) |  |  |






<a name="types.ChainId"></a>

### ChainId



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| magic | [string](#string) |  |  |
| public | [bool](#bool) |  |  |
| mainnet | [bool](#bool) |  |  |
| consensus | [string](#string) |  |  |






<a name="types.ChainInfo"></a>

### ChainInfo
ChainInfo returns chain configuration


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| id | [ChainId](#types.ChainId) |  |  |
| bpNumber | [uint32](#uint32) |  |  |
| maxblocksize | [uint64](#uint64) |  |  |
| maxtokens | [bytes](#bytes) |  |  |
| stakingminimum | [bytes](#bytes) |  |  |
| totalstaking | [bytes](#bytes) |  |  |
| gasprice | [bytes](#bytes) |  |  |
| nameprice | [bytes](#bytes) |  |  |






<a name="types.ChainStats"></a>

### ChainStats
ChainStats corresponds to a chain statistics report.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| report | [string](#string) |  |  |






<a name="types.CommitResult"></a>

### CommitResult



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| hash | [bytes](#bytes) |  |  |
| error | [CommitStatus](#types.CommitStatus) |  |  |
| detail | [string](#string) |  |  |






<a name="types.CommitResultList"></a>

### CommitResultList



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| results | [CommitResult](#types.CommitResult) | repeated |  |






<a name="types.ConfigItem"></a>

### ConfigItem



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| props | [ConfigItem.PropsEntry](#types.ConfigItem.PropsEntry) | repeated |  |






<a name="types.ConfigItem.PropsEntry"></a>

### ConfigItem.PropsEntry



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| key | [string](#string) |  |  |
| value | [string](#string) |  |  |






<a name="types.ConsensusInfo"></a>

### ConsensusInfo
info and bps is json string


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| type | [string](#string) |  |  |
| info | [string](#string) |  |  |
| bps | [string](#string) | repeated |  |






<a name="types.Empty"></a>

### Empty







<a name="types.EventList"></a>

### EventList



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| events | [Event](#types.Event) | repeated |  |






<a name="types.ImportFormat"></a>

### ImportFormat



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| wif | [SingleBytes](#types.SingleBytes) |  |  |
| oldpass | [string](#string) |  |  |
| newpass | [string](#string) |  |  |






<a name="types.Input"></a>

### Input



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| hash | [bytes](#bytes) |  |  |
| address | [bytes](#bytes) | repeated |  |
| value | [bytes](#bytes) |  |  |
| script | [bytes](#bytes) |  |  |






<a name="types.KeyParams"></a>

### KeyParams



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| key | [string](#string) | repeated |  |






<a name="types.ListParams"></a>

### ListParams



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| hash | [bytes](#bytes) |  |  |
| height | [uint64](#uint64) |  |  |
| size | [uint32](#uint32) |  |  |
| offset | [uint32](#uint32) |  |  |
| asc | [bool](#bool) |  |  |






<a name="types.Name"></a>

### Name



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| name | [string](#string) |  |  |
| blockNo | [uint64](#uint64) |  |  |






<a name="types.NameInfo"></a>

### NameInfo



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| name | [Name](#types.Name) |  |  |
| owner | [bytes](#bytes) |  |  |
| destination | [bytes](#bytes) |  |  |






<a name="types.NodeReq"></a>

### NodeReq



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| timeout | [bytes](#bytes) |  |  |
| component | [bytes](#bytes) |  |  |






<a name="types.Output"></a>

### Output



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| index | [uint32](#uint32) |  |  |
| address | [bytes](#bytes) |  |  |
| value | [bytes](#bytes) |  |  |
| script | [bytes](#bytes) |  |  |






<a name="types.PageParams"></a>

### PageParams



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| offset | [uint32](#uint32) |  |  |
| size | [uint32](#uint32) |  |  |






<a name="types.Peer"></a>

### Peer



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| address | [PeerAddress](#types.PeerAddress) |  |  |
| bestblock | [NewBlockNotice](#types.NewBlockNotice) |  |  |
| state | [int32](#int32) |  |  |
| hidden | [bool](#bool) |  |  |
| lashCheck | [int64](#int64) |  |  |
| selfpeer | [bool](#bool) |  |  |
| version | [string](#string) |  |  |






<a name="types.PeerList"></a>

### PeerList



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| peers | [Peer](#types.Peer) | repeated |  |






<a name="types.PeersParams"></a>

### PeersParams



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| noHidden | [bool](#bool) |  |  |
| showSelf | [bool](#bool) |  |  |






<a name="types.Personal"></a>

### Personal



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| passphrase | [string](#string) |  |  |
| account | [Account](#types.Account) |  |  |






<a name="types.ServerInfo"></a>

### ServerInfo



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| status | [ServerInfo.StatusEntry](#types.ServerInfo.StatusEntry) | repeated |  |
| config | [ServerInfo.ConfigEntry](#types.ServerInfo.ConfigEntry) | repeated |  |






<a name="types.ServerInfo.ConfigEntry"></a>

### ServerInfo.ConfigEntry



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| key | [string](#string) |  |  |
| value | [ConfigItem](#types.ConfigItem) |  |  |






<a name="types.ServerInfo.StatusEntry"></a>

### ServerInfo.StatusEntry



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| key | [string](#string) |  |  |
| value | [string](#string) |  |  |






<a name="types.SingleBytes"></a>

### SingleBytes



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| value | [bytes](#bytes) |  |  |






<a name="types.Staking"></a>

### Staking



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| amount | [bytes](#bytes) |  |  |
| when | [uint64](#uint64) |  |  |






<a name="types.VerifyResult"></a>

### VerifyResult



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| tx | [Tx](#types.Tx) |  |  |
| error | [VerifyStatus](#types.VerifyStatus) |  |  |






<a name="types.Vote"></a>

### Vote



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| candidate | [bytes](#bytes) |  |  |
| amount | [bytes](#bytes) |  |  |






<a name="types.VoteInfo"></a>

### VoteInfo



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| id | [string](#string) |  |  |
| candidates | [string](#string) | repeated |  |






<a name="types.VoteList"></a>

### VoteList



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| votes | [Vote](#types.Vote) | repeated |  |
| id | [string](#string) |  |  |






<a name="types.VoteParams"></a>

### VoteParams



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| id | [string](#string) |  |  |
| count | [uint32](#uint32) |  |  |





 <!-- end messages -->


<a name="types.CommitStatus"></a>

### CommitStatus


| Name | Number | Description |
| ---- | ------ | ----------- |
| TX_OK | 0 |  |
| TX_NONCE_TOO_LOW | 1 |  |
| TX_ALREADY_EXISTS | 2 |  |
| TX_INVALID_HASH | 3 |  |
| TX_INVALID_SIGN | 4 |  |
| TX_INVALID_FORMAT | 5 |  |
| TX_INSUFFICIENT_BALANCE | 6 |  |
| TX_HAS_SAME_NONCE | 7 |  |
| TX_INTERNAL_ERROR | 9 |  |



<a name="types.VerifyStatus"></a>

### VerifyStatus


| Name | Number | Description |
| ---- | ------ | ----------- |
| VERIFY_STATUS_OK | 0 |  |
| VERIFY_STATUS_SIGN_NOT_MATCH | 1 |  |
| VERIFY_STATUS_INVALID_HASH | 2 | TODO: not yet impl |


 <!-- end enums -->

 <!-- end HasExtensions -->





<a name="blockchain.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## blockchain.proto


 <!-- end services -->


<a name="types.ABI"></a>

### ABI



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| version | [string](#string) |  |  |
| language | [string](#string) |  |  |
| functions | [Function](#types.Function) | repeated |  |
| state_variables | [StateVar](#types.StateVar) | repeated |  |






<a name="types.AccountProof"></a>

### AccountProof



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| state | [State](#types.State) |  |  |
| inclusion | [bool](#bool) |  |  |
| key | [bytes](#bytes) |  |  |
| proofKey | [bytes](#bytes) |  |  |
| proofVal | [bytes](#bytes) |  |  |
| bitmap | [bytes](#bytes) |  |  |
| height | [uint32](#uint32) |  |  |
| auditPath | [bytes](#bytes) | repeated |  |






<a name="types.Block"></a>

### Block



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| hash | [bytes](#bytes) |  |  |
| header | [BlockHeader](#types.BlockHeader) |  |  |
| body | [BlockBody](#types.BlockBody) |  |  |






<a name="types.BlockBody"></a>

### BlockBody



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| txs | [Tx](#types.Tx) | repeated |  |






<a name="types.BlockHeader"></a>

### BlockHeader



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| chainID | [bytes](#bytes) |  | chain identifier |
| prevBlockHash | [bytes](#bytes) |  | hash of previous block |
| blockNo | [uint64](#uint64) |  | block number |
| timestamp | [int64](#int64) |  | block creation time stamp |
| blocksRootHash | [bytes](#bytes) |  | hash of root of block merkle tree |
| txsRootHash | [bytes](#bytes) |  | hash of root of transaction merkle tree |
| receiptsRootHash | [bytes](#bytes) |  | hash of root of receipt merkle tree |
| confirms | [uint64](#uint64) |  | number of blocks this block is able to confirm |
| pubKey | [bytes](#bytes) |  | block producer's public key |
| coinbaseAccount | [bytes](#bytes) |  | address of account to receive fees |
| sign | [bytes](#bytes) |  | block producer's signature of BlockHeader |






<a name="types.ContractVarProof"></a>

### ContractVarProof



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| value | [bytes](#bytes) |  |  |
| inclusion | [bool](#bool) |  |  |
| key | [string](#string) |  |  |
| proofKey | [bytes](#bytes) |  |  |
| proofVal | [bytes](#bytes) |  |  |
| bitmap | [bytes](#bytes) |  |  |
| height | [uint32](#uint32) |  |  |
| auditPath | [bytes](#bytes) | repeated |  |






<a name="types.Event"></a>

### Event



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| contractAddress | [bytes](#bytes) |  |  |
| eventName | [string](#string) |  |  |
| jsonArgs | [string](#string) |  |  |
| eventIdx | [int32](#int32) |  |  |
| txHash | [bytes](#bytes) |  |  |
| blockHash | [bytes](#bytes) |  |  |
| blockNo | [uint64](#uint64) |  |  |
| txIndex | [int32](#int32) |  |  |






<a name="types.FilterInfo"></a>

### FilterInfo



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| contractAddress | [bytes](#bytes) |  |  |
| eventName | [string](#string) |  |  |
| blockfrom | [uint64](#uint64) |  |  |
| blockto | [uint64](#uint64) |  |  |
| desc | [bool](#bool) |  |  |
| argFilter | [bytes](#bytes) |  |  |
| recentBlockCnt | [int32](#int32) |  |  |






<a name="types.FnArgument"></a>

### FnArgument



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| name | [string](#string) |  |  |






<a name="types.Function"></a>

### Function



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| name | [string](#string) |  |  |
| arguments | [FnArgument](#types.FnArgument) | repeated |  |
| payable | [bool](#bool) |  |  |
| view | [bool](#bool) |  |  |






<a name="types.Query"></a>

### Query



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| contractAddress | [bytes](#bytes) |  |  |
| queryinfo | [bytes](#bytes) |  |  |






<a name="types.Receipt"></a>

### Receipt



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| contractAddress | [bytes](#bytes) |  |  |
| status | [string](#string) |  |  |
| ret | [string](#string) |  |  |
| txHash | [bytes](#bytes) |  |  |
| feeUsed | [bytes](#bytes) |  |  |
| cumulativeFeeUsed | [bytes](#bytes) |  |  |
| bloom | [bytes](#bytes) |  |  |
| events | [Event](#types.Event) | repeated |  |
| blockNo | [uint64](#uint64) |  |  |
| blockHash | [bytes](#bytes) |  |  |
| txIndex | [int32](#int32) |  |  |
| from | [bytes](#bytes) |  |  |
| to | [bytes](#bytes) |  |  |






<a name="types.State"></a>

### State



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| nonce | [uint64](#uint64) |  |  |
| balance | [bytes](#bytes) |  |  |
| codeHash | [bytes](#bytes) |  |  |
| storageRoot | [bytes](#bytes) |  |  |
| sqlRecoveryPoint | [uint64](#uint64) |  |  |






<a name="types.StateQuery"></a>

### StateQuery



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| contractAddress | [bytes](#bytes) |  |  |
| storageKeys | [string](#string) | repeated |  |
| root | [bytes](#bytes) |  |  |
| compressed | [bool](#bool) |  |  |






<a name="types.StateQueryProof"></a>

### StateQueryProof



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| contractProof | [AccountProof](#types.AccountProof) |  |  |
| varProofs | [ContractVarProof](#types.ContractVarProof) | repeated |  |






<a name="types.StateVar"></a>

### StateVar



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| name | [string](#string) |  |  |
| type | [string](#string) |  |  |
| len | [int32](#int32) |  |  |






<a name="types.Tx"></a>

### Tx



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| hash | [bytes](#bytes) |  |  |
| body | [TxBody](#types.TxBody) |  |  |






<a name="types.TxBody"></a>

### TxBody



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| nonce | [uint64](#uint64) |  | increasing number used only once per sender account |
| account | [bytes](#bytes) |  | decoded account address |
| recipient | [bytes](#bytes) |  | decoded account address |
| amount | [bytes](#bytes) |  | variable-length big integer |
| payload | [bytes](#bytes) |  |  |
| gasLimit | [uint64](#uint64) |  | currently not used |
| gasPrice | [bytes](#bytes) |  | variable-length big integer. currently not used |
| type | [TxType](#types.TxType) |  |  |
| chainIdHash | [bytes](#bytes) |  | hash value of chain identifier in the block |
| sign | [bytes](#bytes) |  | sender's signature for this TxBody |






<a name="types.TxIdx"></a>

### TxIdx
TxIdx specifies a transaction's block hash and index within the block body


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| blockHash | [bytes](#bytes) |  |  |
| idx | [int32](#int32) |  |  |






<a name="types.TxInBlock"></a>

### TxInBlock



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| txIdx | [TxIdx](#types.TxIdx) |  |  |
| tx | [Tx](#types.Tx) |  |  |






<a name="types.TxList"></a>

### TxList



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| txs | [Tx](#types.Tx) | repeated |  |





 <!-- end messages -->


<a name="types.TxType"></a>

### TxType


| Name | Number | Description |
| ---- | ------ | ----------- |
| NORMAL | 0 |  |
| GOVERNANCE | 1 |  |


 <!-- end enums -->

 <!-- end HasExtensions -->





<a name="metric.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## metric.proto


 <!-- end services -->


<a name="types.Metrics"></a>

### Metrics



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| peers | [PeerMetric](#types.PeerMetric) | repeated |  |






<a name="types.MetricsRequest"></a>

### MetricsRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| types | [MetricType](#types.MetricType) | repeated |  |






<a name="types.PeerMetric"></a>

### PeerMetric



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| peerID | [bytes](#bytes) |  |  |
| sumIn | [int64](#int64) |  |  |
| avrIn | [int64](#int64) |  |  |
| sumOut | [int64](#int64) |  |  |
| avrOut | [int64](#int64) |  |  |





 <!-- end messages -->


<a name="types.MetricType"></a>

### MetricType


| Name | Number | Description |
| ---- | ------ | ----------- |
| NOTHING | 0 | NOTHING should not be used. |
| P2P_NETWORK | 1 | Metric for p2p network transfer |


 <!-- end enums -->

 <!-- end HasExtensions -->





## Scalar Value Types

| .proto Type | Notes | C++ Type | Java Type | Python Type |
| ----------- | ----- | -------- | --------- | ----------- |
| <a name="double" /> double |  | double | double | float |
| <a name="float" /> float |  | float | float | float |
| <a name="int32" /> int32 | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint32 instead. | int32 | int | int |
| <a name="int64" /> int64 | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint64 instead. | int64 | long | int/long |
| <a name="uint32" /> uint32 | Uses variable-length encoding. | uint32 | int | int/long |
| <a name="uint64" /> uint64 | Uses variable-length encoding. | uint64 | long | int/long |
| <a name="sint32" /> sint32 | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int32s. | int32 | int | int |
| <a name="sint64" /> sint64 | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int64s. | int64 | long | int/long |
| <a name="fixed32" /> fixed32 | Always four bytes. More efficient than uint32 if values are often greater than 2^28. | uint32 | int | int |
| <a name="fixed64" /> fixed64 | Always eight bytes. More efficient than uint64 if values are often greater than 2^56. | uint64 | long | int/long |
| <a name="sfixed32" /> sfixed32 | Always four bytes. | int32 | int | int |
| <a name="sfixed64" /> sfixed64 | Always eight bytes. | int64 | long | int/long |
| <a name="bool" /> bool |  | bool | boolean | boolean |
| <a name="string" /> string | A string must always contain UTF-8 encoded or 7-bit ASCII text. | string | String | str/unicode |
| <a name="bytes" /> bytes | May contain any arbitrary sequence of bytes. | string | ByteString | str |
