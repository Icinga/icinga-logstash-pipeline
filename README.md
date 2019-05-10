# logstash-icinga
Logstash rules for Icinga logs

**These rules are totally work in progress. Use at your own risk.**

## Usage ##

These filters are intended to be used within their own pipeline in Logstash.
If you are not familiar with multi-pipeline setups, please refer to the [Logstash documentation](https://www.elastic.co/guide/en/logstash/current/multiple-pipelines.html).

For ease of use this pipeline will read from a Redis instance listening on localhost. It expects the Icinga logs to be found just as they were on disk in the key `icinga`.

After processing the pipeline places the parsed logs into the same Redis instance but in key `forwarder`. If you don't like this behaviour feel free to change the files `input.conf` and `output.conf`.

If you need a jumpstart, this docs show you a simple configuration for [Filebeat](doc/20-filebeat) and [Logstash](doc/10-logstash).
