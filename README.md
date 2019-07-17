# logstash-icinga
Logstash rules for Icinga logs

## Usage ##

These filters are intended to be used within their own pipeline in Logstash. They include input and output configuration to a local Redis instance with hard coded names for keys in which you should write and read all your Icinga logs. (More details below)

If you are not familiar with multi-pipeline setups, please refer to the [Logstash documentation](https://www.elastic.co/guide/en/logstash/current/multiple-pipelines.html).

For ease of use this pipeline will read from a Redis instance listening on localhost. It expects the Icinga logs to be found just as they were on disk in the key `icinga`.

After processing the pipeline places the parsed logs into the same Redis instance but in key `forwarder`. If you don't like this behaviour feel free to change the files `input.conf` and `output.conf`.

If you need a jumpstart, this docs show you a simple configuration for [Filebeat](doc/20-filebeat.md) and [Logstash](doc/10-logstash.md).

## Capabilities ##

The logs will be parsed and split into fields where we see a possible use. Field names are set according to Elastic Common Schema (ECS) wehere fit and stick to a nomenclature which should not interfere with your other field names. For details see the [docs](doc/doc/30-namingscheme.md). Short version: All fields which are not covered by ECS are subfields of the `icinga` field.

In the `dashboards` directory there are some sample dashboards you can use with this ruleset.

Every rule adds a tag and a field you can use to identify every known logevent. There is a global rule for adding the version of the ruleset, too.

## Status / Constributing ##

Icinga 2 is always changing and so are its logs. So we try to keep the rules as close to the set of possible logentries as possible but we might always be a bit behind the current version.

In fact, the first version is not complete but it should be a good starting point.

If you need more rules, feel free to change the files but please do send us a pull request so we can incorporate them so every use can benefit.
