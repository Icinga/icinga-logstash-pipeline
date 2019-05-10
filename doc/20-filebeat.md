# Filebeat configuration #

You need Filebeat installed on your Icinga hosts. The following Input will collect the logs.

```
- type: log
  enabled: true
  paths:
    - /var/log/icinga2/*log
  fields_under_root: true
  fields:
    program: icinga2
  multiline.pattern: '^\['
  multiline.negate: true
  multiline.match: after
```

Don't forget to deactivate (comment) the `elasticsearch` output in `filebeat.yml`. It's a very common mistake to connect the `elasticsearch` output to logstash which will connect but send broken data. Instead use the following:

```
output.logstash:
  enabled: true
  hosts: ["mylogstashhost:5044",]
  compression_level: 3
```
