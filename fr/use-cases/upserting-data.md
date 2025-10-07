---
description: Learn how to upsert data to Vector Stores with Flowise
---

# Augmenter les données

***

Il existe deux façons fondamentales de renforcer vos données dans un[Vector Store](../integrations/langchain/vector-stores/)en utilisant Flowise, soit via[API calls](broken-reference)ou en utilisant un ensemble de nœuds dédiés, nous avons prêt à cet effet.

Dans ce guide, même s'il est ** hautement recommandé ** que vous préparez vos données en utilisant le[Document Stores](../using-flowise/document-stores.md)Avant de renverser un magasin vectoriel, nous passerons par l'ensemble du processus en utilisant les nœuds spécifiques nécessaires à cette fin, en décrivant les étapes, les avantages de cette approche et les stratégies d'optimisation pour une gestion efficace des données.

## Comprendre le processus de mise en service

La première chose que nous devons comprendre est que le processus de données de mise en valeur d'un[Vector Store](../integrations/langchain/vector-stores/)est une pièce fondamentale pour la formation d'un[Retrieval Augmented Generation (RAG)](multiple-documents-qna.md)système. Cependant, une fois ce processus terminé, le chiffon peut être exécuté indépendamment.

En d'autres termes, dans Flowise, vous pouvez améliorer les données sans configuration complète de chiffon, et vous pouvez exécuter votre chiffon sans les nœuds spécifiques utilisés dans le processus Upsert, ce qui signifie que bien qu'un magasin vectoriel bien peuplé soit crucial pour que le chiffon fonctionne, les processus de récupération et de génération réels ne nécessitent pas de mise en service continu.

<gigne> <img src = "../. GitBook / Assets / UD_01.png" alt = ""> <Figcaption> <p> Upsert vs Rag </p> </gigcaption> </gigne>

## Installation

Supposons que nous ayons un long jeu de données au format PDF que nous avons besoin pour user à notre[Upstash Vector Store](../integrations/langchain/vector-stores/upstash-vector.md)Nous pourrions donc demander à un LLM de récupérer des informations spécifiques de ce document.

Pour ce faire, et pour illustrer ce tutoriel, nous aurions besoin de créer un flux ** à la mise à la hausse ** avec 5 nœuds différents:

<gigne> <img src = "../. GitBook / Assets / UD_02.png" alt = ""> <Figcaption> <p> Flow upsert </p> </gigcaption> </gigust>

## 1. Chargeur de documents

La première étape consiste à ** télécharger nos données PDF dans l'instance Flowise ** en utilisant un[Document Loader node](../integrations/langchain/document-loaders/). Les chargeurs de documents sont des nœuds spécialisés qui gèrent l'ingestion de divers formats de documents, y compris ** pdfs **, ** txt **, ** csv **, pages de notion, et plus encore.

Il est important de mentionner que chaque chargeur de documents est livré avec deux paramètres importants ** supplémentaires ** qui nous permettent d'ajouter et d'omettre les métadonnées de notre ensemble de données à volonté.

<gigne> <img src = "../. GitBook / Assets / UD_03.png" alt = "" width = "375"> <Figcaption> <p> Paramètres supplémentaires </p> </gigcaption> </gigne>

{% hint style = "info"%}
** TIP **: Les paramètres de métadonnées ADD / OMIT, bien qu'ils soient facultatifs, sont très utiles pour cibler notre ensemble de données une fois qu'il est déposé dans un magasin vectoriel ou pour enlever les métadonnées inutiles.
{% EndHint%}

## 2. Splitter de texte

Une fois que nous avons téléchargé notre PDF ou notre ensemble de données, nous devons ** le diviser en pièces, documents ou morceaux plus petits **. Il s'agit d'une étape de prétraitement cruciale pour 2 raisons principales:

* ** Vitesse et pertinence de récupération: ** Le stockage et l'interrogation de gros documents en tant qu'entités uniques dans une base de données vectorielle peuvent conduire à des temps de récupération plus lents et à des résultats potentiellement moins pertinents. La division du document en morceaux plus petits permet une récupération plus ciblée. En interrogeant contre des unités d'information plus petites et plus ciblées, nous pouvons atteindre des temps de réponse plus rapides et améliorer la précision des résultats récupérés.
* ** RETENDANT: ** Puisque nous ne récupérons que des morceaux pertinents plutôt que le document entier, le nombre de jetons traités par le LLM est considérablement réduit. Cette approche de récupération ciblée se traduit directement par une baisse des coûts d'utilisation de notre LLM, car la facturation est généralement basée sur la consommation de jetons. En minimisant la quantité d'informations non pertinentes envoyées à la LLM, nous optimisons également pour le coût.

