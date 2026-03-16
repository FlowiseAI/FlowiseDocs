---
description: Learn how to use the Flowise Document Stores, written by @toi500
---

# Magasins de documents

***

Les magasins de documents de Flowise offrent une approche polyvalente de la gestion des données, vous permettant de télécharger, de fendre et de préparer votre ensemble de données et de le mettre en œuvre en un seul endroit.

Cette approche centralisée simplifie la gestion des données et permet une gestion efficace de divers formats de données, ce qui facilite l'organisation et l'accès à vos données dans l'application Flowise.

## Installation

Dans ce tutoriel, nous installerons un[Retrieval Augmented Generation (RAG)](broken-reference/)Système pour récupérer des informations sur la politique de maison de luxe _LiberTyGuard Policy_, un sujet sur lequel les LLM ne sont pas largement formées.

En utilisant les magasins de documents ** Flowise **, nous préparerons et améliorerons les données sur LibertyGuard et son ensemble de polices d'assurance habitation. Cela permettra à notre système de chiffon de répondre avec précision aux requêtes des utilisateurs sur les offres d'assurance habitation de Libertyguard.

## 1. Ajouter une boutique de documents

Commencez par ajouter un magasin de documents et le nommer. Dans notre cas, "Politique des propriétaires de Libertyguard Deluxe".

<gigne> <img src = "../. GitBook / Assets / DS01.png" alt = ""> <figCaption> </ Figcaption> </gigne>

## 2. Sélectionnez un chargeur de documents

Entrez le magasin de documents que vous venez de créer et sélectionnez le[Document Loader](../integrations/langchain/document-loaders/)vous souhaitez utiliser. Dans notre cas, puisque notre ensemble de données est au format PDF, nous utiliserons le[PDF Loader](../integrations/langchain/document-loaders/pdf-file.md).

Les chargeurs de documents sont des nœuds spécialisés qui gèrent l'ingestion de divers formats de documents.

<gigne> <img src = "../. GitBook / Assets / DS02.png" alt = ""> <figcaption> </gigcaption> </gigust>

<gigne> <img src = "../. GitBook / Assets / DS03.png" alt = ""> <Figcaption> </gigcaption> </gigne>

## 3. Préparez vos données

### Étape 1: chargeur de documents

* Tout d'abord, nous commençons par télécharger notre fichier PDF.
* Ensuite, nous ajoutons une ** clé de métadonnées uniques **. Ceci est facultatif, mais une bonne pratique car elle nous permet de cibler et de filtrer ce même ensemble de données plus tard si nous en avons besoin.
* Chaque chargeur est livré avec des métadonnées préconfigurées, dans certains cas, vous pouvez utiliser des clés de métadonnées omit pour éliminer les métadonnées inutiles.

<gigne> <img src = "../. GitBook / Assets / DS04.png" alt = ""> <Figcaption> </gigcaption> </gigne>

### Étape 2: séparateur de texte

* Sélectionnez le[Text Splitter](../integrations/langchain/text-splitters/)Vous souhaitez utiliser pour sélectionner vos données. Dans notre cas particulier, nous utiliserons le[Recursive Character Text Splitter](../integrations/langchain/text-splitters/recursive-character-text-splitter.md).
*   Le séparateur de texte est utilisé pour diviser les documents chargés en pièces, documents ou morceaux plus petits. Il s'agit d'une étape de prétraitement cruciale pour 2 raisons principales:

    * ** Vitesse et pertinence de récupération: ** Le stockage et l'interrogation de gros documents en tant qu'entités uniques dans une base de données vectorielle peuvent conduire à des temps de récupération plus lents et à des résultats potentiellement moins pertinents. La division du document en morceaux plus petits permet une récupération plus ciblée. En interrogeant contre des unités d'information plus petites et plus ciblées, nous pouvons atteindre des temps de réponse plus rapides et améliorer la précision des résultats récupérés.
    * ** RETENDANT: ** Puisque nous ne récupérons que des morceaux pertinents plutôt que le document entier, le nombre de jetons traités par le LLM est considérablement réduit. Cette approche de récupération ciblée se traduit directement par une baisse des coûts d'utilisation de notre LLM, car la facturation est généralement basée sur la consommation de jetons. En minimisant la quantité d'informations non pertinentes envoyées à la LLM, nous optimisons également pour le coût.

Il existe différentes stratégies de section de texte, notamment:

    * ** Clissage du texte des caractères: ** Divide le texte en morceaux d'un nombre fixe de caractères. Cette méthode est simple mais peut diviser des mots ou des phrases sur des morceaux, perturbant potentiellement le contexte.
    * ** Diffusion du texte de jeton: ** Segmentation du texte en fonction des limites des mots ou des schémas de tokenisation spécifiques au modèle d'intégration choisi. Cette approche conduit souvent à des morceaux plus cohérents sémantiquement, car il préserve les limites des mots et considère la structure linguistique sous-jacente du texte.
    * ** Diffusion du texte récursif du caractère: ** Cette stratégie vise à diviser le texte en morceaux qui maintiennent la cohérence sémantique tout en restant dans une limite de taille spécifiée. Il est particulièrement bien adapté aux documents hiérarchiques avec des sections ou des titres imbriqués. Au lieu de se diviser aveuglément à la limite de caractère, il analyse récursivement le texte pour trouver des points d'arrêt logiques, tels que les fins de phrase ou les ruptures de section. Cette approche garantit que chaque morceau représente une unité d'information significative, même si elle dépasse légèrement la taille cible.
    * ** Splitter de texte de marque: ** Conçu spécifiquement pour les documents formulés Markdown, ce séparateur segmente logiquement le texte basé sur des en-têtes de démarque et des éléments structurels, créant des morceaux qui correspondent à des sections logiques dans le document.
    * ** Splitter de texte de code: ** Adapté pour la division des fichiers de code, cette stratégie considère la structure du code, les définitions de fonction et d'autres éléments spécifiques au langage de programmation pour créer des morceaux significatifs qui conviennent aux tâches telles que la recherche et la documentation de code.
    * ** Splitter de texte HTML à markdown: ** Ce séparateur spécialisé convertit d'abord le contenu HTML à Markdown, puis applique le séparateur de texte Markdown, permettant une segmentation structurée des pages Web et d'autres documents HTML.

