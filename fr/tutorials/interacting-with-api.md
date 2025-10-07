# Interagir avec l'API

Presque toutes les applications Web repose sur des API RESTful. Permettre à LLM d'interagir avec eux élargir son utilité pratique.

Ce tutoriel présente comment LLM peut être utilisé pour passer des appels d'API via l'appel d'outils.

## Préalable - Exemple de serveur de gestion d'événements

Nous allons utiliser un serveur de gestion d'événements simples et montrer comment interagir avec lui.

```javascript
const express = require('express');
const { v4: uuidv4 } = require('uuid');

const app = express();
const PORT = process.env.PORT || 5566;

// Middleware
app.use(express.json());

// Fake database - in-memory storage
let events = [
  {
    id: '1',
    name: 'Tech Conference 2024',
    date: '2024-06-15T09:00:00Z',
    location: 'San Francisco, CA'
  },
  {
    id: '2',
    name: 'Music Festival',
    date: '2024-07-20T18:00:00Z',
    location: 'Austin, TX'
  },
  {
    id: '3',
    name: 'Art Exhibition Opening',
    date: '2024-05-10T14:00:00Z',
    location: 'New York, NY'
  },
  {
    id: '4',
    name: 'Startup Networking Event',
    date: '2024-08-05T17:30:00Z',
    location: 'Seattle, WA'
  },
  {
    id: '5',
    name: 'Food & Wine Tasting',
    date: '2024-09-12T19:00:00Z',
    location: 'Napa Valley, CA'
  }
];

// Helper function to validate event data
const validateEvent = (eventData) => {
  const required = ['name', 'date', 'location'];
  const missing = required.filter(field => !eventData[field]);
  
  if (missing.length > 0) {
    return { valid: false, message: `Missing required fields: ${missing.join(', ')}` };
  }
  
  // Basic date validation
  const dateRegex = /^\d{4}-\d{2}-\d{2}T\d{2}:\d{2}:\d{2}(\.\d{3})?Z?$/;
  if (!dateRegex.test(eventData.date)) {
    return { valid: false, message: 'Date must be in ISO 8601 format (YYYY-MM-DDTHH:mm:ssZ)' };
  }
  
  return { valid: true };
};

// GET /events - List all events
app.get('/events', (req, res) => {
  res.status(200).json(events);
});

// POST /events - Create a new event
app.post('/events', (req, res) => {
  const validation = validateEvent(req.body);
  
  if (!validation.valid) {
    return res.status(400).json({ error: validation.message });
  }
  
  const newEvent = {
    id: req.body.id || uuidv4(),
    name: req.body.name,
    date: req.body.date,
    location: req.body.location
  };
  
  events.push(newEvent);
  res.status(201).json(newEvent);
});

// GET /events/{id} - Retrieve an event by ID
app.get('/events/:id', (req, res) => {
  const event = events.find(e => e.id === req.params.id);
  
  if (!event) {
    return res.status(404).json({ error: 'Event not found' });
  }
  
  res.status(200).json(event);
});

// DELETE /events/{id} - Delete an event by ID
app.delete('/events/:id', (req, res) => {
  const eventIndex = events.findIndex(e => e.id === req.params.id);
  
  if (eventIndex === -1) {
    return res.status(404).json({ error: 'Event not found' });
  }
  
  events.splice(eventIndex, 1);
  res.status(204).send();
});

// PATCH /events/{id} - Update an event's details by ID
app.patch('/events/:id', (req, res) => {
  const eventIndex = events.findIndex(e => e.id === req.params.id);
  
  if (eventIndex === -1) {
    return res.status(404).json({ error: 'Event not found' });
  }
  
  const validation = validateEvent(req.body);
  
  if (!validation.valid) {
    return res.status(400).json({ error: validation.message });
  }
  
  // Update the event
  events[eventIndex] = {
    ...events[eventIndex],
    name: req.body.name,
    date: req.body.date,
    location: req.body.location
  };
  
  res.status(200).json(events[eventIndex]);
});

// Error handling middleware
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ error: 'Something went wrong!' });
});

// 404 handler
app.use((req, res) => {
  res.status(404).json({ error: 'Endpoint not found' });
});

// Start the server
app.listen(PORT, () => {
  console.log(`Event Management API server is running on port ${PORT}`);
  console.log(`Available endpoints:`);
  console.log(`  GET    /events      - List all events`);
  console.log(`  POST   /events      - Create a new event`);
  console.log(`  GET    /events/{id} - Get event by ID`);
  console.log(`  PATCH  /events/{id} - Update event by ID`);
  console.log(`  DELETE /events/{id} - Delete event by ID`);
});

module.exports = app; 
```

