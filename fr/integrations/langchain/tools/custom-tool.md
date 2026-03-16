# Outil personnalisé

Regardez comment utiliser des outils personnalisés

{% embed url = "https://youtu.be/hsp9lkktvy0"%}

## Problème

La fonction prend généralement des données d'entrée structurées. Disons que vous voulez que le LLM puisse appeler AirTable Create Record[API](https://airtable.com/developers/web/api/create-records), les paramètres du corps doivent être structurés de manière spécifique. Par exemple:

```json
"records": [
  {
    "fields": {
      "Address": "some address",
      "Name": "some name",
      "Visited": true
    }
  }
]
```

Idéalement, nous voulons que LLM renvoie des données structurées appropriées comme celle-ci:

```json
{
  "Address": "some address",
  "Name": "some name",
  "Visited": true
}
```

Nous pouvons donc extraire la valeur et l'analyser dans le corps nécessaire à l'API. Cependant, il est difficile de demander à LLM de produire le modèle exact.

Avec le nouveau[OpenAI Function Calling](https://openai.com/blog/function-calling-and-other-api-updates)modèles, c'est maintenant possible.`gpt-4-0613`et`gpt-3.5-turbo-0613`sont spécifiquement formés pour retourner des données structurées. Le modèle choisira intelligemment de sortir un objet JSON contenant des arguments pour appeler ces fonctions.

## Tutoriel

** Objectif **: Demandez à l'agent d'obtenir automatiquement le mouvement du cours de l'action, de récupérer les actualités des actions connexes et d'ajouter un nouveau record à AirTable.

Commençons[🚀](https://emojipedia.org/rocket/)

### Créer des outils

Nous avons besoin de 3 outils pour atteindre l'objectif:

* Obtenez un mouvement du cours des actions
* Obtenir des actualités
* Ajouter un enregistrement Airtable

#### Obtenez un mouvement du cours des actions

Créez un nouvel outil avec les détails suivants (vous pouvez changer comme vous le souhaitez):

* Nom: Get \ _Stock \ _Movers
* Description: Obtenez les actions qui ont des mouvements de prix / volume les plus importants, par exemple actifs, gagnants, perdants, etc.

La description est une pièce importante car Chatgpt s'appuie sur cela pour décider quand utiliser cet outil.

<gigne> <img src = "../../../. GitBook / Assets / Image (6) (3) .png" alt = ""> <figcaption> </gigcaption> </gigust>

* Fonction JavaScript: nous allons utiliser[Morning Star](https://rapidapi.com/apidojo/api/morning-star) `/market/v2/get-movers`API pour obtenir des données. Vous devez d'abord cliquer sur vous abonner au test si vous ne l'avez pas déjà fait, puis copiez le code et collez-le dans la fonction JavaScript.
  * Ajouter`const fetch = require('node-fetch');`en haut pour importer la bibliothèque. Vous pouvez importer tous les nodejs intégrés[modules](https://www.w3schools.com/nodejs/ref_modules.asp)et[external libraries](https://github.com/FlowiseAI/Flowise/blob/main/packages/components/src/utils.ts#L289).
  * Retourner le`result`à la fin.

<gigne> <img src = "../../../. GitBook / Assets / Untitled (4) (1) .png" alt = ""> <Figcaption> </ Figcaption> </gigust>

Le code final doit être:

```javascript
const fetch = require('node-fetch');
const url = 'https://morning-star.p.rapidapi.com/market/v2/get-movers';
const options = {
	method: 'GET',
	headers: {
		'X-RapidAPI-Key': 'replace with your api key',
		'X-RapidAPI-Host': 'morning-star.p.rapidapi.com'
	}
};

try {
	const response = await fetch(url, options);
	const result = await response.text();
	console.log(result);
	return result;
} catch (error) {
	console.error(error);
	return '';
}
```

Vous pouvez maintenant le sauver.

#### Obtenir des actualités

Créez un nouvel outil avec les détails suivants (vous pouvez changer comme vous le souhaitez):

* Nom: get \ _stock \ _news
* Description: Obtenez les dernières nouvelles pour un stock
* Schéma d'entrée:
  * Propriété: PerformanceID
  * Type: chaîne
  * Description: ID du stock, qui est appelé performanceid dans l'API
  * Requis: vrai

Le schéma d'entrée indique à LLM quoi retourner en tant qu'objet JSON. Dans ce cas, nous nous attendons à un objet JSON comme ci-dessous:

<pre class = "Language-Json"> <code class = "Lang-json"> <strong> {"PerformanceId": "Some Ticker"}
</strong> </code> </pre>

<gigne> <img src = "../../../. GitBook / Assets / Image (4) (2) .png" alt = ""> <figcaption> </gigcaption> </gigu

* Fonction JavaScript: nous allons utiliser[Morning Star](https://rapidapi.com/apidojo/api/morning-star) `/news/list`API pour obtenir les données. Vous devez d'abord cliquer sur vous abonner au test si vous ne l'avez pas déjà fait, puis copiez le code et collez-le dans la fonction JavaScript.
  * Ajouter`const fetch = require('node-fetch');`en haut pour importer la bibliothèque. Vous pouvez importer tous les nodejs intégrés[modules](https://www.w3schools.com/nodejs/ref_modules.asp)et[external libraries](https://github.com/FlowiseAI/Flowise/blob/main/packages/components/src/utils.ts#L289).
  * Retourner le`result`à la fin.
* Ensuite, remplacez le paramètre de requête URL à code dur PerformanceId:`0P0000OQN8`à la variable de propriété spécifiée dans le schéma d'entrée:`$performanceId`
* Vous pouvez utiliser toutes les propriétés spécifiées dans le schéma d'entrée comme variables dans la fonction JavaScript en ajoutant un préfixe`# Outil personnalisé

Regardez comment utiliser des outils personnalisés

{% embed url = "https://youtu.be/hsp9lkktvy0"%}

## Problème

La fonction prend généralement des données d'entrée structurées. Disons que vous voulez que le LLM puisse appeler AirTable Create Record[API](https://airtable.com/developers/web/api/create-records), les paramètres du corps doivent être structurés de manière spécifique. Par exemple:

```json
"records": [
  {
    "fields": {
      "Address": "some address",
      "Name": "some name",
      "Visited": true
    }
  }
]
```

Idéalement, nous voulons que LLM renvoie des données structurées appropriées comme celle-ci:

```json
{
  "Address": "some address",
  "Name": "some name",
  "Visited": true
}
```

Nous pouvons donc extraire la valeur et l'analyser dans le corps nécessaire à l'API. Cependant, il est difficile de demander à LLM de produire le modèle exact.

Avec le nouveau[OpenAI Function Calling](https://openai.com/blog/function-calling-and-other-api-updates)modèles, c'est maintenant possible.`gpt-4-0613`et`gpt-3.5-turbo-0613`sont spécifiquement formés pour retourner des données structurées. Le modèle choisira intelligemment de sortir un objet JSON contenant des arguments pour appeler ces fonctions.

## Tutoriel

** Objectif **: Demandez à l'agent d'obtenir automatiquement le mouvement du cours de l'action, de récupérer les actualités des actions connexes et d'ajouter un nouveau record à AirTable.

Commençons[🚀](https://emojipedia.org/rocket/)

### Créer des outils

Nous avons besoin de 3 outils pour atteindre l'objectif:

* Obtenez un mouvement du cours des actions
* Obtenir des actualités
* Ajouter un enregistrement Airtable

#### Obtenez un mouvement du cours des actions

Créez un nouvel outil avec les détails suivants (vous pouvez changer comme vous le souhaitez):

* Nom: Get \ _Stock \ _Movers
* Description: Obtenez les actions qui ont des mouvements de prix / volume les plus importants, par exemple actifs, gagnants, perdants, etc.

La description est une pièce importante car Chatgpt s'appuie sur cela pour décider quand utiliser cet outil.

<gigne> <img src = "../../../. GitBook / Assets / Image (6) (3) .png" alt = ""> <figcaption> </gigcaption> </gigust>

* Fonction JavaScript: nous allons utiliser[Morning Star](https://rapidapi.com/apidojo/api/morning-star) `/market/v2/get-movers`API pour obtenir des données. Vous devez d'abord cliquer sur vous abonner au test si vous ne l'avez pas déjà fait, puis copiez le code et collez-le dans la fonction JavaScript.
  * Ajouter`const fetch = require('node-fetch');`en haut pour importer la bibliothèque. Vous pouvez importer tous les nodejs intégrés[modules](https://www.w3schools.com/nodejs/ref_modules.asp)et[external libraries](https://github.com/FlowiseAI/Flowise/blob/main/packages/components/src/utils.ts#L289).
  * Retourner le`result`à la fin.

<gigne> <img src = "../../../. GitBook / Assets / Untitled (4) (1) .png" alt = ""> <Figcaption> </ Figcaption> </gigust>

Le code final doit être:

```javascript
const fetch = require('node-fetch');
const url = 'https://morning-star.p.rapidapi.com/market/v2/get-movers';
const options = {
	method: 'GET',
	headers: {
		'X-RapidAPI-Key': 'replace with your api key',
		'X-RapidAPI-Host': 'morning-star.p.rapidapi.com'
	}
};

try {
	const response = await fetch(url, options);
	const result = await response.text();
	console.log(result);
	return result;
} catch (error) {
	console.error(error);
	return '';
}
```

Vous pouvez maintenant le sauver.

#### Obtenir des actualités

Créez un nouvel outil avec les détails suivants (vous pouvez changer comme vous le souhaitez):

* Nom: get \ _stock \ _news
* Description: Obtenez les dernières nouvelles pour un stock
* Schéma d'entrée:
  * Propriété: PerformanceID
  * Type: chaîne
  * Description: ID du stock, qui est appelé performanceid dans l'API
  * Requis: vrai

Le schéma d'entrée indique à LLM quoi retourner en tant qu'objet JSON. Dans ce cas, nous nous attendons à un objet JSON comme ci-dessous:

<pre class = "Language-Json"> <code class = "Lang-json"> <strong> {"PerformanceId": "Some Ticker"}
</strong> </code> </pre>

<gigne> <img src = "../../../. GitBook / Assets / Image (4) (2) .png" alt = ""> <figcaption> </gigcaption> </gigu

* Fonction JavaScript: nous allons utiliser[Morning Star](https://rapidapi.com/apidojo/api/morning-star) `/news/list`API pour obtenir les données. Vous devez d'abord cliquer sur vous abonner au test si vous ne l'avez pas déjà fait, puis copiez le code et collez-le dans la fonction JavaScript.
  * Ajouter`const fetch = require('node-fetch');`en haut pour importer la bibliothèque. Vous pouvez importer tous les nodejs intégrés[modules](https://www.w3schools.com/nodejs/ref_modules.asp)et[external libraries](https://github.com/FlowiseAI/Flowise/blob/main/packages/components/src/utils.ts#L289).
  * Retourner le`result`à la fin.
* Ensuite, remplacez le paramètre de requête URL à code dur PerformanceId:`0P0000OQN8`à la variable de propriété spécifiée dans le schéma d'entrée:`$performanceId`
* Vous pouvez utiliser toutes les propriétés spécifiées dans le schéma d'entrée comme variables dans la fonction JavaScript en ajoutant un préfixeà l'avant du nom de la variable.

<gigne> <img src = "../../../. Gitbook / Assets / Untitled (2) (1) (1) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

Code final:

```javascript
const fetch = require('node-fetch');
const url = 'https://morning-star.p.rapidapi.com/news/list?performanceId=' + $performanceId;
const options = {
	method: 'GET',
	headers: {
		'X-RapidAPI-Key': 'replace with your api key',
		'X-RapidAPI-Host': 'morning-star.p.rapidapi.com'
	}
};

try {
	const response = await fetch(url, options);
	const result = await response.text();
	console.log(result);
	return result;
} catch (error) {
	console.error(error);
	return '';
}
```

Vous pouvez maintenant le sauver.

#### Ajouter un enregistrement Airtable

Créez un nouvel outil avec les détails suivants (vous pouvez changer comme vous le souhaitez):

* Nom: Add \ _airable
* Description: Ajouter le stock, le résumé des nouvelles et le déménagement à AirTable
* Schéma d'entrée:
  * Propriété: stock
  * Type: chaîne
  * Description: Ticker d'origine
  * Requis: vrai
  * Propriété: déménager
  * Type: chaîne
  * Description: Déplacement des prix en%
  * Requis: vrai
  * Propriété: nouvelles \ _Summary
  * Type: chaîne
  * Description: Résumé des nouvelles du stock
  * Requis: vrai

Chatgpt renvoie un objet JSON comme ceci:

```json
{ "stock": "SOME TICKER", "move": "20%", "news_summary": "Some summary" }
```

<gigne> <img src = "../../../. GitBook / Assets / Image (36) .png" alt = ""> <Figcaption> </gigcaption> </gigne>

* Fonction JavaScript: nous allons utiliser[Airtable Create Record API](https://airtable.com/developers/web/api/create-records)Pour créer un nouvel enregistrement dans une table existante. Vous pouvez trouver le TableId et BaseID à partir de[here](https://www.highviewapps.com/kb/where-can-i-find-the-airtable-base-id-and-table-id/). Vous devrez également créer un jeton d'accès personnel, trouvez comment le faire[here](https://www.highviewapps.com/kb/how-do-i-create-an-airtable-personal-access-token/).

Le code final devrait ressembler ci-dessous. Notez comment nous passons`$stock`, `$move`et`$news_summary`Comme variables:

```javascript
const fetch = require('node-fetch');
const baseId = 'your-base-id';
const tableId = 'your-table-id';
const token = 'your-token';

const body = {
	"records": [
		{
			"fields": {
				"stock": $stock,
				"move": $move,
				"news_summary": $news_summary,
			}
		}
	]
};

const options = {
	method: 'POST',
	headers: {
		'Authorization': `Bearer ${token}`,
		'Content-Type': 'application/json'
	},
	body: JSON.stringify(body)
};

const url = `https://api.airtable.com/v0/${baseId}/${tableId}`

try {
	const response = await fetch(url, options);
	const text = await response.text();
	return text;
} catch (error) {
	console.error(error);
	return '';
}
```

Vous pouvez maintenant le sauver.

Vous devriez voir 3 outils créés:

<gigne> <img src = "../../../. Gitbook / Assets / Image (3) (3) (1) (1) .png" alt = ""> <Figcaption> </ / Figcaption> </ Figure>

### Créer ChatFlow

Vous pouvez utiliser le modèle ** Fonction Openai ** ** Agent ** à partir de Marketplace et remplacer les outils par ** outil personnalisé **. Sélectionnez l'outil que vous avez créé.

Remarque: L'agent de fonction OpenAI prend uniquement en charge les modèles 0613 actuellement.

<gigne> <img src = "../../../. Gitbook / Assets / Image (15) (1) (1) (1) (1) (1) (1) (1) .png" alt = ""> <Figcaption> </gigcaption> </gigu

Enregistrez le ChatFlow et commencez à le tester. Pour le démarreur, vous pouvez essayer de demander:

_ <Mark Style = "Color: Blue;"> Quelle est l'action qui a le plus gros mouvement des prix aujourd'hui? </CARK> _

_ <Mark Style = "Color: Orange;"> L'action qui a le plus grand mouvement de prix aujourd'hui est Overstock.com (OSTK) avec une décision de prix de 17,47%. </CARK> _

Vous pouvez ensuite suivre une autre question pour obtenir les nouvelles de ce stock particulier:

_ <Mark Style = "Color: Blue;"> Quelles sont les dernières nouvelles de ce stock qui pourraient provoquer le mouvement des prix? </mark> _

_ <Mark Style = "Color: Orange;"> Voici les dernières nouvelles sur Overstock.com (OSTK) qui auraient pu provoquer le mouvement des prix: </mark> _

1. _ <Mark Style = "Color: Orange;"> Titre: "Les actions de Overstock montent sur le succès de 21,5 millions d'offres pour le bain de lit et Beyond Actifs" Source: MarketWatch Publié Date: 22 juin 2023 Résumé: Les actions de OverStock.com ont connu une augmentation significative après avoir réussi à gagner une offre pour le bain de lit et au-delà des actifs d'une valeur de 21,5 millions.
2. _ <Mark Style = "Color: Orange;"> Titre: "Penser aux options de trading ou aux actions en plates-formes Meta, Overstock.com, Walmart, Home Depot ou United Parcel Service?" Source: PR Newswire Publié Date: 22 juin 2023 Résumé: Cet article d'actualité traite des options de négociation potentielles et des actions, y compris Overstock.com, que les investisseurs peuvent considérer. </mark> _

_ <Mark Style = "Color: Orange;"> Veuillez noter que ces articles de presse sont à des fins d'information uniquement et peuvent ne pas être la seule raison du mouvement des prix. Il est toujours recommandé de mener des recherches et des analyses approfondies avant de prendre des décisions d'investissement. </cark> _

Enfin, vous pouvez demander à Chatgpt d'ajouter un nouvel enregistrement à AirTable:

_ <Mark Style = "Color: Blue;"> Pouvez-vous ajouter un enregistrement à AirTable avec le ticker d'origine, le mouvement des prix et le résumé des nouvelles? </mark> _

_ <Mark Style = "Color: Orange;"> J'ai ajouté un enregistrement à AirTable avec les détails suivants: </mark> _

_ <Mark Style = "Color: Orange;"> Stock Ticker: Ostk Price Move: 17,47% News Résumé: Les actions d'OverStock.com ont connu une augmentation significative après avoir réussi à remporter une offre pour Bed Bath & Beyond d'actifs d'une valeur de 21,5 millions de dollars. </cark> _

[🎉](https://emojipedia.org/party-popper/)[🎉](https://emojipedia.org/party-popper/)** Voila! ** C'est ainsi que vous pouvez créer votre propre outil personnalisé et l'utiliser avec l'agent de fonction Openai!

## Supplémentaire

### Passer l'ID de session pour fonctionner

Par défaut, la fonction dans l'outil personnalisé a accès aux configurations de flux suivantes:

```json5
$flow.sessionId 
$flow.chatId
$flow.chatflowId
$flow.input
```

Vous trouverez ci-dessous un exemple de l'envoi du SessionID à Discord WebHook:

{% Tabs%}
{% tab title = "javascript"%}
```javascript
const fetch = require('node-fetch');
const webhookUrl = "https://discord.com/api/webhooks/1124783587267";
const content = $content; // captured from input schema
const sessionId = $flow.sessionId;

const body = {
	"content": `${mycontent} and the sessionid is ${sessionId}`
};

const options = {
	method: 'POST',
	headers: {
		'Content-Type': 'application/json'
	},
	body: JSON.stringify(body)
};

const url = `${webhookUrl}?wait=true`

try {
	const response = await fetch(url, options);
	const text = await response.text();
	return text;
} catch (error) {
	console.error(error);
	return '';
}
```
{% endtab%}
{% endtabs%}

### Passer les variables pour fonctionner

Dans certains cas, vous souhaitez passer les variables à la fonction de l'outil personnalisé.

Par exemple, vous créez un chatbot qui utilise un outil personnalisé. L'outil personnalisé exécute un appel post HTTP et une clé API est nécessaire pour une demande authentifiée réussie. Vous pouvez le passer comme variable.

Par défaut, la fonction dans l'outil personnalisé a accès aux variables:

```
$vars.<variable-name>
```

Exemple de la façon de passer les variables en flux à l'aide de l'API et intégrée:

{% Tabs%}
{% tab title = "JavaScript api"%}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflow-id>",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    );
    const result = await response.json();
    return result;
}

query({
    "question": "Hey, how are you?",
    "overrideConfig": {
        "vars": {
            "apiKey": "abc"
        }
    }
}).then((response) => {
    console.log(response);
});
```
{% endtab%}

{% tab title = "embed"%}
```html
<script type="module">
    import Chatbot from 'https://cdn.jsdelivr.net/npm/flowise-embed/dist/web.js';
    Chatbot.init({
        chatflowid: 'chatflow-id',
        apiHost: 'http://localhost:3000',
        chatflowConfig: {
          vars: {
            apiKey: 'def'
          }
        }
    });
</script>
```
{% endtab%}
{% endtabs%}

Exemple de la façon de recevoir les variables dans l'outil personnalisé:

{% Tabs%}
{% tab title = "javascript"%}
```javascript
const fetch = require('node-fetch');
const webhookUrl = "https://discord.com/api/webhooks/1124783587267";
const content = $content; // captured from input schema
const sessionId = $flow.sessionId;
const apiKey = $vars.apiKey;

const body = {
	"content": `${mycontent} and the sessionid is ${sessionId}`
};

const options = {
	method: 'POST',
	headers: {
		'Content-Type': 'application/json',
		'Authorization': `Bearer ${apiKey}`
	},
	body: JSON.stringify(body)
};

const url = `${webhookUrl}?wait=true`

try {
	const response = await fetch(url, options);
	const text = await response.text();
	return text;
} catch (error) {
	console.error(error);
	return '';
}
```
{% endtab%}
{% endtabs%}

### Remplacer l'outil personnalisé

Les paramètres ci-dessous peuvent être remplacés

| Paramètre | Description |
| ---------------- | ---------------- |
| CustomToolName | Nom de l'outil |
| CustomTooldeSC | Description de l'outil |
| CustomToolSchema | Schéma d'outil |
| CustomToolfunc | Fonction de l'outil |

Exemple d'un appel API pour remplacer les paramètres d'outil personnalisés:

{% Tabs%}
{% tab title = "JavaScript api"%}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflow-id>",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    );
    const result = await response.json();
    return result;
}

query({
    "question": "Hey, how are you?",
    "overrideConfig": {
        "customToolName": "example_tool",
        "customToolSchema": "z.object({title: z.string()})"
    }
}).then((response) => {
    console.log(response);
});
```
{% endtab%}
{% endtabs%}

### Importer des dépendances externes

Vous pouvez importer tous les nodejs intégrés[modules](https://www.w3schools.com/nodejs/ref_modules.asp)et soutenu[external libraries](https://github.com/FlowiseAI/Flowise/blob/main/packages/components/src/utils.ts#L289)en fonction.

1. Pour importer toutes les bibliothèques non soutenues, vous pouvez facilement ajouter le nouveau package NPM à`package.json`dans`packages/components`dossier.

```bash
cd Flowise && cd packages && cd components
pnpm add <your-library>
cd .. && cd ..
pnpm install
pnpm build
```

2. Ensuite, ajoutez les bibliothèques importées à`TOOL_FUNCTION_EXTERNAL_DEP`variable d'environnement. Référer[#builtin-and-external-dependencies](../../../configuration/environment-variables.md#builtin-and-external-dependencies "mention")pour plus de détails.
3. Démarrer l'application

```bash
pnpm start
```

4. Vous pouvez ensuite utiliser la bibliothèque nouvellement ajoutée dans la fonction ** javascript ** comme ça:

```javascript
const axios = require('axios')
```

Regardez comment ajouter des dépendances supplémentaires et des bibliothèques d'importation

{% embed url = "https://youtu.be/0h1rrisc0ok"%}
