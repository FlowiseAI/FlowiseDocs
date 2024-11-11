# Monitoring

Flowise has native support for Prometheus with Grafana and OpenTelemetry. However, only high-level metrics such as API requests, counts of flows/predictions are tracked. Refer [here](https://github.com/FlowiseAI/Flowise/blob/main/packages/server/src/Interface.Metrics.ts#L13) for the lists of counter metrics. For details node by node observability, we recommend using [Analytic](analytic.md).

## Prometheus

[Prometheus](https://prometheus.io/) is an open-source monitoring and alerting solution.

Before setting up Prometheus, configure the following env variables in Flowise:

```properties
ENABLE_METRICS=true
METRICS_PROVIDER=prometheus
METRICS_INCLUDE_NODE_METRICS=true
```

After Prometheus is installed, run it using a configuration file. Flowise provides a default configuration file that can be found [here](https://github.com/FlowiseAI/Flowise/blob/main/metrics/prometheus/prometheus.config.yml).

Remember to have Flowise instance also running. You can open browser and navigate to port 9090. From the dashboard, you should be able to see the metric endpoint - `/api/v1/metrics` is now live.

<figure><img src="../.gitbook/assets/image (178).png" alt=""><figcaption></figcaption></figure>

By default, `/api/v1/metrics` is available for Prometheus to pull the metrics from.

<figure><img src="../.gitbook/assets/image (177).png" alt="" width="563"><figcaption></figcaption></figure>

## Grafana

Prometheus collects rich metrics and provides a powerful querying language; Grafana transforms metrics into meaningful visualizations.

Grafana can be installed in various ways. Refer to the [guide](https://grafana.com/docs/grafana/latest/setup-grafana/installation/).

Grafana by default will expose port 9091:

<figure><img src="../.gitbook/assets/image (179).png" alt=""><figcaption></figcaption></figure>

On the left side bar, click Add new connection, and select Prometheus:

<figure><img src="../.gitbook/assets/image (180).png" alt=""><figcaption></figcaption></figure>

Since our Prometheus is serving at port 9090:

<figure><img src="../.gitbook/assets/image (181).png" alt=""><figcaption></figcaption></figure>

Scroll to the bottom and test the connection:

<figure><img src="../.gitbook/assets/image (182).png" alt=""><figcaption></figcaption></figure>

Take note of the data source ID shown in the toolbar, we'll need this for creating dashboards:

<figure><img src="../.gitbook/assets/image (184).png" alt=""><figcaption></figcaption></figure>

Now that connection is added successfully, we can start adding dashboard. From the left side bar, click Dashboards, and Create Dashboard.

Flowise provides 2 template dashboards:

* [grafana.dashboard.app.json.txt](https://github.com/FlowiseAI/Flowise/blob/main/metrics/grafana/grafana.dashboard.app.json.txt): API metrics such as number of chatflows/agentflows, predictions count, tools, assistant, upserted vectors, etc.
* [grafana.dashboard.server.json.txt](https://github.com/FlowiseAI/Flowise/blob/main/metrics/grafana/grafana.dashboard.server.json.txt): metrics of the Flowise node.js instance such as heap, CPU, RAM usage

If you are using templates above, find and replace all occurence of `cds4j1ybfuhogb` with the data source ID you created and saved earlier.

<figure><img src="../.gitbook/assets/image (183).png" alt=""><figcaption></figcaption></figure>

You can also choose to import first then edit the JSON later:

<figure><img src="../.gitbook/assets/image (185).png" alt=""><figcaption></figcaption></figure>

Now, try to perform some actions on the Flowise, you should be able to see the metrics displayed:

<figure><img src="../.gitbook/assets/image (186).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (187).png" alt=""><figcaption></figcaption></figure>

## OpenTelemetry

[OpenTelemetry](https://opentelemetry.io/) is an open source framework for creating and managing telemetry data. To enable OTel, configure the following env variables in Flowise:

```properties
ENABLE_METRICS=true
METRICS_PROVIDER=open_telemetry
METRICS_INCLUDE_NODE_METRICS=true
METRICS_OPEN_TELEMETRY_METRIC_ENDPOINT=http://localhost:4318/v1/metrics
METRICS_OPEN_TELEMETRY_PROTOCOL=http # http | grpc | proto (default is http)
METRICS_OPEN_TELEMETRY_DEBUG=true
```

Next, we need OpenTelemetry Collector to receive, process and export telemetry data. Flowise provides a [docker compose file](https://github.com/FlowiseAI/Flowise/blob/main/metrics/otel/compose.yaml) which can be used to start the collector container.

```bash
cd Flowise
cd metrics && cd otel
docker compose up -d
```

The collector will be using the [otel.config.yml](https://github.com/FlowiseAI/Flowise/blob/main/metrics/otel/otel.config.yml) file under the same directory for configurations. Currently only [Datadog](https://www.datadoghq.com/) and Prometheus are supported, refer to the [Open Telemetry](https://opentelemetry.io/) documentation to configure different APM tools such as Zipkin, Jeager, New Relic, Splunk and others.

Make sure to replace with the necessary API key for the exporters within the yml file.

