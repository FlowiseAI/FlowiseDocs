# Recherche profonde

Deep Research Agent est un système multi-agents sophistiqué qui peut effectuer des recherches complètes sur n'importe quel sujet en décomposant des requêtes complexes en tâches gérables, en déploiement des agents de recherche spécialisés et en synthétisant les résultats en rapports détaillés.

Cette approche est inspirée par le blog d'Anthropic -[How we built our multi-agent research system](https://www.anthropic.com/engineering/built-multi-agent-research-system)

## Aperçu

Le flux de travail de l'agent de recherche en profondeur se compose de plusieurs composants clés travaillant ensemble:

1. ** Planner Agent **: analyse la requête de recherche et génère une liste de tâches de recherche spécialisées
2. ** itération **: crée plusieurs agents de recherche pour travailler sur différents aspects de la requête
3. ** Sous-agents de recherche **: des agents individuels qui effectuent des recherches ciblées à l'aide de la recherche Web et d'autres outils
4. ** Agent écrivain **: synthétise toutes les résultats dans un rapport cohérent et complet
5. ** Agent de condition **: détermine si des recherches supplémentaires sont nécessaires ou si les résultats sont suffisants
6. ** Loop **: Retour à l'agent du planificateur pour améliorer la qualité de la recherche

<gigne> <img src = "../. GitBook / Assets / Image (12) (1) .png" alt = ""> <Figcaption> </ Figcaption> </gigne>

### Étape 1: Créez le nœud de démarrage

<gigne> <img src = "../. GitBook / Assets / Image (5) (1) (1) (1) .png" alt = "" width = "168"> <figcaption> </gigcaption> </gistre>

1. Commencez par ajouter un nœud ** start ** à votre toile
2. Configurez le nœud de démarrage avec ** Entrée du formulaire ** pour collecter la requête de recherche des utilisateurs
3. Configurez le formulaire avec la configuration suivante:
   * ** Titre de formulaire **: "Recherche"
   * ** Description du formulaire **: "Un agent de recherche qui prend une requête et renvoie un rapport détaillé"
   * ** Types d'entrée de formulaire **: Ajoutez une entrée de chaîne avec l'étiquette "requête" et le nom de la variable "Query"
4. Initialiser l'état de flux avec deux variables clés:
   * `subagents`: Pour stocker la liste des tâches de recherche à effectuer par des sous-agents
   * `findings`: Pour accumuler des résultats de recherche

<gigne> <img src = "../. Gitbook / Assets / image (1) (1) (1) (1) (1) (1) .png" alt = "" width = "407"> <figCaption> </gigcaption> </gigne>

### Étape 2: Ajouter l'agent du planificateur

<gigne> <img src = "../. GitBook / Assets / Image (6) (1) (1) (1) .png" alt = "" width = "331"> <figcaption> </gigcaption> </gigne>

1. Connectez un nœud ** llm ** au nœud de démarrage.
2. Configurez l'invite du système pour agir en tant que responsable de recherche d'experts avec les principales responsabilités suivantes:
   * Analyser et décomposer les requêtes utilisateur
   * Créer des plans de recherche détaillés
   * Générer des tâches spécifiques pour les sous-agents
   * Exemple d'invite -[research\_lead\_agent.md](https://github.com/anthropics/anthropic-cookbook/blob/main/patterns/agents/prompts/research_lead_agent.md)

<gigne> <img src = "../. GitBook / Assets / image (2) (1) (1) (1) (1) (1) .png" alt = "" width = "415"> <figCaption> </ Figcaption> </ Figure>

3. Configurer ** Sortie structurée JSON ** Pour renvoyer une liste de tâches de sous-agent:

```json
{
  "task": {
    "type": "string", 
    "description": "The research task for subagent"
  }
}
```

4. Mettez à jour l'état de flux en stockant la liste des sous-agents générés

<gigne> <img src = "../. GitBook / Assets / Image (3) (1) (1) (1) (1) .png" alt = "" width = "398"> <figCaption> </gigcaption> </gigu

### Étape 3: Créez le bloc d'itération du sous-agent