### Nœuds

En flux, ce processus de division est accompli en utilisant le[Text Splitter nodes](../integrations/langchain/text-splitters/). Ces nœuds fournissent une gamme de stratégies de segmentation de texte, notamment:

* ** Clissage du texte des caractères: ** Divide le texte en morceaux d'un nombre fixe de caractères. Cette méthode est simple mais peut diviser des mots ou des phrases sur des morceaux, perturbant potentiellement le contexte.
* ** Diffusion du texte de jeton: ** Segmentation du texte en fonction des limites des mots ou des schémas de tokenisation spécifiques au modèle d'intégration choisi. Cette approche conduit souvent à des morceaux plus cohérents sémantiquement, car il préserve les limites des mots et considère la structure linguistique sous-jacente du texte.
* ** Diffusion du texte récursif du caractère: ** Cette stratégie vise à diviser le texte en morceaux qui maintiennent la cohérence sémantique tout en restant dans une limite de taille spécifiée. Il est particulièrement bien adapté aux documents hiérarchiques avec des sections ou des titres imbriqués. Au lieu de se diviser aveuglément à la limite de caractère, il analyse récursivement le texte pour trouver des points d'arrêt logiques, tels que les fins de phrase ou les ruptures de section. Cette approche garantit que chaque morceau représente une unité d'information significative, même si elle dépasse légèrement la taille cible.
* ** Splitter de texte de marque: ** Conçu spécifiquement pour les documents formulés Markdown, ce séparateur segmente logiquement le texte basé sur des en-têtes de démarque et des éléments structurels, créant des morceaux qui correspondent à des sections logiques dans le document.
* ** Splitter de texte de code: ** Adapté pour la division des fichiers de code, cette stratégie considère la structure du code, les définitions de fonction et d'autres éléments spécifiques au langage de programmation pour créer des morceaux significatifs qui conviennent aux tâches telles que la recherche et la documentation de code.
* ** Splitter de texte HTML à markdown: ** Ce séparateur spécialisé convertit d'abord le contenu HTML à Markdown, puis applique le séparateur de texte Markdown, permettant une segmentation structurée des pages Web et d'autres documents HTML.

Les nœuds de séparateur de texte fournissent un contrôle granulaire sur la segmentation du texte, permettant la personnalisation de paramètres tels que:

* ** Taille du morceau: ** La taille maximale souhaitée de chaque morceau, généralement définie en caractères ou en jetons.
* ** chevauchement de morceaux: ** Le nombre de caractères ou de jetons à chevaucher entre des morceaux consécutifs, utile pour maintenir le flux contextuel à travers des morceaux.

{% hint style = "info"%}
** Astuce: ** Notez que la taille du morceau et les valeurs de chevauchement des morceaux ne sont pas additives. Sélection`chunk_size=1200`et`chunk_overlap=400`Ne se traduit pas par une taille totale de 1600. La valeur de chevauchement détermine le nombre de jetons du morceau précédent inclus dans le morceau actuel pour maintenir le contexte. Il n'augmente pas la taille globale du morceau.
{% EndHint%}

### Chevauchement de morceaux

Dans le contexte de la récupération basée sur les vecteurs et de la requête LLM, le chevauchement de morceaux joue un ** rôle important dans le maintien de la continuité contextuelle ** et ** Amélioration de la précision de la réponse **, en particulier lorsqu'il s'agit d'une profondeur de récupération limitée ou ** Top K **, qui est le paramètre qui détermine le nombre maximum de la plupart des morceaux similaires qui sont récupérés à partir de la[Vector Store](../integrations/langchain/vector-stores/)en réponse à une requête.

Pendant le traitement des requêtes, le LLM exécute une recherche de similitude contre le magasin vectoriel pour récupérer les morceaux les plus pertinents sémantiquement à la requête donnée. Si la profondeur de récupération, représentée par le paramètre K supérieur, est définie sur une petite valeur, 4 pour par défaut, le LLM utilise initialement des informations uniquement à partir de ces 4 morceaux pour générer sa réponse.

Ce scénario nous présente un problème, car le fait de s'appuyer uniquement sur un nombre limité de morceaux sans chevauchement peut entraîner des réponses incomplètes ou inexactes, en particulier lorsqu'ils traitent des requêtes qui nécessitent des informations couvrant plusieurs morceaux.

Le chevauchement des morceaux aide à ce problème en s'assurant qu'une partie du contexte textuel est partagée sur des morceaux consécutifs, ** augmentant la probabilité que toutes les informations pertinentes pour une requête donnée soient contenues dans les morceaux récupérés **.

