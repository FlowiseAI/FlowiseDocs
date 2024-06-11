---
description: Learn how Flowise collects anonymous app usage information
---

# Telemetry

***

Flowise open source repository has a built-in telemetry that collects anonymous usage information. This helps us to better understand usage of Flowise, enabling us to prioritize our efforts towards developing new features and resolving issues, and enhancing the performance and stability of Flowise.

{% hint style="warning" %}
<mark style="color:red;">**Important**</mark> - We never collect any confidential information about the node input/output, messages, or any sort of credentials and variables. Only events are being sent.
{% endhint %}

You can verify these claims by finding all locations `telemetry.sendTelemetry` is called from the source code.

<table><thead><tr><th width="238">Event</th><th>Metadata</th></tr></thead><tbody><tr><td>chatflow_created</td><td><pre class="language-json"><code class="lang-json">{
    "version": &#x3C;app-version>,
    "chatlowId": &#x3C;chatflow-id>,
    "flowGraph": {
        "nodes": [&#x3C;nodeid-1>, &#x3C;nodeid-2>],
        "edges": [
            {
                "source": &#x3C;nodeid-1>,
                "target": &#x3C;nodeid-2>
            }
        ]
    }
}
</code></pre></td></tr><tr><td>tool_created</td><td><pre class="language-json"><code class="lang-json">{
    "version": &#x3C;app-version>,
    "toolId": &#x3C;tool-id>,
    "toolName": &#x3C;tool-name>
}
</code></pre></td></tr><tr><td>assistant_created</td><td><pre class="language-json"><code class="lang-json">{
    "version": &#x3C;app-version>,
    "assistantId": &#x3C;assistant-id>
}
</code></pre></td></tr><tr><td>vector_upserted</td><td><pre class="language-json"><code class="lang-json">{
    "version": &#x3C;app-version>,
    "chatlowId": &#x3C;chatflow-id>,
    "type": "INTERNAL", // EXTERNAL
    "flowGraph": {
        "nodes": [&#x3C;nodeid-1>, &#x3C;nodeid-2>],
        "edges": [
            {
                "source": &#x3C;nodeid-1>,
                "target": &#x3C;nodeid-2>
            }
        ]
    },
    "stopNodeId": &#x3C;nodeid-1>
}
</code></pre></td></tr><tr><td>prediction_sent</td><td><pre class="language-json"><code class="lang-json">{
    "version": &#x3C;app-version>,
    "chatlowId": &#x3C;chatflow-id>,
    "chatId": &#x3C;chat-id>,
    "type": "INTERNAL", // EXTERNAL
    "flowGraph": {
        "nodes": [&#x3C;nodeid-1>, &#x3C;nodeid-2>],
        "edges": [
            {
                "source": &#x3C;nodeid-1>,
                "target": &#x3C;nodeid-2>
            }
        ]
    }
}
</code></pre></td></tr></tbody></table>

## Disable Telemetry

Users can disable telemetry by setting `DISABLE_FLOWISE_TELEMETRY` to `true` in `.env` file.