<gigne> <img src = "../. GitBook / Assets / Image (13) (1) .png" alt = "" width = "473"> <Figcaption> </ Figcaption> </gigne>

1. Ajoutez un nœud ** itération **.
2. Connectez-le à la sortie du planificateur
3. Configurez l'entrée d'itération à l'état de flux:`{{ $flow.state.subagents }}`. Pour chaque élément du tableau, un sous-agent sera engendré pour effectuer la tâche de recherche. Exemple:

<gigne> <img src = "../. GitBook / Assets / Image (8) (1) (1) .png" alt = "" width = "419"> <Figcaption> </ / Figcaption> </ Figure>

```json
{
  "subagents": [
    {
      "task": "Research the current state and recent developments in autonomous multi-agent systems technology. Focus on defining what autonomous multi-agent systems are, key technical components (coordination algorithms, communication protocols, decision-making frameworks), major technological advances in the last 2-3 years, and leading research institutions/companies working in this space. Use web search to find recent academic papers, industry reports, and technical documentation. Prioritize sources from IEEE, ACM, Nature, Science, and major tech companies' research divisions. Compile findings into a comprehensive technical overview covering definitions, core technologies, recent breakthroughs, and key players in the field."
    },
    {
      "task": "Investigate real-world applications and deployments of autonomous multi-agent systems across different industries. Research specific use cases in robotics (swarm robotics, warehouse automation), transportation (autonomous vehicle fleets, traffic management), manufacturing (coordinated production systems), defense/military applications, smart cities, and any other domains where these systems are actively deployed. For each application area, identify specific companies, products, success stories, and quantitative results where available. Focus on practical implementations rather than theoretical research. Use web search to find case studies, company announcements, industry reports, and news articles about actual deployments."
    }
  ]
}  
```

### Étape 4: Construisez le sous-agent de recherche