Vous pouvez également personnaliser les paramètres tels que:

    * ** Taille du morceau: ** La taille maximale souhaitée de chaque morceau, généralement définie en caractères ou en jetons.
    * ** chevauchement de morceaux: ** Le nombre de caractères ou de jetons à chevaucher entre des morceaux consécutifs, utile pour maintenir le flux contextuel à travers des morceaux.

{% hint style = "info"%}
Dans ce guide, nous avons ajouté une généreuse taille ** de chevauchement ** pour nous assurer qu'aucune donnée pertinente ne manque de morceaux. Cependant, la taille optimale du chevauchement dépend de la complexité de vos données. Vous devrez peut-être ajuster cette valeur en fonction de votre ensemble de données spécifique et de la nature des informations que vous souhaitez extraire.
{% EndHint%}

<gigne> <img src = "../. GitBook / Assets / DS05.png" alt = "" width = "563"> <figcaption> </gigcaption> </gigust>

## 4. Aperçu de vos données

Nous pouvons désormais prévisualiser comment nos données seront ouvertes en utilisant notre actuel[Text Splitter](../integrations/langchain/text-splitters/)configuration;`chunk_size=1500`et`chunk_overlap=750`.

<gigne> <img src = "../. GitBook / Assets / ds06.png" alt = ""> <figcaption> </gigcaption> </gigne>

Il est important d'expérimenter avec différents[Text Splitters](../integrations/langchain/text-splitters/), Tailles de morceaux et se chevaucher des valeurs pour trouver la configuration optimale pour votre ensemble de données spécifique. Cet aperçu vous permet d'affiner le processus de chasse et de vous assurer que les morceaux résultants conviennent à votre système de chiffon.

<gigne> <img src = "../. GitBook / Assets / DS07.png" alt = ""> <Figcaption> </gigcaption> </gigne>

{% hint style = "info"%}
Notez que nos métadonnées personnalisées`company: "liberty"`a été inséré dans chaque morceau. Ces métadonnées nous permettent de filtrer et de récupérer facilement des informations à partir de cet ensemble de données spécifiques plus tard, même si nous utilisons le même index de magasin vectoriel pour d'autres ensembles de données.
{% EndHint%}

### Comprendre le chevauchement de morceaux <a href = "# compréhension-chunk-overl" id = "compréhension-chunk-overlap"> </a>

Dans le contexte de la récupération basée sur les vecteurs et de la requête LLM, le chevauchement de morceaux joue un ** rôle important dans le maintien de la continuité contextuelle ** et ** Amélioration de la précision de la réponse **, en particulier lorsqu'il s'agit d'une profondeur de récupération limitée ou ** Top K **, qui est le paramètre qui détermine le nombre maximum de la plupart des morceaux similaires qui sont récupérés à partir de la[Vector Store](https://docs.flowiseai.com/integrations/langchain/vector-stores)en réponse à une requête.

Pendant le traitement des requêtes, le LLM exécute une recherche de similitude contre le magasin vectoriel pour récupérer les morceaux les plus pertinents sémantiquement à la requête donnée. Si la profondeur de récupération, représentée par le paramètre K supérieur, est définie sur une petite valeur, 4 pour par défaut, le LLM utilise initialement des informations uniquement à partir de ces 4 morceaux pour générer sa réponse.

Ce scénario nous présente un problème, car le fait de s'appuyer uniquement sur un nombre limité de morceaux sans chevauchement peut entraîner des réponses incomplètes ou inexactes, en particulier lorsqu'ils traitent des requêtes qui nécessitent des informations couvrant plusieurs morceaux.

Le chevauchement des morceaux aide à ce problème en s'assurant qu'une partie du contexte textuel est partagée sur des morceaux consécutifs, ** augmentant la probabilité que toutes les informations pertinentes pour une requête donnée soient contenues dans les morceaux récupérés **.

En d'autres termes, ce chevauchement sert de pont entre des morceaux, permettant au LLM d'accéder à une fenêtre contextuelle plus large même lorsqu'elle est limitée à un petit ensemble de morceaux récupérés (haut K). Si une requête est liée à un concept ou à une information qui s'étend au-delà d'un seul morceau, les régions qui se chevauchent augmentent la probabilité de capturer tout le contexte nécessaire.

Par conséquent, en introduisant un chevauchement de morceaux pendant la phase de division du texte, nous améliorons la capacité du LLM à:

