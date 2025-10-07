---
description: Learn how to query multiple documents correctly
---

# Plusieurs documents QNA

***

Du dernier[Web Scrape QnA](web-scrape-qna.md)Exemple, nous ne faisons que relancer et interroger 1 site Web. Et si nous avons plusieurs sites Web ou plusieurs documents? Jetons un coup d'œil et voyons comment nous pouvons y parvenir.

Dans cet exemple, nous allons effectuer QNA sur 2 PDF, qui sont des formulaires-10k d'Apple et Tesla.

<div align = "Left" data-full-width = "false"> <stiguh width = "375"> <Figcaption> </gigcaption> </gigust> </div>

## Ascension

1. Trouvez l'exemple de flux appelé - ** Chaîne de QA de récupération conversationnelle ** à partir des modèles de marché.
2. Nous allons utiliser[PDF File Loader](../integrations/langchain/document-loaders/pdf-file.md)et télécharger les fichiers respectifs:

<gigne> <img src = "../. GitBook / Assets / Multi-Docs-upload.png" alt = ""> <Figcaption> </gigcaption> </gigust>

3. Cliquez sur ** Paramètres supplémentaires ** du chargeur de fichiers PDF et spécifiez l'objet de métadonnées. Par exemple, le fichier PDF avec Apple Form-10K téléchargé peut avoir un objet de métadonnées`{source: apple}`, tandis que le fichier PDF avec Tesla Form-10K téléchargé peut avoir`{source: tesla}`. Ceci est fait pour Seggregate les documents pendant l'heure de récupération.

<div align = "Left"> <Figure> <img src = "../. GitBook / Assets / Multi-Docs-apple.png" alt = "" width = "563"> <Figcaption> </ Figcaption> </ Figure> <figure> <img src = "../. GitBook / Assets / Multi-Docs-Tesla. width = "563"> <Figcaption> </gigcaption> </gigust> </div>

4. Après avoir rempli les informations d'identification pour Pinecone, cliquez sur Upsert:

<Figure> <img src = "../. GitBook / Assets / Multi-Docs-UpSersert.png" alt = ""> <FigCaption> </Figcaption> </gigne>

<gigne> <img src = "../. GitBook / Assets / Image (98) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

