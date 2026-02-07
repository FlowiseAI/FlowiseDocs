---
description: Learn how to use Multi-Agents in Flowise, written by @toi500
---

# Multi-agents

Ce guide a l'intention de fournir une introduction de l'architecture du système d'IA multi-agents dans Flowise, détaillant ses composants, ses contraintes opérationnelles et son flux de travail.

## Concept

Analogue à une équipe d'experts du domaine collaborant sur un projet complexe, un système multi-agents utilise le principe de spécialisation dans l'intelligence artificielle.

Ce système multi-agents utilise un flux de travail hiérarchique et séquentiel, maximisant l'efficacité et la spécialisation.

### 1. Architecture du système

Nous pouvons définir l'architecture d'IA multi-agents comme un système d'IA évolutif capable de gérer des projets complexes en les décomposant en sous-tâches gérables.

Dans Flowise, un système multi-agents comprend deux nœuds ou types d'agents principaux et un utilisateur, interagissant dans un graphique hiérarchique pour traiter les demandes et fournir un résultat ciblé:

1. ** Utilisateur: ** L'utilisateur agit comme le ** point de départ du système **, fournissant l'entrée ou la demande initiale. Bien qu'un système multi-agents puisse être conçu pour gérer une large gamme de demandes, il est important que ces demandes d'utilisateurs s'alignent sur l'objectif prévu du système. Toute demande tombant en dehors de cette portée peut entraîner des résultats inexacts, des boucles inattendues ou même des erreurs système. Par conséquent, les interactions utilisateur, bien que flexibles, devraient toujours s'aligner sur les fonctionnalités principales du système pour des performances optimales.
2. ** Superviseur AI: ** Le superviseur agit comme l'orchestrateur du Système **, supervisant l'ensemble du flux de travail. Il analyse les demandes de l'utilisateur, les décompose en une séquence de sous-tâches, attribue ces sous-tâches aux agents de travailleurs spécialisés, agrége les résultats et présente finalement la sortie traitée à l'utilisateur.
3. ** Équipe des travailleurs AI: ** Cette équipe se compose d'agents d'IA spécialisés, ou travailleurs, chacun a demandé - via des messages rapides - de gérer une tâche spécifique dans le flux de travail. Ces travailleurs fonctionnent indépendamment, recevant des instructions et des données du superviseur, ** exécutant leurs fonctions spécialisées **, en utilisant des outils selon les besoins et renvoyant les résultats au superviseur.

<gigne> <img src = "../../. GitBook / Assets / Multi-Agent-Diagram.svg" alt = ""> <Figcaption> </ Figcaption> </gigne>

### 2. Contraintes opérationnelles

Pour maintenir l'ordre et la simplicité, ce système multi-agents fonctionne sous deux contraintes importantes:

* ** Une tâche à la fois: ** Le superviseur est intentionnellement conçu pour se concentrer sur une seule tâche à la fois. Il attend que le travailleur actif termine sa tâche et renvoie les résultats avant d'analyser l'étape suivante et délégue la tâche suivante. Cela garantit que chaque étape est terminée avec succès avant de passer à autre chose, empêchant la surcomplexité.
* ** Un superviseur par flux: ** Bien qu'il soit théoriquement possible de mettre en œuvre un ensemble de systèmes multi-agents imbriqués pour former une structure hiérarchique plus sophistiquée pour des flux de travail très complexes, ce que Langchain définit comme "[Hierarchical Agent Teams](https://github.com/langchain-ai/langgraph/blob/main/examples/multi\_agent/hierarchical\_agent\_teams.ipynb)", Avec un superviseur de haut niveau et des superviseurs de niveau intermédiaire gérant des équipes de travailleurs, les systèmes multi-agents de Flowise fonctionnent actuellement avec un seul superviseur.

{% hint style = "info"%}
Ces deux contraintes sont importantes lors de la planification du flux de travail de votre application **. Si vous essayez de concevoir un flux de travail où le superviseur doit déléguer plusieurs tâches simultanément, en parallèle, le système ne pourra pas le gérer et vous rencontrerez une erreur.
{% EndHint%}

## Le superviseur

Le superviseur, en tant qu'agent régissant le flux de travail global et responsable de la délégation de tâches au travailleur approprié, nécessite un ensemble de composants pour fonctionner correctement:

* ** Modèle de chat capable d'appeler de fonction ** pour gérer les complexités de la décomposition des tâches, de la délégation et de l'agrégation de résultats.
* ** Mémoire d'agent (facultatif) **: Bien que le superviseur puisse fonctionner sans mémoire d'agent, ce nœud peut améliorer considérablement les workflows qui nécessitent un accès aux états de superviseur passés. Cette ** Préservation de l'État ** pourrait permettre au superviseur de reprendre le travail à partir d'un point spécifique ou de tirer parti des données antérieures pour améliorer la prise de décision.

<gigne> <img src = "../../. GitBook / Assets / Mas07.png" alt = ""> <Figcaption> </gigcaption> </gigne>

### Invite du superviseur