***

## Demander des outils

Il existe 4 outils de demande qui peuvent être utilisés. Cela permet à LLM d'appeler les outils GET, Publier, mettre, supprimer les outils lorsque cela est nécessaire.

### Étape 1: Ajoutez le nœud de démarrage

Le nœud de démarrage est le point d'entrée de votre flux

<gigne> <img src = "../. Gitbook / Assets / Image (4) (1) (1) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "324"> </ figction> </gigcaption> </pigucial>

### Étape 2: Ajouter le nœud d'agent

Ensuite, ajoutez un nœud d'agent. Dans cette configuration, l'agent est configuré avec quatre outils principaux: obtenir, publier, mettre et supprimer. Chaque outil est configuré pour effectuer un type spécifique de demande d'API.

#### Outil 1: obtenir (récupérer les événements)

<gigne> <img src = "../. Gitbook / Assets / image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "335"> <gigcaption> </ / Figcaption> </ Figure>

* ** Objectif: ** Récupérez une liste d'événements ou un événement spécifique de l'API.
* ** Entrées de configuration: **
  * ** URL: **`http://localhost:5566/events`
  * **Nom:**`get_events`
  * ** Description: ** Décrivez quand utiliser cet outil. Par exemple:`Use this when you need to get events`
  * ** En-têtes: ** (Facultatif) Ajouter tous les en-têtes HTTP requis.
  *   ** Schéma de paramètres de requête: ** Un schéma JSON de l'API qui permet à LLM de connaître la structure URL, quel chemin et des paramètres de requête à générer. Par exemple:

      ```json
      {
        "id": {
          "type": "string",
          "in": "path",
          "description": "ID of the item to get. /:id"
        },
        "limit": {
          "type": "string",
          "in": "query",
          "description": "Limit the number of items to get. ?limit=10"
        }
      }
      ```

#### Outil 2: Publier (créer l'événement)

<gigne> <img src = "../. Gitbook / Assets / Image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "335"> <figcaption>

* ** Objectif: ** Créez un nouvel événement dans le système.
* ** Entrées de configuration: **
  * ** URL: **`http://localhost:5566/events`
  * **Nom:**`create_event`
  * **Description:**`Use this when you want to create a new event.`
  * ** En-têtes: ** (Facultatif) Ajouter tous les en-têtes HTTP requis.
  * ** Corps **: objet de corps codé dur qui remplacera le corps généré par LLM
  *   ** Schéma corporel: ** Un schéma JSON du corps de demande d'API qui permet à LLM de savoir comment générer automatiquement le corps JSON correct. Par exemple:

      ```json
      {
        "name": {
          "type": "string",
          "required": true,
          "description": "Name of the event"
        },
        "date": {
          "type": "string",
          "required": true,
          "description": "Date of the event"
        },
        "location": {
          "type": "string",
          "required": true,
          "description": "Location of the event"
        }
      }
      ```

#### Outil 3: put (événement de mise à jour)

<gigne> <img src = "../. Gitbook / Assets / image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "335"> <figCaption> </gigcaption> </gigu

* ** Objectif: ** Mettez à jour les détails d'un événement existant.
* ** Entrées de configuration: **
  * ** URL: **`http://localhost:5566/events`
  * **Nom:**`update_event`
  * **Description:**`Use this when you want to update an event.`
  * ** En-têtes: ** (Facultatif) Ajouter tous les en-têtes HTTP requis.
  * ** Corps **: objet de corps codé dur qui remplacera le corps généré par LLM
  *   ** Schéma corporel: ** Un schéma JSON du corps de demande d'API qui permet à LLM de savoir comment générer automatiquement le corps JSON correct. Par exemple:

      ```json
      {
        "name": {
          "type": "string",
          "required": true,
          "description": "Name of the event"
        },
        "date": {
          "type": "string",
          "required": true,
          "description": "Date of the event"
        },
        "location": {
          "type": "string",
          "required": true,
          "description": "Location of the event"
        }
      }
      ```

#### Outil 4: Supprimer (supprimer l'événement)

<gigne> <img src = "../. GitBook / Assets / image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "335"> <figCaption> </ Figcaption> </ Figure>

