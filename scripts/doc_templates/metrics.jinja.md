# Errors and Metrics

## Error Reporting

By default, Relay logs errors to the configured logger. You can enable error
reporting to your own project in Sentry in the configuration file:

```yaml
sentry:
  enabled: true
  dsn: <your_dsn>
```

## Configuration

Stats can be submitted to a statsd server by configuring `metrics` key. It can
be put to a `ip:port` tuple:

```yaml
metrics:
  statsd: 127.0.0.1:8126
  prefix: mycompany.relay
```

### `statsd`

Configures the host and port of the statsd client. We recommend to run a statsd
client on the same host as Relay for performance reasons.

### `prefix`

Configures a prefix that is prepended to all reported metrics. By default, all
metrics are reported with a `"sentry.relay"` prefix.

### `hostname_tag`

Adds a tag of the given name and sets it to the hostname of the machine that is running Relay. This is useful to separate multiple Relays. For example, setting this to:

```yaml
metrics:
  hostname_tag: pod_name
```

The above example adds a `"pod_name"` tag that is set to the host name to every
metric.

## Metrics

Relay emits the following metrics:

{% for metric_type in all_metrics or []%}
{% for metric in metric_type.metrics or [] %}
### `{{metric.id}}`
{% if metric.feature %}
**Note**: This metric is emitted only when Relay is built with the `{{metric.feature}}` feature.
{% endif %}
_{{metric_type.human_name}}._
{% for line in metric.description or [] %}{{line}}
{% endfor %}
{% endfor %}
{% endfor %}
