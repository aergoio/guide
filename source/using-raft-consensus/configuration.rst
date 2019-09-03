Configuration
================

This page describes the configuration and logging options related to Raft consensus.

.. seealso:: See `Config Reference <../running-node/configuration.html#reference>`__ for a detailed explanation of common config reference.


Config Reference
------------------

+----------------+--------------------+-------------------------------------------------------------------------------------+
| Module         | Property name      | Meaning                                                                             |
+----------------+--------------------+-------------------------------------------------------------------------------------+
| consensus.raft | name               | raft node name. name should be unique in cluster                                    |
+----------------+--------------------+-------------------------------------------------------------------------------------+
|                | newcluster         | initialize a new raft cluster if it doesn't already exist                           |
+----------------+--------------------+-------------------------------------------------------------------------------------+
|                | skipempty          | skip producing block if there is no transactions in block                           |
+----------------+--------------------+-------------------------------------------------------------------------------------+
|                | heartbeattick      | heartbeat time(millisec) of raft node                                               |
+----------------+--------------------+-------------------------------------------------------------------------------------+
|                | electiontickcount  | number of heartbeattick to wait before becomeing a candidate without heartbeat      |
+----------------+--------------------+-------------------------------------------------------------------------------------+
|                | blockfactorytickms | interval(millisec) to check if block factory should run new task                    | 
+----------------+--------------------+-------------------------------------------------------------------------------------+
|                | blockintervalms    | block generation interval(millisec).It overrides BlockInterval of consensus         | 
+----------------+--------------------+-------------------------------------------------------------------------------------+
|                | usebackup          | use backup datafiles for initializing a new cluster or joining an existing cluster  | 
+----------------+--------------------+-------------------------------------------------------------------------------------+
|                | snapfrequency      | frequency which raft make snapshot with log                                         |
+----------------+--------------------+-------------------------------------------------------------------------------------+
|                | recoverbp          | bp info for initializing a new cluster from backup data files                       |
+----------------+--------------------+-------------------------------------------------------------------------------------+
|                | slownodegap        | Max difference of chain height for determining if a node is a slow node             |
+----------------+--------------------+-------------------------------------------------------------------------------------+

Logging options
------------------

To see the log related to Raft consensus, add a raft section to `arglog.toml`.

.. code-block:: toml

  [raft]
  level = "info"
