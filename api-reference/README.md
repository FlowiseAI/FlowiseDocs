# API Reference

Using Flowise public API, you can programmatically execute many of the same tasks as you can in the GUI. This section introduces Flowise REST API.

## Assistants

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/assistants" method="post" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger.yml" path="/assistants" method="get" %}
[swagger.yml](../.gitbook/assets/swagger.yml)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger.yml" path="/assistants/{id}" method="get" %}
[swagger.yml](../.gitbook/assets/swagger.yml)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger.yml" path="/assistants/{id}" method="put" %}
[swagger.yml](../.gitbook/assets/swagger.yml)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger.yml" path="/assistants/{id}" method="delete" %}
[swagger.yml](../.gitbook/assets/swagger.yml)
{% endswagger %}

## Chat Message

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/chatmessage/{id}" method="get" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/chatmessage/{id}" method="delete" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

## Chatflows

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/chatflows" method="get" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/chatflows" method="post" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/chatflows/{id}" method="get" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/chatflows/{id}" method="put" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/chatflows/{id}" method="delete" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/chatflows/apikey/{apikey}" method="get" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

## Document Store

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/document-store/store" method="post" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/document-store/store" method="get" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/document-store/store/{id}" method="get" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/document-store/store/{id}" method="put" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/document-store/store/{id}" method="delete" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/document-store/loader/preview" method="post" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/document-store/loader/process" method="post" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/document-store/vectorstore/save" method="post" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/document-store/vectorstore/insert" method="post" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/document-store/vectorstore/query" method="post" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/document-store/vectorstore/{id}" method="delete" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

## Feedback

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/feedback" method="post" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/feedback/{id}" method="get" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/feedback/{id}" method="put" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

## Leads

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/leads" method="post" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/leads/{id}" method="get" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

## Ping

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/ping" method="get" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

## Prediction

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/prediction/{id}" method="post" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

## Tools

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/tools" method="post" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/tools" method="get" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/tools/{id}" method="get" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/tools/{id}" method="put" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/tools/{id}" method="delete" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

## Upsert History

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/upsert-history/{id}" method="get" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/upsert-history/{id}" method="patch" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

## Variables

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/variables" method="post" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/variables" method="get" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/variables/{id}" method="put" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/variables/{id}" method="delete" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}

## Vector Upsert

{% swagger src="../.gitbook/assets/swagger (1).yml" path="/vector/upsert/{id}" method="post" %}
[swagger (1).yml](<../.gitbook/assets/swagger (1).yml>)
{% endswagger %}
