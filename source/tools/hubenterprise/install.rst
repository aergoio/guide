Install Hub Enterprise
==============
This page explains how to install **Hub Enterprise**.

**Hub Enterprise** designed as orchestrate many number of  machines, which blockchain nodes are run.

Hub Enterprise use kubernetes for create Horde cluster. Follow `kubernetes official guide <https://kubernetes.io/docs/setup/>` to install kubernetes.

There are two kind of machines in Hub Eneterprise :
- Horde Master : Hub Enterprise's modules are installed. Manage and control Horde as kubernetes master.
- Horde : Kubernetes worker cluster that Aergo nodes you made are actually running.


The Horde Master consists of several modules :
- Hub : Front end of user interface to manage and monitoring Aergo node cluster.
- Hub-abc : Back end of Hub.
- Configure chain : Cross-communication window between Hub Enterprise modules and store data securely through blockchain. Hub Enterprise use Aergo node. 
- Horde master control : Control module of blockchain managing.
- Aergo grafana : User interface for monitoring module.
- Elastic search server : Store and process statistic datas.
- Fluentd : Get agent's data and send to elastic search server.
- Metricbeat : Collects machine's status data.
- HordeStat : Collects blockchain network's status data
- Fluent-bit : Collects and parse blockchain logs data, then send to Fluentd.


Install managing modules
------------------------
All modules are installed and run as kubernetes container.

  ::

    kubectl create -f https://raw.githubusercontent.com/aergoio/horde/master/yaml/kube/aergo-hub-service.yaml
    kubectl create -f https://raw.githubusercontent.com/aergoio/horde/master/yaml/kube/aergo-node-service.yaml
    kubectl create -f https://raw.githubusercontent.com/aergoio/horde/master/yaml/kube/hub-abc-service.yaml
    kubectl create -f https://raw.githubusercontent.com/aergoio/horde/master/yaml/kube/hmc.yaml 


Install monitoring modules
--------------------------
**Install from packages**
To run monitoring module, you need to `install docker <https://docs.docker.com/install/>` first.

1. Elastic search server and aergo grafana
  Install at Horde Master machine.
  ::
    docker run -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" --name=aergo-es docker.elastic.co/elasticsearch/elasticsearch:6.5.4
    docker run -p 3000:3000 --name=aergo-grafana aergo/grafana:0.1.0

2. Fluentd
  Install at Horde Master machine.
  To install FluentD, follow `this article <https://support.treasuredata.com/hc/en-us/articles/360001525887-Installing-and-Updating-the-Treasure-Data-CLI>`.
  Before run fluentd, edit td-agent configuration file in /etc/td-agent/td-agent.conf
``` xml
<system>
  log_level debug
</system>
<source>
  @type forward
  @log_level debug
  port 24224
  bind 0.0.0.0
</source>
<source>
  @type tail
  path /dir/to/aergo_chaininfo.log
  tag aergo.chaininfo
  format json
</source>
<match \**>
  @type elasticsearch
  index_name fluentd
  host localhost
  port 9200
  logstash_format true
</match>
```
  Run fluend as service
  ::
    sudo /etc/init.d/td-agent start

3. Metricbeat
  Install at every Horde machine.

  ::
    sudo curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-6.4.2-amd64.deb
    sudo dpkg -i metricbeat-6.4.2-amd64.deb

  Edit metricbeat configuration file in /etc/metricbeat/metricbeat.yml before run metricbeat

``` yaml
########################## Metricbeat Configuration ###########################
# https://www.elastic.co/guide/en/beats/metricbeat/index.html
#============================  Config Reloading ===============================
metricbeat.config.modules:
  path: /etc/metricbeat/metricbeat.yml
  reload.period: 10s
  reload.enabled: false
metricbeat.max_start_delay: 10s
#==========================  Modules configuration ============================
metricbeat.modules:
#------------------------------- System Module -------------------------------
- module: system
  metricsets:
    - cpu             # CPU usage
    - load            # CPU load averages
    - memory          # Memory usage
    - network         # Network IO
    - process         # Per process metrics
    - diskio
    - filesystem
    - fsstat
  enabled: true
  period: 15s

  # Configure the metric types that are included by these metricsets.
  core.metrics: ["percentages", "ticks"]  # The other available option is ticks.
  cpu.metrics:  ["percentages", "normalized_percentages", "ticks"]  # The other available options are normalized_percentages and ticks.
  processes: [".*"]
#------------------------------- File output -----------------------------------
output.file:
  enabled: true
  codec.json:
    escape_html: false
    pretty: false
  path: "/var/log"
  filename: metricbeat
  rotate_every_kb: 10000
  number_of_files: 3
  permissions: 0600
#================================ Logging ======================================
logging.level: info
logging.to_files: true
logging.files:
  path: /var/log/metricbeat-logs
  name: metricbeat
```

4. HordeStat
Install at Horde Master machine.

::
  docker run --name=hordestat -v /dir/to/aergo_chaininfo.log:/var/log/aergo_chaininfo.log --net=host aergo/hordestat:0.1.0 --fullnode={fullnode ip}:{fullnode rpc port} --debug --wait_time=2 --daemon

5. Fluent-bit
Install at Horde Machine.
To install fluent-bit, follow `fluent-bit official install document <https://fluentbit.io/documentation/current/installation/>`
Edit fluent-bit configuration file in /etc/td-agent-bit/td-agent-bit.conf

```
[SERVICE]
    Flush      5
    Daemon     off
    Parsers_File   /etc/td-agent-bit/parsers.conf
    Log_File   /var/log/td-agent-bit.log
    Log_Level  info

[INPUT]
    Tag         log
    Buffer_Chunk_Size    512k
    Buffer_Max_Size    512k
    Name        tail
    Path        /var/lib/docker/containers/\*.log
    DB          /var/log/td-agent-bit-db
    DB.Sync     Off
    Parser      aergolog

[INPUT]
    Parser      json
    Tag         metricbeat
    Buffer_Chunk_Size    128k
    Buffer_Max_Size    128k
    Name        tail
    Path        /var/log/metricbeat
    DB          /var/log/td-agent-bit-db
    DB.Sync     Full

[FILTER]
    Name        record_modifier
    Match       log
    Record hostname ubuntu-xenial


[OUTPUT]
    Name              forward
    Match             *
    Host              IP_OF_ELASTIC_SEARCH_SERVER
    Port              24224
    Retry_Limit       False
```

Insert follow fluent-bit parser in /etc/td-agent-bit/parsers.conf

.. todo::
   fix fluent-bit parser

**Install as kubernetes container**

  ::
  kubectl create -f https://raw.githubusercontent.com/aergoio/horde/master/yaml/kube/es-statefulset.yaml
  kubectl create -f https://raw.githubusercontent.com/aergoio/horde/master/yaml/kube/es-service.yaml
  kubectl create -f https://raw.githubusercontent.com/aergoio/horde/master/yaml/kube/aergo-grafana-service.yaml
  #kubectl create -f fluentd
  kubectl create -f https://raw.githubusercontent.com/aergoio/horde/master/yaml/kube/metricbeat.yaml
  kubectl create -f https://raw.githubusercontent.com/aergoio/horde/master/yaml/kube/fluent-bit-configmap.yaml
  kubectl create -f https://raw.githubusercontent.com/aergoio/horde/master/yaml/kube/fluentbit.yaml

.. todo::
   add fluent-d kubernetes yaml files