1. ** Préserver la continuité contextuelle: ** Les morceaux qui se chevauchent fournissent une transition plus fluide des informations entre les segments consécutifs, permettant au modèle de maintenir une compréhension plus cohérente du texte.
2. ** Améliorer la précision de la récupération: ** En augmentant la probabilité de capturer toutes les informations pertinentes dans le top k cible k récupéré, le chevauchement contribue à des réponses plus précises et plus appropriées.

### Précision vs coût <a href = "# précision-vs.-cost" id = "précision-vs.-cost"> </a>

Ainsi, pour optimiser davantage le compromis entre la précision de la récupération et le coût, deux stratégies primaires peuvent être utilisées:

1. ** Le chevauchement d'augmentation / diminution du morceau: ** L'ajustement du pourcentage de chevauchement pendant la division de texte permet un contrôle à grain fin sur la quantité de contexte partagé entre les morceaux. Des pourcentages de chevauchement plus élevés entraînent généralement une amélioration de la préservation du contexte, mais peuvent également augmenter les coûts, car vous devez utiliser plus de morceaux pour englober l'ensemble du document. À l'inverse, des pourcentages de chevauchement plus faibles peuvent réduire les coûts, mais risquent de perdre des informations contextuelles clés entre les morceaux, conduisant potentiellement à des réponses moins précises ou incomplètes du LLM.
2. ** Augmentation / diminution du top k: ** L'augmentation de la valeur K supérieure par défaut (4) élargit le nombre de morceaux considérés pour la génération de réponse. Bien que cela puisse améliorer la précision, cela augmente également les coûts.

** Astuce: ** Le choix des valeurs optimales ** des chevauchement ** et ** Top K ** dépend de facteurs tels que la complexité du document, les caractéristiques du modèle d'intégration et l'équilibre souhaité entre la précision et le coût. L'expérimentation avec ces valeurs est importante pour trouver la configuration idéale pour un besoin spécifique.

## 5. Traitez vos données

Une fois que vous êtes satisfait du processus de chasse, il est temps de traiter vos données.

<gigne> <img src = "../. GitBook / Assets / DS08.png" alt = ""> <Figcaption> </gigcaption> </gigust>

<gigne> <img src = "../. GitBook / Assets / DS09% 20 (1) .png" alt = ""> <figcaption> </gigcaption> </gigust>

Après avoir traité vos données, vous conservez la possibilité d'affiner des morceaux individuels en supprimant ou en ajoutant du contenu. Ce contrôle granulaire offre plusieurs avantages:

* ** Précision améliorée: ** Identifier et rectifier les inexactitudes ou les incohérences présentes dans les données d'origine, garantissant que les informations utilisées dans votre application sont fiables.
* ** Amélioration de la pertinence: ** Affinez le contenu de morceaux pour souligner les informations clés et supprimer des sections non pertinentes, augmentant ainsi la précision et l'efficacité de votre processus de récupération.
* ** Optimisation des requêtes: ** Taigor Chunks pour mieux s'aligner sur les requêtes utilisateur prévues, ce qui les rend plus ciblés et améliore l'expérience utilisateur globale.

## 6. Configurez le processus Upsert

Avec nos données correctement traitées - chargées via un chargeur de documents et de manière appropriée - nous pouvons maintenant procéder à la configuration du processus Upsert.

<gigne> <img src = "../. GitBook / Assets / Dastore002.png" alt = ""> <Figcaption> </gigcaption> </gigust>

Le processus Upsert comprend trois étapes fondamentales:

* ** Incorporation: ** Nous commençons par choisir le modèle d'intégration approprié pour coder notre ensemble de données. Ce modèle transformera nos données en une représentation vectorielle numérique.
* ** Store vectoriel: ** Ensuite, nous déterminons le magasin vectoriel où résidera notre ensemble de données.
* ** Record Manager (facultatif): ** Enfin, nous avons la possibilité d'implémenter un gestionnaire d'enregistrements. Ce composant fournit les fonctionnalités pour gérer notre ensemble de données une fois qu'il est stocké dans le magasin vectoriel.

<gigne> <img src = "../. GitBook / Assets / Dastore003.png" alt = ""> <figCaption> </gigcaption> </ figure>

### Étape 1: Sélectionnez des incorporations

Cliquez sur la carte "Sélectionner des incorporations" et choisissez votre préférée[embedding model](../integrations/langchain/embeddings/). Dans notre cas, nous sélectionnerons OpenAI comme fournisseur d'incorporation et utiliserons le`text-embedding-ada-002`modéliser avec`1536`dimensions.

L'intégration est le processus de conversion du texte en une représentation numérique qui capture sa signification. Cette représentation numérique, également appelée vecteur d'intégration, est un tableau de nombres multidimensionnel, où chaque dimension représente un aspect spécifique de la signification du texte.

Ces vecteurs permettent aux LLM de comparer et de rechercher des morceaux de texte similaires dans le magasin vectoriel en mesurant la distance ou la similitude entre eux dans cet espace multidimensionnel.

#### Comprendre les dimensions des intégres / vectoriels <a href = "# compréhension-embeddings-vector-store-dimensions" id = "compréhension-embeddings-vect-store-dimensions"> </a>

Le nombre de dimensions dans un indice de magasin vectoriel est déterminé par le modèle d'incorporation utilisé lorsque nous augmentons nos données, et vice versa. Chaque dimension représente une fonction ou un concept spécifique dans les données. Par exemple, une ** dimension ** pourrait ** représenter un sujet, un sentiment ou un autre aspect particulier du texte **.

