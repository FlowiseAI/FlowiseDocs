---
description: Agente utilizado para responder consultas en tablas de Airtable.
---

# Airtable Agent

<figure><img src="../../../.gitbook/assets/image_airtable.png" alt="" width="271"><figcaption><p>Node de Airtable Agent</p></figcaption></figure>

## Funcionalidad del Airtable Agent

El Airtable Agent está diseñado para facilitar las interacciones entre Flowise AI y las tablas de Airtable, permitiendo a los usuarios consultar datos de Airtable de manera conversacional. Al usar este agente, los usuarios pueden hacer preguntas sobre el contenido de su base de Airtable y recibir respuestas relevantes basadas en los datos almacenados. Esto puede ser particularmente útil para extraer rápidamente información específica, automatizar flujos de trabajo o generar resúmenes de los datos almacenados en Airtable.

Por ejemplo, el Airtable Agent puede usarse para responder preguntas como:

* "¿Cuántas tareas están aún incompletas en mi tabla de seguimiento de proyectos?"
* "¿Cuáles son los detalles de contacto de los clientes listados en el CRM?"
* "Dame un resumen de todos los registros añadidos en la última semana."

Esta funcionalidad ayuda a los usuarios a obtener información de sus bases de Airtable sin necesidad de navegar por la interfaz de Airtable, facilitando la gestión y análisis de sus datos de manera fluida e interactiva.

## Inputs

El Airtable Agent requiere los siguientes inputs para funcionar efectivamente:

* **Language Model**: El modelo de lenguaje que se utilizará para procesar las consultas. Este input es requerido y ayuda a determinar la calidad y precisión de las respuestas proporcionadas por el agente.
* **Input Moderation**: Input opcional que habilita la moderación de contenido. Esto ayuda a asegurar que las consultas sean apropiadas y no contengan contenido ofensivo o dañino.
* **Connect Credential**: Input requerido para conectarse a Airtable. Los usuarios deben seleccionar la credencial apropiada que tenga permisos para acceder a sus datos de Airtable.
* **Base ID**: El ID de la base de Airtable a la que conectarse. Este es un campo requerido y se puede encontrar en la documentación de la API de Airtable o en la configuración de la base. Si la URL de tu tabla se ve como `https://airtable.com/app11RobdGoX0YNsC/tblJdmvbrgizbYlCO/viw9UrP77idOCE4ee`, `app11RobdGoX0YNsC` es el Base ID. Se usa para especificar qué base de Airtable contiene los datos a consultar.
* **Table ID**: El ID de la tabla específica dentro de la base de Airtable. Este también es un campo requerido y ayuda al agente a dirigirse a la tabla correcta para la recuperación de datos. En el ejemplo de URL `https://airtable.com/app11RobdGoX0YNsC/tblJdmvbrgizbYlCO/viw9UrP77idOCE4ee`, `tblJdmvbrgizbYlCO` es el Table ID.
* **Additional Parameters**: Parámetros opcionales que pueden usarse para personalizar el comportamiento del agente. Estos parámetros pueden configurarse según casos de uso específicos.
  * **Return All**: Esta opción permite a los usuarios devolver todos los registros de la tabla especificada. Si está habilitada, se recuperarán todos los registros; de lo contrario, solo se devolverá un número limitado.
  * **Limit**: Especifica el número máximo de registros a devolver si **Return All** no está habilitado. El valor por defecto es `100`.

**Nota**: Esta sección está en desarrollo. Agradecemos cualquier ayuda que puedas proporcionar para completar esta sección. Por favor, consulta nuestra [Guía de Contribución](../../../contributing/) para comenzar.