En d'autres termes, ce chevauchement sert de pont entre des morceaux, permettant au LLM d'accéder à une fenêtre contextuelle plus large même lorsqu'elle est limitée à un petit ensemble de morceaux récupérés (haut K). Si une requête est liée à un concept ou à une information qui s'étend au-delà d'un seul morceau, les régions qui se chevauchent augmentent la probabilité de capturer tout le contexte nécessaire.

Par conséquent, en introduisant un chevauchement de morceaux pendant la phase de division du texte, nous améliorons la capacité du LLM à:

1. ** Préserver la continuité contextuelle: ** Les morceaux qui se chevauchent fournissent une transition plus fluide des informations entre les segments consécutifs, permettant au modèle de maintenir une compréhension plus cohérente du texte.
2. ** Améliorer la précision de la récupération: ** En augmentant la probabilité de capturer toutes les informations pertinentes dans le top k cible k récupéré, le chevauchement contribue à des réponses plus précises et plus appropriées.

### Précision vs coût

Ainsi, pour optimiser davantage le compromis entre la précision de la récupération et le coût, deux stratégies primaires peuvent être utilisées:

1. ** Le chevauchement d'augmentation / diminution du morceau: ** L'ajustement du pourcentage de chevauchement pendant la division de texte permet un contrôle à grain fin sur la quantité de contexte partagé entre les morceaux. Des pourcentages de chevauchement plus élevés entraînent généralement une amélioration de la préservation du contexte, mais peuvent également augmenter les coûts, car vous devez utiliser plus de morceaux pour englober l'ensemble du document. À l'inverse, des pourcentages de chevauchement plus faibles peuvent réduire les coûts, mais risquent de perdre des informations contextuelles clés entre les morceaux, conduisant potentiellement à des réponses moins précises ou incomplètes du LLM.
2. ** Augmentation / diminution du top k: ** L'augmentation de la valeur K supérieure par défaut (4) élargit le nombre de morceaux considérés pour la génération de réponse. Bien que cela puisse améliorer la précision, cela augmente également les coûts.

{% hint style = "info"%}
** Astuce: ** Le choix des valeurs optimales ** des chevauchement ** et ** Top K ** dépend de facteurs tels que la complexité du document, les caractéristiques du modèle d'intégration et l'équilibre souhaité entre la précision et le coût. L'expérimentation avec ces valeurs est importante pour trouver la configuration idéale pour un besoin spécifique.
{% EndHint%}

## 3. Incorporer

Nous avons maintenant téléchargé notre ensemble de données et configuré comment nos données seront divisées avant qu'elle ne se renforce à notre[Vector Store](../integrations/langchain/vector-stores/). À ce point,[the embedding nodes](../integrations/langchain/embeddings/)Entrez en jeu, ** convertir tous ces morceaux en une "langue" qu'un LLM peut facilement comprendre **.

Dans ce contexte actuel, l'intégration est le processus de conversion du texte en une représentation numérique qui capture sa signification. Cette représentation numérique, également appelée vecteur d'intégration, est un tableau de nombres multidimensionnel, où chaque dimension représente un aspect spécifique de la signification du texte.

Ces vecteurs permettent aux LLM de comparer et de rechercher des morceaux de texte similaires dans le magasin vectoriel en mesurant la distance ou la similitude entre eux dans cet espace multidimensionnel.

### Comprendre les dimensions des intégres / vecteurs des vecteurs

Le nombre de dimensions dans un indice de magasin vectoriel est déterminé par le modèle d'incorporation utilisé lorsque nous augmentons nos données, et vice versa. Chaque dimension représente une fonction ou un concept spécifique dans les données. Par exemple, une ** dimension ** pourrait ** représenter un sujet, un sentiment ou un autre aspect particulier du texte **.

Plus nous utilisons de dimensions pour intégrer nos données, plus le potentiel de capture de sens nuancé de notre texte est grand. Cependant, cette augmentation se fait au prix des exigences de calcul plus élevées par requête.

En général, un plus grand nombre de dimensions nécessite plus de ressources pour stocker, traiter et comparer les vecteurs d'intégration résultants. Par conséquent, des modèles intégrés comme le Google`embedding-001`, qui utilise 768 dimensions, sont, en théorie, moins chères que d'autres comme l'Openai`text-embedding-3-large`, avec 3072 dimensions.

Il est important de noter que la ** relation entre les dimensions et la capture de sens n'est pas strictement linéaire **; Il y a un point de rendement décroissant où l'ajout de dimensions offre un avantage négligeable pour le coût inutile supplémentaire.