Plus nous utilisons de dimensions pour intégrer nos données, plus le potentiel de capture de sens nuancé de notre texte est grand. Cependant, cette augmentation se fait au prix des exigences de calcul plus élevées par requête.

En général, un plus grand nombre de dimensions nécessite plus de ressources pour stocker, traiter et comparer les vecteurs d'intégration résultants. Par conséquent, des modèles intégrés comme le Google`embedding-001`, qui utilise 768 dimensions, sont, en théorie, moins chères que d'autres comme l'Openai`text-embedding-3-large`, avec 3072 dimensions.

Il est important de noter que la ** relation entre les dimensions et la capture de sens n'est pas strictement linéaire **; Il y a un point de rendement décroissant où l'ajout de dimensions offre un avantage négligeable pour le coût inutile supplémentaire.

{% hint style = "avertissement"%}
Pour garantir la compatibilité entre un modèle d'incorporation et un indice de magasin vectoriel, l'alignement dimensionnel est essentiel. Les deux ** Le modèle d'intégration et l'indice du magasin vectoriel doivent avoir le même nombre de dimensions **. L'inadéquation de la dimensionnalité entraînera des erreurs de mise en service, car le magasin vectoriel est conçu pour gérer les vecteurs d'une taille spécifique déterminée par le modèle d'incorporation choisi.
{% EndHint%}

<gigne> <img src = "../. GitBook / Assets / Dastore004.png" alt = ""> <Figcaption> </gigcaption> </ Figure>

### Étape 2: Sélectionnez le magasin vectoriel

Cliquez sur la carte "Sélectionnez Vector Store" et choisissez votre préféré[Vector Store](../integrations/langchain/vector-stores/). Dans notre cas, comme nous avons besoin d'une option prête pour la production, nous sélectionnerons Upstash.

Vector Store est un type spécial de base de données qui est utilisé pour stocker les incorporations vectorielles. Nous pouvons par des paramètres finetune comme "** top k **" qui détermine le nombre maximum des morceaux les plus similaires qui sont récupérés du magasin vectoriel en réponse à une requête.

{% hint style = "info"%}
Une valeur K supérieure inférieure donnera des résultats moins, mais potentiellement plus pertinents, tandis qu'une valeur plus élevée renverra une gamme plus large de résultats, capturant potentiellement plus d'informations.
{% EndHint%}

<gigne> <img src = "../. GitBook / Assets / Dastore0055.png" alt = ""> <Figcaption> </gigcaption> </gigust>

### Étape 3: Sélectionnez Record Manager

Record Manager est un ajout facultatif mais incroyablement utile à notre flux de mise en valeur. Il nous permet de maintenir des enregistrements de tous les morceaux qui ont été renversés vers notre magasin vectoriel, nous permettant d'ajouter ou de supprimer efficacement des morceaux au besoin.

En d'autres termes, toute modification de vos documents lors d'un nouvel upsert n'entraînera pas les intérêts de vecteur en double stockés dans le magasin vectoriel.

Des instructions détaillées sur la façon de configurer et d'utiliser cette fonctionnalité peuvent être trouvées dans le dédié[guide](../integrations/langchain/record-managers.md).

<Figure> <img src = "../. GitBook / Assets / Dastore006.png" alt = ""> <Figcaption> </gigcaption> </ Figure>

## 7. Upser vos données à un magasin vectoriel

Pour commencer le processus Upsert et transférer vos données dans le magasin vectoriel, cliquez sur le bouton "Upsert".

<gigne> <img src = "../. GitBook / Assets / Dastore013.png" alt = ""> <figcaption> </gigcaption> </gigust>

Comme illustré dans l'image ci-dessous, nos données ont été déposées avec succès dans la base de données Vector Upstash. Les données ont été divisées en 85 morceaux pour optimiser le processus de mise en service et assurer un stockage et une récupération efficaces.

<gigne> <img src = "../. GitBook / Assets / Dastore007.png" alt = "" width = "375"> <Figcaption> </gigcaption> </ Figure>

## 8. Testez votre ensemble de données

Pour tester rapidement les fonctionnalités de votre ensemble de données sans vous éloigner du magasin de documents, utilisez simplement le bouton "Retrouver Requête". Cela initie une requête de test, vous permettant de vérifier la précision et l'efficacité de votre processus de récupération de données.

<gigne> <img src = "../. GitBook / Assets / Dastore010.png" alt = ""> <figcaption> </gigcaption> </gigust>

Dans notre cas, nous voyons que lorsque vous interrogez pour des informations sur la couverture des revêtements de sol de la cuisine dans notre police d'assurance, nous récupérons 4 morceaux pertinents de Upstash, notre magasin vectoriel désigné. Cette récupération est limitée à 4 morceaux selon le paramètre "Top K" défini, garantissant que nous recevons les informations les plus pertinentes sans redondance inutile.

<gigne> <img src = "../. GitBook / Assets / Dastore009.png" alt = ""> <figCaption> </ Figcaption> </gigne>

## 9. Testez votre chiffon

Enfin, notre système de génération (RAG) (RAG) de la récupération est opérationnel. Il convient de noter comment le LLM interprète efficacement la requête et exploite avec succès les informations pertinentes des données en morceaux pour construire une réponse complète.

#### Agent Flow

Avec un nœud d'agent, vous pouvez ajouter la boutique de documents:

