---
description: Learn how to build multi-agents system using Agentflow V2, written by @toi500
---

# Agentflow v2

Ce guide explore l'architecture AgentFlow V2, détaillant ses concepts principaux, ses cas d'utilisation, son état de flux et ses références de nœuds complets.

{% hint style = "avertissement"%}
** Avis de non-responsabilité: ** Cette documentation décrit AgentFlow V2 à sa version officielle actuelle. Les fonctionnalités, les fonctionnalités et les paramètres de nœud sont soumis à un changement dans les futures mises à jour et versions de Flowise. Veuillez vous référer aux dernières notes de publication officielle ou à des informations sur l'application pour les détails les plus récents.
{% EndHint%}

{% embed url = "https://youtu.be/-h4wquzrhhi?si=jkhuefiw06ao6ge"%}

## Concept de base

AgentFlow V2 représente une évolution architecturale significative, introduisant un nouveau paradigme en flux qui se concentre sur une orchestration explicite du flux de travail et une flexibilité accrue. Contrairement à la dépendance principale de V1 sur les cadres externes pour sa logique de graphique d'agent de base, V2 déplace l'attention de la conception de l'ensemble du flux de travail en utilisant un ensemble granulaire de nœuds autonomes spécialisés développés nativement en tant que composants coulissants principaux.

Dans cette architecture V2, chaque nœud fonctionne comme une unité indépendante, exécutant une opération discrète en fonction de sa conception et de sa configuration spécifiques. Les connexions visuelles entre les nœuds de la canevas définissent explicitement le chemin de travail et la séquence de contrôle du workflow, les données peuvent être transmises entre les nœuds en faisant référence aux sorties de tout nœud précédemment exécuté dans le flux actuel, et l'état de flux fournit un mécanisme explicite pour gérer et partager des données tout au long du flux de travail.

L'architecture V2 met en œuvre un système complet de la dépendance aux nœuds et de la file d'attente d'exécution qui respecte précisément ces voies définies tout en maintenant une séparation claire entre les composants, permettant aux flux de travail de devenir à la fois plus sophistiqués et plus faciles à concevoir. Cela permet aux modèles complexes comme les boucles, la ramification conditionnelle, les interactions humaines dans la boucle et d'autres à être réalisables. Cela le rend plus adaptable à divers cas d'utilisation tout en restant plus maintenable et extensible.

<div data -full-width = "false"> <gigust> <img src = "../. gitbook / actifs / agentflowv2 / motifs.png" alt = ""> <figcaption> </gigcaption> </ figure> </div>

## Différence entre AgentFlow et Plateforme d'automatisation

L'une des questions les plus posées: quelle est la différence entre AgentFlow et les plates-formes d'automatisation comme N8N, Make ou Zapier?

### 💬 ** Communication d'agent à agent **

La communication multimodale entre les agents est prise en charge. Un agent de superviseur peut formuler et déléguer des tâches à plusieurs agents de travailleurs, avec des sorties des agents des travailleurs retournés par la suite au superviseur.

À chaque étape, les agents ont accès à l'historique complet de la conversation, permettant au superviseur de déterminer la tâche suivante et les agents des travailleurs pour interpréter la tâche, sélectionner les outils appropriés et exécuter les actions en conséquence.

Cette architecture permet ** la collaboration, la délégation et la gestion des tâches partagées ** sur plusieurs agents, ces capacités ne sont généralement pas offertes par les outils d'automatisation traditionnels.

<gigne> <image> <source srcset = "../. GitBook / Assets / Captures 2025-05-16 153946.png" Media = "(préfère-Color-Scheme: Dark)"> <img Src = "../. (1) (1) (1) (1) .png "alt =" "> </gital> <figcaption> </gigcaption> </stigne>

### 🙋‍ Human dans la boucle

L'exécution est interrompue en attendant l'entrée humaine, sans bloquer le thread en cours d'exécution. Chaque point de contrôle est enregistré, permettant au flux de travail de reprendre à partir du même point même après un redémarrage de l'application.

L'utilisation de points de contrôle permet ** les agents de longue durée et avec état **.

Les agents peuvent également être configurés pour ** demander l'autorisation avant d'exécuter des outils **, similaire à la façon dont Claude demande l'approbation de l'utilisateur avant d'utiliser les outils MCP. Cela permet d'empêcher l'exécution autonome d'actions sensibles sans l'approbation explicite de l'utilisateur.

<gigne> <image> <source srcset = "../. GitBook / Assets / Captures 2025-05-16 154908.png" Media = "(préfers-Color-Scheme: Dark)"> <img src = "../. (1) (1) (1) (1) (1) .png "alt =" "> </ picture> <figcaption> </gigcaption> </gigust>

### 📖 État partagé

L'état partagé permet l'échange de données entre les agents, particulièrement utile pour passer des données entre les branches ou les étapes non adjacentes d'un flux. Se référer à[#understanding-flow-state](agentflowv2.md#understanding-flow-state "mention")

### ⚡ Streaming

Prend en charge les événements de serveur (SSE) pour le streaming en temps réel de réponses LLM ou d'agent. Le streaming permet également aux mises à jour de l'abonnement à l'exécution au fur et à mesure que le flux de travail progresse.

<gigne> <img src = "../. GitBook / Assets / Longgif.gif" alt = ""> <Figcaption> </gigcaption> </gigust>

### 🌐 outils MCP

