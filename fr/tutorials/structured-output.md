# Sortie structurée

Dans de nombreux cas d'utilisation, tels que les chatbots, les modèles devraient répondre aux utilisateurs en langage naturel. Cependant, il existe des situations où les réponses du langage naturel ne sont pas idéales. Par exemple, si nous devons prendre la sortie du modèle, le passer en tant que corps pour la demande HTTP ou stocker dans une base de données, il est essentiel que la sortie s'aligne sur un schéma prédéfini. Cette exigence donne naissance au concept de ** sortie structurée **, où les modèles sont guidés pour générer des réponses dans un format structuré spécifique.

Dans ce tutoriel, nous allons jeter un œil à la façon de générer une sortie structurée à partir de LLM et de la passer comme le corps pour la demande HTTP.

## Condition préalable

Nous allons utiliser le même[Event Management Server](interacting-with-api.md#prerequisite)pour la demande HTTP.

Absolument! Voici un tutoriel pour votre ** flux de sortie structuré ** dans un format cohérent avec votre documentation "Agent As Tool", y compris les explications étape par étape et les espaces réservés d'image.

***

## Aperçu

1. Reçoit l'entrée de l'utilisateur via un nœud de démarrage.
2. Utilise un LLM pour générer un tableau JSON structuré.
3. Boucle via chaque élément du tableau.
4. Envoie chaque élément via HTTP à un point de terminaison externe.

<gigne> <img src = "../. GitBook / Assets / Image (306) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

### Étape 1: Configuration du nœud de démarrage

Commencez par ajouter un nœud ** start ** à votre toile.

<gigne> <img src = "../. GitBook / Assets / Image (307) .png" alt = "" width = "417"> <Figcaption> </ Figcaption> </gigust>

** Paramètres d'entrée de clé: **

* ** Type d'entrée: **
  * `chatInput`(par défaut): le flux commence par un message de chat de l'utilisateur.
  * `formInput`: Le flux commence par un formulaire (si vous souhaitez collecter des données structurées de l'utilisateur).
* ** Mémoire éphémère: **
  * (Facultatif) Si activé, le flux ne conserve pas l'historique de chat entre les exécutions.
* ** État de flux: **
  * (Facultatif) Pré-pupuler les variables d'état.
  *   Exemple:

      ```json
      [
        { "key": "answers", "value": "" }
      ]
      ```
* ** État persiste: **
  * (Facultatif) Si activé, l'état est persisté sur la même session.

### Étape 2: Génération de sortie structurée avec LLM

Ajoutez un nœud LLM et connectez-le au nœud de démarrage.

<gigne> <img src = "../. GitBook / Assets / Image (308) .png" alt = "" width = "563"> <Figcaption> </ Figcaption> </gigust>

** Objectif: ** utilise un modèle de langue pour analyser l'entrée et générer un tableau JSON structuré.

** Paramètres d'entrée de clé: **

* ** Sortie structurée JSON: **
  * **Clé:**`answers`
  * **Taper:**`JSON Array`
  *   ** schéma JSON: **

      ```json
      {
        "name": { "type": "string", "required": true, "description": "Name of the event" },
        "date": { "type": "string", "required": true, "description": "Date of the event" },
        "location": { "type": "string", "required": true, "description": "Location of the event" }
      }
      ```
  * ** Description: ** "Réponse à la requête utilisateur"
* ** Mettez à jour l'état du flux: **
  * Met à jour l'état de flux avec la sortie JSON générée.
  *   Exemple:

      ```json
      [
        {
          "key": "answers",
          "value": "{{ output.answers }}"
        }
      ]
      ```

### Étape 3: faire une boucle dans le tableau JSON

Ajoutez un nœud d'itération et connectez-le à la sortie du nœud LLM.

<gigne> <img src = "../. GitBook / Assets / Image (310) .png" alt = "" width = "563"> <Figcaption> </gigcaption> </ Figure>

** Objectif: ** itère sur chaque élément du tableau JSON généré à partir du nœud LLM.

** Paramètres d'entrée de clé: **

*   ** Entrée du tableau: **

    * Le tableau pour itérer. Réglé sur les réponses de l'état enregistré:

    ```html
    {{ $flow.state.answers }}
    ```

    * Cela signifie que le nœud traversera chaque événement dans le tableau des réponses.

### Étape 4: Envoi de chaque élément via HTTP

À l'intérieur de la boucle, ajoutez un nœud ** http **.

<gigne> <img src = "../. GitBook / Assets / Image (311) .png" alt = "" width = "563"> <Figcaption> </ Figcaption> </gigust>

** Objectif: ** Pour chaque élément du tableau, envoie une demande de message HTTP à un point de terminaison spécifié (par exemple,`http://localhost:5566/events`).

** Paramètres d'entrée de clé: **

* **Méthode:**
  * `POST`(par défaut pour ce cas d'utilisation).
* ** URL: **
  * Le point de terminaison pour envoyer des données.
  *   Exemple:

      ```
      http://localhost:5566/events
      ```
* ** Headers: **
  * (Facultatif) Ajouter tous les en-têtes HTTP requis (par exemple, pour l'authentification).
* ** Paramètres de requête: **
  * (Facultatif) Ajouter tous les paramètres de requête si nécessaire.
* ** Type de corps: **
  * `json`(par défaut): envoie le corps en JSON.
* **Corps:**
  * Les données à envoyer dans le corps de la demande.
  *   Définir sur l'élément actuel dans la boucle:

      ```html
      {{ $iteration }}
      ```
* ** Type de réponse: **
  * `json`(par défaut): attend une réponse JSON.

***

## Exemples d'interactions

** Entrée utilisateur: **

```
create 2 events:
1. JS Conference on next Sat in Netherlands
2. GenAI meetup, Sept 19, in Dublin
```

**Couler:**

* Le nœud de démarrage reçoit l'entrée.
* Le nœud LLM génère un tableau JSON d'événements.
* Le nœud de boucle itère dans chaque événement.
* Le nœud http créez chaque événement via l'API.

<Figure> <img src = "../. GitBook / Assets / Image (304) .png" alt = ""> <figcaption> </gigcaption> </gigust>

<gigne> <img src = "../. GitBook / Assets / Image (305) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

***

## Structure d'écoulement complète

{% fichier src = "../. GitBook / Assets / Structured Output.json"%}

***

## Meilleures pratiques

** Directives de conception: **

1. ** Schéma de sortie effacer: ** Définissez la structure attendue de la sortie LLM pour assurer un traitement fiable en aval.

** Cas d'utilisation courants: **

* ** Traitement des événements: ** Collectez et envoyez des données d'événements à un calendrier ou un système de gestion d'événements.
* ** Entrée de données en masse: ** Générer et soumettre plusieurs enregistrements à une base de données ou à l'API.
* ** Notifications automatisées: ** Envoyer des messages ou des alertes personnalisés pour chaque élément d'une liste.
