# Incorporation locale

## Configuration locale

[**LocalAI** ](https://github.com/go-skynet/LocalAI)est une API de repos de remplacement qui est compatible avec les spécifications de l'API OpenAI pour l'inférence locale. Il vous permet d'exécuter des LLM (et pas seulement) localement ou sur site avec le matériel de qualité grand public, prenant en charge plusieurs familles de modèles compatibles avec le format GGML.

Pour utiliser les incorporations locales dans Flowise, suivez les étapes ci-dessous:

1. ```bash
   git clone https://github.com/go-skynet/LocalAI
   ```
2. <pre class = "Language-Bash"> <code class = "Lang-bash"> <strong> cd localai
</strong> </code> </pre>
3. Localai fournit un[API endpoint](https://localai.io/api-endpoints/index.html#applying-a-model---modelsapply)Pour télécharger / installer le modèle. Dans cet exemple, nous allons utiliser le modèle Bert Embeddings:

<gigne> <img src = "../../../. GitBook / Assets / Image (27) (1) .png" alt = ""> <figcaption> </gigcaption> </gigust>

4. Dans le`/models`dossier, vous devriez pouvoir y voir le modèle téléchargé:

<gigne> <img src = "../../../. GitBook / Assets / Image (23) (1) (2) .png" alt = ""> <Figcaption> </ Figcaption> </gigne>

5. Vous pouvez maintenant tester les intégres:

```bash
curl http://localhost:8080/v1/embeddings -H "Content-Type: application/json" -d '{
    "input": "Test",
    "model": "text-embedding-ada-002"
  }'
```

6. La réponse devrait ressembler:

<gigne> <img src = "../../../. GitBook / Assets / Image (29) .png" alt = "" width = "375"> <Figcaption> </ Figcaption> </ Figure>

## Configuration de flux

Faites glisser et déposez un nouveau composant localembeddings sur toile:

<gigne> <img src = "../../../. Gitbook / Assets / Image (21) (1) (2) .png" alt = ""> <Figcaption> </gigcaption> </ Figure>

Remplissez les champs:

* ** Path de base **: L'URL de base de localai comme[http://localhost:8080/v1](http://localhost:8080/v1)
* ** Nom du modèle **: Le modèle que vous souhaitez utiliser. Notez que ce doit être à l'intérieur`/models`dossier du répertoire localai. Par exemple:`text-embedding-ada-002`

C'est ça! Pour plus d'informations, reportez-vous à Localai[docs](https://localai.io/models/index.html#embeddings-bert).
