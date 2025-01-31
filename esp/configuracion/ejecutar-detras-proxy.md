# Ejecutando Flowise detrás de un proxy corporativo

Si estás ejecutando Flowise en un entorno que requiere un proxy, como dentro de una red organizacional, puedes configurar Flowise para enrutar todas sus solicitudes backend a través de un proxy de tu elección. Esta funcionalidad está impulsada por el paquete `global-agent`.

[https://github.com/gajus/global-agent](https://github.com/gajus/global-agent)

## Configuración

Hay 2 variables de entorno que necesitarás para ejecutar Flowise detrás de un proxy corporativo:

| Variable                   | Propósito                                                                          | Requerido |
| -------------------------- | -------------------------------------------------------------------------------- | -------- |
| `GLOBAL_AGENT_HTTP_PROXY`  | A través de dónde hacer proxy de todas las solicitudes HTTP del servidor          | Sí       |
| `GLOBAL_AGENT_HTTPS_PROXY` | A través de dónde hacer proxy de todas las solicitudes HTTPS del servidor         | No       |
| `GLOBAL_AGENT_NO_PROXY`    | Un patrón de URLs que deberían ser excluidas del proxy. Ej. `*.foo.com,baz.com`  | No       |

## Lista de permitidos de salida

Para el plan empresarial, debes permitir varias conexiones salientes para la verificación de licencia. Por favor, contacta a support@flowiseai.com para más información.
