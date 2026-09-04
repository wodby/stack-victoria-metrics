# VictoriaMetrics application stack for Kubernetes on Wodby

Deploy VictoriaMetrics applications on Kubernetes with Wodby.

This repository defines the Wodby stack manifests and default service
composition for VictoriaMetrics.

<!-- wodby:generated:start -->

## Stack contract

- [VictoriaMetrics stack on Wodby](https://wodby.com/stacks/victoria-metrics)
- [Browse Wodby application stacks](https://wodby.com/stacks)
- [Wodby stack documentation](https://wodby.com/docs/2.0/stacks/)
- [Stack manifest reference](https://wodby.com/docs/2.0/stacks/template/)

## Service definitions

- [VictoriaMetrics service](https://github.com/wodby/service-victoria-metrics)
- [Grafana service](https://github.com/wodby/service-grafana)

## What's included

| Component / service | Default configuration |
| --- | --- |
| VictoriaMetrics<br>`victoria-metrics` | required; enabled by default; volumes: `data` 20 GB |
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
