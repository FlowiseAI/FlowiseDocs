# IBM Watsonx

## Prerequisitos

1. Registra una cuenta en [IBM Watsonx](https://www.ibm.com/watsonx)
2. Crea un nuevo proyecto:

<figure><img src="../../../.gitbook/assets/image (238).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (239).png" alt=""><figcaption></figcaption></figure>

3. Después de que el proyecto haya sido creado, regresa al panel principal y haz clic en **Explore foundation models**:

<figure><img src="../../../.gitbook/assets/image (240).png" alt=""><figcaption></figcaption></figure>

4. Elige el modelo que te gustaría usar y ábrelo en Prompt Lab:

<figure><img src="../../../.gitbook/assets/image (241).png" alt=""><figcaption></figcaption></figure>

5. Desde la esquina superior derecha, haz clic en View Code:

<figure><img src="../../../.gitbook/assets/image (242).png" alt=""><figcaption></figcaption></figure>

6. Toma nota del parámetro `model_id` y `version`. En este caso, es `ibm/granite-3-8b-instruct,` y la versión es `2023-05-29`.
7. Haz clic en la barra de navegación del lado izquierdo y haz clic en Developer access

<figure><img src="../../../.gitbook/assets/image (243).png" alt="" width="308"><figcaption></figcaption></figure>

8. Toma nota de la `watsonx.ai URL`, `Project ID` y crea una nueva API key desde IBM Cloud Console.
9. En este punto, deberías tener la siguiente información:
   * Watsonx.ai URL
   * Project ID
   * API Key
   * Versión del modelo
   * ID del modelo

## Configuración

1. **Chat Models** > arrastra el nodo **ChatIBMWatsonx**

<figure><img src="../../../.gitbook/assets/image (244).png" alt="" width="306"><figcaption></figcaption></figure>

2. Completa el Modelo con el ID del Modelo anterior. Crea una Nueva Credencial y completa todos los detalles.

<figure><img src="../../../.gitbook/assets/image (245).png" alt="" width="419"><figcaption></figcaption></figure>

2. ¡Voilà [🎉](https://emojipedia.org/party-popper/), ahora puedes usar el **nodo ChatIBMWatsonx** en Flowise!

<figure><img src="../../../.gitbook/assets/image (246).png" alt=""><figcaption></figcaption></figure>
