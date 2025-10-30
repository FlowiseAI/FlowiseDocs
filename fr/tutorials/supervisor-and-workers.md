# Superviseur et travailleurs

Le modèle de travailleur superviseur est une conception puissante de workflow où un agent superviseur coordonne plusieurs agents de travailleurs spécialisés pour effectuer des tâches complexes. Ce modèle permet une meilleure délégation des tâches, une expertise spécialisée et un raffinement itératif des solutions.

## Aperçu

Dans ce tutoriel, nous allons créer un système collaboratif avec:

* ** Superviseur **: un LLM qui analyse les tâches et décide quel travailleur doit agir ensuite
* ** Ingénieur logiciel **: Spécialisé dans la conception et la mise en œuvre de solutions logicielles
* ** Code Reviewer **: axé sur la révision de la qualité du code et la fourniture de commentaires
* ** Générateur de réponses finales **: compile le travail collaboratif dans une solution complète

<gigne> <img src = "../. GitBook / Assets / Image (19) .png" alt = ""> <Figcaption> </gigcaption> </gigne>

### Étape 1: Créez le nœud de démarrage

<gigne> <img src = "../. GitBook / Assets / image (7) (1) .png" alt = "" width = "160"> <Figcaption> </gigcaption> </ figure>

Le flux commence par un nœud ** start ** qui capture l'entrée utilisateur et initialise l'état de flux de travail.

1. Ajoutez un nœud ** start ** à votre toile
2. Configurez le ** type d'entrée ** comme "entrée de chat"
3. Configurer ** État de flux ** avec ces variables initiales:
   * `next`: Pour garder une trace du prochain agent
   * `instruction`: Instruction pour le prochain agent sur ce qu'il faut faire

<gigne> <img src = "../. GitBook / Assets / Image (6) (1) .png" alt = "" width = "417"> <Figcaption> </ Figcaption> </gigne>

### Étape 2: Ajouter le superviseur LLM

<gigne> <img src = "../. GitBook / Assets / Image (8) (1) .png" alt = "" width = "227"> <Figcaption> </gigcaption> </ figure>

Le ** superviseur ** est l'orchestrateur qui décide quel travailleur doit gérer chaque partie de la tâche.

1. Connectez un nœud ** llm ** après le nœud de démarrage
2. Étiquetez-le "superviseur"
3. Configurez le message système, par exemple:

```
You are a supervisor tasked with managing a conversation between the following workers:
- Software Engineer  
- Code Reviewer

Given the following user request, respond with the worker to act next.
Each worker will perform a task and respond with their results and status.
When finished, respond with FINISH.
Select strategically to minimize the number of steps taken.
```

4. Configurer ** Sortie structurée JSON ** avec ces champs:
   * `next`: Enum avec les valeurs "Finition, logiciel, critique"
   * `instructions`: Les instructions spécifiques du sous-tâche que le prochain travailleur devrait accomplir
   * `reasoning`: La raison pour laquelle le prochain travailleur est chargé de faire le travail
5. Configurer ** Mettre à jour l'état de flux ** pour stocker:
   * `next`: `{{ output.next }}`
   * `instruction`: `{{ output.instructions }}`
6. Définissez le ** Message d'entrée ** sur: _ "Compte tenu de la conversation ci-dessus, qui devrait agir ensuite? Ou devrions-nous terminer? Sélectionnez un de: logiciel, examinateur." _ Le message d'entrée sera inséré à la fin, comme si l'utilisateur demandait au superviseur d'attribuer le prochain agent.

<gigne> <img src = "../. GitBook / Assets / Untitled-2025-06-19-1011.png" alt = "" width = "563"> <Figcaption> </ Figcaption> </ Figure>

### Étape 3: Créez la condition de routage

<gigne> <img src = "../. GitBook / Assets / image (10) .png" alt = "" width = "323"> <figcaption> </gigcaption> </ figure>

Le ** Vérifiez le nœud de condition du travailleur Next ** achemine le flux en fonction de la décision du superviseur.

1. Ajouter un nœud ** condition ** après le superviseur
2. Configurez deux conditions:
   * ** Condition 0 **:`{{ $flow.state.next }}`est égal à "logiciel"
   * ** Condition 1 **:`{{ $flow.state.next }}`est égal à "réviseur"
