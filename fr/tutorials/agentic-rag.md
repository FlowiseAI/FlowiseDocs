# Chiffon agentique

Le chiffon agentique est une approche basée sur des agents pour effectuer[RAG](rag.md)de manière orchestrée. Cela peut impliquer la récupération des données de diverses sources de documents, la comparaison des résumés et la mise en œuvre d'un mécanisme d'auto-correction automatique.

Dans ce didacticiel, nous explorerons comment créer un système de chiffon auto-corrigé qui vérifie la pertinence des données récupérées et re-génère automatiquement la requête si les résultats ne sont pas pertinents.

## Aperçu

Le flux de chiffon agentique implémente un processus en plusieurs étapes qui:

1. Valide et catégorise les requêtes entrantes
2. Génère des requêtes de recherche optimisées pour la récupération de la base de données vectorielle
3. Évalue la pertinence des documents récupérés
4. S'auto-correction en régénérant les requêtes lorsque les résultats ne sont pas pertinents
5. Fournit des réponses contextuelles basées sur des informations récupérées

<gigne> <img src = "../. Gitbook / Assets / image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) .png" alt = ""> <figcaption>

### Étape 1: Configuration du nœud de démarrage

Commencez par ajouter un nœud ** start ** à votre toile. Cela sert de point d'entrée pour le flux de votre agent.

<gigne> <img src = "../. Gitbook / Assets / image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "563"> <gigcaption> </ / figction> </ figure>

#### Configuration:

* ** Type d'entrée **: Sélectionnez "CHAT ENTRE" pour accepter les questions des utilisateurs
* ** État de flux **: Ajoutez une variable d'état avec la clé "`query`"Et une valeur vide

Le nœud de démarrage initialise l'état de débit avec un vide`query`variable qui sera mise à jour tout au long du processus.

### Étape 2: Ajout de validation de la requête

Ajoutez un nœud d'agent de condition ** ** et connectez-le au nœud de démarrage.

<gigne> <img src = "../. GitBook / Assets / Image (5) (1) (1) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "563"> <figcaption> </gigcaption> </pigu

#### Configuration:

* ** Instructions **: "Vérifiez si l'utilisateur pose des questions sur le sujet lié à l'IA, ou tout simplement une requête générale"
* **Saisir**:`{{ question }}`(référence à l'entrée de l'utilisateur)
* ** Scénarios **:
  * Scénario 1: "lié à l'IA"
  * Scénario 2: "Général"

Ce nœud agit comme un routeur, déterminant si la requête nécessite des connaissances spécialisées d'IA ou peut être répondue en général.

### Étape 3: Création de la branche de réponse générale

Pour les requêtes non liées à AI, ajoutez un nœud ** llm ** connecté à la sortie 1 de l'agent de condition.

<gigne> <img src = "../. Gitbook / Assets / image (7) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "375"> <figcaption> </gigcaption> </ figure>

Cela fournit des réponses directes pour les requêtes générales sans nécessiter de récupération de documents. Vous pouvez également remplacer par le nœud de réponse directe pour renvoyer une réponse prédéfinie.

<gigne> <img src = "../. GitBook / Assets / image (8) (1) (1) (1) (1) (1) .png" alt = "" width = "375"> <figCaption> </gigcaption> </gigu

### Étape 4: Configuration de la génération de requêtes

Pour les requêtes liées à l'AI, ajoutez un nœud ** llm ** connecté à la sortie 0 de l'agent de condition - qui est le scénario pour "lié à l'AI".

<gigne> <img src = "../. GitBook / Assets / Image (9) (1) (1) (1) (1) .png" alt = "" width = "563"> <figCaption> </ Figcaption> </gigne>

#### Configuration:

*   ** Messages **: Ajouter un message système:

    ```
    Given the user question and history, construct a short string that can be used for searching vector database. Only generate the query, no meta comments, no explanation

    Example:
    Question: what are the events happening today?
    Query: today's event

    Example:
    Question: how about the address?
    Query: business address of the shop

    Question: {{ question }}
    Query:
    ```
* ** Mettre à jour l'état de flux **: Définir la clé "Requête" avec une valeur`{{ output }}`. Cela mettra à jour la valeur de "Query" vers la sortie de ce nœud LLM.

Ce nœud transforme la question du langage naturel de l'utilisateur en une requête de recherche optimisée pour la base de données vectorielle.

### Étape 5: Configuration de la base de données vectorielle Retriever

Ajoutez un nœud ** Retriever ** et connectez-le à la "Generate Query" LLM.

<gigne> <img src = "../. GitBook / Assets / image (10) (1) (1) (1) (1) .png" alt = "" width = "563"> <Figcaption> </ Figcaption> </gigu

#### Configuration:

