# Outils et MCP

Dans le précédent[**Interacting with API**](interacting-with-api.md)Tutoriel, nous avons exploré comment permettre aux LLM d'appeler des API externes. Pour améliorer l'expérience utilisateur, Flowise fournit une liste d'outils préfabillés. Reportez-vous au[**Tools**](../integrations/langchain/tools/)Section pour la liste complète des intégrations disponibles.

Dans les cas où l'outil dont vous avez besoin n'est pas encore disponible, vous pouvez créer un ** outil personnalisé ** pour répondre à vos besoins.

## Outil personnalisé

Nous allons utiliser le même[Event Management Server](interacting-with-api.md#prerequisite), et créez un outil personnalisé qui peut appeler la demande HTTP Post pour`/events`.

<gigne> <img src = "../. GitBook / Assets / Image (5) (1) (1) (1) (1) (1) .png" alt = "" width = "563"> <figCaption> </gigcaption> </ figure>

* ** Nom de l'outil: **`create_event`
* ** Description de l'outil: **`Use this when you want to create a new event.`
* ** Schéma d'entrée: ** Un schéma JSON du corps de la demande d'API qui permet à LLM de savoir comment générer automatiquement le corps JSON correct. Par exemple:
* ** Fonction JavaScript **: la fonction réelle à exécuter une fois cet outil appelé

```javascript
const fetch = require('node-fetch');
const url = 'http://localhost:5566/events';
const options = {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      name: $name,
      location: $location,
      date: $date
    })
};
try {
    const response = await fetch(url, options);
    const text = await response.text();
    return text;
} catch (error) {
    console.error(error);
    return '';
}
```

### Comment utiliser la fonction:

* Vous pouvez utiliser toutes les bibliothèques importées dans Flowise.
* Vous pouvez utiliser des propriétés spécifiées dans le schéma d'entrée comme variables avec préfixe`# Outils et MCP

Dans le précédent[**Interacting with API**](interacting-with-api.md)Tutoriel, nous avons exploré comment permettre aux LLM d'appeler des API externes. Pour améliorer l'expérience utilisateur, Flowise fournit une liste d'outils préfabillés. Reportez-vous au[**Tools**](../integrations/langchain/tools/)Section pour la liste complète des intégrations disponibles.

Dans les cas où l'outil dont vous avez besoin n'est pas encore disponible, vous pouvez créer un ** outil personnalisé ** pour répondre à vos besoins.

## Outil personnalisé

Nous allons utiliser le même[Event Management Server](interacting-with-api.md#prerequisite), et créez un outil personnalisé qui peut appeler la demande HTTP Post pour`/events`.

<gigne> <img src = "../. GitBook / Assets / Image (5) (1) (1) (1) (1) (1) .png" alt = "" width = "563"> <figCaption> </gigcaption> </ figure>

* ** Nom de l'outil: **`create_event`
* ** Description de l'outil: **`Use this when you want to create a new event.`
* ** Schéma d'entrée: ** Un schéma JSON du corps de la demande d'API qui permet à LLM de savoir comment générer automatiquement le corps JSON correct. Par exemple:
* ** Fonction JavaScript **: la fonction réelle à exécuter une fois cet outil appelé

```javascript
const fetch = require('node-fetch');
const url = 'http://localhost:5566/events';
const options = {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      name: $name,
      location: $location,
      date: $date
    })
};
try {
    const response = await fetch(url, options);
    const text = await response.text();
    return text;
} catch (error) {
    console.error(error);
    return '';
}
```

### Comment utiliser la fonction:

* Vous pouvez utiliser toutes les bibliothèques importées dans Flowise.
* Vous pouvez utiliser des propriétés spécifiées dans le schéma d'entrée comme variables avec préfixe:
  * Propriété du schéma d'entrée =`name`
  * Variable à utiliser dans la fonction =`$name`
* Vous pouvez obtenir une configuration de flux par défaut:
  * `$flow.sessionId`
  * `$flow.chatId`
  * `$flow.chatflowId`
  * `$flow.input`
  * `$flow.state`
* Vous pouvez obtenir des variables personnalisées:`$vars.<variable-name>`
* Doit renvoyer une valeur de chaîne à la fin de la fonction

### Utilisez un outil personnalisé sur l'agent

Une fois l'outil personnalisé créé, vous pouvez l'utiliser sur le nœud d'agent.

<gigne> <img src = "../. Gitbook / Assets / image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "341"> <figCaption> </gigcaption> </gistre>

Dans la liste déroulante de l'outil, sélectionnez l'outil personnalisé. Vous pouvez également activer ** return direc ** t si vous souhaitez renvoyer directement la sortie de l'outil personnalisé.

<gigne> <img src = "../. GitBook / Assets / Image (2) (1) (1) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "392"> <figcaption> </gigcaption> </piguré>

### Utilisez l'outil personnalisé sur l'outil

Il peut également être utilisé comme nœud d'outil dans un scénario de flux de travail déterminé. \
Dans ce cas, ** Les arguments d'entrée de l'outil doivent être explicitement définis et remplis de valeurs **, car il n'y a pas de LLM pour déterminer automatiquement les valeurs.

<gigne> <img src = "../. GitBook / Assets / Image (3) (1) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "563"> <figcaption> </gigcaption> </pigucial>

## MCP

MCP ([Model Context Protocol](https://modelcontextprotocol.io/introduction)) fournit un moyen standardisé de connecter les modèles d'IA à différentes sources de données et outils. En d'autres termes, au lieu de compter sur des outils Flowise intégrés ou de créer un outil personnalisé, on peut utiliser des serveurs MCP qui ont été créés par d'autres. MCP est largement considéré comme une norme de l'industrie et est généralement soutenu et maintenu par les prestataires officiels. Par exemple, le GitHub MCP est développé et maintenu par l'équipe GitHub, avec un soutien similaire fourni pour Atlassian Jira, Brave Search, et autres. Vous pouvez trouver la liste des serveurs pris en charge[here](https://modelcontextprotocol.io/examples).

<gigne> <img src = "../. GitBook / Assets / Image (1) (1) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "413"> <figcaption> </gigcaption> </pigucial>

## MCP personnalisé

Outre les outils MCP prédéfinis, la fonctionnalité la plus puissante est ** personnalisé MCP **, qui permet aux utilisateurs de se connecter à tout serveur MCP de leur choix.

MCP suit une architecture client-serveur où:

* ** hôtes ** sont des applications LLM (comme Flowise) qui initient des connexions
* ** Clients ** Maintenir des connexions 1: 1 avec des serveurs, à l'intérieur de l'application hôte (comme MCP personnalisé)
* ** Serveurs ** Fournir un contexte, des outils et des invites aux clients (exemple[servers](https://modelcontextprotocol.io/examples))

Pour gérer la communication réelle entre les clients et les serveurs. MCP prend en charge plusieurs mécanismes de transport:

1. ** STdio Transport **
   * Utilise une entrée / sortie standard pour la communication
   * Idéal pour les processus locaux
2. ** Transport HTTP streamable **
   * Utilise HTTP avec des événements de serveur en option pour le streaming
   * HTTP Post pour les messages du client à serveur

### Stdio

STdio Transport permet la communication via des flux d'entrée et de sortie standard. Ceci est particulièrement utile pour les intégrations locales et les outils de ligne de commande.

Utilisez-le uniquement lors de l'utilisation de Flowise localement, pas lorsqu'il est déployé sur les services cloud. C'est parce que l'exécution de la commande comme`npx`Installera le package MCP Server (Ex:`@modelcontextprotocol/server-sequential-thinking`) localement, et cela prend souvent du temps pour cela.

Il est plus adapté à l'application de bureau comme Claude Desktop, vs code, etc.

#### ** Commande NPX **

```json
{
  "command": "npx",
  "args": [
    "-y",
    "@modelcontextprotocol/server-sequential-thinking"
  ]
}
```

<gigne> <img src = "../. GitBook / Assets / Image (16) (1) (1) .png" alt = "" width = "419"> <Figcaption> </ / Figcaption> </ Figure>

Pour Windows, reportez-vous[guide](https://gist.github.com/feveromo/7a340d7795fca1ccd535a5802b976e1f).

#### ** Commande Docker **

La commande docker convient lorsque la machine en cours d'exécution a également accès à Docker. Cependant, il ne convient pas aux déploiements sur les services cloud où l'accès Docker est restreint ou indisponible.

```json
{
  "command": "docker",
  "args": [
    "run",
    "-i",
    "--rm",
    "mcp/sequentialthinking"
  ]
}
```

<gigne> <img src = "../. GitBook / Assets / Image (312) .png" alt = "" width = "416"> <Figcaption> </gigcaption> </gigne>

Docker fournit une liste de serveurs MCP, qui peuvent être trouvés[here](https://hub.docker.com/catalogs/mcp). Voici comment cela fonctionne:

1. Assurez-vous que Docker fonctionne.
2. Localisez la configuration du serveur MCP et ajoutez-le à ** MCP personnalisé **. Par exemple:[https://hub.docker.com/r/mcp/sequentialthinking](https://hub.docker.com/r/mcp/sequentialthinking)
3. Actualisez les ** actions disponibles **. Si l'image n'est pas trouvée localement, Docker tirera automatiquement la dernière image. Une fois l'image tirée, vous verrez la liste des actions disponibles.

```
Unable to find image 'mcp/sequentialthinking:latest' locally
latest: Pulling from mcp/sequentialthinking
f18232174bc9: Already exists
cb2bde55f71f: Pull complete
9d0e0719fbe0: Pull complete
6f063dbd7a5d: Pull complete
93a0fbe48c24: Pull complete
e2e59f8d7891: Pull complete
96ec0bda7033: Pull complete
4f4fb700ef54: Pull complete
d0900e07408c: Pull complete
Digest: sha256:cd3174b2ecf37738654cf7671fb1b719a225c40a78274817da00c4241f465e5f
Status: Downloaded newer image for mcp/sequentialthinking:latest
Sequential Thinking MCP Server running on stdio
```

#### Quand utiliser

* Construire des outils de ligne de commande
* Implémentation de intégrations locales
* Besoin d'une communication de processus simple
* Travailler avec des scripts shell

### HTTP diffusable (recommandé)

Nous utiliserons GitHub Remote MCP comme exemple. La belle partie de[Remote GitHub MCP server](https://github.com/github/github-mcp-server), vous n'avez pas besoin de l'installer ou de l'exécuter localement, les nouvelles mises à jour sont appliquées automatiquement.

#### Étape 1: Créez une variable pour GitHub Pat

Afin d'accéder au serveur MCP, nous devons créer un jeton d'accès personnel à partir de GitHub. Se référer à[guide](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-personal-access-token-classic). Une fois PAT créé, créez une variable pour stocker le jeton. Cette variable sera utilisée dans MCP personnalisé.

<gigne> <img src = "../. GitBook / Assets / Image (1) (1) (1) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "508"> <Figcaption> </Figcaption> </pigucial>

#### Étape 2: Créez un MCP personnalisé

Créez un nœud d'agent et ajoutez un nouvel outil MCP personnalisé. Pour HTTP diffusable, nous avons juste besoin de mettre l'URL et d'autres en-têtes nécessaires. Vous pouvez utiliser[variables](../using-flowise/variables.md)Dans la configuration du serveur MCP avec des accolades à double boucle`{{ }}`et préfixe`$vars.<variableName>`.

```json
{
  "url": "https://api.githubcopilot.com/mcp/",
  "headers": {
    "Authorization": "Bearer {{$vars.githubPAT}}",
  }
}
```

<gigne> <img src = "../. GitBook / Assets / image (2) (1) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "414"> <figcaption> </gigcaption> </pigucial>

#### Étape 3: Sélectionnez les actions

Si la configuration du serveur MCP fonctionne correctement, vous pouvez actualiser les actions ** disponibles ** et Flowise tirera automatiquement toutes les actions disponibles à partir du serveur MCP.

<gigne> <img src = "../. Gitbook / Assets / Image (3) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "359"> <Figcaption> </ Figcaption> </ Figure>

#### Exemples d'interactions:

> Donnez-moi le problème le plus récent

<gigne> <img src = "../. Gitbook / Assets / Image (4) (1) (1) (1) (1) (1) (1) .png" alt = ""> <figcaption> </gigcaption> </stigne>

L'agent est en mesure d'identifier les actions appropriées de MCP et de les utiliser pour répondre à la requête de l'utilisateur.

#### Quand utiliser

Utilisez HTTP Streamable lorsque:

* Construire des intégrations Web
* Besoin de communication client-serveur sur HTTP
* Nécessiter des séances avec état
* Soutenir plusieurs clients simultanés
* Implémentation de connexions à reprise

## Tutoriel vidéo

{% embed url = "https://youtu.be/7fcli-qm3tk?si=zbneshd3nlcroBro"%}
