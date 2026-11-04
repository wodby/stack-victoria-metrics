# VictoriaMetrics application stack for Kubernetes on Wodby

Deploy VictoriaMetrics applications on Kubernetes with Wodby.

This repository defines the Wodby stack manifests and default service
composition for VictoriaMetrics.

<!-- wodby:generated:start -->

## Stack contract

- [VictoriaMetrics stack on Wodby](https://wodby.com/stacks/victoria-metrics)
- [Browse Wodby application stacks](https://wodby.com/stacks)
- [VictoriaMetrics stack guide](https://wodby.com/docs/2.0/stacks/catalog/victoria-metrics/)
- [Wodby stack documentation](https://wodby.com/docs/2.0/stacks/)
- [Stack manifest reference](https://wodby.com/docs/2.0/stacks/template/)

## Service definitions

- [VictoriaMetrics service](https://github.com/wodby/service-victoria-metrics)
- [OpenTelemetry Collector service](https://github.com/wodby/service-opentelemetry-collector)
- [Grafana service](https://github.com/wodby/service-grafana)

## What's included

| Component / service | Default configuration |
| --- | --- |
| VictoriaMetrics<br>`victoria-metrics` | required; enabled by default; volumes: `data` 20 GB |
| OpenTelemetry Collector<br>`otel-collector` | optional; enabled by default; links: `victoria-metrics` → `victoria-metrics` |
| Grafana<br>`grafana` | optional; enabled by default; volumes: `data` 10 GB; links: `victoria-metrics` → `victoria-metrics` |

Enabled optional services are selected by default but can be excluded when an
app is created. Disabled optional services are available but not selected by
default. Required services cannot be excluded.

## Validate the stack manifest

```bash
wodby stack validate-manifest stack.yml --org <org-id>
```

<!-- wodby:generated:end -->

## Grafana

Grafana is enabled by default and provides the stack's public interface. Its
VictoriaMetrics data source is provisioned automatically.

## Send metrics

Applications send metrics to the OpenTelemetry Collector over OTLP. Its HTTP
port has a public HTTPS domain, so applications in other environments, other
clusters or outside Wodby can use it. Every request needs the Collector's
generated `ingest_token`.

```
OTEL_EXPORTER_OTLP_ENDPOINT=https://<Collector domain>
OTEL_EXPORTER_OTLP_PROTOCOL=http/protobuf
OTEL_EXPORTER_OTLP_HEADERS=Authorization=Bearer%20<ingest token>
OTEL_SERVICE_NAME=<application name>
OTEL_METRICS_EXPORTER=otlp
OTEL_TRACES_EXPORTER=none
OTEL_LOGS_EXPORTER=none
```

The Collector accepts metrics only and forwards them to VictoriaMetrics'
OpenTelemetry endpoint. VictoriaMetrics keeps OpenTelemetry attribute names, so
queries use labels such as `{"service.name"="api"}`. The Collector is optional:
services in the same environment can still write to VictoriaMetrics directly.