Par défaut, l'invite de superviseur est formulée d'une manière qui demande au superviseur d'analyser les demandes des utilisateurs, de les décomposer en une séquence de sous-tâches et d'attribuer ces sous-tâches aux agents de travailleurs spécialisés.

Bien que l'invite de superviseur soit personnalisable pour répondre aux besoins d'application spécifiques, il nécessite toujours les deux éléments clés suivants:

* ** La variable {Team \ _Members}: ** Cette variable est cruciale pour la compréhension du superviseur de la main-d'œuvre disponible car elle fournit au superviseur la liste des noms de travailleurs. Cela permet au superviseur de déléguer avec diligence les tâches au travailleur le plus approprié en fonction de son expertise.
* ** Le mot-clé "finition": ** Ce mot clé sert de signal dans l'invite de superviseur. Il indique quand le superviseur doit considérer la tâche terminée et présenter la sortie finale à l'utilisateur. Sans une directive claire "Finish", le superviseur pourrait continuer à déléguer des tâches inutilement ou ne pas fournir un résultat cohérent et finalisé à l'utilisateur. Il signale que tous les sous-tâches nécessaires ont été exécutées et que la demande de l'utilisateur a été réalisée.

<gigne> <img src = "../../. GitBook / Assets / Mas06.png" alt = "" width = "375"> <Figcaption> </ Figcaption> </gigust>

{% hint style = "info"%}
Il est important de comprendre que le superviseur joue un rôle très distinct des travailleurs. Contrairement aux travailleurs, qui peuvent être adaptés à des instructions très spécifiques, le ** superviseur fonctionne le plus efficacement avec les directives générales, qui lui permettent de planifier et de déléguer des tâches telles qu'elle juge appropriée. ** Si vous êtes nouveau dans les systèmes multi-agents, nous vous recommandons de rester avec l'invite de superviseur par défaut.
{% EndHint%}

### Comprendre la limite de récursivité dans le nœud du superviseur:

Ce paramètre restreint la profondeur maximale des appels de fonction imbriqués dans notre application. Dans notre contexte actuel, ** il limite le nombre de fois que le superviseur peut se déclencher dans une seule exécution de workflow **. Ceci est important pour prévenir la récursivité illimitée et garantir que les ressources sont utilisées efficacement.

<gigne> <img src = "../../. GitBook / Assets / MAS04.png" alt = "" width = "375"> <figcaption> </gigcaption> </gigu

### Comment fonctionne le superviseur

Lors de la réception d'une requête utilisateur, le superviseur initie le workflow en analysant la demande et en discernant le résultat prévu de l'utilisateur.

Ensuite, en tirant parti du`{team_members}`Variable Dans l'invite du superviseur, qui fournit uniquement une liste des noms d'IA des travailleurs disponibles, le superviseur déduit la spécialité de chaque travailleur et sélectionne stratégiquement le travailleur le plus approprié pour chaque tâche dans le flux de travail.

{% hint style = "info"%}
Étant donné que le superviseur n'a que les noms des travailleurs pour déduire leurs fonctionnalités à l'intérieur du flux de travail, il est très important que ces noms soient définis en conséquence. ** Les noms clairs, concises et descriptifs qui reflètent avec précision le rôle ou le domaine d'expertise du travailleur sont cruciaux pour que le superviseur prenne des décisions éclairées lors de la délégué des tâches. ** Cela garantit que le bon travailleur est sélectionné pour le bon travail, maximisant la précision du système pour répondre à la demande de l'utilisateur.
{% EndHint%}

***

## ** Le travailleur **

Le travailleur, en tant qu'agent spécialisé a demandé de gérer une tâche spécifique dans le système, nécessite que deux composants essentiels fonctionnent correctement:

* ** Un superviseur: ** Chaque travailleur doit être connecté au superviseur afin qu'il puisse être appelé lorsqu'une tâche doit être déléguée. Cette connexion établit la relation hiérarchique essentielle au sein du système multi-agents, garantissant que le superviseur peut distribuer efficacement le travail aux travailleurs spécialisés appropriés.
* ** Un nœud de modèle de chat capable d'appeler de fonction **: Par défaut, les travailleurs héritent du nœud du modèle de chat du superviseur à moins que ce soit, à moins que ce soit, à moins d'être attribué directement. Cette capacité d'appel de fonction permet au travailleur d'interagir avec des outils conçus pour sa tâche spécialisée.

<gigne> <img src = "../../. GitBook / Assets / MAS05.png" alt = "" width = "375"> <figcaption> </gigcaption> </gigu

{% hint style = "info"%}
La possibilité d'attribuer ** différents modèles de chat à chaque travailleur ** offre des opportunités de flexibilité et d'optimisation importantes pour notre application. En sélectionnant[Chat Models](../../integrations/langchain/chat-models/)Cappés à des tâches spécifiques, nous pouvons tirer parti des solutions plus rentables pour des tâches plus simples et réserver des modèles spécialisés, potentiellement plus chers, lorsqu'ils sont vraiment nécessaires.
{% EndHint%}

