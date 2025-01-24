# Monitoreo

Flowise tiene soporte nativo para Prometheus con Grafana y OpenTelemetry. Sin embargo, solo se rastrean métricas de alto nivel como solicitudes de API, conteos de flujos/predicciones. Consulta [aquí](https://github.com/FlowiseAI/Flowise/blob/main/packages/server/src/Interface.Metrics.ts#L13) para ver la lista de métricas de contador. Para observabilidad detallada nodo por nodo, recomendamos usar [Analítica](analytic.md).

## Prometheus

[Prometheus](https://prometheus.io/) es una solución de monitoreo y alertas de código abierto.

Antes de configurar Prometheus, configura las siguientes variables de entorno en Flowise:

```properties
ENABLE_METRICS=true
METRICS_PROVIDER=prometheus
METRICS_INCLUDE_NODE_METRICS=true
```

Después de instalar Prometheus, ejecútalo usando un archivo de configuración. Flowise proporciona un archivo de configuración predeterminado que se puede encontrar [aquí](https://github.com/FlowiseAI/Flowise/blob/main/metrics/prometheus/prometheus.config.yml).

Recuerda tener la instancia de Flowise también en ejecución. Puedes abrir el navegador y navegar al puerto 9090. Desde el panel de control, deberías poder ver que el punto final de métricas - `/api/v1/metrics` está activo.

<figure><img src="../.gitbook/assets/image (178).png" alt=""><figcaption></figcaption></figure>

Por defecto, `/api/v1/metrics` está disponible para que Prometheus extraiga las métricas.

<figure><img src="../.gitbook/assets/image (177).png" alt="" width="563"><figcaption></figcaption></figure>

## Grafana

Prometheus recopila métricas detalladas y proporciona un lenguaje de consulta potente; Grafana transforma las métricas en visualizaciones significativas.

Grafana se puede instalar de varias maneras. Consulta la [guía](https://grafana.com/docs/grafana/latest/setup-grafana/installation/).

Grafana por defecto expondrá el puerto 9091:

<figure><img src="../.gitbook/assets/image (179).png" alt=""><figcaption></figcaption></figure>

En la barra lateral izquierda, haz clic en Agregar nueva conexión y selecciona Prometheus:

<figure><img src="../.gitbook/assets/image (180).png" alt=""><figcaption></figcaption></figure>

Ya que nuestro Prometheus está sirviendo en el puerto 9090:

<figure><img src="../.gitbook/assets/image (181).png" alt=""><figcaption></figcaption></figure>

Desplázate hasta el final y prueba la conexión:

<figure><img src="../.gitbook/assets/image (182).png" alt=""><figcaption></figcaption></figure>

Toma nota del ID de la fuente de datos mostrado en la barra de herramientas, lo necesitaremos para crear paneles:

<figure><img src="../.gitbook/assets/image (184).png" alt=""><figcaption></figcaption></figure>

Ahora que la conexión se ha agregado exitosamente, podemos comenzar a agregar paneles. Desde la barra lateral izquierda, haz clic en Paneles y Crear Panel.

Flowise proporciona 2 plantillas de paneles:

* [grafana.dashboard.app.json.txt](https://github.com/FlowiseAI/Flowise/blob/main/metrics/grafana/grafana.dashboard.app.json.txt): métricas de API como número de flujos de chat/agentes, conteo de predicciones, herramientas, asistentes, vectores insertados, etc.
* [grafana.dashboard.server.json.txt](https://github.com/FlowiseAI/Flowise/blob/main/metrics/grafana/grafana.dashboard.server.json.txt): métricas de la instancia node.js de Flowise como uso de heap, CPU, RAM

Si estás usando las plantillas anteriores, encuentra y reemplaza todas las ocurrencias de `cds4j1ybfuhogb` con el ID de la fuente de datos que creaste y guardaste anteriormente.

<figure><img src="../.gitbook/assets/image (183).png" alt=""><figcaption></figcaption></figure>

También puedes elegir importar primero y luego editar el JSON más tarde:

<figure><img src="../.gitbook/assets/image (185).png" alt=""><figcaption></figcaption></figure>

Ahora, intenta realizar algunas acciones en Flowise, deberías poder ver las métricas mostradas:

<figure><img src="../.gitbook/assets/image (186).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (187).png" alt=""><figcaption></figcaption></figure>

## OpenTelemetry

[OpenTelemetry](https://opentelemetry.io/) es un marco de trabajo de código abierto para crear y gestionar datos de telemetría. Para habilitar OTel, configura las siguientes variables de entorno en Flowise:

```properties
ENABLE_METRICS=true
METRICS_PROVIDER=open_telemetry
METRICS_INCLUDE_NODE_METRICS=true
METRICS_OPEN_TELEMETRY_METRIC_ENDPOINT=http://localhost:4318/v1/metrics
METRICS_OPEN_TELEMETRY_PROTOCOL=http # http | grpc | proto (por defecto es http)
METRICS_OPEN_TELEMETRY_DEBUG=true
```

A continuación, necesitamos el Recolector OpenTelemetry para recibir, procesar y exportar datos de telemetría. Flowise proporciona un [archivo docker compose](https://github.com/FlowiseAI/Flowise/blob/main/metrics/otel/compose.yaml) que se puede usar para iniciar el contenedor del recolector.

```bash
cd Flowise
cd metrics && cd otel
docker compose up -d
```

El recolector utilizará el archivo [otel.config.yml](https://github.com/FlowiseAI/Flowise/blob/main/metrics/otel/otel.config.yml) bajo el mismo directorio para las configuraciones. Actualmente solo [Datadog](https://www.datadoghq.com/) y Prometheus son soportados, consulta la documentación de [Open Telemetry](https://opentelemetry.io/) para configurar diferentes herramientas APM como Zipkin, Jeager, New Relic, Splunk y otros.

Asegúrate de reemplazar con la clave API necesaria para los exportadores dentro del archivo yml.