* ** Objectif: ** Supprimer un événement du système.
* ** Entrées de configuration: **
  * ** URL: **`http://localhost:5566/events`
  * **Nom:**`delete_event`
  * **Description:**`Use this when you need to delete an event.`
  * ** En-têtes: ** (Facultatif) Ajouter tous les en-têtes HTTP requis.
  *   ** Schéma de paramètres de requête: ** Un schéma JSON de l'API qui permet à LLM de connaître la structure URL, quel chemin et des paramètres de requête à générer. Par exemple:

      ```json
      {
        "id": {
          "type": "string",
          "required": true,
          "in": "path",
          "description": "ID of the item to delete. /:id"
        }
      }
      ```

### Comment l'agent utilise ces outils

* L'agent peut sélectionner dynamiquement l'outil à utiliser en fonction de la demande de l'utilisateur ou de la logique du flux.
* Chaque outil est mappé sur une méthode HTTP spécifique et un point de terminaison, avec des schémas d'entrée clairement définis.
* L'agent exploite le LLM pour interpréter la saisie de l'utilisateur, remplir les paramètres requis et passer l'appel API approprié.

Certainement! Voici quelques ** Exemples d'interactions ** pour votre flux, y compris les exemples de requêtes utilisateur et le comportement attendu pour chacun, mappé à l'outil correspondant (obtenir, publier, mettre, supprimer):

### Exemples d'interactions

#### 1. Récupérer les événements (obtenir)

** Exemple de requête: **

> "Montrez-moi tous les événements à venir."

** Comportement attendu: **

* L'agent sélectionne l'outil ** get **.
* Il envoie une demande de GET à`http://localhost:5566/events`.
* L'agent renvoie une liste de tous les événements à l'utilisateur.

***

** Exemple de requête: **

> "Obtenez les détails de l'événement avec ID 12345."

** Comportement attendu: **

* L'agent sélectionne l'outil ** get **.
* Il envoie une demande de GET à`http://localhost:5566/events/12345`.
* L'agent renvoie les détails de l'événement avec ID`12345`.

***

#### 2. Créez un nouvel événement (post)

** Exemple de requête: **

> "Créer un nouvel événement appelé 'AI Conference' le 2024-07-15 au Tech Hall."

** Comportement attendu: **

* L'agent sélectionne l'outil ** Post **.
*   Il envoie une demande de poste à`http://localhost:5566/events`avec le corps:

    ```json
    {
      "name": "AI Conference",
      "date": "2024-07-15",
      "location": "Tech Hall"
    }
    ```
* L'agent confirme que l'événement a été créé et pourrait retourner les détails du nouvel événement.

***

#### 3. Mettre à jour un événement (put)

** Exemple de requête: **

> "Changez l'emplacement de l'événement« Conférence AI »le 2024-07-15 en« Auditorium principal ».»

** Comportement attendu: **

* L'agent sélectionne l'outil ** put **.
*   Il envoie une demande de vente à`http://localhost:5566/events`avec les détails de l'événement mis à jour:

    ```json
    {
      "name": "AI Conference",
      "date": "2024-07-15",
      "location": "Main Auditorium"
    }
    ```
* L'agent confirme que l'événement a été mis à jour.

***

#### 4. Supprimer un événement (supprimer)

** Exemple de requête: **

> "Supprimer l'événement avec ID 12345."

** Comportement attendu: **

* L'agent sélectionne l'outil ** supprimer **.
* Il envoie une demande de suppression à`http://localhost:5566/events/12345`.
* L'agent confirme que l'événement a été supprimé.

### Débit complet

{% fichier src = "../. gitbook / actifs / demandes d'outils agent.json"%}

***

## Boîte à outils OpenAPI

