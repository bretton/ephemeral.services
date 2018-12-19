# Monitoring

## Logstash

To-do: Ingestion of logs to [Logstash](https://www.elastic.co/products/logstash) howto

### Rough Outline 

(to be removed once documentation ready)

```
- need a logstash server elsewhere, powerful
  - https://www.elastic.co/guide/en/logstash/current/index.html
- need to install filebeat on bitcoind and lnd nodes
  - https://www.elastic.co/guide/en/logstash/current/advanced-pipeline.html
  - https://github.com/elastic/beats/tree/master/filebeat
- filebeat will monitor log files and send to the configured logstash server
- logstash server can implement filters, and export to other messaging & monitoring platforms
  - https://www.elastic.co/guide/en/logstash/current/filter-plugins.html
  - https://www.elastic.co/guide/en/logstash/current/output-plugins.html
```

## Prometheus

To-do: Retrieval of information and input into [Prometheus](https://prometheus.io/), a time-series database that's very useful. 

### Useful References for LND

The following reference material may be relevant to this chapter
* https://github.com/lightningnetwork/lnd/issues/1243
* https://github.com/lightningnetwork/lnd/pull/1355