1. À l'intérieur du bloc d'itération, ajoutez un nœud ** agent **.
2. Configurez l'invite du système pour agir comme un sous-agent de recherche ciblé avec:
   * Capacités de compréhension des tâches claires
   * Planification efficace de la recherche (2-5 appels d'outils par tâche)
   * Évaluation de la qualité de la source
   * Utilisation d'outils parallèles pour l'efficacité
   * Exemple d'invite -[research\_subagent.md](https://github.com/anthropics/anthropic-cookbook/blob/main/patterns/agents/prompts/research_subagent.md)

<gigne> <img src = "../. GitBook / Assets / Image (9) (1) .png" alt = "" width = "401"> <Figcaption> </gigcaption> </gigust>

3. Ajoutez les outils de recherche suivants, vous pouvez utiliser vos propres outils préférés:
   * ** Recherche Google **: pour les liens de recherche Web
   * ** Scraper Web **: pour l'extraction de contenu Web. Cela grattera le contenu des liens de Google Search.
   * ** Recherche Arxiv **: pour rechercher et charger le contenu des articles académiques

<gigne> <img src = "../. GitBook / Assets / image (11) (1) .png" alt = "" width = "389"> <Figcaption> </ Figcaption> </gigne>

4. Définissez le message de l'utilisateur pour passer la tâche d'itération actuelle:`{{ $iteration.task }}`

### Étape 5: Ajouter l'agent de l'écrivain

<gigne> <img src = "../. GitBook / Assets / Image (14) (1) .png" alt = "" width = "397"> <Figcaption> </gigcaption> </gigust>

1. Connectez un nœud ** llm ** une fois l'itération terminée.
2. Un contexte plus grand LLM comme Gemini avec 1 à 2 millions de tailles de contexte est nécessaire pour synthétiser toutes les résultats et générer le rapport.
3. Configurez l'invite du système pour agir en tant que rédacteur de recherche expert qui:
   * Préserve le contexte complet des résultats de la recherche
   * Maintient l'intégrité de la citation
   * Ajoute de la structure et de la clarté
   * Sorte les rapports de marque professionnelle
4. Configurez le message de l'utilisateur pour inclure:
   * Sujet de la recherche:`{{ $form.query }}`
   * Résultats existants:`{{ $flow.state.findings }}`
   * Nouvelles conclusions:`{{ iterationAgentflow_0 }}`

<gigne> <img src = "../. GitBook / Assets / image (15) (1) .png" alt = "" width = "399"> <Figcaption> </ Figcaption> </ Figure>

4. Mettre à jour le`{{ $flow.state.findings }}`avec la sortie de l'agent d'écriture.

<gigne> <img src = "../. GitBook / Assets / Image (16) (1) .png" alt = "" width = "397"> <Figcaption> </ Figcaption> </ Figure>

### Étape 6: Implémentez le contrôle de la condition

<gigne> <img src = "../. GitBook / Assets / Image (17) (1) .png" alt = "" width = "332"> <Figcaption> </gigcaption> </gigne>

1. Ajouter un ** agent de condition. **
2. Configurez la logique de condition pour déterminer si des recherches supplémentaires sont nécessaires
3. Configurer deux scénarios:
   * "Plus de sous-agents sont nécessaires"
   * "Les résultats sont suffisants"
4. Fournir un contexte d'entrée, notamment:
   * Sujet de recherche
   * Liste des sous-agents actuels
   * Résultats accumulés

<gigne> <img src = "../. GitBook / Assets / Image (18) (1) .png" alt = "" width = "407"> <Figcaption> </gigcaption> </gigne>

### Étape 7: Créez le mécanisme de boucle

1. Pour le chemin ** "plus de sous-agents nécessaires" ** Path, ajoutez un nœud de boucle ** **
2. Configurez-le pour remonter au nœud du planificateur
3. Réglez un nombre de boucles maximales de 5 pour empêcher les boucles infinies
4. L'agent de planificateur examinera le rapport actuel et générera des tâches de recherche supplémentaires.

<gigne> <img src = "../. GitBook / Assets / Image (19) (1) .png" alt = "" width = "563"> <Figcaption> </ Figcaption> </gigne>

### Étape 8: Ajouter la sortie finale

1. Pour les résultats "** les résultats sont suffisants **", ajoutez une réponse directe ** **
2. Configurez-le pour publier le rapport final:`{{ $flow.state.findings }}`

<gigne> <img src = "../. GitBook / Assets / Image (20) (1) .png" alt = "" width = "563"> <Figcaption> </gigcaption> </gigust>

<gigne> <img src = "../. GitBook / Assets / Image (21) (1) .png" alt = "" width = "409"> <Figcaption> </gigcaption> </gigne>

## Tester le flux

1. Commencez par un sujet simple comme "Systèmes multi-agents autonomes dans des environnements réels"
2. Observez comment le planificateur décompose la recherche en tâches ciblées
3. Surveiller les sous-agents lorsqu'ils effectuent des recherches parallèles
4. Passez en revue la synthèse des résultats par l'agent de l'écrivain
5. Notez si l'agent de condition demande des recherches supplémentaires

<gigne> <img src = "../. GitBook / Assets / Image (22) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

** Rapport généré: **

{% fichier src = "../. GitBook / Assets / Research Report de Deep.pdf"%}

## Structure d'écoulement complète

{% fichier src = "../. GitBook / Assets / Deep Research Dynamic subagents.json"%}

## Procédure

1. 🧠 Planner Agent - Analyse la requête de recherche et génère une liste de tâches de recherche spécialisées
2. 🖧 Sous-agents - Crée plusieurs sous-agents de recherche, effectuer des recherches ciblées à l'aide de la recherche Web, de l'écrase Web et des outils ArXIV
3. ✍️ Agent écrivain - synthétise toutes les résultats dans un rapport cohérent et complet avec des citations
4. ⇄ Agent de condition - détermine si des recherches supplémentaires sont nécessaires ou si les résultats sont suffisants
5. 🔄 Boucle de retour à l'agent du planificateur pour générer plus de sous-agents

### 🧠 agent de planificateur

Agir comme une recherche experte mène à:

* Analyser et décomposer les requêtes utilisateur
* Créer des plans de recherche détaillés
* Générer des tâches spécifiques pour les sous-agents

Sortie un éventail de tâches de recherche.

<gigne> <img src = "../. GitBook / Assets / Untitled-2025-06-16-1507.png" alt = "" width = "563"> <figcaption> </gigcaption> </stigne>

### 🖧 Sous-agents

Pour chaque tâche de la liste des tâches, un nouveau sous-agent sera engendré pour mener des recherches ciblées.

Chaque sous-agent a:

* Capacités de compréhension des tâches claires
* Planification efficace de la recherche (2-5 appels d'outils par tâche)
* Évaluation de la qualité de la source
* Utilisation d'outils parallèles pour l'efficacité

<gigne> <img src = "../. GitBook / Assets / Subagents.png" alt = "" width = "563"> <figcaption> </gigcaption> </ figure>

Subagent a accès à des outils de recherche Web, Web Scrape et ArXIV.

* 🌐 Recherche Google - pour les liens de recherche Web
* 🗂️ Scraper Web - pour l'extraction du contenu Web. Cela grattera le contenu des liens de Google Search.
* 📑 ArXIV - Rechercher, télécharger et lire le contenu des articles Arxiv

<gigne> <img src = "../. GitBook / Assets / SubangentStool.png" alt = "" width = "563"> <Figcaption> </ Figcaption> </ Figure>

### ✍️ Agent écrivain

Agir en tant que rédacteur de recherche qui transforme les résultats bruts en un rapport Markdown clair et structuré. Conserver tout contexte et citations.

Nous constatons que les Gémeaux sont les meilleurs pour cela, grâce à sa grande fenêtre de contexte qui lui permet de synthétiser efficacement toutes les résultats.

<gigne> <img src = "../. GitBook / Assets / writer.png" alt = "" width = "563"> <Figcaption> </gigcaption> </gigust>

### ⇄ Agent de condition

Avec le rapport généré, nous avons laissé le LLM déterminer si des recherches supplémentaires sont nécessaires ou si les résultats sont suffisants.

Si davantage est nécessaire, l'agent du planificateur passe en revue tous les messages, identifie les domaines d'amélioration, génère des tâches de recherche de suivi et la boucle se poursuit.

Si les résultats sont suffisants, nous renvoyons simplement le rapport final de l'agent de l'écrivain en tant que sortie.

<gigne> <img src = "../. GitBook / Assets / Condition.png" alt = "" width = "563"> <Figcaption> </gigcaption> </gigust>

## Configuration avancée

#### Personnalisation de la profondeur de recherche

Vous pouvez ajuster la profondeur de recherche en modifiant l'invite du système du planificateur à:

* Augmenter le nombre de sous-agents pour des sujets complexes (jusqu'à 10-20)
* Ajustez le budget d'appel à l'outil par sous-agent
* Modifier le nombre de boucles pour plus de recherche itérative

Mais cela comporte également un coût supplémentaire pour plus de consommation de jetons.

#### Ajout d'outils spécialisés

Améliorez les capacités de recherche en ajoutant des outils spécifiques au domaine:

* Outils personnels comme Gmail, Slack, Google Calendar, Teams, etc.
* Autre grattoir Web, outils de recherche Web comme Firecrawl, Exa, Apify, etc.

#### Ajout de contexte de chiffon

Vous pouvez ajouter plus de contexte au LLM avec[RAG](rag.md). Cela permet à LLM de retirer les informations des sources de connaissances existantes pertinentes en cas de besoin.

## Meilleures pratiques

* La sélection des modèles et les options de secours sont cruciales en raison de la grande quantité de résultats qui provoquent un débordement de jeton.
* L'invitation est la clé. Ouverts ouverts ouverts de toute leur structure rapide, couvrant la délégation des tâches, l'utilisation parallèle des outils et les processus de réflexion -[https://github.com/anthropics/anthropic-cookbook/blob/main/patterns/agents/prompts](https://github.com/anthropics/anthropic-cookbook/blob/main/patterns/agents/prompts)
* Les outils doivent être soigneusement conçus, quand utiliser, comment limiter la durée des résultats renvoyés des exécutions d'outils.
* Ceci est très similaire au triangle de compromis, où l'optimisation de deux de l'arbre a souvent un impact négatif sur un autre, dans ce cas - la vitesse, la qualité, le coût.
