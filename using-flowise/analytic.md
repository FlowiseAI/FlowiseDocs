---
description: Aprende cómo analizar y solucionar problemas en tus flujos de chat y flujos de agentes
---

# Analítica

***

Hay varios proveedores de analítica con los que Flowise se integra:

* [LunaryAI](https://lunary.ai/)
* [Langsmith](https://smith.langchain.com/)
* [Langfuse](https://langfuse.com/)
* [LangWatch](https://langwatch.ai/)

## Lunary

[Lunary](https://lunary.ai/) es una plataforma de monitoreo y análisis para chatbots LLM.

Flowise se ha asociado con Lunary para proporcionar una integración completa que soporta seguimiento de usuarios, seguimiento de retroalimentación, repetición de conversaciones y análisis detallado de LLM.

Los usuarios de Flowise pueden obtener un 30% de descuento en el Plan Teams usando el código `FLOWISEFRIENDS` durante el pago.

Lee más sobre cómo configurar Lunary con Flowise [aquí](https://lunary.ai/docs/integrations/flowise).

## Configuración

1. En la esquina superior derecha de tu Flujo de Chat o Flujo de Agente, haz clic en **Configuración** > **Configuración**

<figure><img src="../.gitbook/assets/analytic-1.webp" alt="Captura de pantalla del usuario haciendo clic en el menú de configuración" width="375"><figcaption></figcaption></figure>

2. Luego ve a la sección Analizar Flujo de Chat

<figure><img src="../.gitbook/assets/analytic-2.png" alt="Captura de pantalla de la sección Analizar Flujo de Chat con los diferentes proveedores de Analítica"><figcaption></figcaption></figure>

3. Verás una lista de proveedores, junto con sus campos de configuración

<figure><img src="../.gitbook/assets/image (82).png" alt="Captura de pantalla de un proveedor de analítica con campos de credenciales expandidos"><figcaption></figcaption></figure>

3. Completa las credenciales y otros detalles de configuración, luego activa el proveedor en **ON**

<figure><img src="../.gitbook/assets/image (83).png" alt="Captura de pantalla de proveedores de analítica habilitados"><figcaption></figcaption></figure>

## API

Una vez que la analítica ha sido activada desde la interfaz de usuario, puedes sobrescribir o proporcionar configuración adicional en el cuerpo de la [API de Predicción](api.md#prediction-api):

```json
{
  "question": "hola",
  "overrideConfig": {
    "analytics": {
      "langFuse": {
        // langSmith, langFuse, lunary, langWatch
        "userId": "usuario1"
      }
    }
  }
}
```
