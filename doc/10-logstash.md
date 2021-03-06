# Logstash configuration #

If you want a simple Logstash instance that just reveices logs from [filebeat](20-filebeat.md) and writes them to Elasticsearch you can use the following configuration.

Keep in mind that you're always free to build more configuration and more pipelines for different kinds of logs. This configuration should get you started but you are not restricted to use this Logstash instance just for Icinga.

## Pipelines ##

You need at least three pipelines to use this configuration. You can use this as an example for your `pipelines.yml`.

```
- pipeline.id: shipper
  path.config: "/etc/logstash/conf.d/shipper/*conf"
- pipeline.id: icinga
  path.config: "/etc/logstash/conf.d/icinga/*conf"
- pipeline.id: forwarder
  path.config: "/etc/logstash/conf.d/forwarder/*conf"
```

Clone this repository into `/etc/logstash/conf.d/icinga/*conf`

## Shipper ##

To receive the logs from [Filebeat](20-filebeat.md) you can use the following configuration in `/etc/logstash/conf.d/shipper/shipper.conf`.

```
input {
  beats {
    port => 5044
  }
}
output {
  redis {
    host => "localhost"
    data_type => "list"
    key => "icinga"
  }
}
```

## Forwarder ##

To forward the logs into Elasticsearch running on the same host like Logstash use the following lines in `/etc/logstash/conf.d/forwarder/forwarder.conf`.

```
input {
  redis {
    host => "localhost"
    data_type => "list"
    key => "forwarder"
  }
}
output {
  elasticsearch {
  }
}
```