Les 4 outils de demande fonctionnent très bien si vous avez quelques API, mais imaginez avoir des dizaines ou des centaines d'API, cela pourrait devenir difficile à maintenir. Pour résoudre ce problème, Flowise fournit une boîte à outils OpenAPI qui est capable de prendre un fichier OpenAPI YAML et d'analyser chaque API dans un outil. Le[OpenAPI Specification (OAS)](https://swagger.io/specification/)est une norme universellement acceptée pour décrire les détails des API RESTfuls dans un format que les machines peuvent lire et interpréter.

En utilisant l'API de gestion d'événements, nous pouvons générer un fichier OpenAPI YAML:

```yaml
openapi: 3.0.0
info:
  version: 1.0.0
  title: Event Management API
  description: An API for managing event data

servers:
  - url: http://localhost:5566
    description: Local development server

paths:
  /events:
    get:
      summary: List all events
      operationId: listEvents
      responses:
        '200':
          description: A list of events
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Event'
    
    post:
      summary: Create a new event
      operationId: createEvent
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/EventInput'
      responses:
        '201':
          description: The event was created
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Event'
        '400':
          description: Invalid input
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'

  /events/{id}:
    parameters:
      - name: id
        in: path
        required: true
        schema:
          type: string
        description: The event ID
    
    get:
      summary: Retrieve an event by ID
      operationId: getEventById
      responses:
        '200':
          description: The event
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Event'
        '404':
          description: Event not found
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
    
    patch:
      summary: Update an event's details by ID
      operationId: updateEventDetails
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/EventInput'
      responses:
        '200':
          description: The event's details were updated
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Event'
        '400':
          description: Invalid input
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
        '404':
          description: Event not found
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
    
    delete:
      summary: Delete an event by ID
      operationId: deleteEvent
      responses:
        '204':
          description: The event was deleted
        '404':
          description: Event not found
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'

components:
  schemas:
    Event:
      type: object
      properties:
        id:
          type: string
          description: The unique identifier for the event
        name:
          type: string
          description: The name of the event
        date:
          type: string
          format: date-time
          description: The date and time of the event in ISO 8601 format
        location:
          type: string
          description: The location of the event
      required:
        - name
        - date
        - location
    
    EventInput:
      type: object
      properties:
        name:
          type: string
          description: The name of the event
        date:
          type: string
          format: date-time
          description: The date and time of the event in ISO 8601 format
        location:
          type: string
          description: The location of the event
      required:
        - name
        - date
        - location
    
    Error:
      type: object
      properties:
        error:
          type: string
          description: Error message
```

### Étape 1: Ajoutez le nœud de démarrage

<gigne> <img src = "../. Gitbook / Assets / image (4) (1) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "319"> <figcaption> </gigcaption> </pigucial>

### Étape 2: Ajouter le nœud d'agent

Ensuite, ajoutez un nœud d'agent. Dans cette configuration, l'agent est configuré avec un seul outil - OpenAPI Toolkit

#### Outil: boîte à outils OpenAPI

<gigne> <img src = "../. gitbook / actifs / image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "332"> <figcaption>

* ** Objectif: ** Récupérez la liste des API du fichier YAML et transformez toutes les API en liste des outils
* ** Entrées de configuration: **
  * ** Fichier YAML: ** Le fichier OpenAPI YAML
  * ** RETOUR DIRECT: ** Si vous devez retourner la réponse de l'API directement
  * ** En-têtes: ** (Facultatif) Ajouter tous les en-têtes HTTP requis.
  * ** Supprimer les paramètres nuls: ** Supprimer toutes les clés avec des valeurs nulles des arguments analysés
  * ** Code personnalisé **: Personnalisez la façon dont la réponse est retournée

### Exemples d'interactions:

Nous pouvons utiliser le même exemple de requêtes de l'exemple précédent pour le tester:

<gigne> <img src = "../. GitBook / Assets / image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "563"> <figCaption> </ Figcaption> </ Figure>

***

## Appeler API séquentiellement

À partir des exemples ci-dessus, nous avons vu comment l'agent peut appeler dynamiquement les outils et interagir avec les API. Dans certains cas, il peut être nécessaire d'appeler une API séquentiellement avant ou après certaines actions. Par exemple, vous pouvez récupérer une liste de clients à partir d'un CRM et la transmettre à un agent. Dans de tels cas, vous pouvez utiliser le[HTTP node](../using-flowise/agentflowv2.md#id-6.-http-node).

<gigne> <img src = "../. GitBook / Assets / Image (3) (1) (1) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "563"> </ figction> </gigcaption> </piguré>

## Meilleures pratiques

* L'interaction avec les API est généralement utilisée lorsque vous souhaitez qu'un agent récupére les informations les plus à jour. Par exemple, un agent peut récupérer la disponibilité de votre calendrier, l'état du projet ou d'autres données en temps réel.
* Il est souvent utile d'inclure explicitement l'heure actuelle dans l'invite du système. Flowise fournit une variable appelée`{{current_date_time}}`, qui récupère la date et l'heure actuelles. Cela permet au LLM d'être conscient du moment présent, donc si vous posez des questions sur votre disponibilité pour aujourd'hui, le modèle peut faire référence à la date correcte. Sinon, il peut s'appuyer sur sa dernière date de coupure de formation, qui retournerait des informations obsolètes. Par exemple:

```
You are helpful assistant.

Todays date time is {{current_date_time }}
```
