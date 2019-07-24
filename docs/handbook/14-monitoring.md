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

The following reference material for LND may be relevant to this chapter
* https://github.com/lightningnetwork/lnd/issues/1243
* https://github.com/lightningnetwork/lnd/pull/1355

### LNDMON

[LNDMON](https://github.com/lightninglabs/lndmon) is a drop-in monitoring solution for your lnd node using Prometheus and Grafana.

See the [blog post](https://blog.lightning.engineering/posts/2019/07/24/lndmon-v0.1.html) for more details.

## Splunk

To-do: Retrieval of information and input into [Splunk](https://www.splunk.com)

### Splunk: get-lnd-stats

Periodically query the lnd daemon for stats, then ship it off to Splunk 

[Splunk: get-lnd-stats](https://gist.github.com/tyzbit/0d5e54f7f560a043675963c3e7517550)

## Bash to CSV

The following early guides for monitoring in Bash to CSV files may be useful to expand on.
* [monitor lnd wallet and channel balances](https://gist.github.com/bretton/3248f76e89cc1579da8ce98dab0f8e92)
* [improved monitor for lnd wallet and channel balances and more](https://gist.github.com/tyzbit/ee3d07ec5a1c80379dcb2d9ec24c54f5)