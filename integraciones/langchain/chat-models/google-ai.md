# ChatGoogleGenerativeAI

## Prerequisitos

1. Registra una cuenta de [Google](https://accounts.google.com/InteractiveLogin)
2. Crea una [API key](https://aistudio.google.com/app/apikey)

## Configuración

1. **Chat Models** > arrastra el nodo **ChatGoogleGenerativeAI**

<figure><img src="../../../.gitbook/assets/google_ai/1.png" alt="" width="563"><figcaption></figcaption></figure>

2. **Connect Credential** > haz clic en **Create New**

<figure><img src="../../../.gitbook/assets/google_ai/2.png" alt="" width="278"><figcaption></figcaption></figure>

3. Completa la credencial de **Google AI**

<figure><img src="../../../.gitbook/assets/google_ai/3.png" alt="" width="563"><figcaption></figcaption></figure>

4. ¡Voilà [🎉](https://emojipedia.org/party-popper/), ahora puedes usar el **nodo ChatGoogleGenerativeAI** en Flowise

<figure><img src="../../../.gitbook/assets/google_ai/4.png" alt=""><figcaption></figcaption></figure>

## Configuración de Atributos de Seguridad

1. Haz clic en **Additonal Parameters**

<figure><img src="../../../.gitbook/assets/google_ai/5.png" alt="" width="563"><figcaption></figcaption></figure>

* Al configurar los **Safety Attributes**, la cantidad de selecciones en **Harm Category** y **Harm Block Threshold** debe ser la misma. Si no lo es, se mostrará el error `Harm Category & Harm Block Threshold are not the same length`

* La combinación de **Safety Attributes** que se muestra a continuación resultará en que `Dangerous` se configure como `Low and Above` y `Harassment` como `Medium and Above`

<figure><img src="../../../.gitbook/assets/google_ai/6.png" alt="" width="563"><figcaption></figcaption></figure>

## Recursos

* [LangChain JS ChatGoogleGenerativeAI](https://js.langchain.com/docs/integrations/chat/google_generativeai)
* [Google AI for Developers](https://ai.google.dev/)
* [Documentación de la API de Gemini](https://ai.google.dev/docs)