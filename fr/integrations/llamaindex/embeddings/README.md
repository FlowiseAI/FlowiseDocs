---
description: LlamaIndex Embeddings Nodes
---

# Incorporer

***

Une intégration est un vecteur (liste) de numéros de points flottants. La distance entre deux vecteurs mesure leur relation. De petites distances suggèrent une forte parenté et de grandes distances suggèrent une faible parenté.

Les intégres peuvent être utilisés pour créer une représentation numérique des données textuelles. Cette représentation numérique est utile car elle peut être utilisée pour trouver des documents similaires.

Ils sont couramment utilisés pour:

* Recherche (où les résultats sont classés par pertinence pour une chaîne de requête)
* Clustering (où les chaînes de texte sont regroupées par similitude)
* Recommandations (où les éléments avec des chaînes de texte connexes sont recommandés)
* Détection d'anomalies (où les valeurs aberrantes avec peu de parenté sont identifiées)
* Mesure de la diversité (où les distributions de similitude sont analysées)
* Classification (où les chaînes de texte sont classées par leur étiquette la plus similaire)

### Nœuds d'intégration:

* [Azure OpenAI Embeddings](azure-openai-embeddings.md)
* [OpenAI Embedding](openai-embedding.md)
