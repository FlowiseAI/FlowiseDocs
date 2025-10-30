# ChatLocalAI

## Configuration de LocalAI

[**LocalAI** ](https://github.com/go-skynet/LocalAI) est une API REST de remplacement qui est compatible avec les spécifications de l'API OpenAI pour l'inférence locale. Elle vous permet d'exécuter des LLM (et pas seulement) localement ou sur site avec du matériel de consommation, prenant en charge plusieurs familles de modèles compatibles avec le format ggml.

Pour utiliser ChatLocalAI dans Flowise, suivez les étapes ci-dessous :

1. ```bash
   git clone https://github.com/go-skynet/LocalAI   ```
2. <pre class="language-bash"><code class="lang-bash"><strong>cd LocalAI
   </strong></code></pre>
3. ```bash
   # copy your models to models/
   cp your-model.bin models/
   ```

Pour exemple :

Téléchargez l'un des modèles depuis [gpt4all.io](https://gpt4all.io/index.html)

```bash
# Download gpt4all-j to models/
wget https://gpt4all.io/models/ggml-gpt4all-j.bin -O models/ggml-gpt4all-j
```
In le dossier `/models`, vous devriez pouvoir voir le modèle téléchargé :

<figure><img src="../../../.gitbook/assets/image (22) (1) (1).png" alt=""><figcaption></figcaption></figure>

Consultez [ici](https://localai.io/model-compatibility/index.html) pour la liste des modèles pris en charge.

4. ```bash
   docker compose up -d --pull always
```   ```
5. Now API is accessible at localhost:8080

```bash
# Test API
curl http://localhost:8080/v1/models
# {"object":"list","data":[{"id":"ggml-gpt4all-j.bin","object":"model"}]}
```

## Configuration de Flowise

Glissez-déposez un nouveau composant ChatLocalAI sur le canevas :

<figure><img src="../../../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

Remplissez les champs :

* **Chemin de base** : L'URL de base de LocalAI, comme [http://localhost:8080/v1](http://localhost:8080/v1)
* **Nom du modèle** : Le modèle que vous souhaitez utiliser. Notez qu'il doit se trouver dans le dossier `/models` du répertoire LocalAI. Par exemple : `ggml-gpt4all-j.bin`

{% hint style="info" %}
Si vous exécutez à la fois Flowise et LocalAI sur Docker, vous devrez peut-être changer le chemin de base en [http://host.docker.internal:8080/v1](http://host.docker.internal:8080/v1). Pour les systèmes basés sur Linux, la passerelle Docker par défaut doit être utilisée car host.docker.internal n'est pas disponible : [http://172.17.0.1:8080/v1](http://172.17.0.1:8080/v1)
{% endhint %}

C'est tout ! Pour plus d'informations, consultez la [documentation](https://localai.io/basics/getting_started/index.html) de LocalAI.

Regardez comment vous pouvez utiliser LocalAI sur Flowise

{% embed url="https://youtu.be/0B0oIs8NS9k" %}
