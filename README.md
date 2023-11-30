# OpenNMS Thanos Docker Compose Setup

Borrowed largely from here:
https://blog.ruanbekker.com/blog/2020/02/01/setup-thanos-on-docker-a-highly-available-prometheus/

This sets up the following docker containers:
* thanos-query0
* thanos-query1
* thanos-receive
* mc           
* thanos-remote-write
* thanos-sidecar1
* thanos-sidecar2
* thanos-sidecar0
* prometheus1  
* prometheus2  
* thanos-store 
* prometheus0  
* node-exporter
* minio        
* grafana      


## Vague directions

1. Run the configs.sh script with "bash configs.sh" and it will generate the data directory and configs.

2. Following that, run "docker-compose -f docker-compose.yml up" and hopefully, you'll end up with something that looks like this:

```
Starting node-exporter ... done
Starting minio         ... done
Starting grafana        ... done
Starting prometheus0    ... done
Starting prometheus1     ... done
Starting thanos-receive  ... done
Starting thanos-store    ... done
Starting prometheus2     ... done
Starting mc             ... done
Starting thanos-sidecar0 ... done
Starting thanos-sidecar1     ... done
Starting thanos-sidecar2     ... done
Starting thanos-remote-write ... done
Starting thanos-query1       ... done
Starting thanos-query0       ... done
```

* Thanos UI (and read) can be reached on port 10904.
* Thanos Receive is on port 10908
* Grafana UI can be reached on port 3000.

## For OpenNMS, the following configuration should be applied to the Cortex time series plugin (org.opennms.plugins.tss.cortex.cfg):

```
writeUrl=http://<docker host ip>:10908/api/v1/receive
readUrl=http://<docker host ip>:10904/api/v1
maxConcurrentHttpConnections=100
writeTimeoutInMs=1000
readTimeoutInMs=1000
metricCacheSize=1000
externalTagsCacheSize=1000
bulkheadMaxWaitDurationInMs=9223372036854775807
```

Follow the OpenNMS configuration instructions for the Cortex plugin otherwise.
