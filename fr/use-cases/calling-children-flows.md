---
description: Learn how to effectively use the Chatflow Tool and the Custom Tool
---

# Appeler les enfants coule

***

L'une des caractéristiques puissantes de Flowise est que vous pouvez transformer les flux en outils. Par exemple, avoir un flux principal pour orchestrer qui / quand utiliser les outils nécessaires. Et chaque outil est conçu pour effectuer une nièce / une chose spécifique.

Cela offre quelques avantages:

* Chaque enfant coule comme l'outil s'exécutera seul, avec une mémoire séparée pour permettre une sortie plus propre
* L'agrégation des sorties détaillées de chaque enfant circule vers un agent final, se traduit souvent par une sortie de meilleure qualité

Vous pouvez y parvenir en utilisant les outils suivants:

* Outil Chatflow
* Outil personnalisé

## Outil Chatflow

1. Ayez un ChatFlow prêt. Dans ce cas, nous créons une chaîne de chat de pensée qui peut passer par plusieurs chaînes.

<gigne> <img src = "../. GitBook / Assets / Image (169) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

2. Créez un autre ChatFlow avec l'agent d'outils + outil ChatFlow. Sélectionnez le ChatFlow que vous souhaitez appeler dans l'outil. Dans ce cas, c'était une chaîne de pensée Chatflow. Donnez-lui un nom et une description appropriée pour permettre à LLM de savoir quand utiliser cet outil:

<gigne> <img src = "../. GitBook / Assets / Image (35) .png" alt = "" width = "245"> <figcaption> </gigcaption> </gigust>

3. Testez-le!

<gigne> <img src = "../. GitBook / Assets / Image (168) .png" alt = ""> <Figcaption> </gigcaption> </gigne>

4. Dans la réponse, vous pouvez voir l'entrée et la sortie de l'outil ChatFlow:

<gigne> <img src = "../. GitBook / Assets / Image (170) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

## Outil personnalisé

Avec le même exemple que ci-dessus, nous allons créer un outil personnalisé qui appellera le[Prediction API](broken-reference)de la chaîne de pensée Chatflow.

1. Créer un nouvel outil:

<Bile> <Thead> <Tr> <th width = "180"> Nom de l'outil </th> <th> outil Description </th> </tr> </thead> <tbody> <tr> <td> ideas_flow </td> <td> Utilisez cet outil lorsque vous avez besoin d'atteindre certains objectifs </td> </tr> </tbody> </ Table>

Schéma d'entrée:

<Bile> <THEAD> <TR> <TH> Propriété </th> <th> Type </th> <th> Description </th> <th data-type = "Checkbox"> Obligatoire </th> </tr> </thead> <tdbody> <tr> <td> Entrée </td> <td> string </td> <td> Entrée Question </td> <td> true </td> </tr> </tbody> </ table>

<gigne> <img src = "../. GitBook / Assets / Image (95) (1) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

Fonction JavaScript de l'outil:

```javascript
const fetch = require('node-fetch');
const url = 'http://localhost:3000/api/v1/prediction/<chatflow-id>'; // replace with specific chatflow id

const body = {
	"question": $input
};

const options = {
	method: 'POST',
	headers: {
		'Content-Type': 'application/json'
	},
	body: JSON.stringify(body)
};

try {
	const response = await fetch(url, options);
	const resp = await response.json();
	return resp.text;
} catch (error) {
	console.error(error);
	return '';
}
```

2. Créez un outil d'agent d'outils + personnalisé. Spécifiez l'outil que nous avons créé à l'étape 1 dans l'outil personnalisé.

<gigne> <img src = "../. GitBook / Assets / Image (97) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

3. À partir de la réponse, vous pouvez voir l'entrée et la sortie de l'outil personnalisé:

<gigne> <img src = "../. GitBook / Assets / Image (99) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

## Conclusion

Dans cet exemple, nous avons réussi à démontrer 2 façons de transformer d'autres chatflows en outils, via l'outil ChatFlow et l'outil personnalisé. Les deux utilisent la même logique de code sous le capot.