* ** Connaissances (magasins de documents) **: Sélectionnez votre magasin de documents préconfiguré (par exemple, "Papier AI")
* ** Retriever Query **:`{{ $flow.state.query }}`(utilise la valeur "requête" de l'état partagé)

Ce nœud recherche votre base de données vectorielle à l'aide de la requête optimisée et renvoie les documents pertinents.

### Étape 6: Ajouter la vérification de la pertinence du document

Ajoutez un autre nœud d'agent de condition ** ** connecté au retriever.

<gigne> <img src = "../. GitBook / Assets / image (11) (1) (1) (1) (1) .png" alt = "" width = "563"> <figCaption> </ Figcaption> </gigu

#### Configuration:

* ** Instructions **: "Déterminez si le document est pertinent pour la question de l'utilisateur. La question de l'utilisateur est \ {{question \}}"
* **Saisir**:`{{ retrieverAgentflow_0 }}`(Référence les documents récupérés de l'étape 5)
* ** Scénarios **:
  * Scénario 1: "pertinent"
  * Scénario 2: "hors de propos"

Cela évalue si les documents récupérés contiennent réellement des informations pertinentes pour la question de l'utilisateur.

### Étape 7: Création du générateur de réponse final

Pour les documents pertinents, ajoutez un nœud ** llm ** connecté à la sortie 0 du vérificateur de pertinence - c'est-à-dire lorsque le scénario "pertinent" est égalé.

<gigne> <img src = "../. GitBook / Assets / image (12) (1) (1) (1) (1) .png" alt = "" width = "373"> <Figcaption> </gigcaption> </gigu

#### Configuration:

*   ** Message d'entrée **:

    ```
    Given the question: {{ question }}
    And the findings: {{ retrieverAgentflow_0 }}
    Output the final response
    ```

Ce nœud crée la réponse finale en combinant la question de l'utilisateur avec les documents récupérés pertinents.

### Étape 8: Mise en œuvre de l'auto-correction

Pour les documents non pertinents, ajoutez un nœud ** llm ** connecté à la sortie 1 du vérificateur de pertinence - pour le deuxième scénario - "non pertinent".

<gigne> <img src = "../. GitBook / Assets / Image (13) (1) (1) (1) (1) .png" alt = "" width = "375"> <figCaption> </gigcaption> </gigne>

<gigne> <img src = "../. GitBook / Assets / Image (14) (1) (1) (1) (1) .png" alt = "" width = "563"> <figCaption> </ Figcaption> </ Figure>

#### Configuration:

* ** Messages **: Ajouter un message système: "Vous êtes un assistant utile qui peut transformer la requête pour produire une meilleure question."
*   ** Message d'entrée **:

    ```
    Look at the input and try to reason about the underlying semantic intent / meaning.
    Here is the initial question: {{ $flow.state.query }}
    Formulate an improved question:
    ```
* ** Mettre à jour l'état de flux **: Définir la clé "Requête" avec une valeur`{{ output }}`

<gigne> <img src = "../. GitBook / Assets / Image (15) (1) (1) (1) (1) .png" alt = "" width = "520"> <figCaption> </gigcaption> </stigne>

Ce nœud analyse pourquoi la requête initiale n'a pas renvoyé les résultats pertinents et génère une version améliorée.

### Étape 9: Ajout du mécanisme de la boucle

Ajoutez un nœud ** LOOP ** connecté à la "question de régénération" LLM.

<gigne> <img src = "../. GitBook / Assets / Image (16) (1) (1) (1) (1) .png" alt = "" width = "563"> <figCaption> </gigcaption> </gigu

#### Configuration:

* ** Loop Retour à **: Sélectionnez "RetrieverAgentFlow \ _0-Retriever Vector DB"
* ** Count de boucle maximale **: réglé sur 5 (empêche les boucles infinies)

Cela crée une boucle de rétroaction qui permet au système de réessayer avec des requêtes améliorées lorsque les résultats initiaux ne sont pas satisfaisants.

## Structure d'écoulement complète

{% fichier src = "../. gitbook / actifs / rag de l'agentique v2.json"%}

## Résumé

1. Démarrer → Vérifiez si la requête est valide
2. Vérifiez si la requête est valide (liée à l'IA) → Générer des requêtes
3. Vérifiez si la requête valide (générale) → Réponse générale
4. Générer la requête → Retriever Vector DB
5. Retriever Vector DB → Vérifiez si les documents pertinents
6. Vérifiez si les documents pertinents (pertinents) → Générer une réponse
7. Vérifiez si les documents pertinents (non pertinents) → Regérer la question
8. Régénérer Question → Loop Retour à Retriever

## Tester votre flux

Testez votre flux avec différents types de questions:

* Requêtes liées à l'IA: "Quels sont les derniers développements de l'apprentissage automatique?"
* Requêtes générales: "Quel temps fait-il aujourd'hui?"
* Des requêtes complexes qui pourraient nécessiter un raffinement: "Comment fonctionne cette nouvelle technique?"

<gigne> <img src = "../. GitBook / Assets / Image (17) (1) (1) .png" alt = "" width = "563"> <Figcaption> </figcaption> </ figure>

Ce chiffon agentique fournit un système robuste et auto-amélioré pour la réponse aux questions basée sur des documents qui peut gérer des requêtes simples et complexes tout en maintenant une grande précision grâce à un raffinement itératif.
