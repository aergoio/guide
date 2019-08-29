Config Reference
================
이 페이지에서는 Raft consensus와 관련된 configuration과 logging option에 관해 설명한다.

config에 관한 공통 reference는 `here<../running-node/configuration.html#reference>`__을 참고한다.

## Config Reference

| Module         | Property name      | Meaning                                  |
| -------------- |--------------------|------------------------------------------|
| consensus.raft | name 		 	  | raft node name. name should be unique in cluster |
|                | newcluster         | initialize a new raft cluster if it doesn't already exist |                                       
| 				 | skipempty 		  | skip producing block if there is no transactions in block |
|                | heartbeattick      | heartbeat time(millisec) of raft node |                                                 
|                | electiontickcount  | number of heartbeattick to wait before becomeing a candidate when no heartbeat message comes |
|                | blockfactorytickms | interval(millisec) to check if block factory should run new task |                    
|                | blockintervalms    | block generation interval(millisec) for raft. It overrides BlockInterval of consensus |            
|                | usebackup          | use backup datafiles for initializing a new cluster or joining an existing cluster |            
|                | snapfrequency      | frequency which raft make snapshot with log |
|                | slownodegap        | Criteria for determining if a node is a slow node. Difference in chain height from leader node |
|                | recoverbp  		  | bp info for initializing a new cluster from backup data files |

## Logging options
Raft consensus와 관련된 log를 보기 위해서는 `arglog.toml`에 raft 항목을 추가해야한다.

```toml
[raft]
level = "info"
```