<gigne> <img src = "../. GitBook / Assets / image (4) (1) (1) (1) .png" alt = "" width = "300"> <figcaption> </gigcaption> </gigust>

<gigne> <img src = "../. GitBook / Assets / Image (5) (1) (1) .png" alt = "" width = "407"> <Figcaption> </gigcaption> </gigust>

Ou se connecter directement à la base de données vectorielle et au mode d'intégration:

<gigne> <img src = "../. GitBook / Assets / image (6) (1) (1) .png" alt = "" width = "394"> <Figcaption> </ Figcaption> </gigne>

#### Chatflow

Vous pouvez utiliser le magasin vectoriel configuré plus tôt:

<gigne> <img src = "../. GitBook / Assets / Dastore011.png" alt = ""> <Figcaption> </gigcaption> </gigust>

Ou utilisez la boutique de documents (vecteur):

<gigne> <img src = "../. GitBook / Assets / Image (215) .png" alt = ""> <figcaption> </gigcaption> </gigust>

## 10. API

Il existe également une prise en charge des API pour la création, la mise à jour et la suppression de la boutique de documents. Dans cette section, nous allons mettre en évidence les 2 des API les plus utilisées:

* Ascension
* Rafraîchir

Pour plus de détails, voir le[Document Store API Reference](../api-reference/document-store.md).

### API Upsert

Il existe quelques scénarios différents pour l'amélioration du processus, et chacun a des résultats différents.

#### Scénario 1: Dans le même magasin de documents, utilisez une configuration de chargeur de document existante, Upsert comme nouveau chargeur de documents.

<gigne> <img src = "../. GitBook / Assets / Untitled-2025-02-02-1727.png" alt = "" width = "496"> <Figcaption> </ Figcaption> </ Figure>

{% Hint Style = "Success"%}
**`docId`** représente l'ID de chargeur de document existant. Il est nécessaire dans le corps de la demande pour ce scénario.
{% EndHint%}

{% Tabs%}
{% tab title = "python"%}
```python
import requests
import json

DOC_STORE_ID = "your_doc_store_id"
DOC_LOADER_ID = "your_doc_loader_id"
API_URL = f"http://localhost:3000/api/v1/document-store/upsert/{DOC_STORE_ID}"
API_KEY = "your_api_key_here"

form_data = {
    "files": ('my-another-file.pdf', open('my-another-file.pdf', 'rb'))
}

body_data = {
    "docId": DOC_LOADER_ID
}

headers = {
    "Authorization": f"Bearer {BEARER_TOKEN}"
}

def query(form_data):
    response = requests.post(API_URL, files=form_data, data=body_data, headers=headers)
    print(response)
    return response.json()

output = query(form_data)
print(output)
```
{% endtab%}

{% tab title = "javascript"%}
```javascript
const DOC_STORE_ID = "your_doc_store_id"
const DOC_LOADER_ID = "your_doc_loader_id"

let formData = new FormData();
formData.append("files", input.files[0]);
formData.append("docId", DOC_LOADER_ID)

async function query(formData) {
    const response = await fetch(
        `http://localhost:3000/api/v1/document-store/upsert/${DOC_STORE_ID}`,
        {
            method: "POST",
            headers: {
                "Authorization": "Bearer <your_api_key_here>"
            },
            body: formData
        }
    );
    const result = await response.json();
    return result;
}

query(formData).then((response) => {
    console.log(response);
});
```
{% endtab%}
{% endtabs%}

#### Scénario 2: Dans le même magasin de documents, remplacez un chargeur de document existant par de nouveaux fichiers.

<gigne> <img src = "../. GitBook / Assets / Untitled-2025-03-02-1727.png" alt = "" width = "563"> <figCaption> </gigcaption> </ Figure>

{% Hint Style = "Success"%}
**`docId`** et **`replaceExisting`** sont tous deux requis dans le corps de la demande pour ce scénario.
{% EndHint%}

{% Tabs%}
{% tab title = "python"%}
```python
import requests
import json

DOC_STORE_ID = "your_doc_store_id"
DOC_LOADER_ID = "your_doc_loader_id"
API_URL = f"http://localhost:3000/api/v1/document-store/upsert/{DOC_STORE_ID}"
API_KEY = "your_api_key_here"

form_data = {
    "files": ('my-another-file.pdf', open('my-another-file.pdf', 'rb'))
}

body_data = {
    "docId": DOC_LOADER_ID,
    "replaceExisting": True
}

headers = {
    "Authorization": f"Bearer {BEARER_TOKEN}"
}

def query(form_data):
    response = requests.post(API_URL, files=form_data, data=body_data, headers=headers)
    print(response)
    return response.json()

output = query(form_data)
print(output)
```
{% endtab%}

{% tab title = "javascript"%}
```javascript
const DOC_STORE_ID = "your_doc_store_id";
const DOC_LOADER_ID = "your_doc_loader_id";

let formData = new FormData();
formData.append("files", input.files[0]);
formData.append("docId", DOC_LOADER_ID);
formData.append("replaceExisting", true);

async function query(formData) {
    const response = await fetch(
        `http://localhost:3000/api/v1/document-store/upsert/${DOC_STORE_ID}`,
        {
            method: "POST",
            headers: {
                "Authorization": "Bearer <your_api_key_here>"
            },
            body: formData
        }
    );
    const result = await response.json();
    return result;
}

