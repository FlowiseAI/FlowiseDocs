---
description: Aprende cómo Flowise recopila información anónima del uso de la aplicación
---

# Telemetría

***

El repositorio de código abierto de Flowise tiene una telemetría incorporada que recopila información anónima sobre el uso. Esto nos ayuda a comprender mejor el uso de Flowise, permitiéndonos priorizar nuestros esfuerzos hacia el desarrollo de nuevas funcionalidades y la resolución de problemas, y mejorar el rendimiento y la estabilidad de Flowise.

{% hint style="warning" %}
<mark style="color:red;">**Importante**</mark> - Nunca recopilamos ninguna información confidencial sobre la entrada/salida de los nodos, mensajes o cualquier tipo de credenciales y variables. Solo se envían eventos.
{% endhint %}

Puedes verificar estas afirmaciones encontrando todas las ubicaciones donde se llama a `telemetry.sendTelemetry` en el código fuente.

<table><thead><tr><th width="238">Evento</th><th>Metadatos</th></tr></thead><tbody><tr><td>chatflow_created</td><td><pre class="language-json"><code class="lang-json">{
    "version": &#x3C;versión-app>,
    "chatlowId": &#x3C;id-chatflow>,
    "flowGraph": {
        "nodes": [&#x3C;nodoid-1>, &#x3C;nodoid-2>],
        "edges": [
            {
                "source": &#x3C;nodoid-1>,
                "target": &#x3C;nodoid-2>
            }
        ]
    }
}
</code></pre></td></tr><tr><td>tool_created</td><td><pre class="language-json"><code class="lang-json">{
    "version": &#x3C;versión-app>,
    "toolId": &#x3C;id-herramienta>,
    "toolName": &#x3C;nombre-herramienta>
}
</code></pre></td></tr><tr><td>assistant_created</td><td><pre class="language-json"><code class="lang-json">{
    "version": &#x3C;versión-app>,
    "assistantId": &#x3C;id-asistente>
}
</code></pre></td></tr><tr><td>vector_upserted</td><td><pre class="language-json"><code class="lang-json">{
    "version": &#x3C;versión-app>,
    "chatlowId": &#x3C;id-chatflow>,
    "type": "INTERNAL", // EXTERNAL
    "flowGraph": {
        "nodes": [&#x3C;nodoid-1>, &#x3C;nodoid-2>],
        "edges": [
            {
                "source": &#x3C;nodoid-1>,
                "target": &#x3C;nodoid-2>
            }
        ]
    },
    "stopNodeId": &#x3C;nodoid-1>
}
</code></pre></td></tr><tr><td>prediction_sent</td><td><pre class="language-json"><code class="lang-json">{
    "version": &#x3C;versión-app>,
    "chatlowId": &#x3C;id-chatflow>,
    "chatId": &#x3C;id-chat>,
    "type": "INTERNAL", // EXTERNAL
    "flowGraph": {
        "nodes": [&#x3C;nodoid-1>, &#x3C;nodoid-2>],
        "edges": [
            {
                "source": &#x3C;nodoid-1>,
                "target": &#x3C;nodoid-2>
            }
        ]
    }
}
</code></pre></td></tr></tbody></table>

## Deshabilitar Telemetría

Los usuarios pueden deshabilitar la telemetría configurando `DISABLE_FLOWISE_TELEMETRY` como `true` en el archivo `.env`.
