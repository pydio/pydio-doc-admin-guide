When deployed inside an infrastructure providing monitoring dashboard and tools, Cells provides some specific entry points for grabbing statistics and healtcheck information. 

## Healthcheck Endpoint

Cells provides an optional HTTP endpoint that lists all micro-services and their statuses (in JSON format). If all services are up and running, endpoint returns with a 200 status code, and a 500 if one service is down.

To enable this endpoint, pass a port number via the `CELLS_HEALTHCHECK=6565` environment variable (or via the --healthcheck command flag). Once set, healthcheck endpoint is available with a `GET http://your-cells:6565/healthcheck/` request. Please **beware that this endpoint is not protected**: it is your responsibility to correctly setup your firewall to restrict its access.

## Prometheus / OpenMetrics

Cells code is instrumented with [Prometheus](https://prometheus.io/) gauges for providing metrics about its internals (Golang Goroutines number, memory used per service, etc.). It is best coupled with a Grafana monitoring dashboard.

Prerequisites to collect the metrics are the following:

- Enable the metrics endpoint on Cells by providing the `CELLS_ENABLE_METRICS=true` environment variable or by adding the flag `--enable_metrics` to cells' start command.
- [Prometheus](https://prometheus.io/)
  
and for the `prometheus.yml` configuration append this instruction.

```yaml
- job_name: "cells"
    file_sd_configs:
      - files:
          - ${CELLS_WORKING_DIR}/services/pydio.gateway.metrics/prom_clients.json
```

> Make sure to replace **$CELLS_WORKING_DIR** by your actual working directory.

[:image:2_running_cells_in_production/monitoring_tools/grafana_dashboard.png]

Please refer to the knowledge base for [more information about the Prometheus/Grafana](./kb/deployment/monitoring-cells-prometheus-grafana) configuration.