3. La branche "Else" (condition 2) gérera le boîtier "Finish"

Cela crée trois chemins de sortie: un pour chaque travailleur et un pour l'achèvement.

<gigne> <img src = "../. GitBook / Assets / Image (11) .png" alt = "" width = "395"> <figcaption> </gigcaption> </gigust>

### Étape 4: Configurer l'agent d'ingénieur logiciel

<gigne> <img src = "../. GitBook / Assets / Image (12) .png" alt = "" width = "296"> <figcaption> </gigcaption> </gigu

L'ingénieur logiciel ** se spécialise dans la conception et la mise en œuvre de solutions logicielles.

1. Connectez un nœud ** agent ** à la sortie de condition 0
2. Configurer le message système:

```
As a Senior Software Engineer, you are a pivotal part of our innovative development team. Your expertise and leadership drive the creation of robust, scalable software solutions that meet the needs of our diverse clientele.

Your goal is to lead the development of high-quality software solutions.

Design and implement new features for the given task, ensuring it integrates seamlessly with existing systems and meets performance requirements. Use your understanding of React, TailwindCSS, NodeJS to build this feature. Make sure to adhere to coding standards and follow best practices.

The output should be a fully functional, well-documented feature that enhances our product's capabilities. Include detailed comments in the code.
```

4. Définir ** Message d'entrée ** sur:`{{ $flow.state.instruction }}`. Le message d'entrée sera inséré à la fin, comme si l'utilisateur donne une instruction à l'agent de l'ingénieur logiciel.

<gigne> <img src = "../. GitBook / Assets / Image (13) .png" alt = "" width = "397"> <figcaption> </gigcaption> </gigust>

<gigne> <img src = "../. GitBook / Assets / Image (14) .png" alt = "" width = "563"> <figcaption> </gigcaption> </gigu

### Étape 5: Configurer l'agent de réviseur de code

<gigne> <img src = "../. GitBook / Assets / Image (16) .png" alt = "" width = "267"> <figcaption> </gigcaption> </gigu

Le ** Code Reviewer ** se concentre sur l'assurance qualité et l'examen du code.

1. Connectez un nœud ** agent ** à la sortie de condition 1
2. Configurer le message système:

```
As a Quality Assurance Engineer, you are an integral part of our development team, ensuring that our software products are of the highest quality. Your meticulous attention to detail and expertise in testing methodologies are crucial in identifying defects and ensuring that our code meets the highest standards.

Your goal is to ensure the delivery of high-quality software through thorough code review and testing.

Review the codebase for the new feature designed and implemented by the Senior Software Engineer. Provide constructive feedback, guiding contributors towards best practices and fostering a culture of continuous improvement. Your approach ensures the delivery of high-quality software that is robust, scalable, and aligned with strategic goals.
```

4. Définir ** Message d'entrée ** sur:`{{ $flow.state.instruction }}`. Le message d'entrée sera inséré à la fin, comme si l'utilisateur donne une instruction à l'agent de réviseur de code.

<gigne> <img src = "../. GitBook / Assets / Image (15) .png" alt = "" width = "563"> <figcaption> </gigcaption> </ figure>

### Étape 6: Ajoutez des connexions de retour en boucle

<gigne> <img src = "../. GitBook / Assets / Image (17) .png" alt = "" width = "563"> <figcaption> </gigcaption> </gigust>

Les deux agents des travailleurs doivent retourner au superviseur pour une coordination continue.

1. Ajouter un nœud ** boucle ** après l'ingénieur logiciel
   * Définissez ** la boucle à ** en tant que "superviseur"
   * Set ** MAX LOOP COUNT ** à 5
2. Ajoutez un autre nœud ** LOOP ** après le réviseur de code
   * Définissez ** la boucle à ** en tant que "superviseur"
   * Set ** MAX LOOP COUNT ** à 5

Ces boucles permettent une collaboration itérative entre les agents.

### Étape 7: Créez le générateur de réponses final

<gigne> <img src = "../. GitBook / Assets / Image (18) .png" alt = "" width = "436"> <figcaption> </gigcaption> </gigu

