---
description: Learn the Fundamentals of Sequential Agents in Flowise, written by @toi500
---

# Agents séquentiels

Ce guide offre un aperçu complet de l'architecture séquentielle du système d'agent AI dans Flowise, explorant ses composants principaux et ses principes de conception de workflow.

{% hint style = "avertissement"%}
** Avis de non-responsabilité **: Cette documentation est destinée à aider les utilisateurs à comprendre et à créer des workflows conversationnels à l'aide de l'architecture séquentielle du système d'agent. Il n'est pas destiné à être une référence technique complète pour le cadre Langgraph et ne doit pas être interprété comme la définition des normes de l'industrie ou des concepts de langue de base.
{% EndHint%}

## Concept

Construit au-dessus de[LangGraph](https://www.langchain.com/langgraph), L'architecture des agents séquentiels de Flowise facilite le ** développement de systèmes agentiques conversationnels en structurant le flux de travail en tant que graphique cyclique dirigé (DCG) **, permettant des boucles contrôlées et des processus itératifs.

Ce graphique, composé de nœuds interconnectés, définit le flux séquentiel d'informations et d'actions, permettant aux agents de traiter les entrées, d'exécuter des tâches et de générer des réponses de manière structurée.

<gigne> <img src = "../../../. GitBook / Assets / SEQ-21.Svg" alt = ""> <Figcaption> </ Figcaption> </gigne>

### Comprendre l'architecture DCG des agents séquentiels

Cette architecture simplifie la gestion des flux de travail conversationnels complexes en définissant une séquence d'opérations claire et compréhensible à travers sa structure DCG.

Explorons quelques éléments clés de cette approche:

{% Tabs%}
{% Tab Title = "Core Principles"%}
* ** Traitement basé sur le nœud: ** Chaque nœud du graphique représente une unité de traitement discrète, encapsulant sa propre fonctionnalité comme le traitement du langage, l'exécution d'outils ou la logique conditionnelle.
* ** Le flux de données sous forme de connexions: ** Les bords dans le graphique représentent le flux de données entre les nœuds, où la sortie d'un nœud devient l'entrée pour le nœud suivant, permettant une chaîne d'étapes de traitement.
* ** Gestion de l'État: ** L'état est géré comme un objet partagé, persistant tout au long de la conversation. Cela permet aux nœuds d'accéder aux informations pertinentes à mesure que le flux de travail progresse.
{% endtab%}

{% tab title = "Terminology"%}
* ** Flux: ** Le mouvement ou la direction des données dans le workflow. Il décrit comment l'information passe entre les nœuds lors d'une conversation.
* ** flux de travail: ** La conception globale et la structure du système. C'est le plan qui définit la séquence des nœuds, leurs connexions et la logique qui orchestre le flux de conversation.
* ** État: ** Une structure de données partagée qui représente l'instantané actuel de la conversation. Il comprend l'histoire de la conversation`state.messages`et toutes les variables d'état personnalisées définies par l'utilisateur.
* ** État personnalisé: ** Paires de valeurs clés définies par l'utilisateur ajoutées à l'objet d'état pour stocker des informations supplémentaires pertinentes pour le flux de travail.
* ** Outil: ** Un système, une API ou un service externes qui peuvent être accessibles et exécutés par le workflow pour effectuer des tâches spécifiques, telles que la récupération d'informations, le traitement des données ou l'interaction avec d'autres applications.
* ** Human-in-the-Boop (HITL): ** Une fonctionnalité qui permet une intervention humaine dans le flux de travail, principalement pendant l'exécution de l'outil. Il permet à un examinateur humain d'approuver ou de rejeter un appel d'outil avant son exécution.
* ** Exécution du nœud parallèle: ** Il fait référence à la possibilité d'exécuter plusieurs nœuds simultanément dans un flux de travail en utilisant un mécanisme de ramification. Cela signifie que différentes branches du flux de travail peuvent traiter les informations ou interagir simultanément avec les outils, même si le flux d'exécution global reste séquentiel.
{% endtab%}
{% endtabs%}

***

## Agents séquentiels vs multi-agents

Alors que les systèmes d'agent multi-agents et séquentiels en flux sont construits sur le cadre Langgraph et partagent les mêmes principes fondamentaux, l'architecture d'agent séquentiel fournit un[lower level of abstraction](#user-content-fn-1)[^ 1], offrant un contrôle plus granulaire sur chaque étape du workflow.

** Systèmes multi-agents **, qui se caractérisent par une structure hiérarchique avec un agent de superviseur central déléguant des tâches aux agents de travailleurs spécialisés, ** Excel à gérer des flux de travail complexes en les décomposant en sous-tâches gérables **. Cette décomposition en sous-tâches est rendue possible par des éléments de système de base pré-configuration sous le capot, tels que les nœuds de condition, qui nécessiteraient une configuration manuelle dans un système d'agent séquentiel. En conséquence, les utilisateurs peuvent plus facilement constituer et gérer des équipes d'agents.

En revanche, ** Systèmes d'agent séquentiels ** fonctionnent comme une chaîne de montage rationalisée, où les données circulent séquentiellement à travers une chaîne de nœuds, ce qui les rend idéales pour les tâches exigeant un ordre d'opérations précis et un raffinement incrémentiel de données. Par rapport au système multi-agents, son accès de niveau inférieur à la structure du flux de travail sous-jacent le rend fondamentalement plus ** flexible et personnalisable, offrant une exécution de nœuds parallèles et un contrôle complet sur la logique système **, incorporant les conditions, l'état et les nœuds de boucle dans le flux de travail, permettant la création de nouvelles capacités de ramification dynamique.

### Présentation des nœuds d'état, de boucle et de condition

Les agents séquentiels de Flowise offrent de nouvelles capacités pour la création de systèmes conversationnels qui peuvent s'adapter à l'entrée des utilisateurs, prendre des décisions en fonction du contexte et effectuer des tâches itératives.

Ces capacités sont rendues possibles par l'introduction de quatre nouveaux nœuds principaux; Le nœud d'état, le nœud de boucle et deux nœuds de condition.

<gigne> <img src = "../../../. GitBook / Assets / Seq-20.png" alt = ""> <Figcaption> </gigcaption> </gigust>

* ** Node d'état: ** Nous définissons l'état comme une structure de données partagée qui représente l'instantané actuel de notre application ou flux de travail. Le nœud d'état nous permet d'ajouter un état personnalisé ** à notre flux de travail dès le début de la conversation. Cet état personnalisé est accessible et modifiable par d'autres nœuds du workflow, permettant le comportement dynamique et le partage de données.
* ** Node de boucle: ** Ce nœud ** introduit des cycles contrôlés ** dans le flux de travail de l'agent séquentiel, permettant des processus itératifs où une séquence de nœuds peut être répétée en fonction de conditions spécifiques. Cela permet aux agents d'affiner les sorties, de collecter des informations supplémentaires auprès de l'utilisateur ou d'effectuer des tâches plusieurs fois.
* ** Nœuds de condition: ** Le nœud d'agent de condition et de condition fournit le contrôle nécessaire pour ** Créer des flux de conversation complexes avec des chemins de ramification **. Le nœud de condition évalue directement les conditions, tandis que le nœud d'agent de condition utilise le raisonnement d'un agent pour déterminer la logique de ramification. Cela nous permet de guider dynamiquement le comportement du flux en fonction de l'entrée utilisateur, de l'état personnalisé ou des résultats des actions prises par d'autres nœuds.

### Choisir le bon système

La sélection du système idéal pour votre application dépend de la compréhension de vos besoins spécifiques de flux de travail. Des facteurs tels que la complexité des tâches, le besoin de traitement parallèle et le niveau de contrôle souhaité sur le flux de données sont tous des considérations clés.

* ** Pour la simplicité: ** Si votre flux de travail est relativement simple, où les tâches peuvent être accomplies l'une après l'autre et ne nécessitent donc pas une exécution de nœuds parallèles ou un humain dans la boucle (HITL), l'approche multi-agent offre une facilité d'utilisation et une configuration rapide.
* ** Pour la flexibilité: ** Si votre flux de travail a besoin d'une exécution parallèle, de conversations dynamiques, d'une gestion de l'état personnalisé et de la possibilité d'incorporer HITL, l'approche ** Agent séquentiel ** offre la flexibilité et le contrôle nécessaires.

Voici un tableau comparant les implémentations d'agents multi-agents et séquentielles dans Flowise, mettant en évidence les différences clés et les considérations de conception:

<Bile> <Thead> <Tr> <th width = "173.3333333333331"> </ th> <th width = "281"> Multi-agent </th> <th> Agent séquentiel </th> </tr> </thead> <tbody> <tr> <td> Structure </td> <td> <strong> Hierarch </ Strong>; Le superviseur délégue aux travailleurs spécialisés. </td> <td> <strong> linéaire, cyclique et / ou </strong> <strong> ramification </strong>; Les nœuds se connectent dans une séquence, avec une logique conditionnelle pour la ramification. </td> </tr> <tr> <td> Film de travail </td> <td> flexible; Conçu pour décomposer une tâche complexe en une séquence <strong> de sous-tâches </strong>, en a terminé les unes après les autres. </td> <td> Très flexible; <strong> prend en charge l'exécution du nœud parallèle </strong>, les flux de dialogue complexes, la logique de ramification et les boucles dans un seul tour de conversation. </td> </tr> <tr> <td> Exécution du nœud parallèle </td> <td> <strong> non </strong>; Le superviseur gère une tâche à la fois. </td> <td> <strong> Oui </strong>; peut déclencher plusieurs actions en parallèle dans une seule exécution. </td> </tr> <tr> <td> Gestion de l'état </td> <td> <strong> implicite </strong>; L'état est en place, mais n'est pas explicitement géré par le développeur. </td> <td> <strong> explicite </strong>; L'état est en place, et les développeurs peuvent définir et gérer un état initial ou personnalisé à l'aide du nœud d'état et du champ "State de mise à jour" dans divers nœuds. </td> </tr> <tr> <td> Utilisation d'outils </td> <td> <strong> Les travailleurs </strong> peuvent accéder et utiliser des outils selon les besoins. </td> <td> Les outils sont accessibles et exécutés via <strong> Les nœuds </strong>. </td> </tr> <tr> <td> Human-in-the-Loop (HITL) </td> <td> hitl est <strong> non pris en charge. </strong> </td> <td> <strong> Prise en charge </strong> par le nœud de l'agent et le nœud de l'outil "nécessite l'approbation", permettant une revue humaine et une approbation ou une approbation ou un rejet de l'outil de l'outil de l'outil " exécution. </td> </tr> <tr> <td> complexité </td> <td> Niveau d'abstraction plus élevé; <strong> simplifie la conception du flux de travail. </strong> </td> <td> Niveau d'abstraction inférieur; <strong> Conception de workflow plus complexe </strong>, nécessitant une planification minutieuse des interactions de nœuds, une gestion de l'état personnalisé et une logique conditionnelle. </td> </tr> <Tr> <Td> Cas d'utilisation idéale </td> <td> <ul> <li> Les processus linéaires automatisés (par exemple, l'extraction des données doivent être achevé Autre. </li> </ul> </td> <td> <ul> <li> Construire des systèmes de conversation avec des flux dynamiques. </li> <li> Des flux de travail complexes nécessitant une exécution de nœuds parallèles ou une logique de branchement. </li> <li> Situations où la prise de décision est nécessaire à plusieurs points dans la conversation.

{% hint style = "info"%}
** Remarque **: Même si les systèmes multi-agents sont techniquement une couche de niveau supérieur construite sur l'architecture d'agent séquentiel, ils offrent une expérience utilisateur distincte et une approche de la conception du flux de travail. La comparaison ci-dessus les traite comme des systèmes séparés pour vous aider à sélectionner la meilleure option pour vos besoins spécifiques.
{% EndHint%}

***

## Nœuds d'agents séquentiels

Les agents séquentiels apportent une toute nouvelle dimension à couler, ** Présentation de 10 nœuds spécialisés **, chacun servant un objectif spécifique, offrant plus de contrôle sur la façon dont nos agents conversationnels interagissent avec les utilisateurs, traitent les informations, prennent des décisions et exécutent des actions.

Les sections suivantes visent à fournir une compréhension complète des fonctionnalités, des entrées, des sorties et des meilleures pratiques de chaque nœud, vous permettant finalement de créer des flux de travail conversationnels sophistiqués pour une variété d'applications.

<gigne> <img src = "../../../. GitBook / Assets / SEQ-00.png" alt = ""> <figCaption> </gigcaption> </gigne>

***

## 1. Noeud de démarrage

Comme son nom l'indique, le nœud de démarrage est le ** point d'entrée pour tous les workflows de l'architecture d'agent séquentiel **. Il reçoit la requête utilisateur initiale, initialise l'état de conversation et définit le flux en mouvement.

<gigne> <img src = "../../../. GitBook / Assets / SEQ-02.png" alt = "" width = "300"> <Figcaption> </gigcaption> </gigust>

### Comprendre le nœud de démarrage

Le nœud de démarrage garantit que nos workflows conversationnels ont la configuration et le contexte nécessaires pour fonctionner correctement. ** Il est responsable de la configuration des fonctionnalités clés ** qui seront utilisées dans le reste du workflow:

* ** Définition du LLM par défaut: ** Le nœud de démarrage nous oblige à spécifier un modèle de chat (LLM) compatible avec l'appel de fonction, permettant aux agents du workflow d'interagir avec les outils et les systèmes externes. Ce sera le LLM par défaut utilisé sous le capot dans le workflow.
* ** Initialisation de la mémoire: ** Nous pouvons éventuellement connecter un nœud de mémoire d'agent pour stocker et récupérer l'historique de la conversation, permettant plus de réponses de contexte.
* ** Définition d'un état personnalisé: ** Par défaut, l'état contient un immuable`state.messages`Array, qui agit comme la transcription ou l'historique de la conversation entre l'utilisateur et les agents. Le nœud de démarrage vous permet de connecter un état personnalisé au workflow ajoutant un nœud d'état, permettant le stockage d'informations supplémentaires pertinentes à votre flux de travail
* ** Activant la modération: ** éventuellement, nous pouvons connecter la modération d'entrée pour analyser l'entrée de l'utilisateur et empêcher le contenu potentiellement nocif d'être envoyé à la LLM.

### Entrées

<Bile> <Thead> <Tr> <th width = "212"> </th> <th width = "102"> requis </th> <th> Description </th> </tr> </thead> <tbody> <tr> <td> Chat Modèle </td> <td> <strong> oui </strong> </td> <td> le LLM défaut qui alimentera la conversation. Uniquement compatible avec <strong> des modèles capables de fonctionner l'appel </strong>. </td> </tr> <tr> <td> Node de mémoire de l'agent </strong> Activer la persistance et la préservation du contexte </strong>. </td> </tr> <tr> Nœud </td> <td> non </td> <td> connecter un nœud d'état à <strong> définir un état personnalisé </strong>, un contexte partagé qui peut être accessible et modifié par d'autres nœuds dans le flux de travail. </td> </tr> <tr> <td> Modération de saisie </td> <td> NO </td> <td> Connectez un NODE de modération pour </srof> Détection du texte qui pourrait générer une sortie nocive, empêchant l'empêcher d'être envoyé au llm. </td> </tr> </tbody> </s table>

### Sorties

Le nœud de démarrage peut se connecter aux nœuds suivants comme sorties:

* ** Nœud d'agent: ** achemine le flux de conversation vers un nœud d'agent, qui peut ensuite exécuter des actions ou accéder aux outils en fonction du contexte de la conversation.
* ** NODE LLM: ** ITSINE Le flux de conversation vers un nœud LLM pour le traitement et la génération de réponse.
* ** Node d'agent de condition: ** Se connecte à un nœud d'agent de condition pour implémenter la logique de branchement en fonction de l'évaluation de la conversation par l'agent.
* ** Node de condition: ** Se connecte à un nœud de condition pour implémenter la logique de branchement en fonction des conditions prédéfinies.

### Meilleures pratiques

{% Tabs%}
{% Tab Title = "Pro Tips"%}
** Choisissez le bon modèle de chat **

Assurez-vous que votre LLM sélectionné prend en charge l'appel de la fonction, une fonctionnalité clé pour activer les interactions agent-outil. De plus, choisissez un LLM qui s'aligne sur la complexité et les exigences de votre application. Vous pouvez remplacer le LLM par défaut en le définissant au niveau du nœud d'agent / LLM / Condition d'agent si nécessaire.

** Considérez le contexte et la persistance **

Si votre cas d'utilisation l'exige, utilisez le nœud de mémoire de l'agent pour maintenir le contexte et personnaliser les interactions.
{% endtab%}

{% tab title = "Pièges potentiels"%}
** Sélection du modèle de chat incorrect (LLM) **

* ** Problème: ** Le modèle de chat sélectionné dans le nœud de démarrage ne convient pas aux tâches ou capacités prévues du flux de travail, ce qui entraîne de mauvaises performances ou des réponses inexactes.
* ** Exemple: ** Un flux de travail nécessite un modèle de chat avec de fortes capacités de résumé, mais le nœud de démarrage sélectionne un modèle optimisé pour la génération de code, conduisant à des résumés inadéquats.
* ** Solution: ** Choisissez un modèle de chat qui s'aligne sur les exigences spécifiques de votre flux de travail. Considérez les forces, les faiblesses du modèle et les types de tâches dans lesquelles il excelle. Reportez-vous à la documentation et expérimentez avec différents modèles pour trouver le meilleur ajustement.

** Présentation de la configuration du nœud de mémoire de l'agent **

* ** Problème: ** Le nœud de mémoire de l'agent n'est pas correctement connecté ou configuré, entraînant la perte de données d'historique de conversation entre les sessions.
* ** Exemple: ** Vous avez l'intention d'utiliser la mémoire persistante pour stocker les préférences des utilisateurs, mais le nœud de mémoire de l'agent n'est pas connecté au nœud de démarrage, ce qui fait réinitialiser les préférences sur chaque nouvelle conversation.
* ** Solution: ** Assurez-vous que le nœud de mémoire de l'agent est connecté au nœud de démarrage et configuré avec la base de données appropriée (SQLite). Pour la plupart des cas d'utilisation, la base de données SQLite par défaut sera suffisante.

** Modération d'entrée inadéquate **

* ** Problème: ** La "modération d'entrée" n'est pas activée ou configurée correctement, permettant à l'entrée utilisateur potentiellement nocive ou inappropriée d'atteindre le LLM et de générer des réponses indésirables.
* ** Exemple: ** Un utilisateur soumet un langage offensant, mais la modération d'entrée ne le détecte pas ou n'est pas du tout configurée, permettant à la requête d'atteindre le LLM.
* ** Solution: ** Ajouter et configurer un nœud de modération d'entrée dans le nœud de démarrage pour filtrer le langage potentiellement nocif ou inapproprié. Personnalisez les paramètres de modération pour vous aligner sur vos exigences spécifiques et vos cas d'utilisation.
{% endtab%}
{% endtabs%}

## 2. Node de mémoire de l'agent

Le nœud de mémoire de l'agent ** fournit un mécanisme de stockage de mémoire persistant **, permettant au flux de travail d'agent séquentiel de conserver l'historique de la conversation`state.messages`et tout état personnalisé précédemment défini sur plusieurs interactions

Cette mémoire à long terme est essentielle pour que les agents apprennent des interactions précédentes, maintiennent le contexte par rapport aux conversations étendues et fournissent des réponses plus pertinentes.

<Figure> <img src = "../../../. GitBook / Assets / SEQ-03.png" alt = "" width = "299"> <Figcaption> </gigcaption> </ figure>

### Où les données sont enregistrées

Par défaut, Flowise utilise sa ** Base de données SQLite ** ** pour stocker l'historique de la conversation et les données d'état personnalisées, créant un tableau "** Checkpoints **" pour gérer ces informations persistantes.

#### Comprendre la structure et le format de données des "points de contrôle"

Ce tableau ** stocke des instantanés de l'état du système à différents moments lors d'une conversation **, permettant la persistance et la récupération de l'histoire de la conversation. Chaque ligne représente un point ou un «point de contrôle» spécifique dans l'exécution du workflow.

<gigne> <img src = "../../../. GitBook / Assets / SEQ-12.png" alt = ""> <figCaption> </gigcaption> </gigust>

#### Structure de table

* ** Thread \ _id: ** Un identifiant unique représentant une session de conversation spécifique, ** Notre ID de session **. Il regroupe tous les points de contrôle liés à une seule exécution de workflow.
* ** Checkpoint \ _id: ** Un identifiant unique pour chaque étape d'exécution (exécution du nœud) dans le workflow. Il aide à suivre l'ordre des opérations et à identifier l'état à chaque étape.
* ** Parent \ _id: ** Indique le point de contrôle \ _id de l'étape d'exécution précédente qui a conduit au point de contrôle actuel. Cela établit une relation hiérarchique entre les points de contrôle, permettant la reconstruction du flux d'exécution du flux de travail.
* ** Checkpoint: ** Contient une chaîne JSON représentant l'état actuel du flux de travail à ce point de contrôle spécifique. Cela inclut les valeurs des variables, les messages échangés et toutes les autres données pertinentes capturées à ce stade de l'exécution.
* ** Metadata: ** fournit un contexte supplémentaire sur le point de contrôle, spécifiquement lié aux opérations de nœud.

#### Comment ça marche

Au fur et à mesure qu'un flux de travail d'agent séquentiel s'exécute, le système enregistre un point de contrôle dans ce tableau pour chaque étape significative. Ce mécanisme offre plusieurs avantages:

* ** Suivi d'exécution: ** Les points de contrôle permettent au système de comprendre le chemin d'exécution et l'ordre des opérations dans le workflow.
* ** Gestion de l'État: ** Les points de contrôle stockent l'état du flux de travail à chaque étape, y compris les valeurs variables, l'historique de conversation et toutes les autres données pertinentes. Cela permet au système de maintenir une conscience contextuelle et de prendre des décisions éclairées en fonction de l'état actuel.
* ** Resomption du flux de travail: ** Si le workflow est interrompu ou interrompu (par exemple, en raison d'une erreur système ou d'une demande utilisateur), le système peut utiliser les points de contrôle stockés pour reprendre l'exécution du dernier état enregistré. Cela garantit que la conversation ou la tâche se poursuit à partir de son séjour, préservant les progrès de l'utilisateur et empêchant la perte de données.

### ** Entrées **

Le nœud de mémoire de l'agent n'a ** pas de connexions d'entrée spécifiques **.

### Configuration du nœud

<Bile> <Thead> <Tr> <th width = "189"> </th> <th width = "107"> requis </th> <th> Description </th> </tr> </thead> <tbody> <tr> <td> database </td> <td> <strong> oui </strong> </td> <td> le type de base de base de données pour les histoires de conversation. Actuellement, <strong> seul sqlite est pris en charge </strong>. </td> </tr> </tbody> </s table>

### Paramètres supplémentaires

<Bile> <Thead> <Tr> <th width = "189"> </ th> <th width = "107"> requis </th> <th> Description </th> </tr> </thead> <tbody> <tr> <td> Le fichier pathle </td> <td> non </td> <td> le trajet du fichier du fichier de données SQLite. <strong> Si cela n'est pas fourni, le système utilisera un emplacement par défaut </strong>. </td> </tr> </tbody> </ table>

### ** Sorties **

Le nœud de mémoire de l'agent interagit uniquement avec le nœud ** de démarrage **, rendant l'historique de conversation disponible à partir du tout début du workflow.

### ** meilleures pratiques **

{% Tabs%}
{% Tab Title = "Pro Tips"%}
** Utilisation stratégique **

Utilisez la mémoire d'agent uniquement lorsque cela est nécessaire. Pour les interactions simples et apatrides, cela pourrait être exagéré. Réservez-le pour des scénarios où le conservation des informations à travers les virages ou les séances est essentiel.
{% endtab%}

{% tab title = "Pièges potentiels"%}
** Average inutile **

* ** Le problème: ** Utilisation de la mémoire d'agent pour chaque interaction, même lorsqu'il n'est pas nécessaire, introduit un stockage et un traitement inutiles. Cela peut ralentir les temps de réponse et augmenter la consommation de ressources.
* ** Exemple: ** Un chatbot météo simple qui fournit des informations basées sur une seule demande d'utilisateur n'a pas besoin de stocker l'historique des conversations.
* ** Solution: ** Analyser les exigences de votre système et utiliser la mémoire d'agent lorsque le stockage de données persistant est essentiel pour la fonctionnalité ou l'expérience utilisateur.
{% endtab%}
{% endtabs%}

***

## 3. Node d'état

Le nœud d'état, qui ne peut être connecté qu'au nœud de démarrage, ** fournit un mécanisme pour définir un état défini ou personnalisé ** dans notre flux de travail à partir du début de la conversation. Cet état personnalisé est un objet JSON partagé et peut être mis à jour par les nœuds du graphique, passant d'un nœud à un autre au fur et à mesure que le flux progresse.

<gigne> <img src = "../../../. GitBook / Assets / SEQ-04.png" alt = "" width = "299"> <Figcaption> </ Figcaption> </ Figure>

### Comprendre le nœud d'État

Par défaut, l'état comprend un`state.messages`Array, qui agit comme notre histoire de conversation. Ce tableau stocke tous les messages échangés entre l'utilisateur et les agents, ou tout autre acteur du flux de travail, en le préservant tout au long de l'exécution du workflow.

Car par définition`state.messages`Le tableau est immuable et ne peut pas être modifié, ** Le but du nœud d'état est de nous permettre de définir les paires de valeurs clés personnalisées **, élargissant l'objet d'état pour contenir toute information supplémentaire pertinente pour notre flux de travail.

{% hint style = "info"%}
Lorsqu'aucun nœud de mémoire d'agent ** n'est utilisé, l'état fonctionne en mémoire et n'est pas persisté pour une utilisation future.
{% EndHint%}

### Entrées

Le nœud d'état n'a ** pas de connexions d'entrée spécifiques **.

### Sorties

Le nœud d'état ne peut se connecter qu'au ** Node de démarrage **, permettant la configuration d'un état personnalisé depuis le début du flux de travail et permettant à d'autres nœuds d'accéder et potentiellement de modifier cet état personnalisé partagé.

### Paramètres supplémentaires

<Bile> <Thead> <Tr> <th width = "157"> </th> <th width = "113"> requis </th> <th> Description </th> </tr> </thead> <tbody> <tr> <td> État personnalisé </td> <td> <strong> Oui </strong> </td> <td> un objet JSON représentant </strong> l'état de personnalité </td>. Cet objet peut contenir toutes les paires de valeurs de clé pertinentes pour l'application. </td> </tr> </tbody> </ table>

### Comment définir un état personnalisé <a href = "# alert-dialog-title" id = "alert-dialog-title"> </a>

Spécifiez la clé ** **, ** Type d'opération ** et ** Valeur par défaut ** pour l'objet d'état. Le type d'opération peut être "remplacer" ou "ajouter".

* **Remplacer**
  1. Remplacez la valeur existante par la nouvelle valeur.
  2. Si la nouvelle valeur est nul, la valeur existante sera conservée.
* **Ajouter**
  1. Ajoutez la nouvelle valeur à la valeur existante.
  2. Les valeurs par défaut peuvent être vides ou un tableau. Ex: \ ["a", "b"]
  3. La valeur finale est un tableau.

#### Exemple utilisant JS

{% code overflow = "wrap"%}
```javascript
{
    aggregate: {
        value: (x, y) => x.concat(y), // here we append the new message to the existing messages
        default: () => []
    }
}
```
{% Endcode%}

#### Exemple à l'aide de la table

Pour définir un état personnalisé à l'aide de l'interface de table dans le nœud d'état, suivez ces étapes:

1. ** Ajouter l'élément: ** Cliquez sur le bouton "+ Ajouter l'élément" pour ajouter des lignes au tableau. Chaque ligne représente une paire de valeurs clés dans votre état personnalisé.
2. ** Spécifiez les touches: ** Dans la colonne "clé", entrez le nom de chaque clé que vous souhaitez définir dans votre objet d'état. Par exemple, vous pourriez avoir des clés comme "nom d'utilisateur", "userLocation", etc.
3. ** Choisissez les opérations: ** Dans la colonne "Opération", sélectionnez l'opération souhaitée pour chaque clé. Vous avez deux options:
   * ** Remplacer: ** Cela remplacera la valeur existante de la touche par la nouvelle valeur fournie par un nœud. Si la nouvelle valeur est nul, la valeur existante sera conservée.
   * ** Ajouter: ** Cela ajoutera la nouvelle valeur à la valeur existante de la clé. La valeur finale sera un tableau.
4. ** Définissez les valeurs par défaut: ** Dans la colonne "Valeur par défaut", entrez la valeur initiale pour chaque touche. Cette valeur sera utilisée si aucun autre nœud ne fournit une valeur pour la clé. La valeur par défaut peut être vide ou un tableau.

#### Exemple de table

| Clé | Opération | Valeur par défaut |
| -------- | --------- | ------------- |
| Nom d'utilisateur | Remplacer | NULL |

<gigne> <img src = "../../../. GitBook / Assets / SEQ-14.png" alt = "" width = "375"> <Figcaption> </gigcaption> </gigust>

1. Ce tableau définit une clé à l'état personnalisé:`userName`.
2. Le`userName`La clé utilisera l'opération "Remplacer", ce qui signifie que sa valeur sera mise à jour chaque fois qu'un nœud fournit une nouvelle valeur.
3. Le`userName`Key a une valeur par défaut de _null, _ indiquant qu'il n'a pas de valeur initiale.

{% hint style = "info"%}
N'oubliez pas que cette approche basée sur une table est une alternative à la définition de l'état personnalisé à l'aide de JavaScript. Les deux méthodes obtiennent le même résultat.
{% EndHint%}

#### Exemple utilisant l'API

```json
{
    "question": "hello",
    "overrideConfig": {
        "stateMemory": [
            {
                "Key": "userName",
                "Operation": "Replace",
                "Default Value": "somevalue"
            }
        ]
    }
}
```

### Meilleures pratiques

{% Tabs%}
{% tab title = "pro-tips"%}
** Planifiez votre structure d'état personnalisée **

Avant de construire votre flux de travail, concevez la structure de votre état personnalisé. Un état personnalisé bien organisé rendra votre flux de travail plus facile à comprendre, à gérer et à déboguer.

** Utilisez des noms de clés significatifs **

Choisissez des noms de clés descriptifs et cohérents qui indiquent clairement l'objectif des données qu'ils détiennent. Cela améliorera la lisibilité de votre code et facilitera que les autres (ou vous à l'avenir) comprennent comment l'état personnalisé est utilisé.

** Gardez l'état personnalisé minimal **

Stockez uniquement les informations dans l'état personnalisé qui est essentielle à la logique et à la prise de décision du workflow.

** Considérons la persistance de l'État **

Si vous devez préserver l'état sur plusieurs sessions de conversation (par exemple, pour les préférences des utilisateurs, l'historique des commandes, etc.), utilisez le nœud de mémoire de l'agent pour stocker l'état dans une base de données persistante.
{% endtab%}

{% tab title = "Pièges potentiels"%}
** Mises à jour d'état incohérentes **

* ** Problème: ** La mise à jour de l'état personnalisé en plusieurs nœuds sans stratégie claire peut entraîner des incohérences et un comportement inattendu.
* **Exemple**
  1. Mises à jour de l'agent 1`orderStatus`au "paiement confirmé".
  2. L'agent 2, dans une succursale différente, mises à jour`orderStatus`pour "commander complet" sans vérifier l'état précédent.
* ** Solution: ** Utiliser les nœuds des conditions pour contrôler le flux des mises à jour de l'état personnalisé et s'assurer que les transitions d'état personnalisées se produisent de manière logique et cohérente.
{% endtab%}
{% endtabs%}

***

## 4. Node d'agent

Le nœud d'agent est un composant Core ** de l'architecture d'agent séquentiel. ** Il agit comme un décideur et un orchestrateur dans notre flux de travail.

<gigne> <img src = "../../../. GitBook / Assets / sa-agent.png" alt = "" width = "268"> <Figcaption> </ Figcaption> </ Figure>

### Comprendre le nœud d'agent

En recevant les commentaires des nœuds précédents, qui comprend toujours l'historique complet de la conversation`state.messages`Et tout état personnalisé à ce stade de l'exécution, le nœud d'agent utilise sa "personnalité" définie, établie par l'invite du système, pour déterminer si des outils externes sont nécessaires pour répondre à la demande de l'utilisateur.

* Si des outils sont nécessaires, le nœud d'agent sélectionne et exécute de manière autonome l'outil approprié. Cette exécution peut être automatique ou, pour les tâches sensibles, nécessite l'approbation humaine (HITL) avant de continuer. Une fois l'outil terminé son fonctionnement, le nœud d'agent reçoit les résultats, les traite à l'aide du modèle de chat désigné (LLM) et génère une réponse complète.
* Dans les cas où aucun outil n'est nécessaire, le nœud d'agent exploite directement le modèle de chat (LLM) pour formuler une réponse basée sur le contexte de conversation actuel.

### Entrées

<Bile> <Thead> <Tr> <th width = "195"> </th> <th width = "107"> requis </th> <th> Description </th> </tr> </ thead> <tbody> <tr> <td> Les outils externes </td> <td> no </td> <td> offrent le nœud d'agent avec <strong> accès à une suite d'outils d'EXTERIAL </ Strong>. Pour effectuer des actions et récupérer des informations. </td> </tr> <tr> <td> Modèle de chat </td> <td> non </td> <td> Ajoutez un nouveau modèle de chat à <strong> écraser le modèle de chat par défaut </strong> (LLM) du flux de travail. Compatible uniquement avec des modèles capables d'appels de fonction. </td> </tr> <tr> <td> Démarrer le nœud </td> <td> <strong> oui </strong> </td> <td> reçoit le <strong> Entrée utilisateur initial </strong>, ainsi que l'état personnalisé (IF Set Up) et le reste de l'état par défaut <code>. Node. </td> </tr> <tr> <Td> Condition Node </td> <td> <strong> Oui </strong> </td> <td> reçoit les entrées à partir d'un nœud de condition précédente, permettant au nœud d'agent de prendre des actions ou de guider la conversation en fonction du résultat de l'état de la condition du nœud </strong>. </td> </ tr> Node </td> <td> <strong> Oui </strong> </td> <td> reçoit les entrées d'un nœud d'agent de condition précédente, permettant au nœud de l'agent <strong> prendre des actions ou guider la conversation en fonction du résultat de l'évaluation de l'agent d'agent </strong>. </td> </tr> <Tr> <td> Agent d'agent d'agent </strong>. </td> Node </td> <td> <strong> Oui </strong> </td> <td> reçoit les entrées d'un nœud d'agent précédent, <strong> Activé des actions d'agent chaîné </strong> et en maintenant le contexte de conversation </td> </tr> <tr> <td> LLM NODE </TD> <TD> <strong> Oui </strong> </rd> Permettre au nœud de l'agent <strong> traiter la réponse de la LLM </strong>. </td> </tr> <tr> <td> Node d'outil </td> <td> <strong> Oui </strong> </td> <td> reçoit la sortie d'un nœud d'outil, permettant au nœud d'agent <strong> de traiter et d'intégrer les sorties de l'outil dans son son Réponse </strong>. </td> </tr> </tbody> </ table>

{% hint style = "info"%}
Le nœud d'agent ** nécessite au moins une connexion à partir des nœuds suivants **: nœud de démarrage, nœud d'agent, nœud de condition, nœud d'agent de condition, nœud LLM ou nœud d'outil.
{% EndHint%}

### Sorties

Le nœud d'agent peut se connecter aux nœuds suivants comme sorties:

* ** Node d'agent: ** passe le contrôle à un nœud d'agent ultérieur, permettant le chaînage de plusieurs actions d'agent dans un workflow. Cela permet des flux conversationnels plus complexes et une orchestration des tâches.
* ** Node LLM: ** passe la sortie de l'agent à un nœud LLM, permettant un traitement linguistique, une génération de réponse ou une prise de décision supplémentaires en fonction des actions et des informations de l'agent.
* ** Node d'agent de condition: ** Dirige le flux vers un nœud d'agent de condition. Ce nœud évalue la sortie du nœud d'agent et ses conditions prédéfinies pour déterminer l'étape suivante appropriée dans le flux de travail.
* ** Node de condition: ** Similaire au nœud d'agent de condition, le nœud de condition utilise des conditions prédéfinies pour évaluer la sortie du nœud d'agent, dirigeant le flux le long de différentes branches en fonction du résultat.
* ** Node de fin: ** conclut le flux de conversation.
* ** Node de boucle: ** Redirige le flux vers un nœud précédent, permettant des processus itératifs ou cycliques dans le flux de travail. Ceci est utile pour les tâches qui nécessitent plusieurs étapes ou impliquent de raffiner les résultats en fonction des interactions précédentes. Par exemple, vous pouvez remonter à un nœud d'agent antérieur ou à un nœud LLM pour recueillir des informations supplémentaires ou affiner le flux de conversation en fonction de la sortie du nœud d'agent actuel.

### Configuration du nœud

<Bile> <Thead> <Tr> <Th width = "201"> </ th> <th width = "101"> requis </th> <th> Description </th> </tr> </thead> <tbody> <tr> <td> Nom de l'agent </td> <td> <strong> Oui </strong> </td> <td> <strong> ciblez-le lorsque vous utilisez des boucles </strong> dans le workflow. </td> </tr> <tr> <td> invite du système </td> <td> non </td> <td> définit le <strong> `` personne '' de l'agent </strong> et <strong> guide son comportement </strong>. Par exemple, "<em> vous êtes un agent de service client spécialisé dans le support technique </em> [...]." </td> </tr> <tr> <td> Exiger l'approbation </td> <td> non </td> <td> <strong> active la fonction Human-in-the-Loop (HITL). Si défini sur '<strong> true </strong>,' le nœud d'agent demandera l'approbation humaine avant d'exécuter un outil. Ceci est particulièrement utile pour les opérations sensibles ou lorsque la surveillance humaine est souhaitée. Par défaut est '<strong> false </strong>,' permettant au nœud d'agent d'exécuter des outils de manière autonome. </td> </tr> </tbody> </s table>

### Paramètres supplémentaires

<Bile> <Thead> <Tr> <Th width = "200"> </ th> <th width = "102"> requis </th> <th> Description </th> </tr> </thead> <tbody> <tr> <td> invite humain </td> <td> non </td> <td> Cette invite est en guise <code> State.Messages. Il nous permet d'injecter un message de type humain dans le flux de conversation </strong> après que le nœud d'agent a traité sa contribution et avant que le nœud suivant ne reçoive la sortie du nœud d'agent. </td> </tr> <tr> <Td> Invite d'approbation </td> <td> Non </td> <td> <proprié> Une invite personnalisable présentée à l'Ensembleur humain lorsque la surface Hitl est active. Cette invite fournit un contexte sur l'exécution de l'outil, y compris le nom et le but de l'outil. La variable <code> {outils} </code> dans l'invite sera remplacée dynamiquement par la liste réelle des outils suggérés par l'agent, en veillant à ce que le réviseur humain ait toutes les informations nécessaires pour prendre une décision éclairée. </td> Interface hitl. Cela permet d'adapter le langage au contexte spécifique et d'assurer la clarté du réviseur humain. </td> </tr> <tr> <td> Rejeter le texte du bouton </td> <td> Non </td> <td> Personnalise le texte <strong> affiché sur le bouton pour rejeter l'exécution de l'outil </strong> dans l'interface hitl. Comme le texte du bouton Approuver, cette personnalisation améliore la clarté et fournit une action claire à prendre pour le réviseur humain s'il jugera l'exécution de l'outil inutile ou potentiellement nocif. </td> </tr> <tr> <td> État de mise à jour </td> <td> non </td> <td> fournit un <strong> mécanisme pour modifier l'objet d'état personnalisé partagé dans le cadre du workflow </ Strong>. Ceci est utile pour stocker les informations recueillies par l'agent ou influencer le comportement des nœuds suivants. </td> </tr> <tr> <td> MAX IDERATER </TD> <TD> NON </TD> <TD> Limite le <strong> nombre d'itérations </strong> Un nœud d'agent peut faire dans une seule exécution de workfwing. </td> </r>

### Meilleures pratiques

{% Tabs%}
{% Tab Title = "Pro Tips"%}
** Clear System Invite **

Créez une invite de système concise et sans ambiguïté qui reflète avec précision le rôle et les capacités de l'agent. Cela guide la prise de décision de l'agent et garantit qu'elle agit dans sa portée définie.

** Sélection d'outils stratégiques **

Choisissez et configurez les outils disponibles pour le nœud d'agent, en vous assurant qu'ils s'alignent avec l'objectif de l'agent et les objectifs globaux du flux de travail.

** Hitl pour les tâches sensibles **

Utilisez l'option «exiger l'approbation» pour les tâches impliquant des données sensibles, nécessitant un jugement humain ou comporte un risque de conséquences imprévues.

** Tirez parti des mises à jour d'état personnalisés **

Mettez à jour l'objet d'état personnalisé stratégiquement pour stocker les informations recueillies ou influencer le comportement des nœuds en aval.
{% endtab%}

{% tab title = "Pièges potentiels"%}
** Inaction de l'agent en raison de la surcharge d'outils **

* ** Problème: ** Lorsqu'un nœud d'agent a accès à un grand nombre d'outils dans une seule exécution du flux de travail, il peut avoir du mal à décider quel outil est le plus approprié à utiliser, même lorsqu'un outil est clairement nécessaire. Cela peut conduire à l'agent à ne pas appeler un outil, ce qui entraîne des réponses incomplètes ou inexactes.
* ** Exemple: ** Imaginez un agent de support client conçu pour gérer un large éventail de demandes. Vous l'avez équipé d'outils pour le suivi des commandes, les informations de facturation, les rendements des produits, le support technique, etc. Un utilisateur demande: "Quel est le statut de ma commande?" Mais l'agent, submergé par le nombre d'outils potentiels, répond par une réponse générique comme: "Je peux vous aider. Quel est votre numéro de commande?" sans réellement utiliser l'outil de suivi des commandes.
* **Solution**
  1. ** Affiner les invites du système: ** Fournissez des instructions et des exemples plus clairs dans l'invite du système du nœud d'agent pour le guider vers la bonne sélection d'outils. Si nécessaire, soulignez les capacités spécifiques de chaque outil et les situations dans lesquelles elles doivent être utilisées.
  2. ** Limitez les choix d'outils par nœud: ** Si possible, décomposez les workflows complexes en segments plus petits et plus gérables, chacun avec un ensemble d'outils plus ciblés. Cela peut aider à réduire la charge cognitive sur l'agent et à améliorer sa précision de sélection d'outils.

** négligeant Hitl pour les tâches sensibles **

* ** Problème: ** Le fait de ne pas utiliser la fonctionnalité "exiger l'approbation" du nœud d'agent (HITL) pour les tâches impliquant des informations sensibles, des décisions critiques ou des actions avec des conséquences potentielles du monde réel peut entraîner des résultats involontaires ou des dommages à la confiance des utilisateurs.
* ** Exemple: ** Votre agent de réservation de voyage a accès aux informations de paiement d'un utilisateur et peut réserver automatiquement les vols et les hôtels. Sans HITL, une mauvaise interprétation de l'intention de l'utilisateur ou une erreur dans la compréhension de l'agent pourrait entraîner une réservation incorrecte ou une utilisation non autorisée des détails de paiement de l'utilisateur.
* **Solution**
  1. ** Identifier les actions sensibles: ** Analyser votre flux de travail et identifier toutes les actions qui impliquent l'accès ou le traitement des données sensibles (par exemple, les informations de paiement, les informations personnelles).
  2. ** Implémentez "Require approbation": ** Pour ces actions sensibles, activez l'option "Exiger l'approbation" dans le nœud de l'agent. Cela garantit qu'un humain passe en revue l'action proposée par l'agent et le contexte pertinent avant que toute donnée sensible ne soit accessible ou que toute action irréversible soit prise.
  3. ** Concevoir des invites d'approbation claire: ** Fournir des invites claires et concises pour les examinateurs humains, résumant l'intention de l'agent, l'action proposée et les informations pertinentes nécessaires pour que le réviseur prenne une décision éclairée.

** Invite du système peu claire ou incomplète **

* ** Problème: ** L'invite système fournie au nœud d'agent n'a pas la spécificité et le contexte nécessaires pour guider efficacement l'agent dans l'exécution de ses tâches prévues. Une invite vague ou trop générale peut entraîner des réponses non pertinentes, des difficultés à comprendre l'intention des utilisateurs et une incapacité à tirer parti des outils ou des données de manière appropriée.
* ** Exemple: ** Vous construisez un agent de réservation de voyage, et votre invite système déclare simplement "_ Vous êtes un assistant d'IA utile." Il manque les instructions et le contexte spécifiques nécessaires pour que l'agent guide efficacement les utilisateurs à travers les recherches de vol, les réservations d'hôtels et la planification d'itinéraire.
* ** Solution: ** Élaborez une invite de système détaillée et contextuelle:

{% code overflow = "wrap"%}
```
You are a travel booking agent. Your primary goal is to assist users in planning and booking their trips. 
- Guide them through searching for flights, finding accommodations, and exploring destinations.
- Be polite, patient, and offer travel recommendations based on their preferences.
- Utilize available tools to access flight data, hotel availability, and destination information.
```
{% Endcode%}
{% endtab%}
{% endtabs%}

***

## 5. Node LLM

Comme le nœud d'agent, le nœud LLM est un composant Core ** de l'architecture d'agent séquentiel **. Les deux nœuds utilisent les mêmes modèles de chat (LLMS) par défaut, fournissant les mêmes capacités de traitement du langage de base, mais le nœud LLM se distingue dans ces domaines clés.

<gigne> <img src = "../../../. GitBook / Assets / SA -llm.png" alt = "" width = "341"> <Figcaption> </gigcaption> </ Figure>

### Avantages clés du nœud LLM

Tandis qu'une comparaison détaillée entre le nœud LLM et le nœud d'agent est disponible en[this section](./#agent-node-vs.-llm-node-selecting-the-optimal-node-for-conversational-tasks), voici un bref aperçu des principaux avantages du nœud ** **:

* ** Données structurées: ** Le nœud LLM fournit une fonctionnalité dédiée pour définir un schéma JSON pour sa sortie. Cela rend exceptionnellement facile d'extraire des informations structurées des réponses de la LLM et de transmettre ces données aux nœuds conséquents dans le flux de travail. Le nœud d'agent n'a pas cette fonction de schéma JSON intégré
* ** HITL: ** Bien que les deux nœuds prennent en charge Hitl pour l'exécution de l'outil, le nœud LLM délève ce contrôle au nœud d'outil lui-même, offrant plus de flexibilité dans la conception du flux de travail.

### Entrées

<Bile> <Thead> <tr> <th width = "184"> </ th> <th width = "111"> requis </th> <th> Description </th> </tr> </thead> <tbody> <tr> <td> Modèle de chat </strong> flux de travail. Compatible uniquement avec des modèles capables d'appels de fonction. </td> </tr> <tr> <td> Démarrer le nœud </td> <td> <strong> oui </strong> </td> <td> reçoit le <strong> Entrée utilisateur initial </strong>, ainsi que l'état personnalisé (IF Set Up) et le reste de l'état par défaut <code>. Node. </td> </tr> <tr> <Td> Le nœud d'agent </td> <td> <strong> oui </strong> </td> <td> reçoit la sortie d'un nœud d'agent, qui peut inclure les résultats de l'exécution d'outils ou les réponses générées par l'agent. </td> </tr> <tr> <TD> Condition des réponses. Node </td> <td> <strong> Oui </strong> </td> <td> reçoit les entrées d'un nœud de condition précédente, permettant au nœud LLM <strong> de prendre des actions ou de guider la conversation en fonction du résultat de l'évaluation de l'état de condition </strong>. </td> </tr> <Tr> <td> Condition d'agent de condition d'agent de condition </strong>. </TD> Node </td> <td> <strong> Oui </strong> </td> <td> reçoit les entrées d'un nœud d'agent de condition précédente, permettant au nœud LLM <strong> de prendre des actions ou de guider la conversation en fonction du résultat de l'évaluation de l'agent de condition </strong>. </td> </tr> <td> LLM de l'agent </strong>. Node </td> <td> <strong> oui </strong> </td> <td> reçoit la sortie d'un autre nœud LLM, <strong> activant le raisonnement chaîné </strong> ou le traitement de l'information sur plusieurs nœuds LLM. </td> </tr> <tr> <TD> Node d'outil </td> <td> <strong> oui </strong> <strong> Fournir les résultats de l'exécution des outils pour un traitement ultérieur </strong> ou la génération de réponse. </td> </tr> </tbody> </ table>

{% hint style = "info"%}
Le nœud ** llm nécessite au moins une connexion à partir des nœuds suivants **: nœud de démarrage, nœud d'agent, nœud de condition, nœud d'agent de condition, nœud LLM ou nœud d'outil.
{% EndHint%}

### ** Configuration du nœud **

<Bile> <Thead> <Tr> <th width = "240"> </ th> <th width = "118"> requis </th> <th> Description </th> </tr> </thead> <tbody> <tr> <td> llm Name </td> <td> <strong> oui </strong> </td> <td> Ajouter un nom de description à la llm NODI lisibilité et facilement <strong> ciblez-la lorsque vous utilisez des boucles </strong> dans le workflow. </td> </tr> </tbody> </ table>

### Sorties

Le nœud LLM peut se connecter aux nœuds suivants comme sorties:

* ** Node d'agent: ** passe la sortie de LLM vers un nœud d'agent, qui peut ensuite utiliser les informations pour décider des actions, exécuter des outils ou guider le flux de conversation.
* ** Node LLM: ** passe la sortie à un nœud LLM ultérieur, permettant le chaînage de plusieurs opérations LLM. Ceci est utile pour les tâches telles que le raffinage de la génération de texte, la réalisation de plusieurs analyses ou la rupture du traitement du langage complexe en étapes.
* ** Node d'outil **: passe la sortie vers un nœud d'outil, permettant l'exécution d'un outil spécifique basé sur les instructions du nœud LLM.
* ** Node d'agent de condition: ** Dirige le flux vers un nœud d'agent de condition. Ce nœud évalue la sortie du nœud LLM et ses conditions prédéfinies pour déterminer l'étape suivante appropriée du flux de travail.
* ** Node de condition: ** Similaire au nœud de l'agent de condition, le nœud de condition utilise des conditions prédéfinies pour évaluer la sortie du nœud LLM, dirigeant le flux le long de différentes branches en fonction du résultat.
* ** Node de fin: ** conclut le flux de conversation.
* ** Node de boucle: ** Redirige le flux vers un nœud précédent, permettant des processus itératifs ou cycliques dans le flux de travail. Cela pourrait être utilisé pour affiner la sortie du LLM sur plusieurs itérations.

### Paramètres supplémentaires

<Bile> <Thead> <tr> <th width = "200"> </ th> <th width = "141"> requis </th> <th> Description </th> </tr> </thead> <tbody> <tr> <td> Système invite </td> <td> non </td> <td> définit le <strong> agent "de" Personne "et les guides de son comportement </ Strong>. Par exemple, "<em> vous êtes un agent de service client spécialisé dans la prise en charge technique </em> [...]." </td> </tr> <tr> <td> Invite humain </td> <td> non </td> <td> Cette invite est ajoutée à la <code> state.Messages </code> Array comme message humain. Il nous permet à <strong> injecter un message de type humain dans le flux de conversation </strong> après que le nœud LLM a traité ses entrées et avant que le nœud suivant ne reçoive la sortie structurée du nœud LLM </td> <td> non </r> Schema </strong> (clé, type, valeurs d'énumération, description). </td> </tr> <tr> <td> State de mise à jour </td> <td> Non </td> <td> fournit un mécanisme <strong> pour modifier l'objet à état personnalisé partagé dans le flux de travail </strong>. Ceci est utile pour stocker des informations recueillies par le nœud LLM ou influencer le comportement des nœuds suivants. </td> </tr> </tbody> </s table>

### Meilleures pratiques

{% Tabs%}
{% Tab Title = "Pro Tips"%}
** Clear System Invite **

Crégurez une invite de système concise et sans ambiguïté qui reflète avec précision le rôle et les capacités du nœud LLM. Cela guide la prise de décision du nœud LLM et garantit qu'il agit dans sa portée définie.

** Optimiser pour la sortie structurée **

Gardez vos schémas JSON le plus simple possible, en nous concentrant sur les éléments de données essentiels. Activez uniquement la sortie structurée JSON lorsque vous devez extraire des points de données spécifiques de la réponse de LLM ou lorsque les nœuds en aval nécessitent une entrée JSON.

** Sélection d'outils stratégiques **

Choisissez et configurez les outils disponibles pour le nœud LLM (via le nœud de l'outil), en veillant à ce qu'ils s'alignent sur l'objectif de l'application et les objectifs globaux du flux de travail.

** Hitl pour les tâches sensibles **

Utilisez l'option «exiger l'approbation» pour les tâches impliquant des données sensibles, nécessitant un jugement humain ou comporte un risque de conséquences imprévues.

** Tirez parti des mises à jour d'état **

Mettez à jour l'objet d'état personnalisé stratégiquement pour stocker les informations recueillies ou influencer le comportement des nœuds en aval.
{% endtab%}

{% tab title = "Pièges potentiels"%}
** Exécution d'outils involontaire en raison d'une configuration HITL incorrecte **

* ** Problème: ** Bien que le nœud LLM puisse déclencher des nœuds d'outil, il s'appuie sur la configuration du nœud d'outil pour l'approbation de l'homme dans la boucle (HITL). Le fait de ne pas configurer correctement HITL pour des actions sensibles peut entraîner l'exécution d'outils sans examen humain, provoquant potentiellement des conséquences imprévues.
* ** Exemple: ** Votre nœud LLM est conçu pour interagir avec un outil qui apporte des modifications aux données de l'utilisateur. Vous avez l'intention d'avoir un examen humain de ces modifications avant l'exécution, mais l'option "Require Approbation" du nœud d'outil connecté n'est pas activée. Cela pourrait entraîner l'outil modifiant automatiquement les données utilisateur basées uniquement sur la sortie du LLM, sans aucune surveillance humaine.
* **Solution**
  1. ** Paramètres du nœud d'outil à double vérification: ** Assurez-vous toujours que l'option "Require approbation" est activée dans les paramètres de tout nœud d'outil qui gère les actions sensibles.
  2. ** Testez soigneusement HITL: ** Avant de déployer votre flux de travail, testez le processus HITL pour vous assurer que les étapes d'examen humain sont déclenchées comme prévu et que le mécanisme d'approbation / rejet fonctionne correctement.

** Surutilisation ou malentendu de la sortie structurée JSON **

* ** Problème: ** Bien que la fonction de sortie structurée JSON du nœud LLM soit puissante, il t'utilise ou ne comprend pas pleinement que ses implications peuvent entraîner des erreurs de données.
* ** Exemple: ** Vous définissez un schéma JSON complexe pour la sortie du nœud LLM, même si les tâches en aval ne nécessitent qu'une réponse texte simple. Cela ajoute une complexité inutile et rend votre flux de travail plus difficile à comprendre et à entretenir. De plus, si la sortie de la LLM ne se conforme pas au schéma défini, il peut provoquer des erreurs dans les nœuds suivants.
* **Solution**
  1. ** Utilisez la sortie JSON stratégiquement: ** Activez uniquement la sortie structurée JSON lorsque vous avez un besoin clair d'extraire des points de données spécifiques de la réponse de LLM ou lorsque les nœuds d'outil en aval nécessitent une entrée JSON.
  2. ** Gardez les schémas simples: ** Concevez vos schémas JSON comme simples et concis que possible, en nous concentrant uniquement sur les éléments de données qui sont absolument nécessaires à la tâche.
{% endtab%}
{% endtabs%}

***

## 6. Nœud d'outil

Le nœud d'outil est un composant précieux du système d'agent séquentiel de Flowise, ** permettant l'intégration et l'exécution d'outils externes ** dans les flux de travail conversationnels. Il agit comme un pont entre le traitement basé sur le langage des nœuds LLM et les fonctionnalités spécialisées des outils, API ou services externes.

<gigne> <img src = "../../../. GitBook / Assets / SEQ-07.png" alt = "" width = "300"> <Figcaption> </ Figcaption> </ Figure>

### Comprendre le nœud d'outil

La fonction principale du nœud d'outil consiste à ** exécuter des outils externes ** en fonction des instructions reçues d'un nœud LLM et de ** Fournir une flexibilité pour l'intervention de l'humanité dans la boucle (HITL) ** dans le processus d'exécution de l'outil.

#### Voici une explication étape par étape de la façon dont cela fonctionne

1. ** Réception des appels d'outil: ** Le nœud d'outil reçoit les entrées à partir d'un nœud LLM. Si la sortie de LLM contient le`tool_calls`Propriété, le nœud d'outil procédera à l'exécution de l'outil.
2. ** Exécution: ** Le nœud d'outil passe directement les LLM`tool_calls`(qui inclut le nom de l'outil et tous les paramètres requis) à l'outil externe spécifié. Sinon, le nœud d'outil n'exécute aucun outil dans cette exécution particulière de workflow. Il ne traite ni n'interpréte la sortie de LLM de quelque manière que ce soit.
3. ** Human-in-the-Loop (HITL): ** Le nœud d'outil permet HITL facultatif, permettant l'examen humain et l'approbation ou le rejet de l'exécution de l'outil avant qu'il ne se produise.
4. ** Passage de sortie: ** Après l'exécution de l'outil (soit automatique ou après l'approbation de HITL), le nœud d'outil reçoit la sortie de l'outil et le passe au nœud suivant dans le workflow. Si la sortie du nœud d'outil n'est pas connectée à un nœud ultérieur, la sortie de l'outil est renvoyée au nœud LLM d'origine pour un traitement ultérieur.

### Entrées

<Bile> <Thead> <Tr> <th width = "164"> </th> <th width = "107"> requis </th> <th> Description </th> </tr> </head> <tbody> <tr> <td> llm nœud </td> <td> <tstrong> oui </strong> </td> <td> <code> Propriété TOLL_CALLS </code>. S'il est présent, le nœud d'outil les utilisera pour exécuter l'outil spécifié. </td> </tr> <tr> <td> Outils externes </td> <td> Non </td> <td> fournit le nœud d'outil avec <strong> accès à une suite d'outils externaux </strong>, l'activant pour effectuer des actions et récupérer des informations. </td>

### Configuration du nœud

<Bile> <Thead> <tr> <th width = "183"> </th> <th width = "101"> requis </th> <th> Description </th> </tr> </head> <tbody> <tr> <td> Nom d'outil </td> <td> <strong> Oui </strong> </td> <td> lisibilité. </td> </tr> <tr> <td> nécessitent l'approbation (HITL) </td> <td> non </td> <td> <strong> active la fonction humaine dans la boucle (HITL) </strong>. Si défini sur '<strong> true </strong>,' le nœud d'outil demandera l'approbation humaine avant d'exécuter un outil. Ceci est particulièrement utile pour les opérations sensibles ou lorsque la surveillance humaine est souhaitée. Par défaut est '<strong> false </strong>,' permettant au nœud d'outil d'exécuter des outils de manière autonome. </td> </tr> </tbody> </s table>

### Sorties

Le nœud d'outil peut se connecter aux nœuds suivants comme sorties:

* ** Node d'agent: ** passe la sortie du nœud d'outil (le résultat de l'outil exécuté) à un nœud d'agent. Le nœud d'agent peut ensuite utiliser ces informations pour décider des actions, exécuter d'autres outils ou guider le flux de conversation.
* ** Node LLM: ** passe la sortie à un nœud LLM ultérieur. Cela permet l'intégration des résultats de l'outil dans le traitement de la LLM, permettant une analyse ou un raffinement plus approfondi du flux de conversation en fonction de la sortie de l'outil.
* ** Node d'agent de condition: ** Dirige le flux vers un nœud d'outil de condition. Ce nœud évalue la sortie du nœud d'outil et ses conditions prédéfinies pour déterminer l'étape suivante appropriée dans le flux de travail.
* ** Node de condition: ** Similaire au nœud d'agent de condition, le nœud de condition utilise des conditions prédéfinies pour évaluer la sortie du nœud d'outil, dirigeant le flux le long de différentes branches en fonction du résultat.
* ** Node de fin: ** conclut le flux de conversation.
* ** Node de boucle: ** Redirige le flux vers un nœud précédent, permettant des processus itératifs ou cycliques dans le flux de travail. Cela pourrait être utilisé pour les tâches qui nécessitent plusieurs exécutions d'outils ou impliquent de raffiner la conversation en fonction des résultats de l'outil.

### Paramètres supplémentaires

<Bile> <Thead> <Tr> <th width = "200"> </ th> <th width = "102"> requis </th> <th> Description </th> </tr> </thead> <tbody> <tr> <td> Approbation invite </td> <td> no </td> <td> <port> une invite personnalisable présentée à la revue humaine lorsque la trace de trait est active. Cette invite fournit un contexte sur l'exécution de l'outil, y compris le nom et le but de l'outil. La variable <code> {outils} </code> dans l'invite sera remplacée dynamiquement par la liste réelle des outils suggérés par le nœud LLM, garantissant que le réviseur humain dispose de toutes les informations nécessaires pour prendre une décision éclairée. </td> </tr> <tr> <td> Approver le texte du bouton </td> <td> non </td> Exécution </strong> dans l'interface HITL. Cela permet d'adapter le langage au contexte spécifique et d'assurer la clarté du réviseur humain. </td> </tr> <tr> <td> Rejeter le texte du bouton </td> <td> Non </td> <td> Personnalise le texte <strong> affiché sur le bouton pour rejeter l'exécution de l'outil </strong> dans l'interface hitl. Comme le texte du bouton Approuver, cette personnalisation améliore la clarté et fournit une action claire à prendre pour le réviseur humain s'il jugera l'exécution de l'outil inutile ou potentiellement nocif. </td> </tr> <tr> <td> État de mise à jour </td> <td> Non </td> <td> fournit un <strong> mécanisme pour modifier l'objet d'état personnalisé dans le cadre du travail de travail </ Strong>. Ceci est utile pour stocker les informations recueillies par le nœud d'outil (après l'exécution de l'outil) ou influencer le comportement des nœuds suivants. </td> </tr> </tbody> </s table>

### Meilleures pratiques

{% Tabs%}
{% Tab Title = "Pro Tips"%}
** Placement stratégique HITL **

Considérez quels outils nécessitent une surveillance humaine (HITL) et activer l'option "Require approbation" en conséquence.

** Invite d'approbation informative **

Lorsque vous utilisez HITL, concevez des invites claires et informatives pour les examinateurs humains. Fournir un contexte suffisant de la conversation et résumer l'action prévue de l'outil.
{% endtab%}

{% tab title = "Pièges potentiels"%}
** Formats de sortie de l'outil non géré **

* ** Problème: ** Le nœud d'outil sortira des données dans un format qui n'est pas attendu ou géré par les nœuds suivants dans le workflow, conduisant à des erreurs ou à un traitement incorrect.
* ** Exemple: ** Un nœud d'outil récupère les données à partir d'une API au format JSON, mais le nœud LLM suivant s'attend à une entrée de texte, provoquant une erreur d'analyse.
* ** Solution: ** Assurez-vous que le format de sortie de l'outil externe est compatible avec les exigences d'entrée des nœuds connectés à la sortie du nœud d'outil.
{% endtab%}
{% endtabs%}

***

## 7. Node de condition

Le nœud de condition agit comme un ** point de prise de décision dans les flux de travail d'agent séquentiel **, évaluant un ensemble de conditions prédéfinies pour déterminer le prochain chemin du flux.

<gigne> <img src = "../../../. GitBook / Assets / SEQ-08.png" alt = "" width = "299"> <Figcaption> </ Figcaption> </ Figure>

### Comprendre le nœud de condition

Le nœud de condition est essentiel pour construire des workflows qui s'adaptent à différentes situations et entrées utilisateur. Il examine l'état actuel de la conversation, qui comprend tous les messages échangés et toutes les variables d'état personnalisées précédemment définies. Ensuite, en fonction de l'évaluation des conditions spécifiées dans la configuration du nœud, le nœud de condition dirige le débit vers l'une de ses sorties.

Par exemple, une fois qu'un nœud agent ou LLM a fourni une réponse, un nœud de condition pourrait vérifier si la réponse contient un mot-clé spécifique ou si une certaine condition est remplie à l'état personnalisé. Si c'est le cas, le flux peut être dirigé vers un nœud d'agent pour une action supplémentaire. Sinon, cela pourrait conduire à un chemin différent, peut-être mettre fin à la conversation ou inviter l'utilisateur avec des questions supplémentaires.

Cela nous permet de ** créer des branches dans notre flux de travail **, où le chemin emprunté dépend des données qui circulent dans le système.

#### Voici une explication étape par étape de la façon dont cela fonctionne

1. Le nœud de condition reçoit l'entrée de tout nœud précédent: nœud de démarrage, nœud d'agent, nœud LLM ou nœud d'outil.
2. Il a accès à l'historique complet de la conversation et à l'état personnalisé (le cas échéant), ce qui lui donne beaucoup de contexte avec lequel travailler.
3. Nous définissons une condition que le nœud évaluera. Cela pourrait être de vérifier les mots clés, la comparaison des valeurs dans l'état ou toute autre logique que nous pourrions implémenter via JavaScript.
4. En fonction de la question de savoir si la condition évalue à ** true ** ou ** false **, le nœud de condition envoie le débit dans l'un de ses chemins de sortie prédéfinis. Cela crée une "fourche sur la route" ou une branche pour notre flux de travail.

### Comment mettre en place des conditions

Le nœud de condition nous permet de définir la logique de branchement dynamique dans notre flux de travail en choisissant soit une interface basée sur une table ** ** ou un éditeur de code JavaScript ** ** pour définir les conditions qui contrôleront le flux de conversation.

<gigne> <img src = "../../../. GitBook / Assets / SEQ-16 (1) .png" alt = ""> <figcaption> </figcaption> </ figure>

<Dettots>

<Summary> Conditions utilisant le code </summary>

Le nœud ** condition utilise JavaScript ** pour évaluer les conditions spécifiques dans le flux de conversation.

Nous pouvons configurer des conditions basées sur des mots clés, des changements d'état ou d'autres facteurs pour guider dynamiquement le flux de travail vers différentes branches en fonction du contexte de la conversation. Voici quelques exemples:

** Condition du mot-clé **

Cela vérifie si un mot ou une phrase spécifique existe dans l'histoire de la conversation.

* ** Exemple: ** Nous voulons vérifier si l'utilisateur a dit "oui" dans son dernier message.

{% code overflow = "wrap"%}
```javascript
const lastMessage = $flow.state.messages[$flow.state.messages.length - 1].content; 
return lastMessage.includes("yes") ? "Output 1" : "Output 2";
```
{% Endcode%}

1. Ce code obtient le dernier message de State.Messages et vérifie s'il contient "oui".
2. Si "oui" est trouvé, le flux va à "Output 1"; Sinon, il va à "Output 2".

** Condition de changement d'état **

Cela vérifie si une valeur spécifique à l'état personnalisé est passée à une valeur souhaitée.

* ** Exemple: ** Nous suivons une variable Orderstatus notre état personnalisé, et nous voulons vérifier s'il est devenu "confirmé".

{% code overflow = "wrap"%}
```javascript
return $flow.state.orderStatus === "confirmed" ? "Output 1" : "Output 2";
```
{% Endcode%}

1. Ce code compare directement la valeur Orderstatus dans notre état personnalisé à "confirmé".
2. S'il correspond, le flux va à "Output 1"; Sinon, il va à "Output 2".

</fords>

<Dettots>

<summary> Conditions utilisant le tableau </summary>

Le nœud de condition nous permet de définir des conditions à l'aide d'une ** Interface de table conviviale **, ce qui facilite la création de workflows dynamiques sans écrire de code JavaScript.

Vous pouvez configurer des conditions en fonction des mots clés, des changements d'état ou d'autres facteurs pour guider le flux de conversation le long de différentes branches. Voici quelques exemples:

** Condition du mot-clé **

Cela vérifie si un mot ou une phrase spécifique existe dans l'histoire de la conversation.

* ** Exemple: ** Nous voulons vérifier si l'utilisateur a dit "oui" dans son dernier message.
*   **Installation**

<Table Data-Header-Hidden> <Thead> <Tr> <Th width = "294"> </th> <th width = "116"> </th> <th width = "99"> </th> <th> </th> </tr> </thead> <tbody> <tr> <td> <strong> variable </strong> </rd> <td> <strong> opération </strong> </strong> Nom </strong> </td> </tr> <tr> <td> $ Flow.State.Messages [-1] .Content </td> <td> est </td> <td> oui </td> <td> Sortie 1 </td> </tr> </body> </plow>

    1. Cette entrée de table vérifie si le contenu (.content) du dernier message (\ [- 1])`state.messages`est égal à "oui".
    2. Si la condition est remplie, le débit va à "Output 1". Sinon, le workflow est dirigé vers une sortie "end" par défaut.

** Condition de changement d'état **

Cela vérifie si une valeur spécifique dans notre état personnalisé est passée à une valeur souhaitée.

* ** Exemple: ** Nous suivons une variable Orderstatus dans notre état personnalisé, et nous voulons vérifier s'il est devenu "confirmé".
*   **Installation**

<Table-Headder-Hidden> <Thead> <Tr> <th width = "266"> </th> <th width = "113"> </ th> <th> </th> <th> </th> </tr> </thead> <tbody> <tr> <td> <strong> variable </strong> </rd> <td> <strong> opération </strong> </strong> Nom </strong> </td> </ tr> <tr> <td> $ Flow.State.Orderstatus </td> <Td> est </td> <td> CONFORMÉ </TD> <TD> Sortie 1 </td> </tr> </tbody> </ Table>

    1. Cette entrée de table vérifie si la valeur d'Orderstatus à l'état personnalisé est égale à "confirmée".
    2. Si la condition est remplie, le débit va à "Output 1". Sinon, le workflow est dirigé vers une sortie "end" par défaut.

</fords>

### Définition des conditions à l'aide de l'interface de table

Cette approche visuelle vous permet de configurer facilement des règles qui déterminent le chemin de votre flux de conversation, en fonction de facteurs tels que l'entrée de l'utilisateur, de l'état actuel de la conversation ou des résultats des actions prises par d'autres nœuds.

<Dettots>

<summary> Tableau basé sur la table: Node de condition </summary>

*   ** Mise à jour le 09/08/2024 **

<Bile> <Thead> <tr> <th width = "134"> </ th> <th width = "189"> Description </th> <th> Options / Syntax </th> </tr> </head> <tbody> <tr> <td> <strong> variable </strong> </td> <td> - le variable ou l'élément de données à évaluer dans la condition.$flow.state.messages.length</code> (Total Messages)<br>- <code>$Flow.State.Messages [0] .Con </code> (First Message Content) <br> - <code>$flow.state.messages[-1].con</code> (Last Message Content)<br>- <code>$Vars. <variable-nom> </code> (variable globale) </td> </tr> <tr> <td> <strong> Opération </strong> </td> <td> - La comparaison ou le fonctionnement logique pour effectuer sur la variable. </td> <td> - Contient <br> - ne contient pas <br> - est <br> - ENTRÉE <br> - Is <br> - est <br> - est <br> - ENTROY Pas vide <br> - supérieur à <br> - inférieur à <br> - égal à <br> - pas égal à <br> - supérieur ou égal à <br> - inférieur ou égal à </td> </tr> <tr> <td> <strong> </strong> </td> <td> la valeur pour comparer la variable. Examples: "yes", 10, "Hello"</td></tr><tr><td><strong>Output Name</strong></td><td>The name of the output path to follow if the condition evaluates to <code>true</code>.</td><td>- User-defined name (e.g., "Agent1", "End", "Loop") </td> </tr> </tbody> </ table>

</fords>

### Entrées

<Bile> <Thead> <Tr> <th width = "167"> </ th> <th width = "118"> requis </th> <th> Description </th> </tr> </thead> <tbody> <tr> <td> Démarrer le nœud </td> <td> <strong> oui </strong> </td> <td> reçoit l'état de l'état du nœud de début. Cela permet au nœud de condition d'évaluer les conditions en fonction du contexte initial de la conversation </strong>, y compris tout état personnalisé. </td> </tr> <tr> <td> Node d'agent </td> <td> <strong> oui </strong> </td> <td> reçoit la sortie du nœud de l'agent. Cela permet au nœud de condition de prendre <strong> prendre des décisions en fonction des actions de l'agent </strong> et de l'historique de conversation, y compris tout état personnalisé. </td> </tr> <tr> <td> llm nœud </td> <td> <strong> Oui </strong> </td> <td> reçoit la sortie du nœud LLM. Cela permet au nœud de condition d'évaluer les conditions en fonction de la réponse de la LLM </strong> et de l'historique de conversation, y compris tout état personnalisé. </td> </tr> <tr> <td> Le nœud d'outil </td> <td> <strong> oui </strong> </td> <td> reçoit la sortie du nœud de l'outil. Cela permet au nœud de condition <strong> prendre des décisions en fonction des résultats de l'exécution de l'outil </strong> et de l'historique de conversation, y compris tout état personnalisé. </td> </tr> </tbody> </s table>

{% hint style = "info"%}
Le nœud de condition ** nécessite au moins une connexion à partir des nœuds suivants **: nœud de démarrage, nœud d'agent, nœud LLM ou nœud d'outil.
{% EndHint%}

### Sorties

Le nœud de condition ** détermine dynamiquement son chemin de sortie en fonction des conditions prédéfinies **, en utilisant soit l'interface basée sur la table ou JavaScript. Cela offre une flexibilité dans la direction du flux de travail en fonction des évaluations des conditions.

#### Logique d'évaluation des conditions

* ** Conditions basées sur la table: ** Les conditions du tableau sont évaluées séquentiellement, de haut en bas. La première condition qui évalue à True déclenche sa sortie correspondante. Si aucune des conditions prédéfinies n'est remplie, le workflow est dirigé vers la sortie "end" par défaut.
* ** Conditions basées sur le code: ** Lorsque vous utilisez JavaScript, nous devons explicitement renvoyer le nom du chemin de sortie souhaité, y compris un nom pour la sortie "end" par défaut.
* ** Chemin de sortie unique: ** Un seul chemin de sortie est activé à la fois. Même si plusieurs conditions pouvaient être vraies, seule la première condition de correspondance détermine l'écoulement.

#### Connexion des sorties

Chaque sortie prédéfinie, y compris la sortie "end" par défaut, peut être connectée à l'un des nœuds suivants:

* ** Node d'agent: ** Pour continuer la conversation avec un agent, en prenant potentiellement des mesures en fonction du résultat de la condition.
* ** Node LLM: ** Pour traiter l'historique actuel de l'état et de la conversation avec un LLM, générant des réponses ou prenant d'autres décisions.
* ** Node de fin: ** Pour terminer le flux de conversation. Si une sortie, y compris la sortie "end" par défaut, est connectée à un nœud de fin, le nœud de condition sortira la dernière réponse du nœud précédent et terminera le flux de travail.
* ** Node de boucle: ** Pour rediriger le flux vers un nœud séquentiel précédent, permettant des processus itératifs en fonction du résultat de la condition.

### Configuration du nœud

<Bile> <Thead> <Tr> <th width = "178"> </ th> <th width = "110"> requis </th> <th> Description </th> </tr> </thead> <tbody> <tr> <td> Nom du nœud de condition </td> <td> non </td> <td> un nom facultatif, <strong> le nom de réel humain </ Strong> pour la condition. Ceci est utile pour comprendre le flux de travail en un coup d'œil. </td> </tr> <tr> <td> condition </td> <td> <strong> Oui </strong> </td> <td> C'est là que nous <strong> définir la logique qui sera évaluée pour déterminer les sorties de sortie </strong>. </td> </tr> </tody>

### Meilleures pratiques

{% Tabs%}
{% Tab Title = "Pro Tips"%}
** Condition claire nommer **

Utilisez des noms descriptifs pour vos conditions (par exemple, "Si l'utilisateur est inférieur à 18 ans, alors agent de conseiller politique", "Si la commande est confirmée, alors terminez le nœud") pour rendre votre flux de travail plus facile à comprendre et à déboguer.

** Prioriser les conditions simples **

Commencez par des conditions simples et ajoutez progressivement la complexité au besoin. Cela rend votre flux de travail plus gérable et réduit le risque d'erreurs.
{% endtab%}

{% tab title = "Pièges potentiels"%}
** Conception de la logique et du workflow Conception ** **

* ** Problème: ** Les conditions que vous définissez dans le nœud de condition ne reflètent pas avec précision la logique prévue de votre flux de travail, conduisant à une ramification inattendue ou à des chemins d'exécution incorrects.
* ** Exemple: ** Vous configurez une condition pour vérifier si l'âge de l'utilisateur est supérieur à 18 ans, mais le chemin de sortie de cette condition conduit à une section conçue pour les utilisateurs de moins de 18 ans.
* ** Solution: ** Passez en revue vos conditions et assurez-vous que les chemins de sortie associés à chaque condition correspondent à la logique du flux de travail prévu. Utilisez des noms clairs et descriptifs pour vos sorties pour éviter la confusion.

** Gestion insuffisante de l'État **

* ** Problème: ** Le nœud de condition repose sur une variable d'état personnalisée qui n'est pas mise à jour correctement, conduisant à des évaluations de conditions inexactes et à une branche incorrecte.
* ** Exemple: ** Vous suivez une variable "UserLocation" dans l'état personnalisé, mais la variable n'est pas mise à jour lorsque l'utilisateur fournit son emplacement. Le nœud de condition évalue la condition en fonction de la valeur obsolète, conduisant à un chemin incorrect.
* ** Solution: ** Assurez-vous que toutes les variables d'état personnalisées utilisées dans vos conditions sont mises à jour correctement tout au long du workflow.
{% endtab%}
{% endtabs%}

***

## 8. Nœud d'agent de condition

Le nœud d'agent de condition fournit ** le routage dynamique et intelligent dans les flux d'agent séquentiels **. Il combine les capacités du nœud ** llm ** (sortie structurée LLM et JSON) et le nœud ** de condition ** (conditions définies par l'utilisateur), ce qui nous permet de tirer parti du raisonnement basé sur l'agent et de la logique conditionnelle dans un seul nœud.

<gigne> <img src = "../../../. GitBook / Assets / SEQ-09.png" alt = "" width = "299"> <Figcaption> </ Figcaption> </ Figure>

### Fonctionnalités clés

* ** Routage basé sur des agents unifiés: ** combine le raisonnement d'agent, la sortie structurée et la logique conditionnelle dans un seul nœud, simplifiant la conception du flux de travail.
* ** Conscience contextuelle: ** L'agent examine tout l'historique de conversation et tout état personnalisé lors de l'évaluation des conditions.
* ** Flexibilité: ** fournit à la fois des options basées sur la table et basées sur le code pour définir des conditions, lors de la réalisation de différentes préférences et niveaux de compétences des utilisateurs.

### Configuration du nœud d'agent de condition

Le nœud d'agent de condition agit comme un agent spécialisé qui peut à la fois ** traiter les informations et prendre des décisions de routage **. Voici comment le configurer:

1. ** Définissez le personnage de l'agent **
   * Dans le champ "Invite System", fournissez une description claire et concise du rôle de l'agent et de la tâche dont il a besoin pour effectuer pour le routage conditionnel. Cette invite guidera la compréhension de l'agent de la conversation et de son processus décisionnel.
2. ** Structure la sortie de l'agent (facultatif) **
   * Si vous souhaitez que l'agent produise une sortie structurée, utilisez la fonctionnalité "Sortie structurée JSON". Définissez le schéma souhaité pour la sortie, en spécifiant les clés, les types de données et toutes les valeurs d'énumération. Cette sortie structurée sera utilisée par l'agent lors de l'évaluation des conditions.
3. ** Définir les conditions **
   * Choisissez l'interface basée sur la table ou l'éditeur de code JavaScript pour définir les conditions qui détermineront le comportement de routage.
     * ** Interface basée sur la table: ** Ajouter des lignes au tableau, en spécifiant la variable à vérifier, l'opération de comparaison, la valeur à comparer et le nom de sortie à suivre si la condition est remplie.
     * ** Code JavaScript: ** Écrivez des extraits JavaScript personnalisés pour évaluer les conditions. Utiliser le`return`Instruction pour spécifier le nom du chemin de sortie à suivre en fonction du résultat de la condition.
4. ** Connectez les sorties **
   * Connectez chaque sortie prédéfinie, y compris la sortie "end" par défaut, au nœud ultérieur approprié dans le flux de travail. Il peut s'agir d'un nœud d'agent, d'un nœud LLM, d'un nœud de boucle ou d'un nœud final.

### Comment mettre en place des conditions

Le nœud d'agent de condition nous permet de définir la logique de branchement dynamique dans notre flux de travail en choisissant soit une interface basée sur la table ** ** ou un éditeur de code JavaScript ** ** pour définir les conditions qui contrôleront le flux de conversation.

<gigne> <img src = "../../../. GitBook / Assets / SEQ-16 (1) .png" alt = ""> <figcaption> </figcaption> </ figure>

<Dettots>

<Summary> Conditions utilisant le code </summary>

Le nœud d'agent de condition, comme le nœud de condition **, utilise le code JavaScript pour évaluer les conditions spécifiques ** dans le flux de conversation.

Cependant, le nœud d'agent de condition peut évaluer les conditions en fonction d'une gamme plus large de facteurs, notamment des mots clés, des changements d'état et le contenu de sa propre sortie (soit en tant que texte de forme libre, soit des données JSON structurées). Cela permet des décisions de routage plus nuancées et consacrées au contexte. Voici quelques exemples:

** Condition du mot-clé **

Cela vérifie si un mot ou une phrase spécifique existe dans l'histoire de la conversation.

* ** Exemple: ** Nous voulons vérifier si l'utilisateur a dit "oui" dans son dernier message.

{% code overflow = "wrap"%}
```javascript
const lastMessage = $flow.state.messages[$flow.state.messages.length - 1].content; 
return lastMessage.includes("yes") ? "Output 1" : "Output 2";
```
{% Endcode%}

1. Ce code obtient le dernier message de State.Messages et vérifie s'il contient "oui".
2. Si "oui" est trouvé, le flux va à "Output 1"; Sinon, il va à "Output 2".

** Condition de changement d'état **

Cela vérifie si une valeur spécifique à l'état personnalisé est passée à une valeur souhaitée.

* ** Exemple: ** Nous suivons une variable Orderstatus notre état personnalisé, et nous voulons vérifier s'il est devenu "confirmé".

{% code overflow = "wrap"%}
```javascript
return $flow.state.orderStatus === "confirmed" ? "Output 1" : "Output 2";
```
{% Endcode%}

1. Ce code compare directement la valeur Orderstatus dans notre état personnalisé à "confirmé".
2. S'il correspond, le flux va à "Output 1"; Sinon, il va à "Output 2".

</fords>

<Dettots>

<summary> Conditions utilisant le tableau </summary>

Le nœud d'agent de condition fournit également une interface de table ** conviviale pour la définition des conditions **, similaire au nœud de condition. Vous pouvez configurer des conditions en fonction des mots clés, des modifications d'état ou de la propre sortie de l'agent, vous permettant de créer des workflows dynamiques sans écrire de code JavaScript.

Cette approche basée sur une table simplifie la gestion des conditions et facilite la visualisation de la logique de ramification. Voici quelques exemples:

** Condition du mot-clé **

Cela vérifie si un mot ou une phrase spécifique existe dans l'histoire de la conversation.

* ** Exemple: ** Nous voulons vérifier si l'utilisateur a dit "oui" dans son dernier message.
*   **Installation**

<Table Data-Header-Hidden> <Thead> <Tr> <Th width = "305"> </th> <th width = "116"> </th> <th width = "99"> </th> <th> </th> </tr> </thead> <tbody> <tr> <td> <strong> variable </strong> </rd> <td> <strong> opération </strong> </strong> Nom </strong> </td> </tr> <tr> <td> $ Flow.State.Messages [-1] .Content </td> <td> est </td> <td> oui </td> <td> Sortie 1 </td> </tr> </body> </plow>

    1. Cette entrée de table vérifie si le contenu (.content) du dernier message (\ [- 1])`state.messages`est égal à "oui".
    2. Si la condition est remplie, le débit va à "Output 1". Sinon, le workflow est dirigé vers une sortie "end" par défaut.

** Condition de changement d'état **

Cela vérifie si une valeur spécifique dans notre état personnalisé est passée à une valeur souhaitée.

* ** Exemple: ** Nous suivons une variable Orderstatus dans notre état personnalisé, et nous voulons vérifier s'il est devenu "confirmé".
*   **Installation**

<Table-Headder-Hidden> <Thead> <Tr> <th width = "266"> </th> <th width = "113"> </ th> <th> </th> <th> </th> </tr> </thead> <tbody> <tr> <td> <strong> variable </strong> </rd> <td> <strong> opération </strong> </strong> Nom </strong> </td> </ tr> <tr> <td> $ Flow.State.Orderstatus </td> <Td> est </td> <td> CONFORMÉ </TD> <TD> Sortie 1 </td> </tr> </tbody> </ Table>

    1. Cette entrée de table vérifie si la valeur d'Orderstatus à l'état personnalisé est égale à "confirmée".
    2. Si la condition est remplie, le débit va à "Output 1". Sinon, le workflow est dirigé vers une sortie "end" par défaut.

</fords>

### Définition des conditions à l'aide de l'interface de table

Cette approche visuelle vous permet de configurer facilement des règles qui déterminent le chemin de votre flux de conversation, en fonction de facteurs tels que l'entrée de l'utilisateur, de l'état actuel de la conversation ou des résultats des actions prises par d'autres nœuds.

<Dettots>

<Summary> Tableau basé sur la table: Condition d'agent Node </summary>

*   ** Mise à jour le 09/08/2024 **

<Bile> <Thead> <Tr> <th width = "125"> </th> <th width = "186"> Description </th> <th> Options / Syntax </th> </tr> </head> <tbody> <tr> <td> <strong> variable </strong> </td> <td> l'élément variable ou de données à évaluer dans la condition. Cela peut inclure des données de la sortie de l'agent. </td> <td> - <code>$flow.output.content</code> (Agent Output - string)<br>- <code>$Flow.Output. <remplacer-with-key> </code> (sortie de la touche JSON de l'agent - String / Number) <br> - <code>$flow.state.messages.length</code> (Total Messages)<br>- <code>$Flow.State.Messages [0] .Con </code> (First Message Content) <br> - <code>$flow.state.messages[-1].con</code> (Last Message Content)<br>- <code>$Vars. <variable-nom> </code> (variable globale) </td> </tr> <tr> <td> <strong> Opération </strong> </td> <td> - La comparaison ou le fonctionnement logique pour effectuer sur la variable. </td> <td> - Contient <br> - ne contient pas <br> - est <br> - ENTRÉE <br> - Is <br> - est <br> - est <br> - ENTROY Pas vide <br> - supérieur à <br> - inférieur à <br> - égal à <br> - pas égal à <br> - supérieur ou égal à <br> - inférieur ou égal à </td> </tr> <tr> <td> <strong> </strong> </td> <td> la valeur pour comparer la variable. Examples: "yes", 10, "Hello"</td></tr><tr><td><strong>Output Name</strong></td><td>The name of the output path to follow if the condition evaluates to <code>true</code>.</td><td>- User-defined name (e.g., "Agent1", "End", "Loop") </td> </tr> </tbody> </ table>

</fords>

### Entrées

<Bile> <Thead> <Tr> <th width = "167"> </ th> <th width = "118"> requis </th> <th> Description </th> </tr> </thead> <tbody> <tr> <td> Démarrer le nœud </td> <td> oui </td> <td> reçoit l'état du nœud de départ. Cela permet au nœud de l'agent de condition <strong> évaluer les conditions en fonction du contexte initial </strong> de la conversation, y compris tout état personnalisé. </td> </tr> <tr> <td> Node d'agent </td> <td> oui </td> <td> reçoit la sortie du nœud de l'agent. Cela permet au nœud de l'agent de condition de prendre des décisions en fonction des actions de l'agent </strong> et de l'historique de conversation, y compris tout état personnalisé. </td> </tr> <tr> <td> LLM Node </td> <TD> Oui </TD> <TD> Reçoit la sortie du nœud LLM. Cela permet au nœud d'agent de condition <strong> évaluer les conditions en fonction de la réponse de la LLM </strong> et de l'historique de conversation, y compris tout état personnalisé. </td> </tr> <tr> <td> Le nœud d'outil </td> <td> oui </td> <td> reçoit la sortie du nœud du nœud de l'outil. Cela permet au nœud de l'agent de condition de prendre des décisions en fonction des résultats de l'exécution de l'outil </strong> et de l'historique de conversation, y compris tout état personnalisé. </td> </tr> </tbody> </s table>

{% hint style = "info"%}
Le nœud d'agent de condition ** nécessite au moins une connexion à partir des nœuds suivants **: nœud de démarrage, nœud d'agent, nœud LLM ou nœud d'outil.
{% EndHint%}

### Configuration du nœud

<Bile> <Thead> <Tr> <th width = "178"> Paramètre </th> <th width = "110"> requis </th> <th> Description </th> </tr> </ead> <tbody> <tr> <Td> Name </td> <td> NO </td> <td> Ajouter un nom descriptif à la condition NODE NODNE TO IMPORT facilement. </td> </tr> <tr> <td> Condition </td> <td> <strong> Oui </strong> </td> <td> C'est là que nous <strong> définissons la logique qui sera évaluée pour déterminer les chemins de sortie </strong>. </td> </tr> </tbody> </pally>

### Sorties

Le nœud d'agent de condition, comme le nœud de condition **, détermine dynamiquement son chemin de sortie en fonction des conditions définies **, en utilisant soit l'interface basée sur la table ou JavaScript. Cela offre une flexibilité dans la direction du flux de travail en fonction des évaluations des conditions.

#### Logique d'évaluation des conditions

* ** Conditions basées sur la table: ** Les conditions du tableau sont évaluées séquentiellement, de haut en bas. La première condition qui évalue à True déclenche sa sortie correspondante. Si aucune des conditions prédéfinies n'est remplie, le workflow est dirigé vers la sortie "end" par défaut.
* ** Conditions basées sur le code: ** Lorsque vous utilisez JavaScript, nous devons explicitement renvoyer le nom du chemin de sortie souhaité, y compris un nom pour la sortie "end" par défaut.
* ** Chemin de sortie unique: ** Un seul chemin de sortie est activé à la fois. Même si plusieurs conditions pouvaient être vraies, seule la première condition de correspondance détermine l'écoulement.

#### Connexion des sorties

Chaque sortie prédéfinie, y compris la sortie "end" par défaut, peut être connectée à l'un des nœuds suivants:

* ** Node d'agent: ** Pour continuer la conversation avec un agent, en prenant potentiellement des mesures en fonction du résultat de la condition.
* ** Node LLM: ** Pour traiter l'historique actuel de l'état et de la conversation avec un LLM, générant des réponses ou prenant d'autres décisions.
* ** Node de fin: ** Pour terminer le flux de conversation. Si la sortie "end" par défaut est connectée à un nœud de fin, le nœud de condition sortira la dernière réponse du nœud précédent et terminera la conversation.
* ** Node de boucle: ** Pour rediriger le flux vers un nœud séquentiel précédent, permettant des processus itératifs en fonction du résultat de la condition.

#### Différences clés par rapport au nœud de condition

* Le nœud d'agent de condition ** intègre le raisonnement d'un agent ** et la sortie structurée dans le processus d'évaluation de la condition.
* Il fournit une approche plus intégrée du routage des conditions basées sur les agents.

### Paramètres supplémentaires

<Bile> <Thead> <Tr> <th width = "180"> </th> <th width = "111"> requis </th> <th> Description </th> </tr> </thead> <tbody> <tr> <td> Système invite </td> <td> pas </td> <td> <trofor> Définit la personne d'agent de condition. Exemple: "Vous êtes un agent de service client spécialisé dans le support technique. <code> State.Messages </code> Array en tant que message humain. Il nous permet à <strong> injecter un message de type humain dans le flux de conversation </strong> après que le nœud de l'agent de condition ait traité ses entrées et avant que le nœud suivant ne reçoive la sortie structurée de l'agent </td> <td> non </r> <TD> <TD> pour instruire le nœud d'agent de condition <Strong>. Type, valeurs d'énumération, description). </td> </tr> </tbody> </s table>

### Meilleures pratiques

{% Tabs%}
{% Tab Title = "Pro Tips"%}
** Créez une invite de système claire et ciblée **

Fournissez une personnalité bien définie et des instructions claires à l'agent dans l'invite du système. Cela guidera son raisonnement et l'aidera à générer une sortie pertinente pour la logique conditionnelle.

** Structure Sortie pour des conditions fiables **

Utilisez la fonction de sortie structurée JSON pour définir un schéma pour la sortie de l'agent de condition. Cela garantira que la sortie est cohérente et facilement sanitaire, ce qui le rend plus fiable pour une utilisation dans les évaluations conditionnelles.
{% endtab%}

{% tab title = "Pièges potentiels"%}
** Route peu fiable en raison de la sortie non structurée **

* ** Problème: ** Le nœud d'agent de condition n'est pas configuré pour sortir des données JSON structurées, conduisant à des formats de sortie imprévisibles qui peuvent rendre difficile la définition de conditions fiables.
* ** Exemple: ** Le nœud de l'agent de condition est invité à déterminer le sentiment de l'utilisateur (positif, négatif, neutre) mais publie son évaluation en tant que chaîne de texte de forme libre. La variabilité de la langue de l'agent rend difficile de créer des conditions précises dans le tableau ou le code conditionnel.
* ** Solution: ** Utilisez la fonction de sortie structurée JSON pour définir un schéma pour la sortie de l'agent. Par exemple, spécifiez une clé "Sentiment" avec une énumération de "positif", "négatif" et "neutre". Cela garantira que la sortie de l'agent est systématiquement structurée, ce qui facilite la création de conditions fiables.
{% endtab%}
{% endtabs%}

***

## 9. Nœud de boucle

Le nœud de boucle nous permet de créer des boucles dans notre flux conversationnel, ** redirigeant la conversation vers un point spécifique **. Ceci est utile pour les scénarios où nous devons répéter une certaine séquence d'actions ou de questions basées sur l'entrée utilisateur ou les conditions spécifiques.

<gigne> <img src = "../../../. GitBook / Assets / SA-LOOP.png" alt = "" width = "335"> <Figcaption> </ Figcaption> </ Figure>

### Comprendre le nœud de boucle

Le nœud de boucle agit comme un connecteur, redirigeant le flux vers un point spécifique dans le graphique, nous permettant de créer des boucles dans notre flux conversationnel. ** Il passe l'état actuel, qui comprend la sortie du nœud précédant le nœud de boucle à notre nœud cible. ** Ce transfert de données permet à notre nœud cible de traiter les informations à partir de l'itération précédente de la boucle et ajuster son comportement en conséquence.

Par exemple, disons que nous construisons un chatbot qui aide les utilisateurs à réserver des vols. Nous pourrions utiliser une boucle pour affiner itérativement les critères de recherche en fonction des commentaires des utilisateurs.

#### Voici comment le nœud de boucle pourrait être utilisé

1. ** Node LLM (recherche initiale): ** Le nœud LLM reçoit la demande de vol initiale de l'utilisateur (par exemple, "Trouvez des vols de Madrid à New York en juillet"). Il interroge une API de recherche de vols et renvoie une liste de vols possibles.
2. ** Node d'agent (options actuelles): ** Le nœud d'agent présente les options de vol à l'utilisateur et demande s'ils souhaitent affiner leur recherche (par exemple, "Souhaitez-vous filtrer par prix, compagnie aérienne ou heure de départ?").
3. ** Node d'agent de condition: ** Le nœud d'agent de condition vérifie la réponse de l'utilisateur et a deux sorties:
   * ** Si l'utilisateur souhaite affiner: ** Le flux va au nœud LLM "Recherche affiner".
   * ** Si l'utilisateur est satisfait des résultats: ** Le flux passe au processus de réservation.
4. ** Node LLM (Recherche de recherche): ** Ce nœud LLM rassemble les critères de raffinement de l'utilisateur (par exemple, "Montrez-moi uniquement des vols en moins de 500 $") et met à jour l'état avec les nouveaux paramètres de recherche.
5. ** Node de boucle: ** Le nœud de boucle redirige le flux vers le nœud LLM initial ("Recherche initiale"). Il transmet l'état mis à jour, qui comprend désormais les critères de recherche raffinés.
6. ** itération: ** Le nœud LLM initial effectue une nouvelle recherche en utilisant les critères raffinés, et le processus se répète à partir de l'étape 2.

** Dans cet exemple, le nœud de boucle permet un processus de raffinement de recherche itératif. ** Le système peut continuer à faire boucler et affiner les résultats de recherche jusqu'à ce que l'utilisateur soit satisfait des options présentées.

### Entrées

<Bile> <Thead> <Tr> <th width = "197"> </ th> <th width = "104"> requis </th> <th> Description </th> </tr> </thead> <tbody> <tr> <td> nœud d'agent </td> <td> <strong> oui </strong> </td> <td> reçoit la sortie d'un agent précède Node. Ces données sont ensuite renvoyées au nœud cible spécifié dans le paramètre "LOOP TO". </td> </tr> <tr> <td> LLM Node </td> <td> <strong> Oui </strong> </td> <td> reçoit la sortie d'un nœud LLM précédent. Ces données sont ensuite renvoyées au nœud cible spécifié dans le paramètre "Loop to". </td> </tr> <tr> <Td> Node d'outil </td> <td> <strong> Oui </strong> </td> <td> reçoit la sortie d'un nœud d'outil de précédent. Ces données sont ensuite renvoyées au nœud cible spécifié dans le paramètre "boucle à" </td> </tr> <tr> <td> Node de condition </td> <td> <strong> Oui </strong> </td> <td> reçoit la sortie d'un nœud de condition de condition de précédent. Ces données sont ensuite renvoyées au nœud cible spécifié dans le paramètre "Loop to". </td> </tr> <tr> <td> Le nœud de l'agent de condition </td> <td> <strong> oui </strong> </td> <td> reçoit la sortie d'un nœud d'agent de condition précédant. Ces données sont ensuite renvoyées au nœud cible spécifié dans le paramètre "Loop to". </td> </tr> </tbody> </ table>

{% hint style = "info"%}
Le nœud de boucle ** nécessite au moins une connexion à partir des nœuds suivants **: nœud d'agent, nœud LLM, nœud d'outil, nœud de condition ou nœud d'agent de condition.
{% EndHint%}

### Configuration du nœud

<Bile> <Thead> <tr> <th width = "125"> </ th> <th width = "109"> requis </th> <th> Description </th> </tr> </thead> <tbody> <tr> <td> ("Loop to") où le flux conversationnel doit être redirigé. Ce nœud cible doit être un nœud d'agent <strong> </strong> ou <strong> llm nœud </strong>. </td> </tr> </tbody> </ table>

### Sorties

Le nœud de boucle ** n'a pas de connexions de sortie directe **. Il redirige le flux vers le nœud séquentiel spécifique dans le graphique.

### Meilleures pratiques

{% Tabs%}
{% Tab Title = "Pro Tips"%}
** Objectif de boucle claire **

Définissez un objectif clair pour chaque boucle de votre flux de travail. Si possible, documentez avec une note collante ce que vous essayez de réaliser avec la boucle.
{% endtab%}

{% tab title = "Pièges de potential"%}
** Structure de flux de travail confuse **

* ** Problème: ** Des boucles excessives ou mal conçues rendent le flux de travail difficile à comprendre et à entretenir.
* ** Exemple: ** Vous utilisez plusieurs boucles imbriquées sans but ou étiquettes claires, ce qui rend difficile de suivre le flux de la conversation.
* ** Solution: ** Utilisez des boucles avec parcimonie et uniquement lorsque cela est nécessaire. Documentez clairement vos nœuds de boucle et les nœuds auxquels ils se connectent.

** Boucles infinies en raison de conditions de sortie manquantes ou incorrectes **

* ** Problème: ** La boucle ne se termine jamais car la condition qui devrait déclencher la sortie de la boucle est manquante ou défini à tort.
* ** Exemple: ** Un nœud de boucle est utilisé pour collecter itérativement les informations utilisateur. Cependant, le flux de travail n'a pas de nœud d'agent conditionnel pour vérifier si toutes les informations requises ont été collectées. En conséquence, la boucle se poursuit indéfiniment, demandant à plusieurs reprises à l'utilisateur les mêmes informations.
* ** Solution: ** Définissez toujours des conditions de sortie claires et précises pour les boucles. Utilisez des nœuds de condition pour vérifier les variables d'état, l'entrée de l'utilisateur ou d'autres facteurs qui indiquent quand la boucle doit se terminer.
{% endtab%}
{% endtabs%}

***

## 10. Node de fin

Le nœud de fin marque le point de terminaison définitif ** de la conversation ** dans un flux de travail d'agent séquentiel. Il signifie qu'aucun traitement, actions ou interactions supplémentaires n'est requis.

<gigne> <img src = "../../../. GitBook / Assets / Seq-End-node.png" alt = "" width = "375"> <Figcaption> </ Figcaption> </gistre>

### Comprendre le nœud final

Le nœud final sert de signal dans l'architecture d'agent séquentiel de Flowise, ** indiquant que la conversation a atteint sa conclusion prévue **. En atteignant le nœud final, le système "comprend" que l'objectif conversationnel a été atteint et qu'aucune autre actions ou interactions n'est requise dans le flux.

### Entrées

<Bile> <Thead> <Tr> <th width = "212"> </th> <th width = "103"> requis </th> <th> Description </th> </tr> </thead> <tbody> <tr> <td> nœud </td> <td> <strong> Oui </strong> </td> <td> L'agent reçoit la sortie finale de l'agent précède de l'agent de précons Traitement. </td> </tr> <tr> <td> LLM Node </td> <td> <strong> Oui </strong> </td> <td> reçoit la sortie finale d'un nœud LLM précédent, indiquant l'extrémité du traitement du nœud LLM. </td> </tr> <td> Tool Le nœud </td> <td> <strong> Oui </strong> </td> <td> reçoit la sortie finale d'un nœud d'outil précédent, indiquant l'achèvement de l'exécution du nœud d'outil. </td> </tr> <tr> <td> Condition Node </td> <td> <tstrong> Oui </strong> </td> <td> Réception de la sortie finale de la condition de Node, oui, la condition de Node, vous récepte la condition de Node, oui, la condition de Node> </td> Fin de l'exécution du nœud de condition. </td> </tr> <tr> <td> Node d'agent de condition </td> <td> <strong> Oui </strong> </td> <td> reçoit la sortie finale d'un nœud de condition précédente, indiquant l'achèvement du traitement du nœud de l'agent de condition.

{% hint style = "info"%}
Le nœud de fin ** nécessite au moins une connexion à partir des nœuds suivants **: nœud d'agent, nœud LLM ou nœud d'outil.
{% EndHint%}

### Sorties

Le nœud de fin ** n'a pas de connexions de sortie ** car elle signifie la terminaison du flux d'informations.

### Meilleures pratiques

{% Tabs%}
{% Tab Title = "Pro Tips"%}
** Fournir une réponse finale **

Le cas échéant, connectez le nœud final à un nœud LLM ou agent dédié pour générer un message final ou un résumé pour l'utilisateur, en fournissant la fermeture à la conversation.
{% endtab%}

{% tab title = "Pièges de potential"%}
** terminaison de conversation prématurée **

* ** Problème: ** Le nœud de fin est placé trop tôt dans le workflow, ce qui a fait fin la conversation avant la fin de toutes les étapes nécessaires ou que la demande de l'utilisateur est entièrement traitée.
* ** Exemple: ** Un chatbot conçu pour collecter les commentaires de l'utilisateur met fin à la conversation après que l'utilisateur a fourni son premier commentaire, sans lui donner l'occasion de fournir des commentaires supplémentaires ou de poser des questions.
* ** Solution: ** Passez en revue votre logique de workflow et assurez-vous que le nœud final est placé uniquement une fois toutes les étapes essentielles terminées ou que l'utilisateur a explicitement indiqué leur intention de mettre fin à la conversation.

** Manque de fermeture pour l'utilisateur **

* ** Problème: ** La conversation se termine brusquement sans signal clair à l'utilisateur ou un message final qui donne un sentiment de fermeture.
* ** Exemple: ** Un chatbot de support client termine la conversation immédiatement après avoir résolu un problème, sans confirmer la résolution avec l'utilisateur ou offrir une aide supplémentaire.
* ** Solution: ** Connectez le nœud final à un nœud LLM ou agent dédicatoire pour générer une réponse finale qui résume la conversation, confirme toutes les actions prises et donne un sentiment de fermeture pour l'utilisateur.
{% endtab%}
{% endtabs%}

***

## Nœud de condition vs nœud d'agent de condition

Les nœuds d'agent de condition et de condition sont essentiels dans l'architecture d'agent séquentiel de Flowise pour créer des expériences conversationnelles dynamiques.

Ces nœuds permettent des flux de travail adaptatifs, répondant à la saisie, au contexte et aux décisions complexes, mais diffèrent dans leur approche de l'évaluation et de la sophistication des conditions.

<Dettots>

<summary> <strong> Node de condition </strong> </summary>

**But**

Pour créer des branches basées sur des conditions logiques simples et prédéfinies.

** Évaluation des conditions **

Utilise une interface basée sur une table ou un éditeur de code JavaScript pour définir les conditions qui sont vérifiées par rapport à l'état personnalisé et / ou l'historique complet de la conversation.

** Comportement de sortie **

* Prend en charge plusieurs chemins de sortie, chacun associé à une condition spécifique.
* Les conditions sont évaluées dans l'ordre. La première condition de correspondance détermine la sortie.
* Si aucune condition n'est remplie, le débit suit une sortie par défaut "End".

** le mieux adapté pour **

* Des décisions de routage simples basées sur des conditions facilement définissables.
* Workflows où la logique peut être exprimée à l'aide de comparaisons simples, de vérifications de mots clés ou de valeurs de variable d'état personnalisées.

</fords>

<Dettots>

<summary> <strong> Condition d'agent nœud </strong> </summary>

**But**

Pour créer un routage dynamique basé sur l'analyse par un agent de la conversation et de sa sortie structurée.

** Évaluation des conditions **

* Si aucun modèle de chat n'est connecté, il utilise le System LLM par défaut (à partir du nœud de démarrage) pour traiter l'historique de conversation et tout état personnalisé.
* Il peut générer une sortie structurée, qui est ensuite utilisée pour l'évaluation des conditions.
* Utilise une interface basée sur une table ou un éditeur de code JavaScript pour définir des conditions qui sont vérifiées par rapport à la sortie de l'agent, structurées ou non.

** Comportement de sortie **

Identique au nœud de condition:

* Prend en charge plusieurs chemins de sortie, chacun associé à une condition spécifique.
* Les conditions sont évaluées dans l'ordre. La première condition de correspondance détermine la sortie.
* Si aucune condition n'est remplie, le flux suit la sortie par défaut "End".

** le mieux adapté pour **

* Des décisions de routage plus complexes qui nécessitent une compréhension du contexte de la conversation, de l'intention des utilisateurs ou des facteurs nuancés.
* Scénarios où des conditions logiques simples sont insuffisantes pour capturer la logique de routage souhaitée.
* ** Exemple: ** Un chatbot doit déterminer si la question d'un utilisateur est liée à une catégorie de produit spécifique. Un nœud d'agent de condition peut être utilisé pour analyser la requête de l'utilisateur et sortir un objet JSON avec un champ "Catégorie". Le nœud d'agent de condition peut ensuite utiliser cette sortie structurée pour acheminer l'utilisateur vers le spécialiste du produit approprié.

</fords>

### Résumant

<Bile> <Thead> <Tr> <th width = "218"> </th> <th width = "258"> Condition Node </th> <th> Condition Agent Node </th> </tr> </thead> <tbody> <tr> <td> <strong> Logique de décision </strong> </td> <td> basée sur des conditions logiques prédéfinies. </ TD> <DD> Basée sur des conditions logiques prédéfinies. </ TD> Sortie. </td> </tr> <tr> <td> <strong> Implication de l'agent </strong> </td> <td> Aucun agent impliqué dans l'évaluation de la condition. </td> <td> utilise un agent pour traiter le contexte et générer une sortie pour les conditions. Encouragé pour l'évaluation fiable des conditions. </td> </tr> <tr> <td> <strong> Évaluation de la condition </strong> </td> <td> définir les conditions qui sont vérifiées par rapport à l'historique complet de la conversation. </td> <td> peut définir les conditions qui sont vérifiées contre la sortie de l'agent, structurée ou non. </td> </tr> <tr> <td> <strong> complexité </strong> </td> <td> Convient à la logique de ramification simple. </td> </tr> <td> <td> <fond> Cases idéales à base d'utilisateurs </strong> Mots-clés dans la conversation. </li> </ul> </td> <td> <ul> <li> Routing basé sur le sentiment, l'intention ou les facteurs contextuels complexes. </li> </ul> </td> </tr> </tbody> </pally>

### Choisir le bon nœud

* ** Node de condition: ** Utilisez le nœud de condition lorsque votre logique de routage implique des décisions simples en fonction de conditions facilement définissables. Par exemple, il est parfait pour vérifier les mots clés spécifiques, la comparaison des valeurs dans l'état ou l'évaluation d'autres expressions logiques simples.
* ** Node d'agent de condition: ** Cependant, lorsque votre routage exige une compréhension plus profonde des nuances de la conversation, le nœud d'agent de condition est le meilleur choix. Ce nœud agit comme votre assistant de routage intelligent, tirant parti d'un LLM pour analyser la conversation, faire des jugements en fonction du contexte et fournir une sortie structurée qui pilote un routage plus sophistiqué et dynamique.

***

## Node d'agent vs nœud LLM

Il est important de comprendre que le nœud ** llm et le nœud d'agent peuvent être considérés comme des entités agentiques au sein de notre système **, car ils exploitent tous les deux les capacités d'un modèle de langue grand (LLM) ou d'un modèle de chat.

Cependant, bien que les deux nœuds puissent traiter le langage et interagir avec les outils, ils sont conçus à différentes fins dans un flux de travail.

<Dettots>

<Summary> Node d'agent </summary>

**Se concentrer**

L'objectif principal du nœud d'agent pour simuler les actions et la prise de décision d'un agent humain dans un contexte conversationnel.

Il agit comme un coordinateur de haut niveau au sein du flux de travail, réunissant la compréhension du langage, l'exécution des outils et la prise de décision pour créer une expérience conversationnelle plus humaine.

** Forces **

* Gère efficacement l'exécution de plusieurs outils et intègre leurs résultats.
* Offre une prise en charge intégrée pour l'homme en boucle (HITL), permettant l'examen humain et l'approbation des opérations sensibles.

** le mieux adapté pour **

* Les flux de travail où l'agent doit guider l'utilisateur, collecter des informations, faire des choix et gérer le flux de conversation global.
* Scénarios nécessitant une intégration avec plusieurs outils externes.
* Des tâches impliquant des données sensibles ou des actions où la surveillance humaine est bénéfique, comme l'approbation de la transaction financière

</fords>

<Dettots>

<summary> llm nœud </summary>

**Se concentrer**

Semblable au nœud d'agent, mais il offre plus de flexibilité lors de l'utilisation d'outils et de boucle humaine (HITL), tous deux via le nœud d'outil.

** Forces **

* Permet à la définition des schémas JSON de structurer la sortie de LLM, ce qui facilite l'extraction d'informations spécifiques.
* Offre une flexibilité dans l'intégration des outils, permettant des séquences plus complexes de LLM et d'appels d'outils, et fournissant un contrôle à grain fin sur la fonction HITL.

** le mieux adapté pour **

* Scénarios où les données structurées doivent être extraites de la réponse du LLM.
* Les workflows nécessitant un mélange d'exécutions d'outils automatisées et évaluées par l'homme. Par exemple, un nœud LLM peut appeler un outil pour récupérer les informations du produit (automatisé), puis un outil différent pour traiter un paiement, qui nécessiterait l'approbation de HITL.

</fords>

### Résumant

<Bile> <Thead> <Tr> <th width = "206"> </th> <th width = "253"> Agent nœud </th> <th> llm nœud </th> </tr> </thead> <tbody> <tr> <td> <strong> L'interaction de l'outil </strong> </td> <td> appelle directement les outils multiples, des outils intégrés, hitL. via le nœud d'outil, contrôle hitl granulaire au niveau de l'outil. </td> </tr> <tr> <td> <strong> Human-in-the-Loop (HITL) </strong> </td> <td> HITL contrôlé au niveau du nœud d'agent (tous les outils connectés affectés). </td> <td> HITL Géré au niveau du nœud d'outil individuel (plus La flexibilité). </td> </tr> <tr> <td> <strong> Sortie structurée </strong> </td> <td> s'appuie sur le format de sortie naturel du LLM. </td> <td> Sortie sur le format de sortie naturelle du LLM, mais, si nécessaire, fournit une définition de schéma JSON à la sortie de la structure. Cases</strong></td><td><ul><li>Workflows with complex tool orchestration.</li><li>Simplified HITL at the Agent Level.</li></ul></td><td><ul><li>Extracting structured data from LLM output</li><li>Workflows with complex LLM and tool interactions, requiring mixed HITL niveaux. </li> </ul> </td> </tr> </ tbody> </ table>

### Choisir le bon nœud

* ** Choisissez le nœud d'agent: ** Utilisez le nœud d'agent lorsque vous devez créer un système conversationnel qui peut gérer l'exécution de plusieurs outils, tous partageant le même paramètre HITL (activé ou désactivé pour le nœud d'agent entier). Le nœud d'agent est également bien adapté pour gérer les conversations complexes en plusieurs étapes où un comportement cohérent de type agent est souhaité.
* ** Choisissez le nœud LLM: ** D'un autre côté, utilisez le nœud LLM lorsque vous devez extraire des données structurées à partir de la sortie de LLM à l'aide de la fonction de schéma JSON, une capacité non disponible dans le nœud d'agent. Le nœud LLM excelle également à l'orchestration de l'exécution de l'outil avec un contrôle à grain fin sur HITL au niveau de l'outil individuel, vous permettant de mélanger les exécutions d'outils automatisées et évaluées par l'homme en utilisant plusieurs nœuds d'outils connectés au nœud LLM.

[^ 1]: Dans notre contexte actuel, un niveau d'abstraction inférieur fait référence à un système qui expose un plus grand degré de détail au développeur.