{% hint style = "info"%}
** Astuce: ** Pour garantir la compatibilité entre un modèle d'incorporation et un indice de magasin vectoriel, l'alignement dimensionnel est essentiel. Le modèle et l'index doivent utiliser le même nombre de dimensions pour la représentation vectorielle **. L'inadéquation de la dimensionnalité entraînera des erreurs de mise en service, car le magasin vectoriel est conçu pour gérer les vecteurs d'une taille spécifique déterminée par le modèle d'incorporation choisi.
{% EndHint%}

## 4. Magasin vectoriel

Le[Vector Store node](../integrations/langchain/vector-stores/)est le ** nœud de fin de notre flux de mise en service **. Il agit comme le pont entre notre instance Flowise et notre base de données vectorielle, nous permettant d'envoyer les intérêts générés, ainsi que toutes les métadonnées associées, à notre index du magasin de vecteur cible pour le stockage persistant et la récupération ultérieure.

C'est dans ce nœud où nous pouvons définir des paramètres comme "** TOP K **", qui, comme nous l'avons dit précédemment, est le paramètre qui détermine le nombre maximum des morceaux similaires qui sont récupérés du magasin vectoriel en réponse à une requête.

<gigne> <img src = "../. GitBook / Assets / UD_04.png" alt = "" width = "375"> <figcaption> </gigcaption> </ figure>

{% hint style = "info"%}
** CONSEIL: ** Une valeur K supérieure inférieure donnera des résultats moins, mais potentiellement plus pertinents, tandis qu'une valeur plus élevée renverra une gamme plus large de résultats, capturant potentiellement plus d'informations.
{% EndHint%}

## 5. Record Manager

Le[Record Manager node](../integrations/langchain/record-managers.md)est un ajout facultatif mais incroyablement utile à notre flux de mise en service. Il nous permet de maintenir des enregistrements de tous les morceaux qui ont été renversés vers notre magasin vectoriel, nous permettant d'ajouter ou de supprimer efficacement des morceaux au besoin.

Pour un guide plus approfondi, nous vous référons à[this guide](../integrations/langchain/record-managers.md).

<gigne> <img src = "../. GitBook / Assets / UD_05.png" alt = "" width = "375"> <figcaption> </gigcaption> </ figure>

## 6. Aperçu complet

Enfin, examinons chaque étape, du chargement initial du document à la représentation du vecteur final, mettant en évidence les composants clés et leurs rôles dans le processus de mise en service.

<gigne> <img src = "../. GitBook / Assets / UD_06.png" alt = ""> <figcaption> </gigcaption> </gigust>

1. ** Document Ingesttion **:
   * Nous commençons par nourrir nos données brutes en flux en utilisant le nœud de chargeur de document ** approprié ** pour votre format de données.
2. ** Diffusion stratégique **
   * Ensuite, le nœud ** du séparateur de texte ** divise notre document en morceaux plus petits et plus gérables. Ceci est crucial pour une récupération et un contrôle des coûts efficaces.
   * Nous avons une flexibilité dans la façon dont cette division se produit en sélectionnant le nœud de séparateur de texte approprié et, surtout, par une taille de morceau de réglage fin et un chevauchement de morceaux pour équilibrer la préservation du contexte avec l'efficacité.
3. ** des incorporations significatives **
   * Maintenant, juste avant que nos données ne soient enregistrées dans le magasin vectoriel, le ** nœud d'intégration ** intervient. Il transforme chaque morceau de texte et sa signification en une représentation numérique que notre LLM peut comprendre.
4. ** Index du magasin vectoriel **
   * Enfin, le nœud ** Vector Store ** agit comme le pont entre Flowise et notre base de données. Il envoie nos intérêts, ainsi que toutes les métadonnées associées, à l'indice de magasin vectoriel désigné.
   * Ici, dans ce nœud, nous pouvons contrôler le comportement de récupération en définissant le paramètre ** supérieur k **, qui influence le nombre de morceaux considérés lors de la réponse à une requête.
5. ** Données prêtes **
   * Une fois renversé, nos données sont désormais représentées comme des vecteurs dans le magasin vectoriel, prêts pour la recherche et la récupération de similitude.
6. ** Resteur (facultatif) **
   * Pour les données de contrôle et de gestion améliorées, le nœud ** Record Manager ** garde la trace de tous les morceaux lancés. Cela facilite les mises à jour ou les éliminations faciles à mesure que vos données ou vos besoins évoluent.

Essentiellement, le processus de mise en service transforme nos données brutes en un format prêt pour LLM, optimisé pour une récupération rapide et rentable.
