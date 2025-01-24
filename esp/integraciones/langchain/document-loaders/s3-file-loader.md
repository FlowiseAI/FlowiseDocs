# S3 File Loader

S3 File Loader te permite recuperar un archivo de S3 y usar [Unstructured](https://unstructured.io/) para preprocesarlo en un objeto Document estructurado que está listo para ser convertido en embeddings vectoriales. Unstructured se utiliza para manejar una amplia gama de diferentes tipos de archivos. Independientemente de si tu archivo en S3 es PDF, XML, DOCX, CSV, puede ser procesado por Unstructured. Ver [aquí](https://unstructured-io.github.io/unstructured/api.html#supported-file-types) los tipos de archivos soportados.

## Configuración de Unstructured

Puedes usar la API alojada o ejecutarla localmente a través de Docker.

* [API Alojada](https://unstructured-io.github.io/unstructured/api.html)
* Docker: `docker run -p 8000:8000 -d --rm --name unstructured-api quay.io/unstructured-io/unstructured-api:latest --port 8000 --host 0.0.0.0`

## Configuración de S3 File Loader

1\. Arrastra y suelta el cargador de archivos S3 en el lienzo:

<figure><img src="../../../.gitbook/assets/image (71).png" alt="" width="234"><figcaption></figcaption></figure>

2\. Credencial AWS: Crea una nueva credencial para tu cuenta AWS. Necesitarás la clave de acceso y la clave secreta. Recuerda otorgar la política de bucket S3 a la cuenta asociada. Puedes consultar la guía de políticas [aquí](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/AuroraMySQL.Integrating.Authorizing.IAM.S3CreatePolicy.html).

<figure><img src="../../../.gitbook/assets/image (72).png" alt="" width="551"><figcaption></figcaption></figure>

3. Bucket: Inicia sesión en tu consola AWS y navega a S3. Obtén el nombre de tu bucket:&#x20;

<figure><img src="../../../.gitbook/assets/image (73).png" alt=""><figcaption></figcaption></figure>

4. Key: Haz clic en el objeto que deseas usar y obtén el nombre de la Key:

<figure><img src="../../../.gitbook/assets/image (75).png" alt="" width="228"><figcaption></figcaption></figure>

5. URL de la API de Unstructured: Dependiendo de cómo estés usando Unstructured, ya sea a través de la API Alojada o Docker, cambia el parámetro de URL de la API de Unstructured. Si estás usando la API Alojada, también necesitarás la clave de API.
6. Luego puedes comenzar a chatear con tu archivo desde S3. No necesitas especificar el text splitter para dividir el documento porque eso es manejado automáticamente por Unstructured.

<figure><img src="../../../.gitbook/assets/screely-1698767992182.png" alt=""><figcaption></figcaption></figure>

