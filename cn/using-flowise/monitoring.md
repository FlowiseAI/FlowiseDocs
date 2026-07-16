# 监控

Flowise 通过 Grafana 和 OpenTelemetry 对 Prometheus 提供本机支持。但是，仅跟踪高级指标，例如 API 请求、流量/预测计数。有关计数器指标列表，请参阅[此处](https://github.com/FlowiseAI/Flowise/blob/main/packages/server/src/Interface.Metrics.ts#L13)。有关逐个节点可观察性的详细信息，我们建议使用 [分析](/broken/pages/z1V6RsbL6q6hrrswC3e9)。

## 普罗米修斯

[Prometheus](https://prometheus.io/) 是一种开源监控和警报解决方案。

在设置 Prometheus 之前，请在 Flowise 中配置以下环境变量：

```properties
ENABLE_METRICS=true
METRICS_PROVIDER=prometheus
METRICS_INCLUDE_NODE_METRICS=true
```

### 身份验证设置

`/api/v1/metrics` 端点需要 API 密钥身份验证。您需要：

1. 按照[此处](https://docs.flowiseai.com/configuration/authorization/chatflow-level#api-key)的说明生成API密钥
2. 将 API 密钥保存到 Prometheus 可访问的文件中（例如 `/etc/prometheus/api_key.txt`）
3. 配置 Prometheus 使用不记名令牌身份验证

### 普罗米修斯配置

Prometheus 安装后，使用配置文件运行它。 Flowise 提供了一个默认配置文件，您可以在[此处](https://github.com/FlowiseAI/Flowise/blob/main/metrics/prometheus/prometheus.config.yml)找到该文件。

您需要将身份验证配置添加到 Prometheus 配置文件中：

```yaml
scrape_configs:
  - job_name: 'flowise'
    static_configs:
      - targets: ['localhost:3000']
    metrics_path: '/api/v1/metrics'
    authorization:
      type: Bearer
      credentials_file: '/etc/prometheus/api_key.txt'
```

请记住同时运行 Flowise 实例。您可以打开浏览器并导航到端口 9090。从仪表板中，您应该能够看到指标端点 - `/api/v1/metrics` 现已启用身份验证。

<figure><img src="../.gitbook/assets/image (178).png" alt=""><figcaption></figcaption></figure>

`/api/v1/metrics` 端点可供 Prometheus 从中提取指标，但需要 API 密钥身份验证（如上面配置）。

## 格拉法纳

Prometheus 收集丰富的指标并提供强大的查询语言； Grafana 将指标转化为有意义的可视化。

Grafana 可以通过多种方式安装。请参阅[指南](https://grafana.com/docs/grafana/latest/setup-grafana/installation/)。

Grafana 默认情况下会公开端口 9091：

<figure><img src="../.gitbook/assets/image (179).png" alt=""><figcaption></figcaption></figure>

在左侧栏单击“添加新连接”，然后选择“Prometheus”：

<figure><img src="../.gitbook/assets/image (180).png" alt=""><figcaption></figcaption></figure>

由于我们的 Prometheus 服务于端口 9090：

<figure><img src="../.gitbook/assets/image (181).png" alt=""><figcaption></figcaption></figure>

滚动到底部并测试连接：

<figure><img src="../.gitbook/assets/image (182).png" alt=""><figcaption></figcaption></figure>

记下工具栏中显示的数据源 ID，我们需要它来创建仪表板：

<figure><img src="../.gitbook/assets/image (184).png" alt=""><figcaption></figcaption></figure>

现在连接已成功添加，我们可以开始添加仪表板。在左侧栏中，单击仪表板，然后单击创建仪表板。

Flowise 提供 2 个模板仪表板：

* [grafana.dashboard.app.json.txt](https://github.com/FlowiseAI/Flowise/blob/main/metrics/grafana/grafana.dashboard.app.json.txt)：API 指标，例如聊天流/智能体流程的数量、预测计数、工具、助手、写入更新的向量等。
* [grafana.dashboard.server.json.txt](https://github.com/FlowiseAI/Flowise/blob/main/metrics/grafana/grafana.dashboard.server.json.txt)：Flowise Node.js 实例的指标，例如堆、CPU、RAM 使用情况

如果您使用上述模板，请查找所有出现的 `cds4j1ybfuhogb` 并将其替换为您之前创建并保存的数据源 ID。

<figure><img src="../.gitbook/assets/image (183).png" alt=""><figcaption></figcaption></figure>

您还可以选择先导入，然后再编辑 JSON：

<figure><img src="../.gitbook/assets/image (185).png" alt=""><figcaption></figcaption></figure>

现在，尝试在 Flowise 上执行一些操作，您应该能够看到显示的指标：

<figure><img src="../.gitbook/assets/image (186).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (187).png" alt=""><figcaption></figcaption></figure>

## 开放遥测

[OpenTelemetry](https://opentelemetry.io/) 是一个用于创建和管理遥测数据的开源框架。要启用 OTel，请在 Flowise 中配置以下环境变量：

```properties
ENABLE_METRICS=true
METRICS_PROVIDER=open_telemetry
METRICS_INCLUDE_NODE_METRICS=true
METRICS_OPEN_TELEMETRY_METRIC_ENDPOINT=http://localhost:4318/v1/metrics
METRICS_OPEN_TELEMETRY_PROTOCOL=http # http | grpc | proto (default is http)
METRICS_OPEN_TELEMETRY_DEBUG=true
```

接下来，我们需要 OpenTelemetry Collector 来接收、处理和导出遥测数据。 Flowise 提供了 [docker compose 文件](https://github.com/FlowiseAI/Flowise/blob/main/metrics/otel/compose.yaml)，可用于启动收集器容器。

```bash
cd Flowise
cd metrics && cd otel
docker compose up -d
```

收集器将使用同一目录下的 [otel.config.yml](https://github.com/FlowiseAI/Flowise/blob/main/metrics/otel/otel.config.yml) 文件进行配置。目前仅支持[Datadog](https://www.datadoghq.com/)和Prometheus，请参阅[Open Telemetry](https://opentelemetry.io/)文档来配置不同的APM工具，例如Zipkin、Jeager、New Relic、Splunk等。

确保替换为 yml 文件中导出器所需的 API 键。