L'agent final compile tout le travail collaboratif dans une solution complète.

1. Connectez un nœud ** Agent ** à la sortie de condition 2 (la branche "else")
2. Il est recommandé d'utiliser une taille de contexte plus élevée LLM comme les Gémeaux, en raison de la nature des va-et-vient de la conversation, qui consomme un grand nombre de jetons.
3. Définir ** Message d'entrée. ** Ceci est important car le message d'entrée sera inséré à la fin, comme si l'utilisateur donne une instruction au générateur de réponses final pour examiner toutes les conversations et générer une réponse finale.

```
Given the above conversations, generate a detail solution developed by the software engineer and code reviewer.

Your guiding principles:
1. **Preserve Full Context**
   Include all code implementations, improvements and review from the conversation. Do not omit, summarize, or oversimplify key information.

2. **Markdown Output Only** 
   Your final output must be in Markdown format.
```

## Comment ça marche

Le modèle de travailleur superviseur permet plusieurs avantages clés:

** Délégation de tâche intelligente **: Le superviseur utilise le contexte et le raisonnement pour attribuer le travailleur le plus approprié pour chaque sous-tâche.

** Raffinement itératif **: Les travailleurs peuvent s'appuyer sur la sortie de l'autre, l'ingénieur logiciel implémentant les fonctionnalités et le réviseur de code fournissant des commentaires pour les améliorations.

** Coordination avec état **: Le flux maintient l'état à travers les itérations, permettant au superviseur de prendre des décisions éclairées sur ce qui devrait arriver ensuite.

** Expertise spécialisée **: Chaque agent a un rôle ciblé et une invite spécialisée, conduisant à des résultats de meilleure qualité dans son domaine.

## Exemple d'interaction

Voici comment une interaction typique pourrait couler:

1. ** Utilisateur **: "Créez un composant React pour l'authentification de l'utilisateur avec validation du formulaire"
2. ** Superviseur **: Décide que le logiciel devrait d'abord agir pour implémenter le composant
3. ** Ingénieur logiciel **: Crée un composant d'authentification React avec logique de validation
4. ** Superviseur **: Décide que le réviseur doit examiner la mise en œuvre
5. ** Code Reviewer **: examine le code et suggère des améliorations pour la sécurité et UX
6. ** Superviseur **: Décide le logiciel devrait implémenter les améliorations suggérées
7. ** Ingénieur logiciel **: met à jour le composant en fonction des commentaires
8. ** Superviseur **: détermine que la tâche est complète et les routes pour terminer
9. ** Générateur de réponses finales **: compile la solution complète avec la mise en œuvre et la révision des commentaires

<gigne> <img src = "../. GitBook / Assets / Image (20) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

## Structure d'écoulement complète

{% fichier src = "../. GitBook / Assets / Superviseur Agents Worker.json"%}

## Meilleures pratiques

* Cette architecture consomme beaucoup de jetons en raison des communications de va-et-vient entre les agents, il ne convient donc pas à tous les cas. Il est particulièrement efficace pour:
  * Tâches de développement de logiciels nécessitant à la fois la mise en œuvre et l'examen
  * Résolution de problèmes complexes qui profite de plusieurs perspectives
  * Les flux de travail où la qualité et l'itération sont importantes
  * Tâches qui nécessitent une coordination entre les différents types d'expertise
* Assurez-vous que chaque agent a un rôle spécifique bien défini. Évitez les responsabilités qui se chevauchent qui pourraient entraîner une confusion ou un travail redondant.
* Établir des formats standard pour la façon dont les agents communiquent leurs progrès, leurs résultats et leurs recommandations. Cela aide le superviseur à prendre de meilleures décisions de routage.
* Utilisez les paramètres de mémoire de manière appropriée pour maintenir le contexte de la conversation tout en évitant les problèmes de limite de jetons. Envisagez d'utiliser des paramètres d'optimisation de la mémoire comme le "tampon de résumé de conversation" pour les workflows plus longs.

## Tutoriel vidéo

{% embed url = "https://youtu.be/tbzaj5szcbm?si=e4nxn__hhzjbnwdf"%}