### Comprendre le paramètre d'itération maximum chez les travailleurs

[LangChain](https://python.langchain.com/v0.1/docs/modules/agents/how\_to/max\_iterations/)se réfère à`Max Iterations Cap`comme mécanisme de contrôle important pour prévenir le foin dans un système agentique. Dans notre contexte actuel, il nous sert de garde-corps contre les interactions excessives et potentiellement infinies entre le superviseur et le travailleur.

Contrairement aux nœuds du superviseur`Recursion Limit`, qui restreint combien de fois le superviseur peut s'appeler, le nœud de l'ouvrier`Max Iteration`Limites de paramètre combien de fois un superviseur peut itérer ou interroger un travailleur spécifique.

En plafonnant ou en limitant l'itération maximale, nous nous assurons que les coûts restent sous contrôle, même en cas de comportement du système inattendu.

***

## Exemple: un cas utilisateur pratique

Maintenant que nous avons établi une compréhension fondamentale de la façon dont les systèmes multi-agents fonctionnent dans Flowise, explorons une application pratique.

Imaginez un système multi-agents de sensibilisation ** ** (disponible sur le marché) conçu pour automatiser le processus d'identification, de qualification et de s'engager avec des prospects potentiels. Ce système utiliserait un superviseur pour orchestrer les deux travailleurs suivants:

* ** Chercheur Lead: ** Ce travailleur, utilisant l'outil de recherche Google, sera responsable de la collecte de prospects potentiels en fonction des critères définis par l'utilisateur.
* ** Générateur de ventes principales: ** Ce travailleur utilisera les informations recueillies par le chercheur principal pour créer des ébauches personnalisées pour l'équipe de vente.

<gigne> <img src = "../../. GitBook / Assets / Mas08.png" alt = ""> <Figcaption> </gigcaption> </ Figure>

** Contexte: ** Un utilisateur travaillant chez Solterra Renewables souhaite recueillir des informations disponibles sur Evergreen Energy Group, une société de renouvellement renouvelable située au Royaume-Uni, et ciblera son PDG, Amelia Croft, en tant que prospective potentielle.

** Demande de l'utilisateur: ** L'employé de Solterra Renewables fournit la requête suivante au système multi-agents: "_Je besoin d'informations sur le groupe d'énergie Evergreen et Amelia Croft en tant que nouveau client potentiel pour notre entreprise._"

1. **Superviseur:**
   * Le superviseur reçoit la demande de l'utilisateur et délègue la tâche de "recherche principale" à la`Lead Researcher Worker`.
2. ** Travailleur du chercheur principal: **
   * Le chercheur principal, en utilisant l'outil de recherche Google, recueille des informations sur Evergreen Energy Group, en se concentrant sur:
     * Contexte de l'entreprise, industrie, taille et emplacement.
     * Actualités et développements récents.
     * Les cadres clés, notamment confirmant le rôle d'Amelia Croft en tant que PDG.
   * Le chercheur principal renvoie les informations recueillies au`Supervisor`.
3. **Superviseur:**
   * Le superviseur reçoit les données de recherche du chercheur principal et confirme qu'Amelia Croft est un exemple pertinent.
   * Le superviseur délègue la tâche "générer des e-mails de vente"`Lead Sales Generator Worker`, fournissant:
     * Les informations de recherche sur Evergreen Energy Group.
     * Email d'Amelia Croft.
     * Contexte sur les énergies renouvelables de Solterra.
4. ** Faire un générateur de vente de plomb: **
   * Le travailleur du générateur de vente principale élabore un projet de courrier électronique personnalisé adapté à Amelia Croft, en prenant en compte:
     * Son rôle de PDG et la pertinence des services de Solterra Renewables à son entreprise.
     * Informations provenant de la recherche sur l'orientation ou les projets actuels de l'Evergreen Energy Group.
   * Le travailleur du générateur de ventes de leads renvoie le brouillon de courrier électronique terminé au`Supervisor`.
5. **Superviseur:**
   * Le superviseur reçoit le projet de courrier électronique généré et émet la directive "finir".
   * Le superviseur récupère le brouillon par e-mail à l'utilisateur, le`Solterra Renewables employee`.
6. ** L'utilisateur reçoit la sortie: ** L'employé de Solterra Renewables reçoit un projet de courrier électronique personnalisé prêt à être examiné et envoyé à Amelia Croft.

## Tutoriels vidéo

Ici, vous trouverez une liste de tutoriels vidéo de[Leon's YouTube channel](https://www.youtube.com/@leonvanzyl)montrant comment créer des applications multi-agents dans Flowise à l'aide de non-code.

{% embed url = "https://www.youtube.com/watch?ab_channel=leonvanzyl&v=284z8k7yjre"%}

{% embed url = "https://www.youtube.com/watch?ab_channel=leonvanzyl&v=maqco15y-vs"%}

{% Embed url = "https://www.youtube.com/watch?ab_channel=leonvanzyl&v=EAH7LDGMVES"%}