query(formData).then((response) => {
    console.log(response);
});
```
{% endtab%}
{% endtabs%}

#### Scénario 3: dans le même magasin de documents, Upsert que le nouveau chargeur de documents à partir de zéro.

<gigne> <img src = "../. GitBook / Assets / Untitled-2025-04-02-1727.png" alt = "" width = "439"> <figcaption> </gigcaption> </ Figure>

{% Hint Style = "Success"%}
**`loader`, `splitter`, `embedding`, `vectorStore`** sont tous requis dans le corps de la demande pour ce scénario. **`recordManager`** est facultatif.
{% EndHint%}

{% Tabs%}
{% tab title = "python"%}
```python
import requests
import json

DOC_STORE_ID = "your_doc_store_id"
API_URL = f"http://localhost:3000/api/v1/document-store/upsert/{DOC_STORE_ID}"
API_KEY = "your_api_key_here"

form_data = {
    "files": ('my-another-file.pdf', open('my-another-file.pdf', 'rb'))
}

loader = {
    "name": "pdfFile",
    "config": {} # you can leave empty to use default config
}

splitter = {
    "name": "recursiveCharacterTextSplitter",
    "config": {
        "chunkSize": 1400,
        "chunkOverlap": 100
    }
}

embedding = {
    "name": "openAIEmbeddings",
    "config": {
        "modelName": "text-embedding-ada-002",
        "credential": <your_credential_id>
    }
}

vectorStore = {
    "name": "pinecone",
    "config": {
        "pineconeIndex": "exampleindex",
        "pineconeNamespace": "examplenamespace",
        "credential":  <your_credential_i
    }
}

body_data = {
    "docId": DOC_LOADER_ID,
    "loader": json.dumps(loader),
    "splitter": json.dumps(splitter),
    "embedding": json.dumps(embedding),
    "vectorStore": json.dumps(vectorStore)
}

headers = {
    "Authorization": f"Bearer {BEARER_TOKEN}"
}

def query(form_data):
    response = requests.post(API_URL, files=form_data, data=body_data, headers=headers)
    print(response)
    return response.json()

output = query(form_data)
print(output)
```
{% endtab%}

{% tab title = "javascript"%}
```javascript
const DOC_STORE_ID = "your_doc_store_id";
const API_URL = `http://localhost:3000/api/v1/document-store/upsert/${DOC_STORE_ID}`;
const API_KEY = "your_api_key_here";

const formData = new FormData();
formData.append("files", new Blob([await (await fetch('my-another-file.pdf')).blob()]), "my-another-file.pdf");

const loader = {
    name: "pdfFile",
    config: {} // You can leave empty to use the default config
};

const splitter = {
    name: "recursiveCharacterTextSplitter",
    config: {
        chunkSize: 1400,
        chunkOverlap: 100
    }
};

const embedding = {
    name: "openAIEmbeddings",
    config: {
        modelName: "text-embedding-ada-002",
        credential: "your_credential_id"
    }
};

const vectorStore = {
    name: "pinecone",
    config: {
        pineconeIndex: "exampleindex",
        pineconeNamespace: "examplenamespace",
        credential: "your_credential_id"
    }
};

const bodyData = {
    docId: "DOC_LOADER_ID",
    loader: JSON.stringify(loader),
    splitter: JSON.stringify(splitter),
    embedding: JSON.stringify(embedding),
    vectorStore: JSON.stringify(vectorStore)
};

const headers = {
    "Authorization": `Bearer BEARER_TOKEN`
};

async function query() {
    try {
        const response = await fetch(API_URL, {
            method: "POST",
            headers: headers,
            body: formData
        });

        const result = await response.json();
        console.log(result);
        return result;
    } catch (error) {
        console.error("Error:", error);
    }
}

query();

```
{% endtab%}
{% endtabs%}

{% hint style = "danger"%}
La création à partir de zéro n'est pas recommandée car elle expose votre identifiant d'identification. Le moyen recommandé est de créer un magasin de documents d'espace réservé et de configurer les paramètres de l'interface utilisateur. Utilisez ensuite l'espace réservé comme base pour ajouter un nouveau chargeur de documents ou créer un nouveau magasin de documents.
{% EndHint%}

#### Scénario 4: Créez une nouvelle boutique de documents pour chaque upsert

<gigne> <img src = "../. GitBook / Assets / Untitled-2025-056-02-1727.png" alt = "" width = "533"> <Figcaption> </ Figcaption> </ Figure>

{% Hint Style = "Success"%}
**`createNewDocStore`** et **`docStore`** sont tous deux requis dans le corps de la demande pour ce scénario.
{% EndHint%}

{% Tabs%}
{% tab title = "python"%}
```python
import requests
import json

DOC_STORE_ID = "your_doc_store_id"
DOC_LOADER_ID = "your_doc_loader_id"
API_URL = f"http://localhost:3000/api/v1/document-store/upsert/{DOC_STORE_ID}"
API_KEY = "your_api_key_here"

form_data = {
    "files": ('my-another-file.pdf', open('my-another-file.pdf', 'rb'))
}

body_data = {
    "docId": DOC_LOADER_ID,
    "createNewDocStore": True,
    "docStore": json.dumps({"name":"My NEW Doc Store"})
}

headers = {
    "Authorization": f"Bearer {BEARER_TOKEN}"
}

def query(form_data):
    response = requests.post(API_URL, files=form_data, data=body_data, headers=headers)
    print(response)
    return response.json()