5. Sur[Pinecone console](https://app.pinecone.io)Vous pourrez voir les nouveaux vecteurs qui ont été ajoutés.

<gigne> <img src = "../. GitBook / Assets / Multi-Docs-Console.png" Alt = ""> <Figcaption> </Figcaption> </Figge>

## Requête

1. Une fois que les données ont été renvoyées sur Pinecone, nous pouvons maintenant commencer à poser une question dans le chat!

<gigne> <img src = "../. GitBook / Assets / Image (100) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

2. Cependant, le contexte récupéré utilisé pour retourner la réponse est un mélange de documents Apple et Tesla. Comme vous pouvez le voir sur les documents source:

<div align = "Left"> <Figure> <img src = "../. GitBook / Assets / Untitled (7) .png" alt = "" width = "563"> <figcaption> </gigcaption> </gigu width = "563"> <Figcaption> </gigcaption> </gigust> </div>

3. Nous pouvons résoudre ce problème en spécifiant un filtre de métadonnées à partir du nœud de pinone. Par exemple, si nous voulons seulement récupérer le contexte d'Apple Form-10k, nous pouvons regarder en arrière les métadonnées que nous avons spécifiées plus tôt dans le[#upsert](multiple-documents-qna.md#upsert "mention")étape, puis utilisez la même chose dans le filtre des métadonnées ci-dessous:

<gigne> <img src = "../. GitBook / Assets / image (102) .png" alt = ""> <Figcaption> </ Figcaption> </gigne>

4. Posons à nouveau la même question, nous devrions maintenant voir tout le contexte récupéré provient en effet d'Apple Form-10k:

<gigne> <img src = "../. GitBook / Assets / Image (103) .png" alt = ""> <figcaption> </gigcaption> </gigust>

{% hint style = "info"%}
Chaque fournisseur de données vectorielle a un format différent de syntaxe de filtrage, recommande de lire la documentation de la base de données vectorielle respective
{% EndHint%}

5. Cependant, le problème avec cela est que le filtrage des métadonnées est une sorte de _ ** "codé dur" ** _. Idéalement, nous devons laisser le LLM décider quel document récupérer en fonction de la question.

## Agent d'outils

Nous pouvons résoudre le problème du filtre de métadonnées _ ** "codé dur" ** _ en utilisant[Tool Agent](../integrations/langchain/agents/tool-agent.md).

En fournissant des outils à l'agent, nous pouvons laisser l'agent à décider quel outil est approprié à utiliser en fonction de la question.

1. Créer un[Retriever Tool](../integrations/langchain/tools/retriever-tool.md)avec le nom et la description suivants:

<Bile> <Thead> <Tr> <th width = "178"> name </th> <th> Description </th> </tr> </thead> <tbody> <tr> <td> search_apple </td> <td> Utilisez cette fonction pour répondre aux questions des utilisateurs sur Apple Inc (Appl). Il contient un dossier de formulaire SEC 10K décrivant les finances d'Apple Inc (Appl) pour la période de 2022. </td> </tr> </tbody> </s table>

2. Connectez-vous au nœud de pignon avec un filtre de métadonnées`{source: apple}`

<gigne> <img src = "../. GitBook / Assets / Image (104) .png" alt = "" width = "563"> <Figcaption> </gigcaption> </ Figure>

3. Répétez la même chose pour Tesla:

<Bile> <Thead> <tr> <th width = "175"> name </th> <th width = "322"> Description </th> <th> PineCone Metadata Filter </th> </tr> </head> <tbody> <tr> <td> Search_tsla </td> <td> Utiliser cette fonction pour répondre aux questions d'utilisation sur Tesla Inc (Tsla). Il contient un dossier SEC 10K décrivant les finances de Tesla Inc (TSLA) pour la période de 2022. </td> <td> <code> {source: Tesla} </code> </td> </tr> </tbody> </ table>

{% hint style = "info"%}
Il est important de spécifier une description claire et concise. Cela permet à LLM de mieux décider quand utiliser quel outil
{% EndHint%}

Votre flux devrait ressembler ci-dessous:

<gigne> <img src = "../. GitBook / Assets / Image (154) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

4. Maintenant, nous devons créer une instruction générale à l'agent d'outils. Cliquez sur ** Paramètres supplémentaires ** du nœud et spécifiez le ** Message système **. Par exemple:

```
You are an expert financial analyst that always answers questions with the most relevant information using the tools at your disposal.
These tools have information regarding companies that the user has expressed interest in.
Here are some guidelines that you must follow:
* For financial questions, you must use the tools to find the answer and then write a response.
* Even if it seems like your tools won't be able to answer the question, you must still use them to find the most relevant information and insights. Not using them will appear as if you are not doing your job.
* You may assume that the users financial questions are related to the documents they've selected.
* For any user message that isn't related to financial analysis, respectfully decline to respond and suggest that the user ask a relevant question.
* If your tools are unable to find an answer, you should say that you haven't found an answer but still relay any useful information the tools found.
* Dont ask clarifying questions, just return answer.

The tools at your disposal have access to the following SEC documents that the user has selected to discuss with you:
- Apple Inc (APPL) FORM 10K 2022
- Tesla Inc (TSLA) FORM 10K 2022

The current date is: 2024-01-28
```

5. Enregistrez le chat et commencez à poser une question!

<gigne> <img src = "../. GitBook / Assets / Image (110) .png" alt = ""> <Figcaption> </ Figcaption> </gigne>

<div align = "Left"> <gigne> <img src = "../. GitBook / Assets / Untitled (9) .png" alt = "" width = "375"> <figcaption> </gigcaption> </gigu width = "375"> <Figcaption> </gigcaption> </gigust> </div>

6. Suivi avec Tesla:

<gigne> <img src = "../. GitBook / Assets / Image (111) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

7. Nous sommes maintenant en mesure de poser des questions sur tous les documents que nous avons précédemment renversés dans la base de données vectorielle sans "codage dur" le filtrage des métadonnées en utilisant des outils + agent.

## Retriever des métadonnées

Avec l'approche de l'agent d'outils, l'utilisateur doit créer plusieurs outils Retriever pour récupérer des documents à partir de différentes sources. Cela pourrait être un problème s'il existe un grand nombre de sources de documents avec différentes métadonnées. En utilisant l'exemple ci-dessus avec uniquement Apple et Tesla, nous pourrions potentiellement s'étendre à d'autres sociétés telles que Disney, Amazon, etc. Il serait une tâche fastidieuse de créer un outil de retrever pour chaque entreprise.

Metadata Retriever entre en jeu. L'idée est de demander à LLM d'extraire les métadonnées de la question de l'utilisateur, puis de l'utiliser comme filtre lors de la recherche dans les bases de données vectorielles.

Par exemple, si un utilisateur pose des questions liées à Apple, un filtre de métadonnées`{source: apple}`sera automatiquement appliqué sur la recherche de base de données vectorielle.

<div align = "Left"> <Figure> <img src = "../. GitBook / Assets / Image (235) .png" alt = "" width = "297"> <Figcaption> </ Figcaption> </ Figure> <Figure> <img src = "../. GitBook / Assets / Screenshot 2024-11-29 155926.png" alt = " width = "526"> <Figcaption> </gigcaption> </gigust> </div>

Dans ce scénario, nous pouvons avoir un seul outil Retriever et placer le ** Metadata Retriever ** entre la base de données vectorielle et l'outil Retriever.

<gigne> <img src = "../. GitBook / Assets / Image (236) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

## Agent XML

Pour certaines LLM, les capacités des appels de fonction ne sont pas prises en charge. Dans ce cas, nous pouvons utiliser l'agent XML pour inviter le LLM dans un format / syntaxe plus structuré, dans le but d'utiliser les outils fournis.

Il a l'invite sous-jacente:

```xml
You are a helpful assistant. Help the user answer any questions.

You have access to the following tools:

{tools}

In order to use a tool, you can use <tool></tool> and <tool_input></tool_input> tags. You will then get back a response in the form <observation></observation>
For example, if you have a tool called 'search' that could run a google search, in order to search for the weather in SF you would respond:

<tool>search</tool><tool_input>weather in SF</tool_input>
<observation>64 degrees</observation>

When you are done, respond with a final answer between <final_answer></final_answer>. For example:

<final_answer>The weather in SF is 64 degrees</final_answer>

Begin!

Previous Conversation:
{chat_history}

Question: {input}
{agent_scratchpad}
```

<gigne> <img src = "../. GitBook / Assets / Image (20) (1) (1) (1) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

## Conclusion

Nous avons couvert l'utilisation de la chaîne QA de récupération conversationnelle et sa limitation lors de l'interrogation de plusieurs documents. Et nous avons pu surmonter le problème en utilisant l'agent de fonction OpenAI / Agent XML + outils. Vous pouvez trouver les modèles ci-dessous:

{% fichier src = "../. gitbook / actifs / toolagent chatflow.json"%}

{% fichier src = "../. GitBook / Assets / xmlagent chatflow.json"%}
