# Humain dans la boucle

Dans les didacticiels précédents, nous avons exploré comment un agent peut utiliser dynamiquement des outils pour répondre aux requêtes ou effectuer des tâches assignées. ** Human-in-the-Loop ** Ajoute une couche de contrôle en permettant à l'agent de demander une entrée, une approbation ou une rétroaction humaine avant de continuer.

Il y a 2 façons dont l'homme dans la boucle peut être utilisé:

* En utilisant[Human Input](../using-flowise/agentflowv2.md#id-11.-human-input-node)nœud pour arrêter l'exécution
* Activer ** Exiger une entrée humaine ** pour les outils d'agent

## Nœud d'entrée humain

Le nœud ** entrée humaine ** permet de faire une pause exécution et de reprendre seulement après qu'un humain a fourni des commentaires pour approuver ou rejeter l'action.

Dans ce tutoriel, nous apprendrons comment créer un agent de réponse par e-mail automatisé qui demande des commentaires des utilisateurs avant d'envoyer l'e-mail.

### Aperçu

L'objectif de ce cas d'utilisation est de créer un système de réponse par e-mail intelligent qui:

1. Reçoit les demandes de courrier électronique entrantes
2. Génère des réponses par e-mail professionnelles à l'aide de l'IA
3. Demande l'approbation humaine avant d'envoyer
4. Permet des révisions et des améliorations
5. Envoie automatiquement l'e-mail approuvé

<gigne> <img src = "../. GitBook / Assets / Image (23) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

#### Étape 1: Configuration du nœud de démarrage

1. Faites glisser et déposez le nœud ** start ** sur la toile. Ce sera le point d'entrée pour les données de messagerie entrantes.
2. Configurez le nœud de démarrage avec les paramètres suivants:
   * ** Type d'entrée **: Sélectionnez "Form Entrée" pour capturer les données d'e-mail structurées
   * ** Titre du formulaire **: "Enquête par e-mail"
   * ** DESCRIPTION DU FORME **: "Investigation des e-mails entrants"
3. Ajouter les types d'entrée de formulaire suivants:
   * ** Sujet ** (String): Pour capturer la ligne d'objet de l'e-mail
   * ** Body ** (String): Pour capturer le contenu de l'e-mail
   * ** De ** (String): Pour capturer l'adresse e-mail de l'expéditeur

<gigne> <img src = "../. Gitbook / Assets / image (1) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "398"> <figcaption> </gigcaption> </gigu

#### Étape 2: Création de l'agent de réponse par e-mail

1. Ajoutez un nœud ** Agent ** et connectez-le au nœud de démarrage. Cet agent analysera l'e-mail entrant et générera une réponse appropriée.
2.  Ajouter un message système, par exemple:

    ```
    You are a customer support agent working in Flowise Inc. Write a professional email reply to user's query. Use the web search tools to get more details about the prospect.

    Always reply as Samantha, Customer Support Representative in Flowise. Don't use placeholders.
    ```
3. Ajoutez les outils suivants pour améliorer les capacités de l'agent:
   * ** Recherche personnalisée Google **: pour rechercher des informations sur les clients et fournir un contexte pertinent
   * ** DateTime actuel **: Inclure des horodatages précis dans les réponses

<gigne> <img src = "../. Gitbook / Assets / Image (2) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "563"> <Figcaption> </gigcaption> </gigu

#### Étape 3: Ajout de contribution humaine pour approbation

1. Ajoutez un nœud ** entrée humaine ** et connectez-le à l'agent de réponse par e-mail. Cela crée le point de contrôle humain dans la boucle.
2. Configurer le nœud d'entrée humain:
   * ** Type de description **: "Correction"
   * ** Description **: "Êtes-vous sûr de vouloir continuer?"
   * ** Activer les commentaires **: TRUE (permet aux humains de fournir des commentaires supplémentaires)
3. Ce nœud mettra en pause le flux de travail et présentera la réponse générée par l'AI à un examinateur humain. Le réviseur peut soit:
   * ** Continuez **: approuver la réponse et continuer à envoyer un e-mail
   * ** Rejeter **: envoyer des commentaires et remettre à l'agent pour les améliorations

<gigne> <img src = "../. GitBook / Assets / Image (3) (1) (1) (1) (1) (1) .png" alt = "" width = "563"> <figCaption> </gigcaption> </ figure>

#### Étape 4: Configuration du mécanisme de la boucle

1. Ajoutez un nœud ** boucle ** pour gérer les scénarios de rejet. Cela permet au workflow de revenir à l'agent de réponse par e-mail pour des améliorations.
2. Configurer le nœud de boucle:
   * ** Loop Retour à **: Sélectionnez "Agent de réponse par e-mail" dans la liste déroulante
   * ** Count de boucles max **: 5 (empêche les boucles infinies)
3. Connectez la sortie "Rejeter" du nœud d'entrée humain à ce nœud de boucle. Lorsqu'un humain rejette la réponse, le flux de travail reviendra à l'agent avec la rétroaction pour l'amélioration.

<gigne> <img src = "../. GitBook / Assets / image (5) (1) (1) (1) (1) .png" alt = "" width = "563"> <figCaption> </ Figcaption> </gigu

#### Étape 5: Création du sujet de messagerie et du générateur de corps

1. Ajoutez un nœud ** llm ** et connectez-le à la sortie "Procéder" du nœud d'entrée humain. Ce nœud structurera la réponse approuvée au format de messagerie approprié.
2. Configurer la sortie structurée JSON:
   * ** Key **: "Sujet", ** Type **: "String", ** Description **: "Sujet de l'e-mail"
   * ** Key **: "Body", ** Type **: "String", ** Description **: "Corps de l'e-mail"

<gigne> <img src = "../. GitBook / Assets / Image (6) (1) (1) (1) (1) .png" alt = "" width = "563"> <figCaption> </gigcaption> </gigu

#### Étape 6: Configuration de l'envoi des e-mails

1. Ajoutez un nœud ** Tool ** et connectez-le au nœud Email Subject & Body LLM. Cela gérera l'envoi du courrier électronique réel.
2. Configurer le nœud d'outil:
   * ** Outil **: Sélectionnez "Gmail" dans les outils disponibles
   * ** Actions du message **: "SendMessage"
3. Configurer les arguments d'entrée de l'outil:
   * ** à **: utilisez la variable`{{ $form.from }}`Pour répondre à l'expéditeur d'origine
   * ** Sujet **: Utiliser`{{ llmAgentflow_0.output.subject }}`Pour obtenir le sujet généré à partir de l'étape 5
   * ** Corps **: Utiliser`{{ llmAgentflow_0.output.body }}`Pour obtenir le corps de messagerie généré à partir de l'étape 5

<gigne> <img src = "../. GitBook / Assets / Image (7) (1) (1) (1) (1) .png" alt = "" width = "563"> <figCaption> </gigcaption> </gigu

### Comment fonctionne le flux de travail

Lorsqu'une demande de courrier électronique arrive, voici ce qui se passe:

1. ** Entrée de formulaire **: Le système capture les informations sur le sujet de l'e-mail, le corps et l'expéditeur
2. ** Analyse AI **: L'agent de réponse par e-mail analyse la demande et génère une réponse professionnelle à l'aide de la recherche Web pour un contexte supplémentaire
3. ** Révision humaine **: Le flux de travail s'arrête et présente la réponse générée par l'AI à un critique humain
4. ** Point de décision **: L'humain peut soit:
   * ** Approuver **: La réponse procède au formatage par e-mail et à l'envoi
   * ** Rejeter **: La réponse revient à l'agent avec des commentaires pour l'amélioration
5. ** Formatage des e-mails **: Si approuvé, la réponse est structurée en format de messagerie approprié avec le sujet et le corps
6. ** Email Envoi **: L'e-mail final est automatiquement envoyé via Gmail à l'expéditeur d'origine

### Tester le workflow

1. Démarrez le flux de travail en remplissant le formulaire avec un exemple de demande de courrier électronique

<gigne> <img src = "../. Gitbook / Assets / image (8) (1) (1) (1) .png" alt = "" width = "527"> <figcaption> </gigcaption> </gigust>

2. Passez en revue la réponse de l'agent dans l'étape d'entrée humaine

<gigne> <img src = "../. GitBook / Assets / Image (9) (1) (1) .png" alt = "" width = "563"> <Figcaption> </gigcaption> </gigust>

3. Rejeter la réponse et fournir plus de commentaires:

<gigne> <img src = "../. GitBook / Assets / image (10) (1) (1) .png" alt = "" width = "563"> <Figcaption> </ Figcaption> </gigust>

4. Passez en revue la réponse révisée de l'agent:

<gigne> <img src = "../. GitBook / Assets / image (11) (1) (1) .png" alt = "" width = "563"> <Figcaption> </ Figcaption> </gigne>

5. Continuez et vérifiez que le courrier électronique est envoyé correctement:

<gigne> <img src = "../. GitBook / Assets / image (12) (1) (1) .png" alt = "" width = "563"> <Figcaption> </ Figcaption> </gigust>

### Structure d'écoulement complète

{% fichier src = "../. gitbook / actifs / humain dans le boucle agent.json"%}

## Exiger une entrée humaine sur les outils d'agent

Lorsqu'un agent décide d'utiliser des outils, ce qui suit se produit sous le capot:

1. Compte tenu d'une requête utilisateur, le LLM détermine si les appels d'outils sont nécessaires.
2. Si les appels d'outils sont identifiés à partir de la réponse de sortie LLM, Flowise localise les outils de correspondance et exécute les fonctions correspondantes.
3. Les résultats des exécutions d'outils sont renvoyés au LLM.
4. Le LLM décide alors si des appels d'outils supplémentaires sont requis ou s'il a suffisamment d'informations pour retourner la réponse finale.

<gigne> <img src = "../. GitBook / Assets / Untitled-2025-06-15-0132.png" alt = "" width = "375"> <Figcaption> </ Figcaption> </ Figure>

Lorsque l'exigence de l'entrée humaine est activée, nous plaçons un point de contrôle supplémentaire après la détection des appels d'outil:

<gigne> <img src = "../. GitBook / Assets / Untitled-2025-06-15-0132 (1) .png" alt = "" width = "563"> <Figcaption> </ Figcaption> </ Figure>

Ceci est crucial pour les appels d'outils sensibles tels que passer des commandes, des réservations, des réunions, l'envoi d'e-mails, etc., où vous avez besoin de confirmation et de révision humaines.

Nous pouvons utiliser l'exemple de système de réponse par e-mail ci-dessus, mais simplifier pour avoir un seul agent.

<gigne> <img src = "../. GitBook / Assets / Image (313) .png" alt = "" width = "563"> <Figcaption> </gigcaption> </gigust>

### Configuration

1. Ajoutez un nœud ** Agent ** et connectez-le au nœud de démarrage. Cet agent unique gérera à la fois l'analyse par e-mail et l'approbation humaine.
2.  Ajoutez un message système à l'agent, par exemple:

    ```
    You are a customer support agent working in Flowise Inc. Create a draft professional email reply to user's query. Use the web search tools to get more details about the prospect.

    Always reply as Samantha, Customer Support Representative in Flowise. Don't use placeholders.

    Today's date is {{ current_date_time }}.
    ```
3. Ajouter les outils suivants:
   * ** Recherche personnalisée Google **: pour rechercher des informations sur les clients
   * ** gmail **: pour créer des brouillons par e-mail avec l'approbation humaine
4. Configurer l'outil Gmail:
   * ** Type de Gmail **: "Drafts"
   * ** Draft Actions **: "CreateLaft"
   * ** Exiger une entrée humaine **: ✅ ** Activer cette option ** - c'est la caractéristique clé qui crée la fonctionnalité HITL

<gigne> <img src = "../. GitBook / Assets / Image (314) .png" alt = "" width = "481"> <Figcaption> </ Figcaption> </gigust>

### Comment fonctionne le flux simplifié

1. ** Entrée du formulaire **: L'utilisateur soumet les détails de la demande par e-mail
2. ** Analyse AI **: L'agent analyse l'e-mail et utilise Google Rechercher un contexte supplémentaire
3. ** Création du projet **: Lorsque l'agent tente de créer un brouillon Gmail, le workflow s'arrête
4. ** Révision humaine **: Le système présente le projet de courrier électronique pour l'approbation humaine
5. ** Décision **: L'humain peut approuver (créer un projet) ou rejeter (fournir des commentaires et réessayer)

### Tester l'agent

1.  Démarrez le flux de travail en remplissant le formulaire avec un exemple de demande de courrier électronique

<gigne> <img src = "../. Gitbook / Assets / image (8) (1) (1) (1) .png" alt = "" width = "527"> <figcaption> </gigcaption> </gigust>


2. Avant que l'agent ne crée le projet Gmail, il demandera à l'utilisateur l'approbation ou le rejet.

<gigne> <img src = "../. GitBook / Assets / Image (315) .png" alt = "" width = "563"> <Figcaption> </ Figcaption> </gigne>

3. Si l'outil est approuvé, l'agent procédera à l'appel de l'outil et créera le projet dans Gmail. L'agent est suffisamment intelligent pour déterminer le sujet, le corps et le destinataire appropriés pour l'e-mail.

<gigne> <img src = "../. GitBook / Assets / Image (316) .png" alt = "" width = "563"> <Figcaption> </gigcaption> </ Figure>

### Structure d'écoulement complète

{% fichier src = "../. GitBook / Assets / Email Agent.json"%}

## Partage des traces d'exécution pour l'examen et l'approbation externes

1. Dans la barre latérale gauche du tableau de bord, cliquez sur ** Exécutions. **
2. Trouvez la trace d'exécution et cliquez sur ** Partager. **

<gigne> <img src = "../. GitBook / Assets / Image (13) (1) (1) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

3. La trace d'exécution est désormais disponible en tant que lien public. Vous pouvez partager ce lien avec d'autres pour examen.

<gigne> <img src = "../. GitBook / Assets / Image (14) (1) (1) .png" alt = "" width = "541"> <Figcaption> </ / Figcaption> </ Figure>

4. Les utilisateurs en dehors de Flowise peuvent rejeter ou approuver:

<gigne> <img src = "../. GitBook / Assets / Image (15) (1) (1) .png" alt = ""> <Figcaption> </gigcaption> </gigust>
