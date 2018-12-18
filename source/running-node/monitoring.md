Monitoring
==========

Aergo server has 5 major subcomponents: Account, Chain, MemPool, RPC, and P2P.

We can get status of component using aergocli node command (see [here](https://github.com/aergoio/aergo/wiki/tools_aergocli) for detail).

```shell
$ aergocli node 
{
        "AccountsSvc": {
                "status": "started",
                "acc_processed_msg": 3,
                "msg_queue_len": 0,
                "msg_latency": "69.851µs",
                "error": "",
                "actor": null
        },
        "ChainSvc": {
                "status": "started",
                "acc_processed_msg": 238,
                "msg_queue_len": 0,
                "msg_latency": "68.849µs",
                "error": "",
                "actor": {
                        "orphan": 0
                }
        },
        "MemPoolSvc": {
                "status": "started",
                "acc_processed_msg": 237,
                "msg_queue_len": 0,
                "msg_latency": "59.462µs",
                "error": "",
                "actor": {
                        "cache_len": 0,
                        "dead": 0,
                        "orphan": 0
                }
        },
        "RPCSvc": {
                "status": "started",
                "acc_processed_msg": 120,
                "msg_queue_len": 0,
                "msg_latency": "71.785µs",
                "error": "",
                "actor": null
        },
        "p2pSvc": {
                "status": "started",
                "acc_processed_msg": 120,
                "msg_queue_len": 0,
                "msg_latency": "54.783µs",
                "error": "",
                "actor": null
        }
}
```

As it shown above, each component has 6 values, which are 
 * status *
 * acc_processed_msg 
    * number of message processed by the component 
    * message is basic unit for communicate with other components
 * msg_queue_len
    * number of pending messages for the component
 * msg_latency
    * average latency for processing a message
 * error * 
 * actor
    * component specific statistics are shown
    * for example, ChainSvc
     <code>
          "actor": {
                        "orphan": 0
                }
     </code> 
     orphan represents number of orphan block the component has 
