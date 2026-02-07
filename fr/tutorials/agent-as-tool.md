# Agent comme outil

Dans ce tutoriel, nous allons voir comment tirer parti des autres flux en tant qu'outils pour un agent parent. Cette approche vous permet de créer un agent parent qui peut déléguer des tâches spécifiques à des agents enfants spécialisés

## Aperçu

1. Reçoit la saisie des utilisateurs via un agent parent
2. L'agent décide de récupérer les données du magasin de documents ou d'appeler l'outil AgentFlow.

<gigne> <img src = "../. GitBook / Assets / Image (295) .png" alt = "" width = "375"> <Figcaption> </gigcaption> </gigne>

### Étape 1: Configuration du nœud de démarrage

Commencez par ajouter un nœud ** start ** à votre toile. Cela sert de point d'entrée pour votre système d'agent.

### Étape 2: Création de l'agent parent

Ajoutez un nœud ** Agent ** et connectez-le au nœud de démarrage.

### Étape 3: Configuration de l'outil d'agent

La caractéristique clé de ce flux est de configurer un autre agent en tant qu'outil. Dans la section des outils ** de l'agent parent **:

<gigne> <img src = "../. GitBook / Assets / Image (296) .png" alt = "" width = "354"> <Figcaption> </ Figcaption> </gigust>

#### Configuration de l'outil:

* ** Outil **: Sélectionnez "** Agent comme outil **"

#### Paramètres de l'outil d'agent:

* ** Agent sélectionné **: Choisissez votre enfant d'agentflow
* ** Nom **: Nom de l'agentflow
* ** Description **: Décrivez quand cet agentflow est utile. Exemple:

```
Useful for searching user availability, scheduling meetings and email related query
```

{% hint style = "avertissement"%}
Le nom et la description de l'outil sont extrêmement importants! Ils doivent être clairs et décrire correctement l'objectif de l'outil. Se référer à[best practices](https://platform.openai.com/docs/guides/function-calling?api-mode=chat#best-practices-for-defining-functions)guide.
{% EndHint%}

### Étape 4: Ajout de sources de connaissances

Configurez la section ** connaissances (magasins de documents) ** pour donner à votre agent parent l'accès aux informations pertinentes. C'est la même chose que[RAG](rag.md)tutoriel.

<gigne> <img src = "../. GitBook / Assets / Image (297) .png" alt = "" width = "518"> <Figcaption> </ Figcaption> </gigust>

#### Configuration du magasin de documents:

* ** Magasin de documents **: Sélectionnez votre magasin de documents préconfiguré (par exemple, "AI-Paper")
* ** Décrivez les connaissances **: Décrivez de quoi parle les connaissances

***

## Exemples d'interactions

#### Exemples de requêtes et comportement attendu:

** Requête de planification: **

* Utilisateur: "Pouvez-vous vérifier ma disponibilité mardi prochain?"
* Flux: agent parent → outil personnel \ _asiste → Réponse de planification spécialisée

<gigne> <img src = "../. GitBook / Assets / Image (301) .png" alt = "" width = "563"> <Figcaption> </gigcaption> </ Figure>

** Requête technique: **

* Utilisateur: "Qu'est-ce que l'AIGC et comment ça marche?"
* Flux: agent parent → base de connaissances AI-Paper → Explication technique avec des sources

<gigne> <img src = "../. GitBook / Assets / Image (300) .png" alt = "" width = "563"> <Figcaption> </ Figcaption> </gigust>

** Requête générale: **

* Utilisateur: "Bonjour comment allez-vous?"
* Flux: agent parent → Réponse directe (aucun outil nécessaire)

** Requête complexe: **

* Utilisateur: "Planifiez une réunion sur la mise en œuvre de l'AIGC mardi prochain, extraire des informations clés et les points de discussion"
* Flux: agent parent → à la fois l'outil personnel \ _ASSISTANT et les connaissances sur papier → Réponse coordonnée

<gigne> <img src = "../. GitBook / Assets / Image (302) .png" alt = "" width = "563"> <Figcaption> </gigcaption> </gigne>

***

## Meilleures pratiques

#### Lignes directrices de conception:

1. ** Descriptions d'outils effacés **: Faites le nom et les descriptions de l'outil
2. ** Délégation appropriée **: Une meilleure invite système pour que l'agent parent de déléguer efficacement

#### Cas d'utilisation courants:

* ** Service client **: Agent parent avec des outils spécialisés pour la facturation, le support technique et les demandes générales
* ** Assistant de recherche **: parent avec des outils pour différents domaines de recherche (étude de marché légale, technique,)
* ** Gestion de projet **: parent avec des outils pour la planification, l'allocation des ressources et le suivi des progrès
* ** Création de contenu **: parent avec des outils pour l'écriture, l'édition, la recherche et la mise en forme