Alors que les plates-formes d'automatisation traditionnelles présentent souvent de vastes bibliothèques d'intégrations prédéfinies, AgentFlow permet à MCP ([Model Context Protocol](https://github.com/modelcontextprotocol)) outils à connecter dans le cadre du flux de travail, plutôt que de fonctionner uniquement en tant qu'outils d'agent.

Les MCP personnalisés peuvent également être créés indépendamment, sans dépendre des intégrations fournies par la plate-forme. MCP est largement considéré comme une norme de l'industrie et est généralement soutenu et maintenu par les prestataires officiels. Par exemple, le GitHub MCP est développé et maintenu par l'équipe GitHub, avec un soutien similaire fourni pour Atlassian Jira, Brave Search, et autres.

<gigne> <image> <source srcset = "../. GitBook / Assets / Captures 2025-05-16 160752.png" Media = "(préfère-Color-Scheme: Dark)"> <img Src = "../. (1) .png "alt =" "> </ picture> <gigon> </figcaption> </gigne>

## Référence du nœud AgentFlow V2

Cette section fournit une référence détaillée pour chaque nœud disponible, décrivant son objectif spécifique, les paramètres de configuration des clés, les entrées attendues, les sorties générées et son rôle dans l'architecture AgentFlow V2.

<gigne> <mage> <source srcset = "../. GitBook / Assets / Agentflowv2 / DarkMode / V2-01-D (1) .png" Media = "(préfers-Color-Scheme: Dark)"> <img src = "../. GitBook / Assets / Agentflowv2 / V2-all-Node.png" alt = ""> </ picture> <figction> </gigcaption> </figucion>

***

### ** 1. Démarrer le nœud **

Le point d'entrée désigné pour lancer n'importe quelle exécution de workflow AgentFlow V2. Chaque flux doit commencer par ce nœud.

* ** Fonctionnalité: ** Définit comment le flux de travail est déclenché et configure les conditions initiales. Il peut accepter les entrées directement à partir de l'interface de chat ou via un formulaire personnalisable présenté à l'utilisateur. Il permet également l'initialisation de`Flow State`Variables au début de l'exécution et peut gérer la façon dont la mémoire de conversation est gérée pour l'exécution.
* ** Paramètres de configuration **
  * ** Type d'entrée **: détermine comment l'exécution du flux de travail est initiée, soit par`Chat Input`de l'utilisateur ou via un soumis`Form Input`.
    * ** Titre du formulaire, description du formulaire, types d'entrée de formulaire **: If`Form Input`est sélectionné, ces champs configurent l'apparence du formulaire présenté à l'utilisateur, permettant divers types de champs de saisie avec des étiquettes définies et des noms de variables.
  * ** Mémoire éphémère **: si elle est activée, demande au workflow de commencer l'exécution sans considérer les messages passés du thread de conversation, en commençant efficacement par une ardoise de mémoire propre.
  * ** État de flux **: définit l'ensemble complet des paires de valeurs clés initiales pour l'état d'exécution du workflow`$flow.state`. Toutes les clés d'état qui seront utilisées ou mises à jour par les nœuds suivantes doivent être déclarées et initialisées ici.
* ** Entrées: ** Reçoit les données initiales qui déclenchent le workflow, qui sera soit un message de chat, soit les données soumises via un formulaire.
* ** Sorties: ** Fournit une seule ancre de sortie pour se connecter au premier nœud opérationnel, passant les données d'entrée initiales et l'état de flux initialisé.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-02-d.png" media = "(préfère-Color-Scheme: Dark)"> <img src = "../. width = "343"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 2. Node LLM **

Fournit un accès direct à un modèle de grande langue (LLM) configuré pour exécuter des tâches AI, permettant au workflow d'effectuer une extraction structurée de données si nécessaire.

* ** Fonctionnalité: ** Ce nœud envoie des demandes à un LLM basé sur des instructions (messages) et un contexte fourni. Il peut être utilisé pour la génération de texte, le résumé, la traduction, l'analyse, la réponse aux questions et la génération de sortie JSON structurée selon un schéma défini. Il a accès à la mémoire pour le thread de conversation et peut lire / écrire`Flow State`.
* ** Paramètres de configuration **
  * ** Modèle **: Spécifie le modèle AI à partir d'un service choisi - par exemple, GPT-4O d'OpenAI ou Google Gemini.
  * ** Messages **: Définissez l'entrée conversationnelle pour le LLM, en la structurant comme une séquence de rôles - système, utilisateur, assistant, développeur - pour guider la réponse de l'IA. Les données dynamiques peuvent être insérées en utilisant`{{ variable }}`.
  * ** Mémoire **: Si vous activez, détermine si le LLM doit considérer l'historique du thread de conversation actuel lors de la génération de sa réponse.
    * ** Type de mémoire, taille de la fenêtre, limite de jeton maximale **: Si la mémoire est utilisée, ces paramètres affinent comment l'historique de la conversation est géré et présenté au LLM - par exemple, s'il faut inclure tous les messages, seulement une fenêtre récente de virages ou une version résumée.
    * ** Message d'entrée **: Spécifie la variable ou le texte qui sera annexé comme le message utilisateur le plus récent à la fin du contexte de conversation existant - y compris le contexte initial et la mémoire - avant d'être traités par le LLM / Agent.
  * ** Retour Response As **: Configure comment la sortie de LLM est classée - comme un`User Message`ou`Assistant Message`- qui peut influencer la façon dont il est géré par les systèmes de mémoire ou la journalisation ultérieurs.
  * ** Sortie structurée JSON **: Demande au LLM de formater sa sortie en fonction d'un schéma JSON spécifique - y compris des clés, des types de données et des descriptions - garantissant des données prévisibles et lisibles par la machine.
  * ** Mettre à jour l'état de flux **: permet au nœud de modifier l'état d'exécution du workflow`$flow.state`Pendant l'exécution en mettant à jour les clés prédéfinies. Cela permet, par exemple, de stocker la sortie de ce nœud LLM sous une telle clé, ce qui le rend accessible aux nœuds suivants.
* ** Entrées: ** Ce nœud utilise des données du déclencheur initial du workflow ou des sorties des nœuds précédents, incorporant ces données dans le`Messages`ou`Input Message`champs. Il peut également récupérer des valeurs de`$flow.state`Lorsque les variables d'entrée le font référence.
* ** Sorties: ** produit la réponse de LLM, qui sera soit du texte brut, soit un objet JSON structuré. La catégorisation de cette sortie - en tant qu'utilisateur ou assistant - est déterminée par le`Return Response`paramètre.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-03-d.png" media = "(préfère-Color-Scheme: Dark)"> <img src = "../. width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 3. Node d'agent **

Représente une entité d'IA autonome capable de raisonner, de planifier et d'interagir avec des outils ou des sources de connaissances pour atteindre un objectif donné.

* ** Fonctionnalité: ** Ce nœud utilise un LLM pour décider dynamiquement d'une séquence d'actions. En fonction de l'objectif de l'utilisateur - fourni via des messages / entrées - il peut choisir d'utiliser des outils disponibles ou des magasins de documents de requête pour recueillir des informations ou effectuer des actions. Il gère son propre cycle de raisonnement et peut utiliser la mémoire pour le fil de conversation et`Flow State`. Convient aux tâches nécessitant un raisonnement en plusieurs étapes ou interagissant dynamiquement avec des systèmes ou des outils externes.
* ** Paramètres de configuration **
  * ** Modèle **: Spécifie le modèle AI à partir d'un service choisi - par exemple, GPT-4O ou Google Gemini d'OpenAI - qui conduira les processus de raisonnement et de prise de décision de l'agent.
  * ** Messages **: Définissez l'entrée conversationnelle initiale, l'objectif ou le contexte pour l'agent, en le structurant comme une séquence de rôles - système, utilisateur, assistant, développeur - pour guider la compréhension de l'agent et les actions ultérieures. Les données dynamiques peuvent être insérées en utilisant`{{ variable }}`.
  * ** Outils **: Spécifiez quels outils fluide prédéfinis l'agent est autorisé à utiliser pour atteindre ses objectifs.
    * Pour chaque outil sélectionné, un ** facultatif ** nécessite un indicateur d'entrée humain ** indique si l'opération de l'outil peut elle-même s'arrêter pour demander une intervention humaine.
  * ** Magasins de connaissances / documents **: Configurer l'accès aux informations dans les magasins de documents gérés par flux.
    * ** Magasin de documents **: Choisissez une boutique de documents préconfigurée à partir de laquelle l'agent peut récupérer des informations. Ces magasins doivent être mis en place et peuplés à l'avance.
    * ** Décrire les connaissances **: Fournir une description du langage naturel du contenu et du but de ce magasin de documents. Cette description guide l'agent pour comprendre quel type d'informations le magasin contient et quand il serait approprié de les interroger.
  * ** Connaissances / Vector intégrés **: Configurez l'accès aux magasins de vecteurs externes et préexistants comme sources de connaissances supplémentaires pour l'agent.
    * ** Vector Store **: Sélectionne la base de données vectorielle spécifique et préconfigurée que l'agent peut interroger.
    * ** Modèle d'intégration **: Spécifie le modèle d'intégration associé au magasin vectoriel sélectionné, assurant la compatibilité des requêtes.
    * ** Nom de la connaissance **: attribue un court nom descriptif à cette source de connaissances basée sur un vecteur, que l'agent peut utiliser pour référence.
    * ** Décrire les connaissances **: Fournir une description du langage naturel du contenu et du but de ce magasin vectoriel, en guidant l'agent sur quand et comment utiliser cette source de connaissances spécifique.
    * ** RETOUR DOCUMENTS SOURCES **: Si vous êtes activé, demande à l'agent d'inclure des informations sur les documents source avec les données récupérées du magasin vectoriel.
  * ** Mémoire **: Si vous êtes activé, détermine si l'agent doit considérer l'historique du thread de conversation actuel lors de la prise de décisions et de la génération de réponses.
    * ** Type de mémoire, taille de la fenêtre, limite de jeton maximale **: Si la mémoire est utilisée, ces paramètres affinent comment l'historique de la conversation est géré et présenté à l'agent - par exemple, que ce soit pour inclure tous les messages, seulement une fenêtre récente ou une version résumée.
    * ** Message d'entrée **: Spécifie la variable ou le texte qui sera annexé comme le message utilisateur le plus récent à la fin du contexte de conversation existant - y compris le contexte initial et la mémoire - avant d'être traités par le LLM / Agent.
  * ** RETOUR RÉPONSE **: Configure comment la sortie ou le message final de l'agent est classé - en tant que message utilisateur ou message assistant - qui peut influencer la façon dont il est géré par des systèmes de mémoire ultérieurs ou la journalisation.
  * ** Mettre à jour l'état de flux **: permet au nœud de modifier l'état d'exécution du workflow`$flow.state`Pendant l'exécution en mettant à jour les clés prédéfinies. Cela permet, par exemple, de stocker la sortie de ce nœud d'agent sous une telle clé, ce qui le rend accessible aux nœuds suivants.
* ** Entrées: ** Ce nœud utilise les données du déclencheur initial du workflow ou des sorties des nœuds précédents, souvent incorporés dans le`Messages`ou`Input Message`champs. Il accède aux outils configurés et aux sources de connaissances selon les besoins.
* ** Sorties: ** produit le résultat ou la réponse finale générée par l'agent une fois qu'il a terminé son raisonnement, sa planification et toute interaction avec des outils ou des sources de connaissances.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-04-d.png" media = "(préfère-Color-Scheme: Dark)"> <img src = "../. width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 4. Node d'outil **

Fournit un mécanisme pour exécuter directement et de manière déterministe un outil fluide spécifique et prédéfini dans la séquence de workflow. Contrairement au nœud d'agent, où le LLM choisit dynamiquement un outil basé sur le raisonnement, le nœud d'outil exécute exactement l'outil sélectionné par le concepteur de workflow pendant la configuration.

* ** Fonctionnalité: ** Ce nœud est utilisé lorsque le workflow nécessite l'exécution d'une capacité spécifique connue à un point défini, avec des entrées facilement disponibles. Il garantit une action déterministe sans impliquer le raisonnement LLM pour la sélection des outils.
* ** Comment ça marche **
  1. ** TRANGERS: ** Lorsque l'exécution du workflow atteint un nœud d'outil, il s'active.
  2. ** Identification de l'outil: ** Il identifie l'outil de flux spécifique sélectionné dans sa configuration.
  3. ** Résolution de l'argument d'entrée: ** Il examine la configuration des arguments d'entrée de l'outil. Pour chaque paramètre d'entrée requis de l'outil sélectionné.
  4. ** Exécution: ** Il invoque le code sous-jacent ou l'appel API associé à l'outil Flowise sélectionné, passant les arguments d'entrée résolus.
  5. ** Génération de sortie: ** Il reçoit le résultat renvoyé par l'exécution de l'outil.
  6. ** Propagation de sortie: ** Il rend ce résultat disponible via son ancre de sortie pour les nœuds suivants.
* ** Paramètres de configuration **
  * ** Sélection d'outils **: Choisissez l'outil Flowise spécifique et enregistré que ce nœud exécutera à partir d'une liste déroulante.
  * ** Arguments d'entrée **: Définissez comment les données de votre flux de travail sont fournies à l'outil sélectionné. Cette section s'adapte dynamiquement en fonction de l'outil choisi, présentant ses paramètres d'entrée spécifiques:
    * ** Nom de l'argument de la carte **: Pour chaque entrée, l'outil sélectionné nécessite (par exemple,`input`Pour une calculatrice), ce champ affichera le nom du paramètre attendu tel que défini par l'outil lui-même.
    * ** Fournir une valeur d'argument **: Définissez la valeur de ce paramètre correspondant, en utilisant une variable dynamique comme`{{ previousNode.output }}`, `{{ $flow.state.someKey }}`, ou en entrant un texte statique.
  * ** Mettre à jour l'état de flux **: permet au nœud de modifier l'état d'exécution du workflow`$flow.state`Pendant l'exécution en mettant à jour les clés prédéfinies. Cela permet, par exemple, de stocker la sortie de cet outil sous une telle clé, ce qui le rend accessible aux nœuds suivants.
* ** Entrées: ** reçoit les données nécessaires pour les arguments de l'outil via le`Input Arguments`mappage, valeurs d'approvisionnement à partir des sorties de nœud précédentes,`$flow.state`ou configurations statiques.
* ** Sorties: ** produit la sortie brute générée par l'outil exécuté - par exemple, une chaîne JSON à partir d'une API, un résultat de texte ou une valeur numérique.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-16-d.png" media = "(préfers-color-scheme: dark)"> <img src = "../. gitbook / actifs / agentflowv2 / v2-05.png" alt = "" " width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 5. Retriever Node **

Effectue une récupération d'informations ciblée à partir des magasins de documents configurés.

* ** Fonctionnalité: ** Ce nœud interroge un ou plusieurs magasins de documents spécifiés, récupérant des morceaux de document pertinents basés sur la similitude sémantique. C'est une alternative ciblée à l'utilisation d'un nœud d'agent lorsque la seule action requise est la récupération et la sélection des outils dynamiques par un LLM n'est pas nécessaire.
* ** Paramètres de configuration **
  * ** Magasins de connaissances / documents **: Spécifiez quel (s) magasin de documents préconfigurés et peuplés, ce nœud doit interroger pour trouver des informations pertinentes.
  * ** Retriever Query **: Définissez la requête texte qui sera utilisée pour rechercher les magasins de documents sélectionnés. Les données dynamiques peuvent être insérées en utilisant`{{ variables }}`.
  * ** Format de sortie **: Choisissez comment les informations récupérées doivent être présentées - soit comme simple`Text`ou comme`Text with Metadata`, qui peut inclure des détails tels que les noms de documents source ou les emplacements.
  * ** Mettre à jour l'état de flux **: permet au nœud de modifier l'état d'exécution du workflow`$flow.state`Pendant l'exécution en mettant à jour les clés prédéfinies. Cela permet, par exemple, de stocker la sortie de ce nœud Retriever sous une telle clé, ce qui le rend accessible aux nœuds suivants.
* ** Entrées: ** nécessite une chaîne de requête - souvent fournie comme une variable à partir d'une étape précédente ou d'une entrée utilisateur - et accède aux magasins de documents sélectionnés pour plus d'informations.
* ** sorties: ** produit les morceaux de document récupérés de la base de connaissances, formaté selon les choisis`Output Format`.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-06-d.png" media = "(préfère-Color-Scheme: Dark)"> <img src = "../. GitBook / Assets / Agentflowv2 / V2-06.png" alt = "" width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### 6. nœud http

Facilite la communication directe avec les services Web externes et les API via le protocole de transfert hypertexte (HTTP).

* ** Fonctionnalité: ** Ce nœud permet au workflow d'interagir avec tout système externe accessible via HTTP. Il peut envoyer différents types de demandes (obtenir, publier, mettre, supprimer, patcher) à une URL spécifiée, permettant une intégration avec des API tierces, récupérer des données à partir de ressources Web ou déclencher des webhooks externes. Le nœud prend en charge la configuration des méthodes d'authentification, des en-têtes personnalisés, des paramètres de requête et différents types de corps de demande pour répondre aux diverses exigences d'API.
* ** Paramètres de configuration **
  * ** HTTP Idedential **: Sélectionnez éventuellement des informations d'identification préconfigurées - telles que l'authentification de base, le jeton de support ou la clé API - pour authentifier les demandes au service cible.
  * ** Méthode de la demande **: Spécifiez la méthode HTTP à utiliser pour la demande - par exemple,`GET`, `POST`, `PUT`, `DELETE`, `PATCH`.
  * ** URL cible **: Définissez l'URL complète du point de terminaison externe auquel la demande sera envoyée.
  * ** En-têtes de demande **: Définissez tous les en-têtes HTTP nécessaires en paires de valeurs clés à inclure dans la demande.
  * ** Paramètres de requête URL **: Définissez les paires de valeurs clés qui seront annexées à l'URL en tant que paramètres de requête.
  * ** Type de corps de demande **: Choisissez le format de la charge utile de demande si l'envoi de données - les options incluent`JSON`, `Raw text`, `Form Data`, ou`x-www-form-urlencoded`.
  * ** Demande Body **: Fournissez la charge utile de données réelle pour des méthodes comme le poste ou le put. Le format doit correspondre au sélectionné`Body Type`et les données dynamiques peuvent être insérées en utilisant`{{ variables }}`.
  * ** Type de réponse **: Spécifiez comment le flux de travail doit interpréter la réponse reçue du serveur - les options incluent`JSON`, `Text`, `Array Buffer`, ou`Base64`pour les données binaires.
* ** Entrées: ** reçoit des données de configuration telles que l'URL, la méthode, les en-têtes et le corps, incorporant souvent des valeurs dynamiques à partir d'étapes de work`$flow.state`.
* ** Sorties: ** produit la réponse reçue du serveur externe, analysé selon le sélectionné`Response Type`.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-07-d.png" media = "(préfère-Color-Scheme: Dark)"> <img src = "../. Gitbook / Assets / Agentflowv2 / V2-07.png" alt = "" width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 7. Node de condition **

Implémente la logique de ramification déterministe dans le flux de travail sur la base des règles définies.

* ** Fonctionnalité: ** Ce nœud agit comme un point de décision, évaluant une ou plusieurs conditions spécifiées pour diriger le flux de travail dans différents chemins. Il compare les valeurs d'entrée - qui peuvent être des chaînes, des nombres ou des booléens - en utilisant une variété d'opérateurs logiques, tels que les égaux, contient, supérieur ou vide. Sur la base de la question de savoir si ces conditions évaluent en vrai ou fausse, l'exécution du flux de travail passe le long de l'une des branches de sortie distinctes connectées à ce nœud.
* ** Paramètres de configuration **
  * ** Conditions **: Configurez l'ensemble des règles logiques que le nœud évaluera.
    * ** Type **: Spécifiez le type de données comparées pour cette règle -`String`, `Number`, ou`Boolean`.
    * ** Valeur 1 **: Définissez la première valeur pour la comparaison. Les données dynamiques peuvent être insérées en utilisant`{{ variables }}`.
    * ** Opération **: Sélectionnez l'opérateur logique à appliquer entre la valeur 1 et la valeur 2 - par exemple,`equal`, `notEqual`, `contains`, `larger`, `isEmpty`.
    * ** Valeur 2 **: Définissez la deuxième valeur pour la comparaison, si nécessaire par l'opération choisie. Les données dynamiques peuvent également être insérées ici en utilisant`{{ variables }}`.
* ** Entrées: ** nécessite les données pour`Value 1`et`Value 2`pour chaque condition évaluée. Ces valeurs sont fournies à partir des sorties de nœud précédentes ou récupérées à partir de`$flow.state`.
* ** Sorties: ** fournit plusieurs ancres de sortie, correspondant au résultat booléen (vrai / faux) des conditions évaluées. Le flux de travail continue le long du chemin spécifique connecté à l'ancre de sortie qui correspond au résultat.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-08-d.png" media = "(préfère-Color-Scheme: Dark)"> <img src = "../. width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 8. Node d'agent de condition **

Fournit une ramification dynamique basée sur l'IA basée sur les instructions et le contexte du langage naturel.

* ** Fonctionnalité: ** Ce nœud utilise un modèle grand langage (LLM) pour acheminer le workflow. Les analyses des analyses ont fourni des données d'entrée par rapport à un ensemble de "scénarios" définis par l'utilisateur - résultats ou catégories potentiels - guidés par des "instructions" de langage naturel de haut niveau qui définissent la tâche de prise de décision. Le LLM détermine ensuite quel scénario correspond le mieux au contexte d'entrée actuel. Sur la base de cette classification dirigée par l'IA, l'exécution du flux de travail réduit le chemin de sortie spécifique correspondant au scénario choisi. Ce nœud est particulièrement utile pour les tâches telles que la reconnaissance de l'intention des utilisateurs, le routage conditionnel complexe ou la prise de décision situationnelle nuancée où des règles simples et prédéfinies - comme dans le nœud de condition - sont insuffisantes.
* ** Paramètres de configuration **
  * ** Modèle **: Spécifie le modèle AI à partir d'un service choisi qui effectuera l'analyse et la classification des scénarios.
  * ** Instructions **: Définissez l'objectif ou la tâche globale du LLM en langage naturel - par exemple, "Déterminez si la demande de l'utilisateur concerne les ventes, le support ou la demande générale."
  * ** Entrée **: spécifiez les données, souvent du texte à partir d'une étape précédente ou d'une entrée utilisateur, en utilisant`{{ variables }}`, que le LLM analysera pour prendre sa décision de routage.
  * ** Scénarios **: Configurer un tableau définissant les résultats possibles ou les chemins distincts que le flux de travail peut prendre. Chaque scénario est décrit dans le langage naturel - par exemple, «Enquête sur les ventes», «demande de support», «question générale» - et chacune correspond à une ancre de sortie unique sur le nœud.
* ** Entrées: ** nécessite le`Input`Données pour l'analyse et le`Instructions`Pour guider le LLM.
* ** sorties: ** fournit plusieurs ancres de sortie, une pour chaque définie`Scenario`. Le workflow continue le long du chemin spécifique connecté à l'ancre de sortie que le LLM détermine le meilleur correspond à l'entrée.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-09-d.png" media = "(préfère-Color-Scheme: Dark)"> <img src = "../. width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 9. Node d'itération **

Exécute un "sous-flux" défini - une séquence de nœuds imbriqués - pour chaque élément d'un tableau d'entrée, implémentant une boucle "for-een".

* ** Fonctionnalité: ** Ce nœud est conçu pour le traitement des collections de données. Il prend un tableau, fourni directement ou référencé via une variable, comme entrée. Pour chaque élément individuel à l'intérieur de ce tableau, le nœud d'itération exécute séquentiellement la séquence d'autres nœuds qui sont visuellement placés à l'intérieur de ses limites sur la toile.
* ** Paramètres de configuration **
  * ** Entrée du tableau **: Spécifie le tableau d'entrée que le nœud iratera. Ceci est fourni en faisant référence à une variable qui contient un tableau à partir de la sortie d'un nœud précédent ou du`$flow.state`- par exemple,`{{ $flow.state.itemList }}`.
* ** Entrées: ** nécessite un tableau à fournir à son`Array Input`paramètre.
* ** Sorties: ** Fournit une seule anage de sortie qui ne devient active qu'après que le sous-flux imbriqué a terminé l'exécution pour tous les éléments du tableau d'entrée. Les données transmises par cette sortie peuvent inclure des résultats agrégés ou l'état final des variables modifié dans la boucle, selon la conception du sous-flux. Les nœuds placés à l'intérieur du bloc d'itération ont leurs propres connexions d'entrée et de sortie distinctes qui définissent la séquence d'opérations pour chaque élément.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-10-d (1) .png" media = "(prefers-color-scheme: sombre)"> <img src = "../. gitbook / actets / agentflowv2 / v2-10.png" alt = "" " width = "563"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 10. Node de boucle **

Redirige explicitement l'exécution du workflow vers un nœud précédemment exécuté.

* ** Fonctionnalité: ** Ce nœud permet la création de cycles ou de tentatives itératives dans un workflow. Lorsque le flux d'exécution atteint le nœud de boucle, il n'atteint pas un nouveau nœud; Au lieu de cela, il "remonte" à un nœud cible spécifié qui a déjà été exécuté plus tôt dans l'exécution actuelle du flux de travail. Cette action provoque la réexécution de ce nœud cible et de tous les nœuds suivants dans cette partie de l'écoulement.
* ** Paramètres de configuration **
  * ** Loop Retour à **: Sélectionne l'ID unique d'un nœud précédemment exécuté dans le flux de travail actuel auquel l'exécution doit retourner.
  * ** MAX LOOP COUNT **: Définit le nombre maximal de fois que cette opération de boucle peut être effectuée dans une seule exécution de workflow, sauvegarde contre les cycles infinis. La valeur par défaut est 5.
* ** Entrées: ** Reçoit le signal d'exécution pour activer. Il suit en interne le nombre de fois que la boucle s'est produite pour l'exécution actuelle.
* ** Sorties: ** Ce nœud n'a pas d'ancre de sortie standard à pointant, car sa fonction principale est de rediriger le flux d'exécution vers l'arrière vers le`Loop Back To`Node cible, d'où le flux de travail continue alors.

<gigne> <mage> <source srcset = "../. GitBook / Assets / Agentflowv2 / DarkMode / V2-11-D.png" Media = "(préfère-Color-Scheme: Dark)"> <img src = "../. GitBook / Assets / Agentflowv2 / V2-11.png" Alt = "" width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 11. Node d'entrée humain **

Utilise l'exécution du workflow pour demander des entrées, une approbation ou des commentaires explicites d'un utilisateur humain - un composant clé pour les processus humains dans la boucle (HITL).

* ** Fonctionnalité: ** Ce nœud arrête la progression automatisée du flux de travail et présente des informations ou une question à un utilisateur humain, via l'interface de chat. Le contenu affiché à l'utilisateur peut être un texte statique prédéfini ou généré dynamiquement par un LLM basé sur le contexte de workflow actuel. L'utilisateur reçoit des choix d'action distincts - par exemple, «procéder», «rejeter» - et, s'il est activé, un champ pour fournir des commentaires textuels. Une fois que l'utilisateur fait une sélection et soumet sa réponse, le flux de travail reprend l'exécution le long du chemin de sortie spécifique correspondant à son action choisie.
* ** Paramètres de configuration **
  * ** Type de description **: détermine comment le message ou la question présentée à l'utilisateur est généré - soit`Fixed`(texte statique) ou`Dynamic`(généré par un LLM).
    * ** Si le type de description est`Fixed`**
      * ** Description **: Ce champ contient le texte exact à afficher à l'utilisateur. Il prend en charge l'insertion de données dynamiques en utilisant`{{ variables }}`
    * **Si`Description Type`est`Dynamic`**
      * ** Modèle **: sélectionne le modèle AI dans un service choisi qui générera le message orienté utilisateur.
      * ** invite **: fournit les instructions ou l'invite pour le LLM sélectionné pour générer le message affiché à l'utilisateur.
  * ** Feedback: ** Si activé, l'utilisateur sera invité avec une fenêtre de rétroaction pour laisser ses commentaires, et ces commentaires seront annexés à la sortie du nœud.
* ** Entrées: ** Reçoit le signal d'exécution pour suspendre le workflow. Il peut utiliser les données des étapes précédentes ou`$flow.state`à travers des variables dans le`Description`ou`Prompt`champs s'ils sont configurés pour le contenu dynamique.
* ** Sorties: ** Fournit deux ancres de sortie, chacune correspondant à une action utilisateur distincte - une ancre pour "procéder" et une autre pour "rejeter". Le flux de travail continue le long du chemin connecté à l'ancre correspondant à la sélection de l'utilisateur.

<gigne> <mage> <source srcset = "../. GitBook / Assets / Agentflowv2 / DarkMode / V2-12-D.png" Media = "(Prefers-Color-Scheme: Dark)"> <img src = "../. GitBook / Assets / Agentflowv2 / V2-12.png" Alt = "" width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 12. Node de réponse directe **

Envoie un message final à l'utilisateur et termine le chemin d'exécution actuel.

* ** Fonctionnalité: ** Ce nœud sert de point de terminaison pour une branche spécifique ou l'intégralité d'un workflow. Il prend un message configuré - qui peut être du texte statique ou du contenu dynamique d'une variable - et le livre directement à l'utilisateur final via l'interface de chat. Lors de l'envoi de ce message, l'exécution le long de ce chemin particulier du workflow conclut; Aucun autre nœud connecté à partir de ce point ne sera traité.
* ** Paramètres de configuration **
  * ** Message **: Définissez le texte ou la variable`{{ variable }}`Cela contient le contenu à envoyer comme réponse finale à l'utilisateur.
* ** Entrées: ** reçoit le contenu du message, qui provient de la sortie d'un nœud précédent ou d'une valeur stockée dans`$flow.state`.
* ** Sorties: ** Ce nœud n'a pas d'ancres de sortie, car sa fonction est de terminer le chemin d'exécution après avoir envoyé la réponse.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-13-d.png" media = "(préfers-color-scheme: dark)"> <img src = "../. gitbook / actifs / agentflowv2 / v2-13.png" alt = "" " width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 13. Node de fonction personnalisé **

Fournit un mécanisme pour exécuter le code JavaScript côté serveur personnalisé dans le workflow.

* ** Fonctionnalité: ** Ce nœud permet d'écrire et d'exécuter des extraits arbitraires JavaScript, offrant un moyen efficace d'implémenter des transformations de données complexes, une logique métier sur mesure ou des interactions avec des ressources non directement prises en charge par d'autres nœuds standard. Le code exécuté fonctionne dans un environnement Node.js et a des moyens spécifiques d'accéder aux données:
  * ** Variables d'entrée: ** Valeurs passées via le`Input Variables`La configuration est accessible dans la fonction, généralement préfixée avec`---
description: Learn how to build multi-agents system using Agentflow V2, written by @toi500
---

# Agentflow v2

Ce guide explore l'architecture AgentFlow V2, détaillant ses concepts principaux, ses cas d'utilisation, son état de flux et ses références de nœuds complets.

{% hint style = "avertissement"%}
** Avis de non-responsabilité: ** Cette documentation décrit AgentFlow V2 à sa version officielle actuelle. Les fonctionnalités, les fonctionnalités et les paramètres de nœud sont soumis à un changement dans les futures mises à jour et versions de Flowise. Veuillez vous référer aux dernières notes de publication officielle ou à des informations sur l'application pour les détails les plus récents.
{% EndHint%}

{% embed url = "https://youtu.be/-h4wquzrhhi?si=jkhuefiw06ao6ge"%}

## Concept de base

AgentFlow V2 représente une évolution architecturale significative, introduisant un nouveau paradigme en flux qui se concentre sur une orchestration explicite du flux de travail et une flexibilité accrue. Contrairement à la dépendance principale de V1 sur les cadres externes pour sa logique de graphique d'agent de base, V2 déplace l'attention de la conception de l'ensemble du flux de travail en utilisant un ensemble granulaire de nœuds autonomes spécialisés développés nativement en tant que composants coulissants principaux.

Dans cette architecture V2, chaque nœud fonctionne comme une unité indépendante, exécutant une opération discrète en fonction de sa conception et de sa configuration spécifiques. Les connexions visuelles entre les nœuds de la canevas définissent explicitement le chemin de travail et la séquence de contrôle du workflow, les données peuvent être transmises entre les nœuds en faisant référence aux sorties de tout nœud précédemment exécuté dans le flux actuel, et l'état de flux fournit un mécanisme explicite pour gérer et partager des données tout au long du flux de travail.

L'architecture V2 met en œuvre un système complet de la dépendance aux nœuds et de la file d'attente d'exécution qui respecte précisément ces voies définies tout en maintenant une séparation claire entre les composants, permettant aux flux de travail de devenir à la fois plus sophistiqués et plus faciles à concevoir. Cela permet aux modèles complexes comme les boucles, la ramification conditionnelle, les interactions humaines dans la boucle et d'autres à être réalisables. Cela le rend plus adaptable à divers cas d'utilisation tout en restant plus maintenable et extensible.

<div data -full-width = "false"> <gigust> <img src = "../. gitbook / actifs / agentflowv2 / motifs.png" alt = ""> <figcaption> </gigcaption> </ figure> </div>

## Différence entre AgentFlow et Plateforme d'automatisation

L'une des questions les plus posées: quelle est la différence entre AgentFlow et les plates-formes d'automatisation comme N8N, Make ou Zapier?

### 💬 ** Communication d'agent à agent **

La communication multimodale entre les agents est prise en charge. Un agent de superviseur peut formuler et déléguer des tâches à plusieurs agents de travailleurs, avec des sorties des agents des travailleurs retournés par la suite au superviseur.

À chaque étape, les agents ont accès à l'historique complet de la conversation, permettant au superviseur de déterminer la tâche suivante et les agents des travailleurs pour interpréter la tâche, sélectionner les outils appropriés et exécuter les actions en conséquence.

Cette architecture permet ** la collaboration, la délégation et la gestion des tâches partagées ** sur plusieurs agents, ces capacités ne sont généralement pas offertes par les outils d'automatisation traditionnels.

<gigne> <image> <source srcset = "../. GitBook / Assets / Captures 2025-05-16 153946.png" Media = "(préfère-Color-Scheme: Dark)"> <img Src = "../. (1) (1) (1) (1) .png "alt =" "> </gital> <figcaption> </gigcaption> </stigne>

### 🙋‍ Human dans la boucle

L'exécution est interrompue en attendant l'entrée humaine, sans bloquer le thread en cours d'exécution. Chaque point de contrôle est enregistré, permettant au flux de travail de reprendre à partir du même point même après un redémarrage de l'application.

L'utilisation de points de contrôle permet ** les agents de longue durée et avec état **.

Les agents peuvent également être configurés pour ** demander l'autorisation avant d'exécuter des outils **, similaire à la façon dont Claude demande l'approbation de l'utilisateur avant d'utiliser les outils MCP. Cela permet d'empêcher l'exécution autonome d'actions sensibles sans l'approbation explicite de l'utilisateur.

<gigne> <image> <source srcset = "../. GitBook / Assets / Captures 2025-05-16 154908.png" Media = "(préfers-Color-Scheme: Dark)"> <img src = "../. (1) (1) (1) (1) (1) .png "alt =" "> </ picture> <figcaption> </gigcaption> </gigust>

### 📖 État partagé

L'état partagé permet l'échange de données entre les agents, particulièrement utile pour passer des données entre les branches ou les étapes non adjacentes d'un flux. Se référer à[#understanding-flow-state](agentflowv2.md#understanding-flow-state "mention")

### ⚡ Streaming

Prend en charge les événements de serveur (SSE) pour le streaming en temps réel de réponses LLM ou d'agent. Le streaming permet également aux mises à jour de l'abonnement à l'exécution au fur et à mesure que le flux de travail progresse.

<gigne> <img src = "../. GitBook / Assets / Longgif.gif" alt = ""> <Figcaption> </gigcaption> </gigust>

### 🌐 outils MCP

Alors que les plates-formes d'automatisation traditionnelles présentent souvent de vastes bibliothèques d'intégrations prédéfinies, AgentFlow permet à MCP ([Model Context Protocol](https://github.com/modelcontextprotocol)) outils à connecter dans le cadre du flux de travail, plutôt que de fonctionner uniquement en tant qu'outils d'agent.

Les MCP personnalisés peuvent également être créés indépendamment, sans dépendre des intégrations fournies par la plate-forme. MCP est largement considéré comme une norme de l'industrie et est généralement soutenu et maintenu par les prestataires officiels. Par exemple, le GitHub MCP est développé et maintenu par l'équipe GitHub, avec un soutien similaire fourni pour Atlassian Jira, Brave Search, et autres.

<gigne> <image> <source srcset = "../. GitBook / Assets / Captures 2025-05-16 160752.png" Media = "(préfère-Color-Scheme: Dark)"> <img Src = "../. (1) .png "alt =" "> </ picture> <gigon> </figcaption> </gigne>

## Référence du nœud AgentFlow V2

Cette section fournit une référence détaillée pour chaque nœud disponible, décrivant son objectif spécifique, les paramètres de configuration des clés, les entrées attendues, les sorties générées et son rôle dans l'architecture AgentFlow V2.

<gigne> <mage> <source srcset = "../. GitBook / Assets / Agentflowv2 / DarkMode / V2-01-D (1) .png" Media = "(préfers-Color-Scheme: Dark)"> <img src = "../. GitBook / Assets / Agentflowv2 / V2-all-Node.png" alt = ""> </ picture> <figction> </gigcaption> </figucion>

***

### ** 1. Démarrer le nœud **

Le point d'entrée désigné pour lancer n'importe quelle exécution de workflow AgentFlow V2. Chaque flux doit commencer par ce nœud.

* ** Fonctionnalité: ** Définit comment le flux de travail est déclenché et configure les conditions initiales. Il peut accepter les entrées directement à partir de l'interface de chat ou via un formulaire personnalisable présenté à l'utilisateur. Il permet également l'initialisation de`Flow State`Variables au début de l'exécution et peut gérer la façon dont la mémoire de conversation est gérée pour l'exécution.
* ** Paramètres de configuration **
  * ** Type d'entrée **: détermine comment l'exécution du flux de travail est initiée, soit par`Chat Input`de l'utilisateur ou via un soumis`Form Input`.
    * ** Titre du formulaire, description du formulaire, types d'entrée de formulaire **: If`Form Input`est sélectionné, ces champs configurent l'apparence du formulaire présenté à l'utilisateur, permettant divers types de champs de saisie avec des étiquettes définies et des noms de variables.
  * ** Mémoire éphémère **: si elle est activée, demande au workflow de commencer l'exécution sans considérer les messages passés du thread de conversation, en commençant efficacement par une ardoise de mémoire propre.
  * ** État de flux **: définit l'ensemble complet des paires de valeurs clés initiales pour l'état d'exécution du workflow`$flow.state`. Toutes les clés d'état qui seront utilisées ou mises à jour par les nœuds suivantes doivent être déclarées et initialisées ici.
* ** Entrées: ** Reçoit les données initiales qui déclenchent le workflow, qui sera soit un message de chat, soit les données soumises via un formulaire.
* ** Sorties: ** Fournit une seule ancre de sortie pour se connecter au premier nœud opérationnel, passant les données d'entrée initiales et l'état de flux initialisé.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-02-d.png" media = "(préfère-Color-Scheme: Dark)"> <img src = "../. width = "343"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 2. Node LLM **

Fournit un accès direct à un modèle de grande langue (LLM) configuré pour exécuter des tâches AI, permettant au workflow d'effectuer une extraction structurée de données si nécessaire.

* ** Fonctionnalité: ** Ce nœud envoie des demandes à un LLM basé sur des instructions (messages) et un contexte fourni. Il peut être utilisé pour la génération de texte, le résumé, la traduction, l'analyse, la réponse aux questions et la génération de sortie JSON structurée selon un schéma défini. Il a accès à la mémoire pour le thread de conversation et peut lire / écrire`Flow State`.
* ** Paramètres de configuration **
  * ** Modèle **: Spécifie le modèle AI à partir d'un service choisi - par exemple, GPT-4O d'OpenAI ou Google Gemini.
  * ** Messages **: Définissez l'entrée conversationnelle pour le LLM, en la structurant comme une séquence de rôles - système, utilisateur, assistant, développeur - pour guider la réponse de l'IA. Les données dynamiques peuvent être insérées en utilisant`{{ variable }}`.
  * ** Mémoire **: Si vous activez, détermine si le LLM doit considérer l'historique du thread de conversation actuel lors de la génération de sa réponse.
    * ** Type de mémoire, taille de la fenêtre, limite de jeton maximale **: Si la mémoire est utilisée, ces paramètres affinent comment l'historique de la conversation est géré et présenté au LLM - par exemple, s'il faut inclure tous les messages, seulement une fenêtre récente de virages ou une version résumée.
    * ** Message d'entrée **: Spécifie la variable ou le texte qui sera annexé comme le message utilisateur le plus récent à la fin du contexte de conversation existant - y compris le contexte initial et la mémoire - avant d'être traités par le LLM / Agent.
  * ** Retour Response As **: Configure comment la sortie de LLM est classée - comme un`User Message`ou`Assistant Message`- qui peut influencer la façon dont il est géré par les systèmes de mémoire ou la journalisation ultérieurs.
  * ** Sortie structurée JSON **: Demande au LLM de formater sa sortie en fonction d'un schéma JSON spécifique - y compris des clés, des types de données et des descriptions - garantissant des données prévisibles et lisibles par la machine.
  * ** Mettre à jour l'état de flux **: permet au nœud de modifier l'état d'exécution du workflow`$flow.state`Pendant l'exécution en mettant à jour les clés prédéfinies. Cela permet, par exemple, de stocker la sortie de ce nœud LLM sous une telle clé, ce qui le rend accessible aux nœuds suivants.
* ** Entrées: ** Ce nœud utilise des données du déclencheur initial du workflow ou des sorties des nœuds précédents, incorporant ces données dans le`Messages`ou`Input Message`champs. Il peut également récupérer des valeurs de`$flow.state`Lorsque les variables d'entrée le font référence.
* ** Sorties: ** produit la réponse de LLM, qui sera soit du texte brut, soit un objet JSON structuré. La catégorisation de cette sortie - en tant qu'utilisateur ou assistant - est déterminée par le`Return Response`paramètre.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-03-d.png" media = "(préfère-Color-Scheme: Dark)"> <img src = "../. width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 3. Node d'agent **

Représente une entité d'IA autonome capable de raisonner, de planifier et d'interagir avec des outils ou des sources de connaissances pour atteindre un objectif donné.

* ** Fonctionnalité: ** Ce nœud utilise un LLM pour décider dynamiquement d'une séquence d'actions. En fonction de l'objectif de l'utilisateur - fourni via des messages / entrées - il peut choisir d'utiliser des outils disponibles ou des magasins de documents de requête pour recueillir des informations ou effectuer des actions. Il gère son propre cycle de raisonnement et peut utiliser la mémoire pour le fil de conversation et`Flow State`. Convient aux tâches nécessitant un raisonnement en plusieurs étapes ou interagissant dynamiquement avec des systèmes ou des outils externes.
* ** Paramètres de configuration **
  * ** Modèle **: Spécifie le modèle AI à partir d'un service choisi - par exemple, GPT-4O ou Google Gemini d'OpenAI - qui conduira les processus de raisonnement et de prise de décision de l'agent.
  * ** Messages **: Définissez l'entrée conversationnelle initiale, l'objectif ou le contexte pour l'agent, en le structurant comme une séquence de rôles - système, utilisateur, assistant, développeur - pour guider la compréhension de l'agent et les actions ultérieures. Les données dynamiques peuvent être insérées en utilisant`{{ variable }}`.
  * ** Outils **: Spécifiez quels outils fluide prédéfinis l'agent est autorisé à utiliser pour atteindre ses objectifs.
    * Pour chaque outil sélectionné, un ** facultatif ** nécessite un indicateur d'entrée humain ** indique si l'opération de l'outil peut elle-même s'arrêter pour demander une intervention humaine.
  * ** Magasins de connaissances / documents **: Configurer l'accès aux informations dans les magasins de documents gérés par flux.
    * ** Magasin de documents **: Choisissez une boutique de documents préconfigurée à partir de laquelle l'agent peut récupérer des informations. Ces magasins doivent être mis en place et peuplés à l'avance.
    * ** Décrire les connaissances **: Fournir une description du langage naturel du contenu et du but de ce magasin de documents. Cette description guide l'agent pour comprendre quel type d'informations le magasin contient et quand il serait approprié de les interroger.
  * ** Connaissances / Vector intégrés **: Configurez l'accès aux magasins de vecteurs externes et préexistants comme sources de connaissances supplémentaires pour l'agent.
    * ** Vector Store **: Sélectionne la base de données vectorielle spécifique et préconfigurée que l'agent peut interroger.
    * ** Modèle d'intégration **: Spécifie le modèle d'intégration associé au magasin vectoriel sélectionné, assurant la compatibilité des requêtes.
    * ** Nom de la connaissance **: attribue un court nom descriptif à cette source de connaissances basée sur un vecteur, que l'agent peut utiliser pour référence.
    * ** Décrire les connaissances **: Fournir une description du langage naturel du contenu et du but de ce magasin vectoriel, en guidant l'agent sur quand et comment utiliser cette source de connaissances spécifique.
    * ** RETOUR DOCUMENTS SOURCES **: Si vous êtes activé, demande à l'agent d'inclure des informations sur les documents source avec les données récupérées du magasin vectoriel.
  * ** Mémoire **: Si vous êtes activé, détermine si l'agent doit considérer l'historique du thread de conversation actuel lors de la prise de décisions et de la génération de réponses.
    * ** Type de mémoire, taille de la fenêtre, limite de jeton maximale **: Si la mémoire est utilisée, ces paramètres affinent comment l'historique de la conversation est géré et présenté à l'agent - par exemple, que ce soit pour inclure tous les messages, seulement une fenêtre récente ou une version résumée.
    * ** Message d'entrée **: Spécifie la variable ou le texte qui sera annexé comme le message utilisateur le plus récent à la fin du contexte de conversation existant - y compris le contexte initial et la mémoire - avant d'être traités par le LLM / Agent.
  * ** RETOUR RÉPONSE **: Configure comment la sortie ou le message final de l'agent est classé - en tant que message utilisateur ou message assistant - qui peut influencer la façon dont il est géré par des systèmes de mémoire ultérieurs ou la journalisation.
  * ** Mettre à jour l'état de flux **: permet au nœud de modifier l'état d'exécution du workflow`$flow.state`Pendant l'exécution en mettant à jour les clés prédéfinies. Cela permet, par exemple, de stocker la sortie de ce nœud d'agent sous une telle clé, ce qui le rend accessible aux nœuds suivants.
* ** Entrées: ** Ce nœud utilise les données du déclencheur initial du workflow ou des sorties des nœuds précédents, souvent incorporés dans le`Messages`ou`Input Message`champs. Il accède aux outils configurés et aux sources de connaissances selon les besoins.
* ** Sorties: ** produit le résultat ou la réponse finale générée par l'agent une fois qu'il a terminé son raisonnement, sa planification et toute interaction avec des outils ou des sources de connaissances.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-04-d.png" media = "(préfère-Color-Scheme: Dark)"> <img src = "../. width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 4. Node d'outil **

Fournit un mécanisme pour exécuter directement et de manière déterministe un outil fluide spécifique et prédéfini dans la séquence de workflow. Contrairement au nœud d'agent, où le LLM choisit dynamiquement un outil basé sur le raisonnement, le nœud d'outil exécute exactement l'outil sélectionné par le concepteur de workflow pendant la configuration.

* ** Fonctionnalité: ** Ce nœud est utilisé lorsque le workflow nécessite l'exécution d'une capacité spécifique connue à un point défini, avec des entrées facilement disponibles. Il garantit une action déterministe sans impliquer le raisonnement LLM pour la sélection des outils.
* ** Comment ça marche **
  1. ** TRANGERS: ** Lorsque l'exécution du workflow atteint un nœud d'outil, il s'active.
  2. ** Identification de l'outil: ** Il identifie l'outil de flux spécifique sélectionné dans sa configuration.
  3. ** Résolution de l'argument d'entrée: ** Il examine la configuration des arguments d'entrée de l'outil. Pour chaque paramètre d'entrée requis de l'outil sélectionné.
  4. ** Exécution: ** Il invoque le code sous-jacent ou l'appel API associé à l'outil Flowise sélectionné, passant les arguments d'entrée résolus.
  5. ** Génération de sortie: ** Il reçoit le résultat renvoyé par l'exécution de l'outil.
  6. ** Propagation de sortie: ** Il rend ce résultat disponible via son ancre de sortie pour les nœuds suivants.
* ** Paramètres de configuration **
  * ** Sélection d'outils **: Choisissez l'outil Flowise spécifique et enregistré que ce nœud exécutera à partir d'une liste déroulante.
  * ** Arguments d'entrée **: Définissez comment les données de votre flux de travail sont fournies à l'outil sélectionné. Cette section s'adapte dynamiquement en fonction de l'outil choisi, présentant ses paramètres d'entrée spécifiques:
    * ** Nom de l'argument de la carte **: Pour chaque entrée, l'outil sélectionné nécessite (par exemple,`input`Pour une calculatrice), ce champ affichera le nom du paramètre attendu tel que défini par l'outil lui-même.
    * ** Fournir une valeur d'argument **: Définissez la valeur de ce paramètre correspondant, en utilisant une variable dynamique comme`{{ previousNode.output }}`, `{{ $flow.state.someKey }}`, ou en entrant un texte statique.
  * ** Mettre à jour l'état de flux **: permet au nœud de modifier l'état d'exécution du workflow`$flow.state`Pendant l'exécution en mettant à jour les clés prédéfinies. Cela permet, par exemple, de stocker la sortie de cet outil sous une telle clé, ce qui le rend accessible aux nœuds suivants.
* ** Entrées: ** reçoit les données nécessaires pour les arguments de l'outil via le`Input Arguments`mappage, valeurs d'approvisionnement à partir des sorties de nœud précédentes,`$flow.state`ou configurations statiques.
* ** Sorties: ** produit la sortie brute générée par l'outil exécuté - par exemple, une chaîne JSON à partir d'une API, un résultat de texte ou une valeur numérique.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-16-d.png" media = "(préfers-color-scheme: dark)"> <img src = "../. gitbook / actifs / agentflowv2 / v2-05.png" alt = "" " width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 5. Retriever Node **

Effectue une récupération d'informations ciblée à partir des magasins de documents configurés.

* ** Fonctionnalité: ** Ce nœud interroge un ou plusieurs magasins de documents spécifiés, récupérant des morceaux de document pertinents basés sur la similitude sémantique. C'est une alternative ciblée à l'utilisation d'un nœud d'agent lorsque la seule action requise est la récupération et la sélection des outils dynamiques par un LLM n'est pas nécessaire.
* ** Paramètres de configuration **
  * ** Magasins de connaissances / documents **: Spécifiez quel (s) magasin de documents préconfigurés et peuplés, ce nœud doit interroger pour trouver des informations pertinentes.
  * ** Retriever Query **: Définissez la requête texte qui sera utilisée pour rechercher les magasins de documents sélectionnés. Les données dynamiques peuvent être insérées en utilisant`{{ variables }}`.
  * ** Format de sortie **: Choisissez comment les informations récupérées doivent être présentées - soit comme simple`Text`ou comme`Text with Metadata`, qui peut inclure des détails tels que les noms de documents source ou les emplacements.
  * ** Mettre à jour l'état de flux **: permet au nœud de modifier l'état d'exécution du workflow`$flow.state`Pendant l'exécution en mettant à jour les clés prédéfinies. Cela permet, par exemple, de stocker la sortie de ce nœud Retriever sous une telle clé, ce qui le rend accessible aux nœuds suivants.
* ** Entrées: ** nécessite une chaîne de requête - souvent fournie comme une variable à partir d'une étape précédente ou d'une entrée utilisateur - et accède aux magasins de documents sélectionnés pour plus d'informations.
* ** sorties: ** produit les morceaux de document récupérés de la base de connaissances, formaté selon les choisis`Output Format`.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-06-d.png" media = "(préfère-Color-Scheme: Dark)"> <img src = "../. GitBook / Assets / Agentflowv2 / V2-06.png" alt = "" width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### 6. nœud http

Facilite la communication directe avec les services Web externes et les API via le protocole de transfert hypertexte (HTTP).

* ** Fonctionnalité: ** Ce nœud permet au workflow d'interagir avec tout système externe accessible via HTTP. Il peut envoyer différents types de demandes (obtenir, publier, mettre, supprimer, patcher) à une URL spécifiée, permettant une intégration avec des API tierces, récupérer des données à partir de ressources Web ou déclencher des webhooks externes. Le nœud prend en charge la configuration des méthodes d'authentification, des en-têtes personnalisés, des paramètres de requête et différents types de corps de demande pour répondre aux diverses exigences d'API.
* ** Paramètres de configuration **
  * ** HTTP Idedential **: Sélectionnez éventuellement des informations d'identification préconfigurées - telles que l'authentification de base, le jeton de support ou la clé API - pour authentifier les demandes au service cible.
  * ** Méthode de la demande **: Spécifiez la méthode HTTP à utiliser pour la demande - par exemple,`GET`, `POST`, `PUT`, `DELETE`, `PATCH`.
  * ** URL cible **: Définissez l'URL complète du point de terminaison externe auquel la demande sera envoyée.
  * ** En-têtes de demande **: Définissez tous les en-têtes HTTP nécessaires en paires de valeurs clés à inclure dans la demande.
  * ** Paramètres de requête URL **: Définissez les paires de valeurs clés qui seront annexées à l'URL en tant que paramètres de requête.
  * ** Type de corps de demande **: Choisissez le format de la charge utile de demande si l'envoi de données - les options incluent`JSON`, `Raw text`, `Form Data`, ou`x-www-form-urlencoded`.
  * ** Demande Body **: Fournissez la charge utile de données réelle pour des méthodes comme le poste ou le put. Le format doit correspondre au sélectionné`Body Type`et les données dynamiques peuvent être insérées en utilisant`{{ variables }}`.
  * ** Type de réponse **: Spécifiez comment le flux de travail doit interpréter la réponse reçue du serveur - les options incluent`JSON`, `Text`, `Array Buffer`, ou`Base64`pour les données binaires.
* ** Entrées: ** reçoit des données de configuration telles que l'URL, la méthode, les en-têtes et le corps, incorporant souvent des valeurs dynamiques à partir d'étapes de work`$flow.state`.
* ** Sorties: ** produit la réponse reçue du serveur externe, analysé selon le sélectionné`Response Type`.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-07-d.png" media = "(préfère-Color-Scheme: Dark)"> <img src = "../. Gitbook / Assets / Agentflowv2 / V2-07.png" alt = "" width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 7. Node de condition **

Implémente la logique de ramification déterministe dans le flux de travail sur la base des règles définies.

* ** Fonctionnalité: ** Ce nœud agit comme un point de décision, évaluant une ou plusieurs conditions spécifiées pour diriger le flux de travail dans différents chemins. Il compare les valeurs d'entrée - qui peuvent être des chaînes, des nombres ou des booléens - en utilisant une variété d'opérateurs logiques, tels que les égaux, contient, supérieur ou vide. Sur la base de la question de savoir si ces conditions évaluent en vrai ou fausse, l'exécution du flux de travail passe le long de l'une des branches de sortie distinctes connectées à ce nœud.
* ** Paramètres de configuration **
  * ** Conditions **: Configurez l'ensemble des règles logiques que le nœud évaluera.
    * ** Type **: Spécifiez le type de données comparées pour cette règle -`String`, `Number`, ou`Boolean`.
    * ** Valeur 1 **: Définissez la première valeur pour la comparaison. Les données dynamiques peuvent être insérées en utilisant`{{ variables }}`.
    * ** Opération **: Sélectionnez l'opérateur logique à appliquer entre la valeur 1 et la valeur 2 - par exemple,`equal`, `notEqual`, `contains`, `larger`, `isEmpty`.
    * ** Valeur 2 **: Définissez la deuxième valeur pour la comparaison, si nécessaire par l'opération choisie. Les données dynamiques peuvent également être insérées ici en utilisant`{{ variables }}`.
* ** Entrées: ** nécessite les données pour`Value 1`et`Value 2`pour chaque condition évaluée. Ces valeurs sont fournies à partir des sorties de nœud précédentes ou récupérées à partir de`$flow.state`.
* ** Sorties: ** fournit plusieurs ancres de sortie, correspondant au résultat booléen (vrai / faux) des conditions évaluées. Le flux de travail continue le long du chemin spécifique connecté à l'ancre de sortie qui correspond au résultat.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-08-d.png" media = "(préfère-Color-Scheme: Dark)"> <img src = "../. width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 8. Node d'agent de condition **

Fournit une ramification dynamique basée sur l'IA basée sur les instructions et le contexte du langage naturel.

* ** Fonctionnalité: ** Ce nœud utilise un modèle grand langage (LLM) pour acheminer le workflow. Les analyses des analyses ont fourni des données d'entrée par rapport à un ensemble de "scénarios" définis par l'utilisateur - résultats ou catégories potentiels - guidés par des "instructions" de langage naturel de haut niveau qui définissent la tâche de prise de décision. Le LLM détermine ensuite quel scénario correspond le mieux au contexte d'entrée actuel. Sur la base de cette classification dirigée par l'IA, l'exécution du flux de travail réduit le chemin de sortie spécifique correspondant au scénario choisi. Ce nœud est particulièrement utile pour les tâches telles que la reconnaissance de l'intention des utilisateurs, le routage conditionnel complexe ou la prise de décision situationnelle nuancée où des règles simples et prédéfinies - comme dans le nœud de condition - sont insuffisantes.
* ** Paramètres de configuration **
  * ** Modèle **: Spécifie le modèle AI à partir d'un service choisi qui effectuera l'analyse et la classification des scénarios.
  * ** Instructions **: Définissez l'objectif ou la tâche globale du LLM en langage naturel - par exemple, "Déterminez si la demande de l'utilisateur concerne les ventes, le support ou la demande générale."
  * ** Entrée **: spécifiez les données, souvent du texte à partir d'une étape précédente ou d'une entrée utilisateur, en utilisant`{{ variables }}`, que le LLM analysera pour prendre sa décision de routage.
  * ** Scénarios **: Configurer un tableau définissant les résultats possibles ou les chemins distincts que le flux de travail peut prendre. Chaque scénario est décrit dans le langage naturel - par exemple, «Enquête sur les ventes», «demande de support», «question générale» - et chacune correspond à une ancre de sortie unique sur le nœud.
* ** Entrées: ** nécessite le`Input`Données pour l'analyse et le`Instructions`Pour guider le LLM.
* ** sorties: ** fournit plusieurs ancres de sortie, une pour chaque définie`Scenario`. Le workflow continue le long du chemin spécifique connecté à l'ancre de sortie que le LLM détermine le meilleur correspond à l'entrée.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-09-d.png" media = "(préfère-Color-Scheme: Dark)"> <img src = "../. width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 9. Node d'itération **

Exécute un "sous-flux" défini - une séquence de nœuds imbriqués - pour chaque élément d'un tableau d'entrée, implémentant une boucle "for-een".

* ** Fonctionnalité: ** Ce nœud est conçu pour le traitement des collections de données. Il prend un tableau, fourni directement ou référencé via une variable, comme entrée. Pour chaque élément individuel à l'intérieur de ce tableau, le nœud d'itération exécute séquentiellement la séquence d'autres nœuds qui sont visuellement placés à l'intérieur de ses limites sur la toile.
* ** Paramètres de configuration **
  * ** Entrée du tableau **: Spécifie le tableau d'entrée que le nœud iratera. Ceci est fourni en faisant référence à une variable qui contient un tableau à partir de la sortie d'un nœud précédent ou du`$flow.state`- par exemple,`{{ $flow.state.itemList }}`.
* ** Entrées: ** nécessite un tableau à fournir à son`Array Input`paramètre.
* ** Sorties: ** Fournit une seule anage de sortie qui ne devient active qu'après que le sous-flux imbriqué a terminé l'exécution pour tous les éléments du tableau d'entrée. Les données transmises par cette sortie peuvent inclure des résultats agrégés ou l'état final des variables modifié dans la boucle, selon la conception du sous-flux. Les nœuds placés à l'intérieur du bloc d'itération ont leurs propres connexions d'entrée et de sortie distinctes qui définissent la séquence d'opérations pour chaque élément.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-10-d (1) .png" media = "(prefers-color-scheme: sombre)"> <img src = "../. gitbook / actets / agentflowv2 / v2-10.png" alt = "" " width = "563"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 10. Node de boucle **

Redirige explicitement l'exécution du workflow vers un nœud précédemment exécuté.

* ** Fonctionnalité: ** Ce nœud permet la création de cycles ou de tentatives itératives dans un workflow. Lorsque le flux d'exécution atteint le nœud de boucle, il n'atteint pas un nouveau nœud; Au lieu de cela, il "remonte" à un nœud cible spécifié qui a déjà été exécuté plus tôt dans l'exécution actuelle du flux de travail. Cette action provoque la réexécution de ce nœud cible et de tous les nœuds suivants dans cette partie de l'écoulement.
* ** Paramètres de configuration **
  * ** Loop Retour à **: Sélectionne l'ID unique d'un nœud précédemment exécuté dans le flux de travail actuel auquel l'exécution doit retourner.
  * ** MAX LOOP COUNT **: Définit le nombre maximal de fois que cette opération de boucle peut être effectuée dans une seule exécution de workflow, sauvegarde contre les cycles infinis. La valeur par défaut est 5.
* ** Entrées: ** Reçoit le signal d'exécution pour activer. Il suit en interne le nombre de fois que la boucle s'est produite pour l'exécution actuelle.
* ** Sorties: ** Ce nœud n'a pas d'ancre de sortie standard à pointant, car sa fonction principale est de rediriger le flux d'exécution vers l'arrière vers le`Loop Back To`Node cible, d'où le flux de travail continue alors.

<gigne> <mage> <source srcset = "../. GitBook / Assets / Agentflowv2 / DarkMode / V2-11-D.png" Media = "(préfère-Color-Scheme: Dark)"> <img src = "../. GitBook / Assets / Agentflowv2 / V2-11.png" Alt = "" width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 11. Node d'entrée humain **

Utilise l'exécution du workflow pour demander des entrées, une approbation ou des commentaires explicites d'un utilisateur humain - un composant clé pour les processus humains dans la boucle (HITL).

* ** Fonctionnalité: ** Ce nœud arrête la progression automatisée du flux de travail et présente des informations ou une question à un utilisateur humain, via l'interface de chat. Le contenu affiché à l'utilisateur peut être un texte statique prédéfini ou généré dynamiquement par un LLM basé sur le contexte de workflow actuel. L'utilisateur reçoit des choix d'action distincts - par exemple, «procéder», «rejeter» - et, s'il est activé, un champ pour fournir des commentaires textuels. Une fois que l'utilisateur fait une sélection et soumet sa réponse, le flux de travail reprend l'exécution le long du chemin de sortie spécifique correspondant à son action choisie.
* ** Paramètres de configuration **
  * ** Type de description **: détermine comment le message ou la question présentée à l'utilisateur est généré - soit`Fixed`(texte statique) ou`Dynamic`(généré par un LLM).
    * ** Si le type de description est`Fixed`**
      * ** Description **: Ce champ contient le texte exact à afficher à l'utilisateur. Il prend en charge l'insertion de données dynamiques en utilisant`{{ variables }}`
    * **Si`Description Type`est`Dynamic`**
      * ** Modèle **: sélectionne le modèle AI dans un service choisi qui générera le message orienté utilisateur.
      * ** invite **: fournit les instructions ou l'invite pour le LLM sélectionné pour générer le message affiché à l'utilisateur.
  * ** Feedback: ** Si activé, l'utilisateur sera invité avec une fenêtre de rétroaction pour laisser ses commentaires, et ces commentaires seront annexés à la sortie du nœud.
* ** Entrées: ** Reçoit le signal d'exécution pour suspendre le workflow. Il peut utiliser les données des étapes précédentes ou`$flow.state`à travers des variables dans le`Description`ou`Prompt`champs s'ils sont configurés pour le contenu dynamique.
* ** Sorties: ** Fournit deux ancres de sortie, chacune correspondant à une action utilisateur distincte - une ancre pour "procéder" et une autre pour "rejeter". Le flux de travail continue le long du chemin connecté à l'ancre correspondant à la sélection de l'utilisateur.

<gigne> <mage> <source srcset = "../. GitBook / Assets / Agentflowv2 / DarkMode / V2-12-D.png" Media = "(Prefers-Color-Scheme: Dark)"> <img src = "../. GitBook / Assets / Agentflowv2 / V2-12.png" Alt = "" width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 12. Node de réponse directe **

Envoie un message final à l'utilisateur et termine le chemin d'exécution actuel.

* ** Fonctionnalité: ** Ce nœud sert de point de terminaison pour une branche spécifique ou l'intégralité d'un workflow. Il prend un message configuré - qui peut être du texte statique ou du contenu dynamique d'une variable - et le livre directement à l'utilisateur final via l'interface de chat. Lors de l'envoi de ce message, l'exécution le long de ce chemin particulier du workflow conclut; Aucun autre nœud connecté à partir de ce point ne sera traité.
* ** Paramètres de configuration **
  * ** Message **: Définissez le texte ou la variable`{{ variable }}`Cela contient le contenu à envoyer comme réponse finale à l'utilisateur.
* ** Entrées: ** reçoit le contenu du message, qui provient de la sortie d'un nœud précédent ou d'une valeur stockée dans`$flow.state`.
* ** Sorties: ** Ce nœud n'a pas d'ancres de sortie, car sa fonction est de terminer le chemin d'exécution après avoir envoyé la réponse.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-13-d.png" media = "(préfers-color-scheme: dark)"> <img src = "../. gitbook / actifs / agentflowv2 / v2-13.png" alt = "" " width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 13. Node de fonction personnalisé **

Fournit un mécanisme pour exécuter le code JavaScript côté serveur personnalisé dans le workflow.

* ** Fonctionnalité: ** Ce nœud permet d'écrire et d'exécuter des extraits arbitraires JavaScript, offrant un moyen efficace d'implémenter des transformations de données complexes, une logique métier sur mesure ou des interactions avec des ressources non directement prises en charge par d'autres nœuds standard. Le code exécuté fonctionne dans un environnement Node.js et a des moyens spécifiques d'accéder aux données:
  * ** Variables d'entrée: ** Valeurs passées via le`Input Variables`La configuration est accessible dans la fonction, généralement préfixée avec- par exemple, si une variable d'entrée`userid`est défini, il est accessible comme`$userid`.
  * ** Contexte de flux: ** Les variables de configuration de flux par défaut sont disponibles, telles que`$flow.sessionId`, `$flow.chatId`, `$flow.chatflowId`, `$flow.input`- l'entrée initiale qui a commencé le flux de travail - et l'ensemble`$flow.state`objet.
  * ** Variables personnalisées: ** Toutes les variables personnalisées configurées dans Flowise - par exemple,`$vars.<variable-name>`.
  * ** Bibliothèques: ** La fonction peut utiliser toutes les bibliothèques qui ont été importées et rendues disponibles dans l'environnement backend Flowise. ** La fonction doit renvoyer une valeur de chaîne à la fin de son exécution **.
* ** Paramètres de configuration **
  * ** Variables d'entrée **: Configurez un tableau de définitions d'entrée qui seront transmises sous forme de variables dans la portée de votre fonction JavaScript. Pour chaque variable que vous souhaitez définir, vous spécifierez:
    * ** Nom de la variable **: le nom que vous utiliserez pour vous référer à cette variable dans votre code JavaScript, généralement préfixé avec un`---
description: Learn how to build multi-agents system using Agentflow V2, written by @toi500
---

# Agentflow v2

Ce guide explore l'architecture AgentFlow V2, détaillant ses concepts principaux, ses cas d'utilisation, son état de flux et ses références de nœuds complets.

{% hint style = "avertissement"%}
** Avis de non-responsabilité: ** Cette documentation décrit AgentFlow V2 à sa version officielle actuelle. Les fonctionnalités, les fonctionnalités et les paramètres de nœud sont soumis à un changement dans les futures mises à jour et versions de Flowise. Veuillez vous référer aux dernières notes de publication officielle ou à des informations sur l'application pour les détails les plus récents.
{% EndHint%}

{% embed url = "https://youtu.be/-h4wquzrhhi?si=jkhuefiw06ao6ge"%}

## Concept de base

AgentFlow V2 représente une évolution architecturale significative, introduisant un nouveau paradigme en flux qui se concentre sur une orchestration explicite du flux de travail et une flexibilité accrue. Contrairement à la dépendance principale de V1 sur les cadres externes pour sa logique de graphique d'agent de base, V2 déplace l'attention de la conception de l'ensemble du flux de travail en utilisant un ensemble granulaire de nœuds autonomes spécialisés développés nativement en tant que composants coulissants principaux.

Dans cette architecture V2, chaque nœud fonctionne comme une unité indépendante, exécutant une opération discrète en fonction de sa conception et de sa configuration spécifiques. Les connexions visuelles entre les nœuds de la canevas définissent explicitement le chemin de travail et la séquence de contrôle du workflow, les données peuvent être transmises entre les nœuds en faisant référence aux sorties de tout nœud précédemment exécuté dans le flux actuel, et l'état de flux fournit un mécanisme explicite pour gérer et partager des données tout au long du flux de travail.

L'architecture V2 met en œuvre un système complet de la dépendance aux nœuds et de la file d'attente d'exécution qui respecte précisément ces voies définies tout en maintenant une séparation claire entre les composants, permettant aux flux de travail de devenir à la fois plus sophistiqués et plus faciles à concevoir. Cela permet aux modèles complexes comme les boucles, la ramification conditionnelle, les interactions humaines dans la boucle et d'autres à être réalisables. Cela le rend plus adaptable à divers cas d'utilisation tout en restant plus maintenable et extensible.

<div data -full-width = "false"> <gigust> <img src = "../. gitbook / actifs / agentflowv2 / motifs.png" alt = ""> <figcaption> </gigcaption> </ figure> </div>

## Différence entre AgentFlow et Plateforme d'automatisation

L'une des questions les plus posées: quelle est la différence entre AgentFlow et les plates-formes d'automatisation comme N8N, Make ou Zapier?

### 💬 ** Communication d'agent à agent **

La communication multimodale entre les agents est prise en charge. Un agent de superviseur peut formuler et déléguer des tâches à plusieurs agents de travailleurs, avec des sorties des agents des travailleurs retournés par la suite au superviseur.

À chaque étape, les agents ont accès à l'historique complet de la conversation, permettant au superviseur de déterminer la tâche suivante et les agents des travailleurs pour interpréter la tâche, sélectionner les outils appropriés et exécuter les actions en conséquence.

Cette architecture permet ** la collaboration, la délégation et la gestion des tâches partagées ** sur plusieurs agents, ces capacités ne sont généralement pas offertes par les outils d'automatisation traditionnels.

<gigne> <image> <source srcset = "../. GitBook / Assets / Captures 2025-05-16 153946.png" Media = "(préfère-Color-Scheme: Dark)"> <img Src = "../. (1) (1) (1) (1) .png "alt =" "> </gital> <figcaption> </gigcaption> </stigne>

### 🙋‍ Human dans la boucle

L'exécution est interrompue en attendant l'entrée humaine, sans bloquer le thread en cours d'exécution. Chaque point de contrôle est enregistré, permettant au flux de travail de reprendre à partir du même point même après un redémarrage de l'application.

L'utilisation de points de contrôle permet ** les agents de longue durée et avec état **.

Les agents peuvent également être configurés pour ** demander l'autorisation avant d'exécuter des outils **, similaire à la façon dont Claude demande l'approbation de l'utilisateur avant d'utiliser les outils MCP. Cela permet d'empêcher l'exécution autonome d'actions sensibles sans l'approbation explicite de l'utilisateur.

<gigne> <image> <source srcset = "../. GitBook / Assets / Captures 2025-05-16 154908.png" Media = "(préfers-Color-Scheme: Dark)"> <img src = "../. (1) (1) (1) (1) (1) .png "alt =" "> </ picture> <figcaption> </gigcaption> </gigust>

### 📖 État partagé

L'état partagé permet l'échange de données entre les agents, particulièrement utile pour passer des données entre les branches ou les étapes non adjacentes d'un flux. Se référer à[#understanding-flow-state](agentflowv2.md#understanding-flow-state "mention")

### ⚡ Streaming

Prend en charge les événements de serveur (SSE) pour le streaming en temps réel de réponses LLM ou d'agent. Le streaming permet également aux mises à jour de l'abonnement à l'exécution au fur et à mesure que le flux de travail progresse.

<gigne> <img src = "../. GitBook / Assets / Longgif.gif" alt = ""> <Figcaption> </gigcaption> </gigust>

### 🌐 outils MCP

Alors que les plates-formes d'automatisation traditionnelles présentent souvent de vastes bibliothèques d'intégrations prédéfinies, AgentFlow permet à MCP ([Model Context Protocol](https://github.com/modelcontextprotocol)) outils à connecter dans le cadre du flux de travail, plutôt que de fonctionner uniquement en tant qu'outils d'agent.

Les MCP personnalisés peuvent également être créés indépendamment, sans dépendre des intégrations fournies par la plate-forme. MCP est largement considéré comme une norme de l'industrie et est généralement soutenu et maintenu par les prestataires officiels. Par exemple, le GitHub MCP est développé et maintenu par l'équipe GitHub, avec un soutien similaire fourni pour Atlassian Jira, Brave Search, et autres.

<gigne> <image> <source srcset = "../. GitBook / Assets / Captures 2025-05-16 160752.png" Media = "(préfère-Color-Scheme: Dark)"> <img Src = "../. (1) .png "alt =" "> </ picture> <gigon> </figcaption> </gigne>

## Référence du nœud AgentFlow V2

Cette section fournit une référence détaillée pour chaque nœud disponible, décrivant son objectif spécifique, les paramètres de configuration des clés, les entrées attendues, les sorties générées et son rôle dans l'architecture AgentFlow V2.

<gigne> <mage> <source srcset = "../. GitBook / Assets / Agentflowv2 / DarkMode / V2-01-D (1) .png" Media = "(préfers-Color-Scheme: Dark)"> <img src = "../. GitBook / Assets / Agentflowv2 / V2-all-Node.png" alt = ""> </ picture> <figction> </gigcaption> </figucion>

***

### ** 1. Démarrer le nœud **

Le point d'entrée désigné pour lancer n'importe quelle exécution de workflow AgentFlow V2. Chaque flux doit commencer par ce nœud.

* ** Fonctionnalité: ** Définit comment le flux de travail est déclenché et configure les conditions initiales. Il peut accepter les entrées directement à partir de l'interface de chat ou via un formulaire personnalisable présenté à l'utilisateur. Il permet également l'initialisation de`Flow State`Variables au début de l'exécution et peut gérer la façon dont la mémoire de conversation est gérée pour l'exécution.
* ** Paramètres de configuration **
  * ** Type d'entrée **: détermine comment l'exécution du flux de travail est initiée, soit par`Chat Input`de l'utilisateur ou via un soumis`Form Input`.
    * ** Titre du formulaire, description du formulaire, types d'entrée de formulaire **: If`Form Input`est sélectionné, ces champs configurent l'apparence du formulaire présenté à l'utilisateur, permettant divers types de champs de saisie avec des étiquettes définies et des noms de variables.
  * ** Mémoire éphémère **: si elle est activée, demande au workflow de commencer l'exécution sans considérer les messages passés du thread de conversation, en commençant efficacement par une ardoise de mémoire propre.
  * ** État de flux **: définit l'ensemble complet des paires de valeurs clés initiales pour l'état d'exécution du workflow`$flow.state`. Toutes les clés d'état qui seront utilisées ou mises à jour par les nœuds suivantes doivent être déclarées et initialisées ici.
* ** Entrées: ** Reçoit les données initiales qui déclenchent le workflow, qui sera soit un message de chat, soit les données soumises via un formulaire.
* ** Sorties: ** Fournit une seule ancre de sortie pour se connecter au premier nœud opérationnel, passant les données d'entrée initiales et l'état de flux initialisé.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-02-d.png" media = "(préfère-Color-Scheme: Dark)"> <img src = "../. width = "343"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 2. Node LLM **

Fournit un accès direct à un modèle de grande langue (LLM) configuré pour exécuter des tâches AI, permettant au workflow d'effectuer une extraction structurée de données si nécessaire.

* ** Fonctionnalité: ** Ce nœud envoie des demandes à un LLM basé sur des instructions (messages) et un contexte fourni. Il peut être utilisé pour la génération de texte, le résumé, la traduction, l'analyse, la réponse aux questions et la génération de sortie JSON structurée selon un schéma défini. Il a accès à la mémoire pour le thread de conversation et peut lire / écrire`Flow State`.
* ** Paramètres de configuration **
  * ** Modèle **: Spécifie le modèle AI à partir d'un service choisi - par exemple, GPT-4O d'OpenAI ou Google Gemini.
  * ** Messages **: Définissez l'entrée conversationnelle pour le LLM, en la structurant comme une séquence de rôles - système, utilisateur, assistant, développeur - pour guider la réponse de l'IA. Les données dynamiques peuvent être insérées en utilisant`{{ variable }}`.
  * ** Mémoire **: Si vous activez, détermine si le LLM doit considérer l'historique du thread de conversation actuel lors de la génération de sa réponse.
    * ** Type de mémoire, taille de la fenêtre, limite de jeton maximale **: Si la mémoire est utilisée, ces paramètres affinent comment l'historique de la conversation est géré et présenté au LLM - par exemple, s'il faut inclure tous les messages, seulement une fenêtre récente de virages ou une version résumée.
    * ** Message d'entrée **: Spécifie la variable ou le texte qui sera annexé comme le message utilisateur le plus récent à la fin du contexte de conversation existant - y compris le contexte initial et la mémoire - avant d'être traités par le LLM / Agent.
  * ** Retour Response As **: Configure comment la sortie de LLM est classée - comme un`User Message`ou`Assistant Message`- qui peut influencer la façon dont il est géré par les systèmes de mémoire ou la journalisation ultérieurs.
  * ** Sortie structurée JSON **: Demande au LLM de formater sa sortie en fonction d'un schéma JSON spécifique - y compris des clés, des types de données et des descriptions - garantissant des données prévisibles et lisibles par la machine.
  * ** Mettre à jour l'état de flux **: permet au nœud de modifier l'état d'exécution du workflow`$flow.state`Pendant l'exécution en mettant à jour les clés prédéfinies. Cela permet, par exemple, de stocker la sortie de ce nœud LLM sous une telle clé, ce qui le rend accessible aux nœuds suivants.
* ** Entrées: ** Ce nœud utilise des données du déclencheur initial du workflow ou des sorties des nœuds précédents, incorporant ces données dans le`Messages`ou`Input Message`champs. Il peut également récupérer des valeurs de`$flow.state`Lorsque les variables d'entrée le font référence.
* ** Sorties: ** produit la réponse de LLM, qui sera soit du texte brut, soit un objet JSON structuré. La catégorisation de cette sortie - en tant qu'utilisateur ou assistant - est déterminée par le`Return Response`paramètre.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-03-d.png" media = "(préfère-Color-Scheme: Dark)"> <img src = "../. width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 3. Node d'agent **

Représente une entité d'IA autonome capable de raisonner, de planifier et d'interagir avec des outils ou des sources de connaissances pour atteindre un objectif donné.

* ** Fonctionnalité: ** Ce nœud utilise un LLM pour décider dynamiquement d'une séquence d'actions. En fonction de l'objectif de l'utilisateur - fourni via des messages / entrées - il peut choisir d'utiliser des outils disponibles ou des magasins de documents de requête pour recueillir des informations ou effectuer des actions. Il gère son propre cycle de raisonnement et peut utiliser la mémoire pour le fil de conversation et`Flow State`. Convient aux tâches nécessitant un raisonnement en plusieurs étapes ou interagissant dynamiquement avec des systèmes ou des outils externes.
* ** Paramètres de configuration **
  * ** Modèle **: Spécifie le modèle AI à partir d'un service choisi - par exemple, GPT-4O ou Google Gemini d'OpenAI - qui conduira les processus de raisonnement et de prise de décision de l'agent.
  * ** Messages **: Définissez l'entrée conversationnelle initiale, l'objectif ou le contexte pour l'agent, en le structurant comme une séquence de rôles - système, utilisateur, assistant, développeur - pour guider la compréhension de l'agent et les actions ultérieures. Les données dynamiques peuvent être insérées en utilisant`{{ variable }}`.
  * ** Outils **: Spécifiez quels outils fluide prédéfinis l'agent est autorisé à utiliser pour atteindre ses objectifs.
    * Pour chaque outil sélectionné, un ** facultatif ** nécessite un indicateur d'entrée humain ** indique si l'opération de l'outil peut elle-même s'arrêter pour demander une intervention humaine.
  * ** Magasins de connaissances / documents **: Configurer l'accès aux informations dans les magasins de documents gérés par flux.
    * ** Magasin de documents **: Choisissez une boutique de documents préconfigurée à partir de laquelle l'agent peut récupérer des informations. Ces magasins doivent être mis en place et peuplés à l'avance.
    * ** Décrire les connaissances **: Fournir une description du langage naturel du contenu et du but de ce magasin de documents. Cette description guide l'agent pour comprendre quel type d'informations le magasin contient et quand il serait approprié de les interroger.
  * ** Connaissances / Vector intégrés **: Configurez l'accès aux magasins de vecteurs externes et préexistants comme sources de connaissances supplémentaires pour l'agent.
    * ** Vector Store **: Sélectionne la base de données vectorielle spécifique et préconfigurée que l'agent peut interroger.
    * ** Modèle d'intégration **: Spécifie le modèle d'intégration associé au magasin vectoriel sélectionné, assurant la compatibilité des requêtes.
    * ** Nom de la connaissance **: attribue un court nom descriptif à cette source de connaissances basée sur un vecteur, que l'agent peut utiliser pour référence.
    * ** Décrire les connaissances **: Fournir une description du langage naturel du contenu et du but de ce magasin vectoriel, en guidant l'agent sur quand et comment utiliser cette source de connaissances spécifique.
    * ** RETOUR DOCUMENTS SOURCES **: Si vous êtes activé, demande à l'agent d'inclure des informations sur les documents source avec les données récupérées du magasin vectoriel.
  * ** Mémoire **: Si vous êtes activé, détermine si l'agent doit considérer l'historique du thread de conversation actuel lors de la prise de décisions et de la génération de réponses.
    * ** Type de mémoire, taille de la fenêtre, limite de jeton maximale **: Si la mémoire est utilisée, ces paramètres affinent comment l'historique de la conversation est géré et présenté à l'agent - par exemple, que ce soit pour inclure tous les messages, seulement une fenêtre récente ou une version résumée.
    * ** Message d'entrée **: Spécifie la variable ou le texte qui sera annexé comme le message utilisateur le plus récent à la fin du contexte de conversation existant - y compris le contexte initial et la mémoire - avant d'être traités par le LLM / Agent.
  * ** RETOUR RÉPONSE **: Configure comment la sortie ou le message final de l'agent est classé - en tant que message utilisateur ou message assistant - qui peut influencer la façon dont il est géré par des systèmes de mémoire ultérieurs ou la journalisation.
  * ** Mettre à jour l'état de flux **: permet au nœud de modifier l'état d'exécution du workflow`$flow.state`Pendant l'exécution en mettant à jour les clés prédéfinies. Cela permet, par exemple, de stocker la sortie de ce nœud d'agent sous une telle clé, ce qui le rend accessible aux nœuds suivants.
* ** Entrées: ** Ce nœud utilise les données du déclencheur initial du workflow ou des sorties des nœuds précédents, souvent incorporés dans le`Messages`ou`Input Message`champs. Il accède aux outils configurés et aux sources de connaissances selon les besoins.
* ** Sorties: ** produit le résultat ou la réponse finale générée par l'agent une fois qu'il a terminé son raisonnement, sa planification et toute interaction avec des outils ou des sources de connaissances.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-04-d.png" media = "(préfère-Color-Scheme: Dark)"> <img src = "../. width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 4. Node d'outil **

Fournit un mécanisme pour exécuter directement et de manière déterministe un outil fluide spécifique et prédéfini dans la séquence de workflow. Contrairement au nœud d'agent, où le LLM choisit dynamiquement un outil basé sur le raisonnement, le nœud d'outil exécute exactement l'outil sélectionné par le concepteur de workflow pendant la configuration.

* ** Fonctionnalité: ** Ce nœud est utilisé lorsque le workflow nécessite l'exécution d'une capacité spécifique connue à un point défini, avec des entrées facilement disponibles. Il garantit une action déterministe sans impliquer le raisonnement LLM pour la sélection des outils.
* ** Comment ça marche **
  1. ** TRANGERS: ** Lorsque l'exécution du workflow atteint un nœud d'outil, il s'active.
  2. ** Identification de l'outil: ** Il identifie l'outil de flux spécifique sélectionné dans sa configuration.
  3. ** Résolution de l'argument d'entrée: ** Il examine la configuration des arguments d'entrée de l'outil. Pour chaque paramètre d'entrée requis de l'outil sélectionné.
  4. ** Exécution: ** Il invoque le code sous-jacent ou l'appel API associé à l'outil Flowise sélectionné, passant les arguments d'entrée résolus.
  5. ** Génération de sortie: ** Il reçoit le résultat renvoyé par l'exécution de l'outil.
  6. ** Propagation de sortie: ** Il rend ce résultat disponible via son ancre de sortie pour les nœuds suivants.
* ** Paramètres de configuration **
  * ** Sélection d'outils **: Choisissez l'outil Flowise spécifique et enregistré que ce nœud exécutera à partir d'une liste déroulante.
  * ** Arguments d'entrée **: Définissez comment les données de votre flux de travail sont fournies à l'outil sélectionné. Cette section s'adapte dynamiquement en fonction de l'outil choisi, présentant ses paramètres d'entrée spécifiques:
    * ** Nom de l'argument de la carte **: Pour chaque entrée, l'outil sélectionné nécessite (par exemple,`input`Pour une calculatrice), ce champ affichera le nom du paramètre attendu tel que défini par l'outil lui-même.
    * ** Fournir une valeur d'argument **: Définissez la valeur de ce paramètre correspondant, en utilisant une variable dynamique comme`{{ previousNode.output }}`, `{{ $flow.state.someKey }}`, ou en entrant un texte statique.
  * ** Mettre à jour l'état de flux **: permet au nœud de modifier l'état d'exécution du workflow`$flow.state`Pendant l'exécution en mettant à jour les clés prédéfinies. Cela permet, par exemple, de stocker la sortie de cet outil sous une telle clé, ce qui le rend accessible aux nœuds suivants.
* ** Entrées: ** reçoit les données nécessaires pour les arguments de l'outil via le`Input Arguments`mappage, valeurs d'approvisionnement à partir des sorties de nœud précédentes,`$flow.state`ou configurations statiques.
* ** Sorties: ** produit la sortie brute générée par l'outil exécuté - par exemple, une chaîne JSON à partir d'une API, un résultat de texte ou une valeur numérique.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-16-d.png" media = "(préfers-color-scheme: dark)"> <img src = "../. gitbook / actifs / agentflowv2 / v2-05.png" alt = "" " width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 5. Retriever Node **

Effectue une récupération d'informations ciblée à partir des magasins de documents configurés.

* ** Fonctionnalité: ** Ce nœud interroge un ou plusieurs magasins de documents spécifiés, récupérant des morceaux de document pertinents basés sur la similitude sémantique. C'est une alternative ciblée à l'utilisation d'un nœud d'agent lorsque la seule action requise est la récupération et la sélection des outils dynamiques par un LLM n'est pas nécessaire.
* ** Paramètres de configuration **
  * ** Magasins de connaissances / documents **: Spécifiez quel (s) magasin de documents préconfigurés et peuplés, ce nœud doit interroger pour trouver des informations pertinentes.
  * ** Retriever Query **: Définissez la requête texte qui sera utilisée pour rechercher les magasins de documents sélectionnés. Les données dynamiques peuvent être insérées en utilisant`{{ variables }}`.
  * ** Format de sortie **: Choisissez comment les informations récupérées doivent être présentées - soit comme simple`Text`ou comme`Text with Metadata`, qui peut inclure des détails tels que les noms de documents source ou les emplacements.
  * ** Mettre à jour l'état de flux **: permet au nœud de modifier l'état d'exécution du workflow`$flow.state`Pendant l'exécution en mettant à jour les clés prédéfinies. Cela permet, par exemple, de stocker la sortie de ce nœud Retriever sous une telle clé, ce qui le rend accessible aux nœuds suivants.
* ** Entrées: ** nécessite une chaîne de requête - souvent fournie comme une variable à partir d'une étape précédente ou d'une entrée utilisateur - et accède aux magasins de documents sélectionnés pour plus d'informations.
* ** sorties: ** produit les morceaux de document récupérés de la base de connaissances, formaté selon les choisis`Output Format`.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-06-d.png" media = "(préfère-Color-Scheme: Dark)"> <img src = "../. GitBook / Assets / Agentflowv2 / V2-06.png" alt = "" width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### 6. nœud http

Facilite la communication directe avec les services Web externes et les API via le protocole de transfert hypertexte (HTTP).

* ** Fonctionnalité: ** Ce nœud permet au workflow d'interagir avec tout système externe accessible via HTTP. Il peut envoyer différents types de demandes (obtenir, publier, mettre, supprimer, patcher) à une URL spécifiée, permettant une intégration avec des API tierces, récupérer des données à partir de ressources Web ou déclencher des webhooks externes. Le nœud prend en charge la configuration des méthodes d'authentification, des en-têtes personnalisés, des paramètres de requête et différents types de corps de demande pour répondre aux diverses exigences d'API.
* ** Paramètres de configuration **
  * ** HTTP Idedential **: Sélectionnez éventuellement des informations d'identification préconfigurées - telles que l'authentification de base, le jeton de support ou la clé API - pour authentifier les demandes au service cible.
  * ** Méthode de la demande **: Spécifiez la méthode HTTP à utiliser pour la demande - par exemple,`GET`, `POST`, `PUT`, `DELETE`, `PATCH`.
  * ** URL cible **: Définissez l'URL complète du point de terminaison externe auquel la demande sera envoyée.
  * ** En-têtes de demande **: Définissez tous les en-têtes HTTP nécessaires en paires de valeurs clés à inclure dans la demande.
  * ** Paramètres de requête URL **: Définissez les paires de valeurs clés qui seront annexées à l'URL en tant que paramètres de requête.
  * ** Type de corps de demande **: Choisissez le format de la charge utile de demande si l'envoi de données - les options incluent`JSON`, `Raw text`, `Form Data`, ou`x-www-form-urlencoded`.
  * ** Demande Body **: Fournissez la charge utile de données réelle pour des méthodes comme le poste ou le put. Le format doit correspondre au sélectionné`Body Type`et les données dynamiques peuvent être insérées en utilisant`{{ variables }}`.
  * ** Type de réponse **: Spécifiez comment le flux de travail doit interpréter la réponse reçue du serveur - les options incluent`JSON`, `Text`, `Array Buffer`, ou`Base64`pour les données binaires.
* ** Entrées: ** reçoit des données de configuration telles que l'URL, la méthode, les en-têtes et le corps, incorporant souvent des valeurs dynamiques à partir d'étapes de work`$flow.state`.
* ** Sorties: ** produit la réponse reçue du serveur externe, analysé selon le sélectionné`Response Type`.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-07-d.png" media = "(préfère-Color-Scheme: Dark)"> <img src = "../. Gitbook / Assets / Agentflowv2 / V2-07.png" alt = "" width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 7. Node de condition **

Implémente la logique de ramification déterministe dans le flux de travail sur la base des règles définies.

* ** Fonctionnalité: ** Ce nœud agit comme un point de décision, évaluant une ou plusieurs conditions spécifiées pour diriger le flux de travail dans différents chemins. Il compare les valeurs d'entrée - qui peuvent être des chaînes, des nombres ou des booléens - en utilisant une variété d'opérateurs logiques, tels que les égaux, contient, supérieur ou vide. Sur la base de la question de savoir si ces conditions évaluent en vrai ou fausse, l'exécution du flux de travail passe le long de l'une des branches de sortie distinctes connectées à ce nœud.
* ** Paramètres de configuration **
  * ** Conditions **: Configurez l'ensemble des règles logiques que le nœud évaluera.
    * ** Type **: Spécifiez le type de données comparées pour cette règle -`String`, `Number`, ou`Boolean`.
    * ** Valeur 1 **: Définissez la première valeur pour la comparaison. Les données dynamiques peuvent être insérées en utilisant`{{ variables }}`.
    * ** Opération **: Sélectionnez l'opérateur logique à appliquer entre la valeur 1 et la valeur 2 - par exemple,`equal`, `notEqual`, `contains`, `larger`, `isEmpty`.
    * ** Valeur 2 **: Définissez la deuxième valeur pour la comparaison, si nécessaire par l'opération choisie. Les données dynamiques peuvent également être insérées ici en utilisant`{{ variables }}`.
* ** Entrées: ** nécessite les données pour`Value 1`et`Value 2`pour chaque condition évaluée. Ces valeurs sont fournies à partir des sorties de nœud précédentes ou récupérées à partir de`$flow.state`.
* ** Sorties: ** fournit plusieurs ancres de sortie, correspondant au résultat booléen (vrai / faux) des conditions évaluées. Le flux de travail continue le long du chemin spécifique connecté à l'ancre de sortie qui correspond au résultat.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-08-d.png" media = "(préfère-Color-Scheme: Dark)"> <img src = "../. width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 8. Node d'agent de condition **

Fournit une ramification dynamique basée sur l'IA basée sur les instructions et le contexte du langage naturel.

* ** Fonctionnalité: ** Ce nœud utilise un modèle grand langage (LLM) pour acheminer le workflow. Les analyses des analyses ont fourni des données d'entrée par rapport à un ensemble de "scénarios" définis par l'utilisateur - résultats ou catégories potentiels - guidés par des "instructions" de langage naturel de haut niveau qui définissent la tâche de prise de décision. Le LLM détermine ensuite quel scénario correspond le mieux au contexte d'entrée actuel. Sur la base de cette classification dirigée par l'IA, l'exécution du flux de travail réduit le chemin de sortie spécifique correspondant au scénario choisi. Ce nœud est particulièrement utile pour les tâches telles que la reconnaissance de l'intention des utilisateurs, le routage conditionnel complexe ou la prise de décision situationnelle nuancée où des règles simples et prédéfinies - comme dans le nœud de condition - sont insuffisantes.
* ** Paramètres de configuration **
  * ** Modèle **: Spécifie le modèle AI à partir d'un service choisi qui effectuera l'analyse et la classification des scénarios.
  * ** Instructions **: Définissez l'objectif ou la tâche globale du LLM en langage naturel - par exemple, "Déterminez si la demande de l'utilisateur concerne les ventes, le support ou la demande générale."
  * ** Entrée **: spécifiez les données, souvent du texte à partir d'une étape précédente ou d'une entrée utilisateur, en utilisant`{{ variables }}`, que le LLM analysera pour prendre sa décision de routage.
  * ** Scénarios **: Configurer un tableau définissant les résultats possibles ou les chemins distincts que le flux de travail peut prendre. Chaque scénario est décrit dans le langage naturel - par exemple, «Enquête sur les ventes», «demande de support», «question générale» - et chacune correspond à une ancre de sortie unique sur le nœud.
* ** Entrées: ** nécessite le`Input`Données pour l'analyse et le`Instructions`Pour guider le LLM.
* ** sorties: ** fournit plusieurs ancres de sortie, une pour chaque définie`Scenario`. Le workflow continue le long du chemin spécifique connecté à l'ancre de sortie que le LLM détermine le meilleur correspond à l'entrée.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-09-d.png" media = "(préfère-Color-Scheme: Dark)"> <img src = "../. width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 9. Node d'itération **

Exécute un "sous-flux" défini - une séquence de nœuds imbriqués - pour chaque élément d'un tableau d'entrée, implémentant une boucle "for-een".

* ** Fonctionnalité: ** Ce nœud est conçu pour le traitement des collections de données. Il prend un tableau, fourni directement ou référencé via une variable, comme entrée. Pour chaque élément individuel à l'intérieur de ce tableau, le nœud d'itération exécute séquentiellement la séquence d'autres nœuds qui sont visuellement placés à l'intérieur de ses limites sur la toile.
* ** Paramètres de configuration **
  * ** Entrée du tableau **: Spécifie le tableau d'entrée que le nœud iratera. Ceci est fourni en faisant référence à une variable qui contient un tableau à partir de la sortie d'un nœud précédent ou du`$flow.state`- par exemple,`{{ $flow.state.itemList }}`.
* ** Entrées: ** nécessite un tableau à fournir à son`Array Input`paramètre.
* ** Sorties: ** Fournit une seule anage de sortie qui ne devient active qu'après que le sous-flux imbriqué a terminé l'exécution pour tous les éléments du tableau d'entrée. Les données transmises par cette sortie peuvent inclure des résultats agrégés ou l'état final des variables modifié dans la boucle, selon la conception du sous-flux. Les nœuds placés à l'intérieur du bloc d'itération ont leurs propres connexions d'entrée et de sortie distinctes qui définissent la séquence d'opérations pour chaque élément.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-10-d (1) .png" media = "(prefers-color-scheme: sombre)"> <img src = "../. gitbook / actets / agentflowv2 / v2-10.png" alt = "" " width = "563"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 10. Node de boucle **

Redirige explicitement l'exécution du workflow vers un nœud précédemment exécuté.

* ** Fonctionnalité: ** Ce nœud permet la création de cycles ou de tentatives itératives dans un workflow. Lorsque le flux d'exécution atteint le nœud de boucle, il n'atteint pas un nouveau nœud; Au lieu de cela, il "remonte" à un nœud cible spécifié qui a déjà été exécuté plus tôt dans l'exécution actuelle du flux de travail. Cette action provoque la réexécution de ce nœud cible et de tous les nœuds suivants dans cette partie de l'écoulement.
* ** Paramètres de configuration **
  * ** Loop Retour à **: Sélectionne l'ID unique d'un nœud précédemment exécuté dans le flux de travail actuel auquel l'exécution doit retourner.
  * ** MAX LOOP COUNT **: Définit le nombre maximal de fois que cette opération de boucle peut être effectuée dans une seule exécution de workflow, sauvegarde contre les cycles infinis. La valeur par défaut est 5.
* ** Entrées: ** Reçoit le signal d'exécution pour activer. Il suit en interne le nombre de fois que la boucle s'est produite pour l'exécution actuelle.
* ** Sorties: ** Ce nœud n'a pas d'ancre de sortie standard à pointant, car sa fonction principale est de rediriger le flux d'exécution vers l'arrière vers le`Loop Back To`Node cible, d'où le flux de travail continue alors.

<gigne> <mage> <source srcset = "../. GitBook / Assets / Agentflowv2 / DarkMode / V2-11-D.png" Media = "(préfère-Color-Scheme: Dark)"> <img src = "../. GitBook / Assets / Agentflowv2 / V2-11.png" Alt = "" width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 11. Node d'entrée humain **

Utilise l'exécution du workflow pour demander des entrées, une approbation ou des commentaires explicites d'un utilisateur humain - un composant clé pour les processus humains dans la boucle (HITL).

* ** Fonctionnalité: ** Ce nœud arrête la progression automatisée du flux de travail et présente des informations ou une question à un utilisateur humain, via l'interface de chat. Le contenu affiché à l'utilisateur peut être un texte statique prédéfini ou généré dynamiquement par un LLM basé sur le contexte de workflow actuel. L'utilisateur reçoit des choix d'action distincts - par exemple, «procéder», «rejeter» - et, s'il est activé, un champ pour fournir des commentaires textuels. Une fois que l'utilisateur fait une sélection et soumet sa réponse, le flux de travail reprend l'exécution le long du chemin de sortie spécifique correspondant à son action choisie.
* ** Paramètres de configuration **
  * ** Type de description **: détermine comment le message ou la question présentée à l'utilisateur est généré - soit`Fixed`(texte statique) ou`Dynamic`(généré par un LLM).
    * ** Si le type de description est`Fixed`**
      * ** Description **: Ce champ contient le texte exact à afficher à l'utilisateur. Il prend en charge l'insertion de données dynamiques en utilisant`{{ variables }}`
    * **Si`Description Type`est`Dynamic`**
      * ** Modèle **: sélectionne le modèle AI dans un service choisi qui générera le message orienté utilisateur.
      * ** invite **: fournit les instructions ou l'invite pour le LLM sélectionné pour générer le message affiché à l'utilisateur.
  * ** Feedback: ** Si activé, l'utilisateur sera invité avec une fenêtre de rétroaction pour laisser ses commentaires, et ces commentaires seront annexés à la sortie du nœud.
* ** Entrées: ** Reçoit le signal d'exécution pour suspendre le workflow. Il peut utiliser les données des étapes précédentes ou`$flow.state`à travers des variables dans le`Description`ou`Prompt`champs s'ils sont configurés pour le contenu dynamique.
* ** Sorties: ** Fournit deux ancres de sortie, chacune correspondant à une action utilisateur distincte - une ancre pour "procéder" et une autre pour "rejeter". Le flux de travail continue le long du chemin connecté à l'ancre correspondant à la sélection de l'utilisateur.

<gigne> <mage> <source srcset = "../. GitBook / Assets / Agentflowv2 / DarkMode / V2-12-D.png" Media = "(Prefers-Color-Scheme: Dark)"> <img src = "../. GitBook / Assets / Agentflowv2 / V2-12.png" Alt = "" width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 12. Node de réponse directe **

Envoie un message final à l'utilisateur et termine le chemin d'exécution actuel.

* ** Fonctionnalité: ** Ce nœud sert de point de terminaison pour une branche spécifique ou l'intégralité d'un workflow. Il prend un message configuré - qui peut être du texte statique ou du contenu dynamique d'une variable - et le livre directement à l'utilisateur final via l'interface de chat. Lors de l'envoi de ce message, l'exécution le long de ce chemin particulier du workflow conclut; Aucun autre nœud connecté à partir de ce point ne sera traité.
* ** Paramètres de configuration **
  * ** Message **: Définissez le texte ou la variable`{{ variable }}`Cela contient le contenu à envoyer comme réponse finale à l'utilisateur.
* ** Entrées: ** reçoit le contenu du message, qui provient de la sortie d'un nœud précédent ou d'une valeur stockée dans`$flow.state`.
* ** Sorties: ** Ce nœud n'a pas d'ancres de sortie, car sa fonction est de terminer le chemin d'exécution après avoir envoyé la réponse.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-13-d.png" media = "(préfers-color-scheme: dark)"> <img src = "../. gitbook / actifs / agentflowv2 / v2-13.png" alt = "" " width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 13. Node de fonction personnalisé **

Fournit un mécanisme pour exécuter le code JavaScript côté serveur personnalisé dans le workflow.

* ** Fonctionnalité: ** Ce nœud permet d'écrire et d'exécuter des extraits arbitraires JavaScript, offrant un moyen efficace d'implémenter des transformations de données complexes, une logique métier sur mesure ou des interactions avec des ressources non directement prises en charge par d'autres nœuds standard. Le code exécuté fonctionne dans un environnement Node.js et a des moyens spécifiques d'accéder aux données:
  * ** Variables d'entrée: ** Valeurs passées via le`Input Variables`La configuration est accessible dans la fonction, généralement préfixée avec`---
description: Learn how to build multi-agents system using Agentflow V2, written by @toi500
---

# Agentflow v2

Ce guide explore l'architecture AgentFlow V2, détaillant ses concepts principaux, ses cas d'utilisation, son état de flux et ses références de nœuds complets.

{% hint style = "avertissement"%}
** Avis de non-responsabilité: ** Cette documentation décrit AgentFlow V2 à sa version officielle actuelle. Les fonctionnalités, les fonctionnalités et les paramètres de nœud sont soumis à un changement dans les futures mises à jour et versions de Flowise. Veuillez vous référer aux dernières notes de publication officielle ou à des informations sur l'application pour les détails les plus récents.
{% EndHint%}

{% embed url = "https://youtu.be/-h4wquzrhhi?si=jkhuefiw06ao6ge"%}

## Concept de base

AgentFlow V2 représente une évolution architecturale significative, introduisant un nouveau paradigme en flux qui se concentre sur une orchestration explicite du flux de travail et une flexibilité accrue. Contrairement à la dépendance principale de V1 sur les cadres externes pour sa logique de graphique d'agent de base, V2 déplace l'attention de la conception de l'ensemble du flux de travail en utilisant un ensemble granulaire de nœuds autonomes spécialisés développés nativement en tant que composants coulissants principaux.

Dans cette architecture V2, chaque nœud fonctionne comme une unité indépendante, exécutant une opération discrète en fonction de sa conception et de sa configuration spécifiques. Les connexions visuelles entre les nœuds de la canevas définissent explicitement le chemin de travail et la séquence de contrôle du workflow, les données peuvent être transmises entre les nœuds en faisant référence aux sorties de tout nœud précédemment exécuté dans le flux actuel, et l'état de flux fournit un mécanisme explicite pour gérer et partager des données tout au long du flux de travail.

L'architecture V2 met en œuvre un système complet de la dépendance aux nœuds et de la file d'attente d'exécution qui respecte précisément ces voies définies tout en maintenant une séparation claire entre les composants, permettant aux flux de travail de devenir à la fois plus sophistiqués et plus faciles à concevoir. Cela permet aux modèles complexes comme les boucles, la ramification conditionnelle, les interactions humaines dans la boucle et d'autres à être réalisables. Cela le rend plus adaptable à divers cas d'utilisation tout en restant plus maintenable et extensible.

<div data -full-width = "false"> <gigust> <img src = "../. gitbook / actifs / agentflowv2 / motifs.png" alt = ""> <figcaption> </gigcaption> </ figure> </div>

## Différence entre AgentFlow et Plateforme d'automatisation

L'une des questions les plus posées: quelle est la différence entre AgentFlow et les plates-formes d'automatisation comme N8N, Make ou Zapier?

### 💬 ** Communication d'agent à agent **

La communication multimodale entre les agents est prise en charge. Un agent de superviseur peut formuler et déléguer des tâches à plusieurs agents de travailleurs, avec des sorties des agents des travailleurs retournés par la suite au superviseur.

À chaque étape, les agents ont accès à l'historique complet de la conversation, permettant au superviseur de déterminer la tâche suivante et les agents des travailleurs pour interpréter la tâche, sélectionner les outils appropriés et exécuter les actions en conséquence.

Cette architecture permet ** la collaboration, la délégation et la gestion des tâches partagées ** sur plusieurs agents, ces capacités ne sont généralement pas offertes par les outils d'automatisation traditionnels.

<gigne> <image> <source srcset = "../. GitBook / Assets / Captures 2025-05-16 153946.png" Media = "(préfère-Color-Scheme: Dark)"> <img Src = "../. (1) (1) (1) (1) .png "alt =" "> </gital> <figcaption> </gigcaption> </stigne>

### 🙋‍ Human dans la boucle

L'exécution est interrompue en attendant l'entrée humaine, sans bloquer le thread en cours d'exécution. Chaque point de contrôle est enregistré, permettant au flux de travail de reprendre à partir du même point même après un redémarrage de l'application.

L'utilisation de points de contrôle permet ** les agents de longue durée et avec état **.

Les agents peuvent également être configurés pour ** demander l'autorisation avant d'exécuter des outils **, similaire à la façon dont Claude demande l'approbation de l'utilisateur avant d'utiliser les outils MCP. Cela permet d'empêcher l'exécution autonome d'actions sensibles sans l'approbation explicite de l'utilisateur.

<gigne> <image> <source srcset = "../. GitBook / Assets / Captures 2025-05-16 154908.png" Media = "(préfers-Color-Scheme: Dark)"> <img src = "../. (1) (1) (1) (1) (1) .png "alt =" "> </ picture> <figcaption> </gigcaption> </gigust>

### 📖 État partagé

L'état partagé permet l'échange de données entre les agents, particulièrement utile pour passer des données entre les branches ou les étapes non adjacentes d'un flux. Se référer à[#understanding-flow-state](agentflowv2.md#understanding-flow-state "mention")

### ⚡ Streaming

Prend en charge les événements de serveur (SSE) pour le streaming en temps réel de réponses LLM ou d'agent. Le streaming permet également aux mises à jour de l'abonnement à l'exécution au fur et à mesure que le flux de travail progresse.

<gigne> <img src = "../. GitBook / Assets / Longgif.gif" alt = ""> <Figcaption> </gigcaption> </gigust>

### 🌐 outils MCP

Alors que les plates-formes d'automatisation traditionnelles présentent souvent de vastes bibliothèques d'intégrations prédéfinies, AgentFlow permet à MCP ([Model Context Protocol](https://github.com/modelcontextprotocol)) outils à connecter dans le cadre du flux de travail, plutôt que de fonctionner uniquement en tant qu'outils d'agent.

Les MCP personnalisés peuvent également être créés indépendamment, sans dépendre des intégrations fournies par la plate-forme. MCP est largement considéré comme une norme de l'industrie et est généralement soutenu et maintenu par les prestataires officiels. Par exemple, le GitHub MCP est développé et maintenu par l'équipe GitHub, avec un soutien similaire fourni pour Atlassian Jira, Brave Search, et autres.

<gigne> <image> <source srcset = "../. GitBook / Assets / Captures 2025-05-16 160752.png" Media = "(préfère-Color-Scheme: Dark)"> <img Src = "../. (1) .png "alt =" "> </ picture> <gigon> </figcaption> </gigne>

## Référence du nœud AgentFlow V2

Cette section fournit une référence détaillée pour chaque nœud disponible, décrivant son objectif spécifique, les paramètres de configuration des clés, les entrées attendues, les sorties générées et son rôle dans l'architecture AgentFlow V2.

<gigne> <mage> <source srcset = "../. GitBook / Assets / Agentflowv2 / DarkMode / V2-01-D (1) .png" Media = "(préfers-Color-Scheme: Dark)"> <img src = "../. GitBook / Assets / Agentflowv2 / V2-all-Node.png" alt = ""> </ picture> <figction> </gigcaption> </figucion>

***

### ** 1. Démarrer le nœud **

Le point d'entrée désigné pour lancer n'importe quelle exécution de workflow AgentFlow V2. Chaque flux doit commencer par ce nœud.

* ** Fonctionnalité: ** Définit comment le flux de travail est déclenché et configure les conditions initiales. Il peut accepter les entrées directement à partir de l'interface de chat ou via un formulaire personnalisable présenté à l'utilisateur. Il permet également l'initialisation de`Flow State`Variables au début de l'exécution et peut gérer la façon dont la mémoire de conversation est gérée pour l'exécution.
* ** Paramètres de configuration **
  * ** Type d'entrée **: détermine comment l'exécution du flux de travail est initiée, soit par`Chat Input`de l'utilisateur ou via un soumis`Form Input`.
    * ** Titre du formulaire, description du formulaire, types d'entrée de formulaire **: If`Form Input`est sélectionné, ces champs configurent l'apparence du formulaire présenté à l'utilisateur, permettant divers types de champs de saisie avec des étiquettes définies et des noms de variables.
  * ** Mémoire éphémère **: si elle est activée, demande au workflow de commencer l'exécution sans considérer les messages passés du thread de conversation, en commençant efficacement par une ardoise de mémoire propre.
  * ** État de flux **: définit l'ensemble complet des paires de valeurs clés initiales pour l'état d'exécution du workflow`$flow.state`. Toutes les clés d'état qui seront utilisées ou mises à jour par les nœuds suivantes doivent être déclarées et initialisées ici.
* ** Entrées: ** Reçoit les données initiales qui déclenchent le workflow, qui sera soit un message de chat, soit les données soumises via un formulaire.
* ** Sorties: ** Fournit une seule ancre de sortie pour se connecter au premier nœud opérationnel, passant les données d'entrée initiales et l'état de flux initialisé.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-02-d.png" media = "(préfère-Color-Scheme: Dark)"> <img src = "../. width = "343"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 2. Node LLM **

Fournit un accès direct à un modèle de grande langue (LLM) configuré pour exécuter des tâches AI, permettant au workflow d'effectuer une extraction structurée de données si nécessaire.

* ** Fonctionnalité: ** Ce nœud envoie des demandes à un LLM basé sur des instructions (messages) et un contexte fourni. Il peut être utilisé pour la génération de texte, le résumé, la traduction, l'analyse, la réponse aux questions et la génération de sortie JSON structurée selon un schéma défini. Il a accès à la mémoire pour le thread de conversation et peut lire / écrire`Flow State`.
* ** Paramètres de configuration **
  * ** Modèle **: Spécifie le modèle AI à partir d'un service choisi - par exemple, GPT-4O d'OpenAI ou Google Gemini.
  * ** Messages **: Définissez l'entrée conversationnelle pour le LLM, en la structurant comme une séquence de rôles - système, utilisateur, assistant, développeur - pour guider la réponse de l'IA. Les données dynamiques peuvent être insérées en utilisant`{{ variable }}`.
  * ** Mémoire **: Si vous activez, détermine si le LLM doit considérer l'historique du thread de conversation actuel lors de la génération de sa réponse.
    * ** Type de mémoire, taille de la fenêtre, limite de jeton maximale **: Si la mémoire est utilisée, ces paramètres affinent comment l'historique de la conversation est géré et présenté au LLM - par exemple, s'il faut inclure tous les messages, seulement une fenêtre récente de virages ou une version résumée.
    * ** Message d'entrée **: Spécifie la variable ou le texte qui sera annexé comme le message utilisateur le plus récent à la fin du contexte de conversation existant - y compris le contexte initial et la mémoire - avant d'être traités par le LLM / Agent.
  * ** Retour Response As **: Configure comment la sortie de LLM est classée - comme un`User Message`ou`Assistant Message`- qui peut influencer la façon dont il est géré par les systèmes de mémoire ou la journalisation ultérieurs.
  * ** Sortie structurée JSON **: Demande au LLM de formater sa sortie en fonction d'un schéma JSON spécifique - y compris des clés, des types de données et des descriptions - garantissant des données prévisibles et lisibles par la machine.
  * ** Mettre à jour l'état de flux **: permet au nœud de modifier l'état d'exécution du workflow`$flow.state`Pendant l'exécution en mettant à jour les clés prédéfinies. Cela permet, par exemple, de stocker la sortie de ce nœud LLM sous une telle clé, ce qui le rend accessible aux nœuds suivants.
* ** Entrées: ** Ce nœud utilise des données du déclencheur initial du workflow ou des sorties des nœuds précédents, incorporant ces données dans le`Messages`ou`Input Message`champs. Il peut également récupérer des valeurs de`$flow.state`Lorsque les variables d'entrée le font référence.
* ** Sorties: ** produit la réponse de LLM, qui sera soit du texte brut, soit un objet JSON structuré. La catégorisation de cette sortie - en tant qu'utilisateur ou assistant - est déterminée par le`Return Response`paramètre.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-03-d.png" media = "(préfère-Color-Scheme: Dark)"> <img src = "../. width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 3. Node d'agent **

Représente une entité d'IA autonome capable de raisonner, de planifier et d'interagir avec des outils ou des sources de connaissances pour atteindre un objectif donné.

* ** Fonctionnalité: ** Ce nœud utilise un LLM pour décider dynamiquement d'une séquence d'actions. En fonction de l'objectif de l'utilisateur - fourni via des messages / entrées - il peut choisir d'utiliser des outils disponibles ou des magasins de documents de requête pour recueillir des informations ou effectuer des actions. Il gère son propre cycle de raisonnement et peut utiliser la mémoire pour le fil de conversation et`Flow State`. Convient aux tâches nécessitant un raisonnement en plusieurs étapes ou interagissant dynamiquement avec des systèmes ou des outils externes.
* ** Paramètres de configuration **
  * ** Modèle **: Spécifie le modèle AI à partir d'un service choisi - par exemple, GPT-4O ou Google Gemini d'OpenAI - qui conduira les processus de raisonnement et de prise de décision de l'agent.
  * ** Messages **: Définissez l'entrée conversationnelle initiale, l'objectif ou le contexte pour l'agent, en le structurant comme une séquence de rôles - système, utilisateur, assistant, développeur - pour guider la compréhension de l'agent et les actions ultérieures. Les données dynamiques peuvent être insérées en utilisant`{{ variable }}`.
  * ** Outils **: Spécifiez quels outils fluide prédéfinis l'agent est autorisé à utiliser pour atteindre ses objectifs.
    * Pour chaque outil sélectionné, un ** facultatif ** nécessite un indicateur d'entrée humain ** indique si l'opération de l'outil peut elle-même s'arrêter pour demander une intervention humaine.
  * ** Magasins de connaissances / documents **: Configurer l'accès aux informations dans les magasins de documents gérés par flux.
    * ** Magasin de documents **: Choisissez une boutique de documents préconfigurée à partir de laquelle l'agent peut récupérer des informations. Ces magasins doivent être mis en place et peuplés à l'avance.
    * ** Décrire les connaissances **: Fournir une description du langage naturel du contenu et du but de ce magasin de documents. Cette description guide l'agent pour comprendre quel type d'informations le magasin contient et quand il serait approprié de les interroger.
  * ** Connaissances / Vector intégrés **: Configurez l'accès aux magasins de vecteurs externes et préexistants comme sources de connaissances supplémentaires pour l'agent.
    * ** Vector Store **: Sélectionne la base de données vectorielle spécifique et préconfigurée que l'agent peut interroger.
    * ** Modèle d'intégration **: Spécifie le modèle d'intégration associé au magasin vectoriel sélectionné, assurant la compatibilité des requêtes.
    * ** Nom de la connaissance **: attribue un court nom descriptif à cette source de connaissances basée sur un vecteur, que l'agent peut utiliser pour référence.
    * ** Décrire les connaissances **: Fournir une description du langage naturel du contenu et du but de ce magasin vectoriel, en guidant l'agent sur quand et comment utiliser cette source de connaissances spécifique.
    * ** RETOUR DOCUMENTS SOURCES **: Si vous êtes activé, demande à l'agent d'inclure des informations sur les documents source avec les données récupérées du magasin vectoriel.
  * ** Mémoire **: Si vous êtes activé, détermine si l'agent doit considérer l'historique du thread de conversation actuel lors de la prise de décisions et de la génération de réponses.
    * ** Type de mémoire, taille de la fenêtre, limite de jeton maximale **: Si la mémoire est utilisée, ces paramètres affinent comment l'historique de la conversation est géré et présenté à l'agent - par exemple, que ce soit pour inclure tous les messages, seulement une fenêtre récente ou une version résumée.
    * ** Message d'entrée **: Spécifie la variable ou le texte qui sera annexé comme le message utilisateur le plus récent à la fin du contexte de conversation existant - y compris le contexte initial et la mémoire - avant d'être traités par le LLM / Agent.
  * ** RETOUR RÉPONSE **: Configure comment la sortie ou le message final de l'agent est classé - en tant que message utilisateur ou message assistant - qui peut influencer la façon dont il est géré par des systèmes de mémoire ultérieurs ou la journalisation.
  * ** Mettre à jour l'état de flux **: permet au nœud de modifier l'état d'exécution du workflow`$flow.state`Pendant l'exécution en mettant à jour les clés prédéfinies. Cela permet, par exemple, de stocker la sortie de ce nœud d'agent sous une telle clé, ce qui le rend accessible aux nœuds suivants.
* ** Entrées: ** Ce nœud utilise les données du déclencheur initial du workflow ou des sorties des nœuds précédents, souvent incorporés dans le`Messages`ou`Input Message`champs. Il accède aux outils configurés et aux sources de connaissances selon les besoins.
* ** Sorties: ** produit le résultat ou la réponse finale générée par l'agent une fois qu'il a terminé son raisonnement, sa planification et toute interaction avec des outils ou des sources de connaissances.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-04-d.png" media = "(préfère-Color-Scheme: Dark)"> <img src = "../. width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 4. Node d'outil **

Fournit un mécanisme pour exécuter directement et de manière déterministe un outil fluide spécifique et prédéfini dans la séquence de workflow. Contrairement au nœud d'agent, où le LLM choisit dynamiquement un outil basé sur le raisonnement, le nœud d'outil exécute exactement l'outil sélectionné par le concepteur de workflow pendant la configuration.

* ** Fonctionnalité: ** Ce nœud est utilisé lorsque le workflow nécessite l'exécution d'une capacité spécifique connue à un point défini, avec des entrées facilement disponibles. Il garantit une action déterministe sans impliquer le raisonnement LLM pour la sélection des outils.
* ** Comment ça marche **
  1. ** TRANGERS: ** Lorsque l'exécution du workflow atteint un nœud d'outil, il s'active.
  2. ** Identification de l'outil: ** Il identifie l'outil de flux spécifique sélectionné dans sa configuration.
  3. ** Résolution de l'argument d'entrée: ** Il examine la configuration des arguments d'entrée de l'outil. Pour chaque paramètre d'entrée requis de l'outil sélectionné.
  4. ** Exécution: ** Il invoque le code sous-jacent ou l'appel API associé à l'outil Flowise sélectionné, passant les arguments d'entrée résolus.
  5. ** Génération de sortie: ** Il reçoit le résultat renvoyé par l'exécution de l'outil.
  6. ** Propagation de sortie: ** Il rend ce résultat disponible via son ancre de sortie pour les nœuds suivants.
* ** Paramètres de configuration **
  * ** Sélection d'outils **: Choisissez l'outil Flowise spécifique et enregistré que ce nœud exécutera à partir d'une liste déroulante.
  * ** Arguments d'entrée **: Définissez comment les données de votre flux de travail sont fournies à l'outil sélectionné. Cette section s'adapte dynamiquement en fonction de l'outil choisi, présentant ses paramètres d'entrée spécifiques:
    * ** Nom de l'argument de la carte **: Pour chaque entrée, l'outil sélectionné nécessite (par exemple,`input`Pour une calculatrice), ce champ affichera le nom du paramètre attendu tel que défini par l'outil lui-même.
    * ** Fournir une valeur d'argument **: Définissez la valeur de ce paramètre correspondant, en utilisant une variable dynamique comme`{{ previousNode.output }}`, `{{ $flow.state.someKey }}`, ou en entrant un texte statique.
  * ** Mettre à jour l'état de flux **: permet au nœud de modifier l'état d'exécution du workflow`$flow.state`Pendant l'exécution en mettant à jour les clés prédéfinies. Cela permet, par exemple, de stocker la sortie de cet outil sous une telle clé, ce qui le rend accessible aux nœuds suivants.
* ** Entrées: ** reçoit les données nécessaires pour les arguments de l'outil via le`Input Arguments`mappage, valeurs d'approvisionnement à partir des sorties de nœud précédentes,`$flow.state`ou configurations statiques.
* ** Sorties: ** produit la sortie brute générée par l'outil exécuté - par exemple, une chaîne JSON à partir d'une API, un résultat de texte ou une valeur numérique.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-16-d.png" media = "(préfers-color-scheme: dark)"> <img src = "../. gitbook / actifs / agentflowv2 / v2-05.png" alt = "" " width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 5. Retriever Node **

Effectue une récupération d'informations ciblée à partir des magasins de documents configurés.

* ** Fonctionnalité: ** Ce nœud interroge un ou plusieurs magasins de documents spécifiés, récupérant des morceaux de document pertinents basés sur la similitude sémantique. C'est une alternative ciblée à l'utilisation d'un nœud d'agent lorsque la seule action requise est la récupération et la sélection des outils dynamiques par un LLM n'est pas nécessaire.
* ** Paramètres de configuration **
  * ** Magasins de connaissances / documents **: Spécifiez quel (s) magasin de documents préconfigurés et peuplés, ce nœud doit interroger pour trouver des informations pertinentes.
  * ** Retriever Query **: Définissez la requête texte qui sera utilisée pour rechercher les magasins de documents sélectionnés. Les données dynamiques peuvent être insérées en utilisant`{{ variables }}`.
  * ** Format de sortie **: Choisissez comment les informations récupérées doivent être présentées - soit comme simple`Text`ou comme`Text with Metadata`, qui peut inclure des détails tels que les noms de documents source ou les emplacements.
  * ** Mettre à jour l'état de flux **: permet au nœud de modifier l'état d'exécution du workflow`$flow.state`Pendant l'exécution en mettant à jour les clés prédéfinies. Cela permet, par exemple, de stocker la sortie de ce nœud Retriever sous une telle clé, ce qui le rend accessible aux nœuds suivants.
* ** Entrées: ** nécessite une chaîne de requête - souvent fournie comme une variable à partir d'une étape précédente ou d'une entrée utilisateur - et accède aux magasins de documents sélectionnés pour plus d'informations.
* ** sorties: ** produit les morceaux de document récupérés de la base de connaissances, formaté selon les choisis`Output Format`.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-06-d.png" media = "(préfère-Color-Scheme: Dark)"> <img src = "../. GitBook / Assets / Agentflowv2 / V2-06.png" alt = "" width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### 6. nœud http

Facilite la communication directe avec les services Web externes et les API via le protocole de transfert hypertexte (HTTP).

* ** Fonctionnalité: ** Ce nœud permet au workflow d'interagir avec tout système externe accessible via HTTP. Il peut envoyer différents types de demandes (obtenir, publier, mettre, supprimer, patcher) à une URL spécifiée, permettant une intégration avec des API tierces, récupérer des données à partir de ressources Web ou déclencher des webhooks externes. Le nœud prend en charge la configuration des méthodes d'authentification, des en-têtes personnalisés, des paramètres de requête et différents types de corps de demande pour répondre aux diverses exigences d'API.
* ** Paramètres de configuration **
  * ** HTTP Idedential **: Sélectionnez éventuellement des informations d'identification préconfigurées - telles que l'authentification de base, le jeton de support ou la clé API - pour authentifier les demandes au service cible.
  * ** Méthode de la demande **: Spécifiez la méthode HTTP à utiliser pour la demande - par exemple,`GET`, `POST`, `PUT`, `DELETE`, `PATCH`.
  * ** URL cible **: Définissez l'URL complète du point de terminaison externe auquel la demande sera envoyée.
  * ** En-têtes de demande **: Définissez tous les en-têtes HTTP nécessaires en paires de valeurs clés à inclure dans la demande.
  * ** Paramètres de requête URL **: Définissez les paires de valeurs clés qui seront annexées à l'URL en tant que paramètres de requête.
  * ** Type de corps de demande **: Choisissez le format de la charge utile de demande si l'envoi de données - les options incluent`JSON`, `Raw text`, `Form Data`, ou`x-www-form-urlencoded`.
  * ** Demande Body **: Fournissez la charge utile de données réelle pour des méthodes comme le poste ou le put. Le format doit correspondre au sélectionné`Body Type`et les données dynamiques peuvent être insérées en utilisant`{{ variables }}`.
  * ** Type de réponse **: Spécifiez comment le flux de travail doit interpréter la réponse reçue du serveur - les options incluent`JSON`, `Text`, `Array Buffer`, ou`Base64`pour les données binaires.
* ** Entrées: ** reçoit des données de configuration telles que l'URL, la méthode, les en-têtes et le corps, incorporant souvent des valeurs dynamiques à partir d'étapes de work`$flow.state`.
* ** Sorties: ** produit la réponse reçue du serveur externe, analysé selon le sélectionné`Response Type`.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-07-d.png" media = "(préfère-Color-Scheme: Dark)"> <img src = "../. Gitbook / Assets / Agentflowv2 / V2-07.png" alt = "" width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 7. Node de condition **

Implémente la logique de ramification déterministe dans le flux de travail sur la base des règles définies.

* ** Fonctionnalité: ** Ce nœud agit comme un point de décision, évaluant une ou plusieurs conditions spécifiées pour diriger le flux de travail dans différents chemins. Il compare les valeurs d'entrée - qui peuvent être des chaînes, des nombres ou des booléens - en utilisant une variété d'opérateurs logiques, tels que les égaux, contient, supérieur ou vide. Sur la base de la question de savoir si ces conditions évaluent en vrai ou fausse, l'exécution du flux de travail passe le long de l'une des branches de sortie distinctes connectées à ce nœud.
* ** Paramètres de configuration **
  * ** Conditions **: Configurez l'ensemble des règles logiques que le nœud évaluera.
    * ** Type **: Spécifiez le type de données comparées pour cette règle -`String`, `Number`, ou`Boolean`.
    * ** Valeur 1 **: Définissez la première valeur pour la comparaison. Les données dynamiques peuvent être insérées en utilisant`{{ variables }}`.
    * ** Opération **: Sélectionnez l'opérateur logique à appliquer entre la valeur 1 et la valeur 2 - par exemple,`equal`, `notEqual`, `contains`, `larger`, `isEmpty`.
    * ** Valeur 2 **: Définissez la deuxième valeur pour la comparaison, si nécessaire par l'opération choisie. Les données dynamiques peuvent également être insérées ici en utilisant`{{ variables }}`.
* ** Entrées: ** nécessite les données pour`Value 1`et`Value 2`pour chaque condition évaluée. Ces valeurs sont fournies à partir des sorties de nœud précédentes ou récupérées à partir de`$flow.state`.
* ** Sorties: ** fournit plusieurs ancres de sortie, correspondant au résultat booléen (vrai / faux) des conditions évaluées. Le flux de travail continue le long du chemin spécifique connecté à l'ancre de sortie qui correspond au résultat.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-08-d.png" media = "(préfère-Color-Scheme: Dark)"> <img src = "../. width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 8. Node d'agent de condition **

Fournit une ramification dynamique basée sur l'IA basée sur les instructions et le contexte du langage naturel.

* ** Fonctionnalité: ** Ce nœud utilise un modèle grand langage (LLM) pour acheminer le workflow. Les analyses des analyses ont fourni des données d'entrée par rapport à un ensemble de "scénarios" définis par l'utilisateur - résultats ou catégories potentiels - guidés par des "instructions" de langage naturel de haut niveau qui définissent la tâche de prise de décision. Le LLM détermine ensuite quel scénario correspond le mieux au contexte d'entrée actuel. Sur la base de cette classification dirigée par l'IA, l'exécution du flux de travail réduit le chemin de sortie spécifique correspondant au scénario choisi. Ce nœud est particulièrement utile pour les tâches telles que la reconnaissance de l'intention des utilisateurs, le routage conditionnel complexe ou la prise de décision situationnelle nuancée où des règles simples et prédéfinies - comme dans le nœud de condition - sont insuffisantes.
* ** Paramètres de configuration **
  * ** Modèle **: Spécifie le modèle AI à partir d'un service choisi qui effectuera l'analyse et la classification des scénarios.
  * ** Instructions **: Définissez l'objectif ou la tâche globale du LLM en langage naturel - par exemple, "Déterminez si la demande de l'utilisateur concerne les ventes, le support ou la demande générale."
  * ** Entrée **: spécifiez les données, souvent du texte à partir d'une étape précédente ou d'une entrée utilisateur, en utilisant`{{ variables }}`, que le LLM analysera pour prendre sa décision de routage.
  * ** Scénarios **: Configurer un tableau définissant les résultats possibles ou les chemins distincts que le flux de travail peut prendre. Chaque scénario est décrit dans le langage naturel - par exemple, «Enquête sur les ventes», «demande de support», «question générale» - et chacune correspond à une ancre de sortie unique sur le nœud.
* ** Entrées: ** nécessite le`Input`Données pour l'analyse et le`Instructions`Pour guider le LLM.
* ** sorties: ** fournit plusieurs ancres de sortie, une pour chaque définie`Scenario`. Le workflow continue le long du chemin spécifique connecté à l'ancre de sortie que le LLM détermine le meilleur correspond à l'entrée.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-09-d.png" media = "(préfère-Color-Scheme: Dark)"> <img src = "../. width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 9. Node d'itération **

Exécute un "sous-flux" défini - une séquence de nœuds imbriqués - pour chaque élément d'un tableau d'entrée, implémentant une boucle "for-een".

* ** Fonctionnalité: ** Ce nœud est conçu pour le traitement des collections de données. Il prend un tableau, fourni directement ou référencé via une variable, comme entrée. Pour chaque élément individuel à l'intérieur de ce tableau, le nœud d'itération exécute séquentiellement la séquence d'autres nœuds qui sont visuellement placés à l'intérieur de ses limites sur la toile.
* ** Paramètres de configuration **
  * ** Entrée du tableau **: Spécifie le tableau d'entrée que le nœud iratera. Ceci est fourni en faisant référence à une variable qui contient un tableau à partir de la sortie d'un nœud précédent ou du`$flow.state`- par exemple,`{{ $flow.state.itemList }}`.
* ** Entrées: ** nécessite un tableau à fournir à son`Array Input`paramètre.
* ** Sorties: ** Fournit une seule anage de sortie qui ne devient active qu'après que le sous-flux imbriqué a terminé l'exécution pour tous les éléments du tableau d'entrée. Les données transmises par cette sortie peuvent inclure des résultats agrégés ou l'état final des variables modifié dans la boucle, selon la conception du sous-flux. Les nœuds placés à l'intérieur du bloc d'itération ont leurs propres connexions d'entrée et de sortie distinctes qui définissent la séquence d'opérations pour chaque élément.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-10-d (1) .png" media = "(prefers-color-scheme: sombre)"> <img src = "../. gitbook / actets / agentflowv2 / v2-10.png" alt = "" " width = "563"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 10. Node de boucle **

Redirige explicitement l'exécution du workflow vers un nœud précédemment exécuté.

* ** Fonctionnalité: ** Ce nœud permet la création de cycles ou de tentatives itératives dans un workflow. Lorsque le flux d'exécution atteint le nœud de boucle, il n'atteint pas un nouveau nœud; Au lieu de cela, il "remonte" à un nœud cible spécifié qui a déjà été exécuté plus tôt dans l'exécution actuelle du flux de travail. Cette action provoque la réexécution de ce nœud cible et de tous les nœuds suivants dans cette partie de l'écoulement.
* ** Paramètres de configuration **
  * ** Loop Retour à **: Sélectionne l'ID unique d'un nœud précédemment exécuté dans le flux de travail actuel auquel l'exécution doit retourner.
  * ** MAX LOOP COUNT **: Définit le nombre maximal de fois que cette opération de boucle peut être effectuée dans une seule exécution de workflow, sauvegarde contre les cycles infinis. La valeur par défaut est 5.
* ** Entrées: ** Reçoit le signal d'exécution pour activer. Il suit en interne le nombre de fois que la boucle s'est produite pour l'exécution actuelle.
* ** Sorties: ** Ce nœud n'a pas d'ancre de sortie standard à pointant, car sa fonction principale est de rediriger le flux d'exécution vers l'arrière vers le`Loop Back To`Node cible, d'où le flux de travail continue alors.

<gigne> <mage> <source srcset = "../. GitBook / Assets / Agentflowv2 / DarkMode / V2-11-D.png" Media = "(préfère-Color-Scheme: Dark)"> <img src = "../. GitBook / Assets / Agentflowv2 / V2-11.png" Alt = "" width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 11. Node d'entrée humain **

Utilise l'exécution du workflow pour demander des entrées, une approbation ou des commentaires explicites d'un utilisateur humain - un composant clé pour les processus humains dans la boucle (HITL).

* ** Fonctionnalité: ** Ce nœud arrête la progression automatisée du flux de travail et présente des informations ou une question à un utilisateur humain, via l'interface de chat. Le contenu affiché à l'utilisateur peut être un texte statique prédéfini ou généré dynamiquement par un LLM basé sur le contexte de workflow actuel. L'utilisateur reçoit des choix d'action distincts - par exemple, «procéder», «rejeter» - et, s'il est activé, un champ pour fournir des commentaires textuels. Une fois que l'utilisateur fait une sélection et soumet sa réponse, le flux de travail reprend l'exécution le long du chemin de sortie spécifique correspondant à son action choisie.
* ** Paramètres de configuration **
  * ** Type de description **: détermine comment le message ou la question présentée à l'utilisateur est généré - soit`Fixed`(texte statique) ou`Dynamic`(généré par un LLM).
    * ** Si le type de description est`Fixed`**
      * ** Description **: Ce champ contient le texte exact à afficher à l'utilisateur. Il prend en charge l'insertion de données dynamiques en utilisant`{{ variables }}`
    * **Si`Description Type`est`Dynamic`**
      * ** Modèle **: sélectionne le modèle AI dans un service choisi qui générera le message orienté utilisateur.
      * ** invite **: fournit les instructions ou l'invite pour le LLM sélectionné pour générer le message affiché à l'utilisateur.
  * ** Feedback: ** Si activé, l'utilisateur sera invité avec une fenêtre de rétroaction pour laisser ses commentaires, et ces commentaires seront annexés à la sortie du nœud.
* ** Entrées: ** Reçoit le signal d'exécution pour suspendre le workflow. Il peut utiliser les données des étapes précédentes ou`$flow.state`à travers des variables dans le`Description`ou`Prompt`champs s'ils sont configurés pour le contenu dynamique.
* ** Sorties: ** Fournit deux ancres de sortie, chacune correspondant à une action utilisateur distincte - une ancre pour "procéder" et une autre pour "rejeter". Le flux de travail continue le long du chemin connecté à l'ancre correspondant à la sélection de l'utilisateur.

<gigne> <mage> <source srcset = "../. GitBook / Assets / Agentflowv2 / DarkMode / V2-12-D.png" Media = "(Prefers-Color-Scheme: Dark)"> <img src = "../. GitBook / Assets / Agentflowv2 / V2-12.png" Alt = "" width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 12. Node de réponse directe **

Envoie un message final à l'utilisateur et termine le chemin d'exécution actuel.

* ** Fonctionnalité: ** Ce nœud sert de point de terminaison pour une branche spécifique ou l'intégralité d'un workflow. Il prend un message configuré - qui peut être du texte statique ou du contenu dynamique d'une variable - et le livre directement à l'utilisateur final via l'interface de chat. Lors de l'envoi de ce message, l'exécution le long de ce chemin particulier du workflow conclut; Aucun autre nœud connecté à partir de ce point ne sera traité.
* ** Paramètres de configuration **
  * ** Message **: Définissez le texte ou la variable`{{ variable }}`Cela contient le contenu à envoyer comme réponse finale à l'utilisateur.
* ** Entrées: ** reçoit le contenu du message, qui provient de la sortie d'un nœud précédent ou d'une valeur stockée dans`$flow.state`.
* ** Sorties: ** Ce nœud n'a pas d'ancres de sortie, car sa fonction est de terminer le chemin d'exécution après avoir envoyé la réponse.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-13-d.png" media = "(préfers-color-scheme: dark)"> <img src = "../. gitbook / actifs / agentflowv2 / v2-13.png" alt = "" " width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 13. Node de fonction personnalisé **

Fournit un mécanisme pour exécuter le code JavaScript côté serveur personnalisé dans le workflow.

* ** Fonctionnalité: ** Ce nœud permet d'écrire et d'exécuter des extraits arbitraires JavaScript, offrant un moyen efficace d'implémenter des transformations de données complexes, une logique métier sur mesure ou des interactions avec des ressources non directement prises en charge par d'autres nœuds standard. Le code exécuté fonctionne dans un environnement Node.js et a des moyens spécifiques d'accéder aux données:
  * ** Variables d'entrée: ** Valeurs passées via le`Input Variables`La configuration est accessible dans la fonction, généralement préfixée avec- par exemple, si une variable d'entrée`userid`est défini, il est accessible comme`$userid`.
  * ** Contexte de flux: ** Les variables de configuration de flux par défaut sont disponibles, telles que`$flow.sessionId`, `$flow.chatId`, `$flow.chatflowId`, `$flow.input`- l'entrée initiale qui a commencé le flux de travail - et l'ensemble`$flow.state`objet.
  * ** Variables personnalisées: ** Toutes les variables personnalisées configurées dans Flowise - par exemple,`$vars.<variable-name>`.
  * ** Bibliothèques: ** La fonction peut utiliser toutes les bibliothèques qui ont été importées et rendues disponibles dans l'environnement backend Flowise. ** La fonction doit renvoyer une valeur de chaîne à la fin de son exécution **.
* ** Paramètres de configuration **
  * ** Variables d'entrée **: Configurez un tableau de définitions d'entrée qui seront transmises sous forme de variables dans la portée de votre fonction JavaScript. Pour chaque variable que vous souhaitez définir, vous spécifierez:
    * ** Nom de la variable **: le nom que vous utiliserez pour vous référer à cette variable dans votre code JavaScript, généralement préfixé avec un- par exemple, si vous entrez`myValue`Ici, vous pourriez y accéder comme`$myValue`Dans le script, correspondant à la façon dont les propriétés du schéma d'entrée sont mappées.
    * ** Valeur variable **: les données réelles à affecter à cette variable, qui peut être un texte statique ou, plus souvent, une valeur dynamique provenant du flux de travail - par exemple,`{{ previousNode.output }}`ou`{{ $flow.state.someKey }}`.
  * ** Fonction JavaScript **: le champ de l'éditeur de code où la fonction JavaScript côté serveur est écrite. Cette fonction doit finalement renvoyer une valeur de chaîne.
  * ** Mettre à jour l'état de flux **: permet au nœud de modifier l'état d'exécution du workflow`$flow.state`Pendant l'exécution en mettant à jour les clés prédéfinies. Cela permet, par exemple, de stocker la sortie de chaîne de ce nœud de fonction personnalisé sous une telle clé, ce qui le rend accessible aux nœuds suivants.
* ** Entrées: ** reçoit des données via les variables configurées dans`Input Variables`. Peut également accéder implicitement aux éléments du`$flow`contexte et`$vars`.
* ** Sorties: ** produit la valeur de chaîne renvoyée par la fonction JavaScript exécutée.

<gigne> <mage> <source srcset = "../. GitBook / Assets / Agentflowv2 / DarkMode / V2-14-D.png" Media = "(Prefers-Color-Scheme: Dark)"> <img src = "../. GitBook / Assets / Agentflowv2 / V2-14.png" Alt = "" width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

***

### ** 14. Exécuter le nœud de flux **

Permet l'invocation et l'exécution d'un autre ChatFlow Flowise complet ou d'agentflow à partir du flux de travail actuel.

* ** Fonctionnalité: ** Ce nœud fonctionne comme un appelant sous-travail, faisant la promotion de la conception modulaire et de la réutilisabilité de la logique. Il permet au flux de travail actuel de déclencher un flux de travail préexistant séparé - identifié par son nom ou son ID dans l'instance FLUSED - passez une entrée initiale à lui, remplace éventuellement des configurations spécifiques du flux cible pour cette exécution particulière, puis reçoit sa sortie finale dans le flux de travail d'appel pour continuer le traitement.
* ** Paramètres de configuration **
  * ** Connectez les informations d'identification **: Fournissez éventuellement les informations d'identification de l'API ChatFlow si le flux cible étant appelé nécessite une authentification spécifique ou des autorisations d'exécution.
  * ** Sélectionnez Flow **: Spécifiez le ChatFlow ou AgentFlow particulier que ce nœud exécutera à partir de la liste des flux disponibles dans votre instance Flowise.
  * ** Entrée **: Définissez les données - Texte statique ou`{{ variable }}`- qui sera transmis comme entrée principale au workflow cible lorsqu'il sera invoqué.
  * ** Remplacez la configuration **: Fournissez éventuellement un objet JSON contenant des paramètres qui remplaceront la configuration par défaut du flux de travail cible spécifiquement pour cette instance d'exécution - par exemple, modifiant temporairement un modèle ou une invite utilisée dans le sous-flux.
  * ** URL de base **: Spécifiez éventuellement une URL de base alternative pour l'instance fluide qui héberge le flux cible. Ceci est utile dans les configurations distribuées ou lorsque les débits sont accessibles via différents itinéraires, défautant à l'URL de l'instance actuelle si ce n'est pas défini.
  * ** RETOUR RÉPONSE AS **: Déterminez comment la sortie finale du sous-flux exécuté doit être classée lorsqu'elle est retournée au flux de travail actuel - en tant que`User Message`ou`Assistant Message`.
  * ** Mettre à jour l'état de flux **: permet au nœud de modifier l'état d'exécution du workflow`$flow.state`Pendant l'exécution en mettant à jour les clés prédéfinies. Cela permet, par exemple, de stocker la sortie de cette exécution du nœud de flux sous une telle clé, ce qui le rend accessible aux nœuds suivants.
* ** Entrées: ** nécessite la sélection d'un flux cible et du`Input`données pour cela.
* ** sorties: ** produit la sortie finale renvoyée par le flux de travail cible exécuté, formaté en fonction du`Return Response As`paramètre.

<gigne> <mage> <source srcset = "../. gitbook / actifs / agentflowv2 / darkmode / v2-15-d.png" media = "(préfers-color-scheme: dark)"> <img src = "../. gitbook / actifs / agentflowv2 / v2-15.png" alt = "" " width = "375"> </ picture> <figcaption> </gigcaption> </ figure>

## Comprendre l'état de flux

Une caractéristique architecturale clé permettant la flexibilité et les capacités de gestion des données d'agentflow v2 est l'état de flux ** **. Ce mécanisme fournit un moyen de gérer et de partager les données dynamiquement tout au long de l'exécution d'une seule instance de workflow.

### ** Qu'est-ce que l'état de flux? **

* État de flux (`$flow.state`) est un ** Runtime, Key-Value Store ** qui est partagé entre les nœuds en une seule exécution.
* Il fonctionne comme une mémoire temporaire ou un contexte partagé qui existe uniquement pour la durée de cette exécution / exécution particulière.

### ** But de l'état de flux **

Le but principal de`$flow.state`est d'activer ** le partage et la communication explicites de données entre les nœuds, en particulier ceux qui peuvent ne pas être directement connectés ** dans le graphique de workflow, ou lorsque les données doivent être intentionnellement persistantes et modifiées sur plusieurs étapes. Il relève plusieurs défis d'orchestration courants:

1. ** transmettre des données sur les branches: ** Si un flux de travail se divise dans les chemins conditionnels, les données générées ou mises à jour dans une branche peuvent être stockées dans`$flow.state`pour accéder plus tard si les chemins fusionnent ou si d'autres branches ont besoin de ces informations.
2. ** L'accès aux données sur des étapes non adjacentes: ** Les informations initialisées ou mises à jour par un nœud précoce peuvent être récupérées par un nœud beaucoup plus tard sans avoir à le passer explicitement via les entrées et sorties de chaque nœud intermédiaire.

### ** Comment fonctionne l'état de flux **

1. ** Initialisation / Déclaration des clés **
   * Toutes les clés d'état qui seront utilisées tout au long du workflow ** doivent être initialisées ** avec leurs valeurs par défaut (même si vides) en utilisant le`Flow State`Paramètre dans le nœud ** de démarrage **. Cette étape déclare efficacement le schéma ou la structure de votre`$flow.state`pour ce flux de travail. Vous définissez ici les paires de valeurs clés initiales.

<gigne> <image> <source srcset = "../. GitBook / Assets / Capture 2025-05-16 160038.png" Media = "(préfère-Color-Scheme: Dark)"> <img Src = "../. (1) (1) .png "alt =" "> </ image> <figcaption> </gigcaption> </gigust>

2. ** Mise à jour de l'état / Modification des clés existantes **

* De nombreux nœuds opérationnels - par exemple,`LLM`, `Agent`, `Tool`, `HTTP`, `Retriever`, `Custom Function`- Inclure un`Update Flow State`paramètre dans leur configuration.
* Ce paramètre permet au nœud ** de modifier les valeurs des clés préexistantes ** à l'intérieur`$flow.state`.
* La valeur peut être du texte statique, la sortie directe du nœud actuel, la sortie du nœud précédent et de nombreuses autres variables. Taper`{{`affichera toutes les variables disponibles.
* Lorsque le nœud s'exécute avec succès, il ** met à jour ** la ou les touches spécifiées dans`$flow.state`avec la (s) valeur (s). ** Les nouvelles clés ne peuvent pas être créées par des nœuds opérationnels; Seules les clés prédéfinies peuvent être mises à jour. **

<gigne> <mage> <source srcset = "../. GitBook / Assets / Captures 2025-05-16 160347.png" Media = "(préfers-Color-Scheme: Dark)"> <img src = "../. GitBook / Assets / Screenshot 2025-05-16 160427.png" alt = ""> </ picture> <figction> </gigcaption> </figucion>

3. ** Lire de l'état **

* Tout paramètre d'entrée de nœud qui accepte les variables peut lire les valeurs de l'état de flux.
* Utilisez la syntaxe spécifique:`{{ $flow.state.yourKey }}`- remplacer`yourKey`avec le nom de clé réel qui a été initialisé dans le nœud de démarrage.
* Par exemple, l'invite d'un nœud LLM peut inclure`"...based on the user status: {{ $flow.state.customerStatus }}"`.

<gigne> <mage> <source srcset = "../. GitBook / Assets / Captures 2025-05-16 161711.png" Media = "(Prefers-Color-Scheme: Dark)"> <img src = "../. GitBook / Assets / Screenshot 2025-05-16 161605.png" alt = ""> </ picture> <figction> </gigcaption> </figucion>

### ** Portée et persistance: **

* Il est créé et initialisé lorsqu'une exécution de workflow commence et est détruite lorsque cette exécution spécifique se termine.
* Il ** ne persiste pas dans différentes sessions utilisateur ou des exécutions séparées du même flux de travail.
* Chaque exécution simultanée du flux de travail maintient son propre`$flow.state`.

## Ressources vidéo

{% embed url = "https://youtu.be/slvvduibibe?si=VU1M_BTFDZVNL-PP"%}

{% embed url = "https://youtu.be/h9n9wcrp9u4?si=8-9a9fktpxaykxxh"%}