output = query(form_data)
print(output)
```
{% endtab%}

{% tab title = "javascript"%}
```javascript
const DOC_STORE_ID = "your_doc_store_id";
const DOC_LOADER_ID = "your_doc_loader_id";

let formData = new FormData();
formData.append("files", input.files[0]);
formData.append("docId", DOC_LOADER_ID);
formData.append("createNewDocStore", true);
formData.append("docStore", JSON.stringify({ "name": "My NEW Doc Store" }));

async function query(formData) {
    const response = await fetch(
        `http://localhost:3000/api/v1/document-store/upsert/${DOC_STORE_ID}`,
        {
            method: "POST",
            headers: {
                "Authorization": "Bearer <your_api_key_here>"
            },
            body: formData
        }
    );
    const result = await response.json();
    return result;
}

query(formData).then((response) => {
    console.log(response);
});
```
{% endtab%}
{% endtabs%}

#### Q: Où trouver l'ID de la boutique de documents et l'ID de chargeur de documents?

R: Vous pouvez trouver les ID respectifs de l'URL.

<gigne> <img src = "../. GitBook / Assets / Picture1 (1) .png" alt = ""> <Figcaption> </gigcaption> </gigne>

#### Q: Où puis-je trouver les configurations disponibles pour remplacer?

A: Vous pouvez trouver les configurations disponibles à partir du bouton API ** Affichage ** sur chaque chargeur de document:

<gigne> <img src = "../. GitBook / Assets / Image (4) (3) .png" alt = ""> <FigCaption> </gigcaption> </gigust>

<gigne> <img src = "../. GitBook / Assets / Image (2) (6) .png" alt = ""> <Figcaption> </ Figcaption> </gigne>

Pour chaque usiser, il y a 5 éléments impliqués:

* **`loader`**
* **`splitter`**
* **`embedding`**
* **`vectorStore`**
* **`recordManager`**

Vous pouvez remplacer la configuration existante avec le **`config`** Corps de l'élément. Par exemple, en utilisant la capture d'écran ci-dessus, vous pouvez créer un nouveau chargeur de document avec un nouveau **`url`**:

{% Tabs%}
{% tab title = "python"%}
```python
import requests

API_URL = "http://localhost:3000/api/v1/document-store/upsert/<storeId>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()

output = query({
    "docId": <docLoaderId>,
    # override existing configuration
    "loader": {
        "config": {
            "url": "https://new-url.com"
        }
    }
})
print(output)
```
{% endtab%}

{% tab title = "javascript"%}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/document-store/upsert/<storeId>",
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
    "docId": <docLoaderId>,
    // override existing configuration
    "loader": {
        "config": {
            "url": "https://new-url.com"
        }
    }
}).then((response) => {
    console.log(response);
});
```
{% endtab%}
{% endtabs%}

Et si le chargeur a le téléchargement de fichiers? Oui, vous l'avez bien deviné, nous devons utiliser les données de formulaire comme corps!

En utilisant l'image ci-dessous comme exemple, nous pouvons remplacer le **`usage`** Paramètre du chargeur de fichiers PDF comme tel:

<gigne> <img src = "../. GitBook / Assets / Image (4) (3) (1) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

{% Tabs%}
{% tab title = "python"%}
```python
import requests
import json

API_URL = "http://localhost:3000/api/v1/document-store/upsert/<storeId>"
API_KEY = "your_api_key_here"

form_data = {
    "files": ('my-another-file.pdf', open('my-another-file.pdf', 'rb'))
}

override_loader_config = {
    "config": {
        "usage": "perPage"
    }
}

body_data = {
    "docId": <docLoaderId>,
    "loader": json.dumps(override_loader_config) # Override existing configuration
}

headers = {
    "Authorization": f"Bearer {BEARER_TOKEN}"
}

def query(form_data):
    response = requests.post(API_URL, files=form_data, data=body_data, headers=headers)
    print(response)
    return response.json()

output = query(form_data)
print(output)
```
{% endtab%}

{% tab title = "javascript"%}
```javascript
const DOC_STORE_ID = "your_doc_store_id";
const DOC_LOADER_ID = "your_doc_loader_id";

const overrideLoaderConfig = {
    "config": {
        "usage": "perPage"
    }
}

let formData = new FormData();
formData.append("files", input.files[0]);
formData.append("docId", DOC_LOADER_ID);
formData.append("loader", JSON.stringify(overrideLoaderConfig));

async function query(formData) {
    const response = await fetch(
        `http://localhost:3000/api/v1/document-store/upsert/${DOC_STORE_ID}`,
        {
            method: "POST",
            headers: {
                "Authorization": "Bearer <your_api_key_here>"
            },
            body: formData
        }
    )
    const result = await response.json();
    return result;
}

query(formData).then((response) => {
    console.log(response);
});e
```
{% endtab%}
{% endtabs%}

#### Q: Quand utiliser les données de formulaire vs JSON comme le corps de la demande d'API?

R: pour[Document Loaders](../integrations/langchain/document-loaders/)qui ont des fonctionnalités de téléchargement de fichiers, telles que PDF, DOCX, TXT, etc., le corps doit être envoyé sous forme de données de formulaire.

{% hint style = "avertissement"%}
Assurez-vous que le type de fichier envoyé est compatible avec le type de fichier attendu à partir du chargeur de documents.

Par exemple, si un[PDF File Loader](../integrations/langchain/document-loaders/pdf-file.md)est utilisé, vous ne devriez envoyer que **. PDF ** Fichiers.

Pour éviter d'avoir des chargeurs séparés pour différents types de fichiers, nous vous recommandons d'utiliser[File Loader](../integrations/langchain/document-loaders/file-loader.md)
{% EndHint%}

{% Tabs%}
{% Tab Title = "Python API"%}
```python
import requests
import json

API_URL = "http://localhost:3000/api/v1/document-store/upsert/<storeId>"

# use form data to upload files
form_data = {
    "files": ('my-another-file.pdf', open('my-another-file.pdf', 'rb'))
}

body_data = {
    "docId": <docId>
}

def query(form_data):
    response = requests.post(API_URL, files=form_data, data=body_data)
    print(response)
    return response.json()

output = query(form_data)
print(output)
```
{% endtab%}

{% tab title = "JavaScript api"%}
```javascript
// use FormData to upload files
let formData = new FormData();
formData.append("files", input.files[0]);
formData.append("docId", <docId>);

async function query(formData) {
    const response = await fetch(
        "http://localhost:3000/api/v1/document-store/upsert/<storeId>",
        {
            method: "POST",
            body: formData
        }
    );
    const result = await response.json();
    return result;
}

query(formData).then((response) => {
    console.log(response);
});
```
{% endtab%}
{% endtabs%}

Pour d'autres[Document Loaders](https://docs.flowiseai.com/integrations/langchain/document-loaders)Nœuds sans télécharger la fonctionnalité du fichier, le corps de l'API est au format ** json **:

{% Tabs%}
{% Tab Title = "Python API"%}
```python
import requests

API_URL = "http://localhost:3000/api/v1/document-store/upsert/<storeId>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()

output = query({
    "docId": <docId>
})
print(output)
```
{% endtab%}

{% tab title = "JavaScript api"%}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/document-store/upsert/<storeId>",
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
    "docId": <docId>
}).then((response) => {
    console.log(response);
});
```
{% endtab%}
{% endtabs%}

#### Q: Puis-je ajouter de nouvelles métadonnées?

R: Vous pouvez fournir de nouvelles métadonnées en passant le **`metadata`** À l'intérieur de la demande du corps:

```json
{
    "docId": <doc-id>,
    "metadata": {
        "source: "abc"
    }
}
```

### API de rafraîchissement

Souvent, vous voudrez peut-être revoir tous les chargeurs de documents dans le magasin de documents pour récupérer les dernières données, et Upsert to Vector Store, pour tout garder en synchronisation. Cela peut être fait via une API de rafraîchissement:

{% Tabs%}
{% Tab Title = "Python API"%}
```python
import requests

API_URL = "http://localhost:3000/api/v1/document-store/refresh/<storeId>"

def query():
    response = requests.post(API_URL)
    return response.json()

output = query()
print(output)
```
{% endtab%}

{% tab title = "JavaScript api"%}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/document-store/refresh/<storeId>",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            }
        }
    );
    const result = await response.json();
    return result;
}

query().then((response) => {
    console.log(response);
});
```
{% endtab%}
{% endtabs%}

Vous pouvez également remplacer la configuration existante du chargeur de documents spécifique:

{% Tabs%}
{% Tab Title = "Python API"%}
```python
import requests

API_URL = "http://localhost:3000/api/v1/document-store/refresh/<storeId>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()

output = query(
{
    "items": [
        {
            "docId": <docId>,
            "splitter": {
                "name": "recursiveCharacterTextSplitter",
                "config": {
                    "chunkSize": 2000,
                    "chunkOverlap": 100
                }
            }
        }
    ]
}
)
print(output)
```
{% endtab%}

{% tab title = "JavaScript api"%}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/document-store/refresh/<storeId>",
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
    "items": [
        {
            "docId": <docId>,
            "splitter": {
                "name": "recursiveCharacterTextSplitter",
                "config": {
                    "chunkSize": 2000,
                    "chunkOverlap": 100
                }
            }
        }
    ]
}).then((response) => {
    console.log(response);
});
```
{% endtab%}
{% endtabs%}

## 11. Résumé

Nous avons commencé par créer un magasin de documents pour organiser les données politiques de LibertyGuard Deluxe Homeowners. Ces données ont ensuite été préparées en téléchargeant, en se répandant, en les traitementant et en les faisant augmenter, ce qui le prépare pour notre système de chiffon.

** Avantages du magasin de documents: **

Les magasins de documents offrent plusieurs avantages pour la gestion et la préparation des données pour la récupération des systèmes de génération augmentée (RAG):

* ** Organisation et gestion: ** Ils fournissent un emplacement central pour stocker, gérer et préparer vos données.
* ** Qualité des données: ** Le processus de chasse aide à structurer les données pour une récupération et une analyse précises.
* ** Flexibilité: ** Les magasins de documents permettent d'affiner et d'ajuster les données au besoin, en améliorant la précision et la pertinence de votre système de chiffon.

## 12. Tutoriels vidéo

### Raging Like a Boss - Flowise Document Store Tutorial

Dans cette vidéo,[Leon](https://youtube.com/@leonvanzyl)Fournit un tutoriel étape par étape sur l'utilisation des magasins de documents pour gérer facilement vos bases de connaissances de chiffon dans FlowiSeai.

{% embed url = "https://youtu.be/plusfakohoa"%}
