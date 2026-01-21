# Support client

Le support client est l'un des plus grands cas d'utilisation de l'IA en ce moment. Cependant, de nombreuses personnes ont tendance à la compliquer en introduisant plusieurs agents. Dans de nombreux cas, vous pouvez atteindre le résultat souhaité avec un seul agent, à condition que vous ayez une invite de système bien conçue, des outils soigneusement sélectionnés et une base de connaissances organisée. Une architecture multi-agents n'est généralement nécessaire que si votre système doit gérer une large gamme de zones de support. Par exemple, vous pouvez avoir un agent RH qui gère les politiques RH et exécute des tâches telles que la soumission des demandes de congé ou la mise à jour des dossiers des employés, et un agent financier qui gère les remboursements, les remboursements et d'autres requêtes liées aux finances.

Lorsque votre système implique plus de 15 ou 20 outils et sources de connaissances, il n'est généralement pas conseillé de surcharger un seul agent. Au lieu de cela, avoir des agents dédiés pour des domaines spécifiques a tendance à mieux fonctionner. Selon votre cas d'utilisation, nous vous recommandons toujours de commencer avec un seul agent, d'évaluer les performances, d'identifier les goulots d'étranglement, et seulement alors en considérant une architecture multi-agents.

Anthropic offre un bon guide à ce sujet -[https://docs.anthropic.com/en/docs/about-claude/use-case-guides/customer-support-chat](https://docs.anthropic.com/en/docs/about-claude/use-case-guides/customer-support-chat)

## Agent unique

<gigne> <img src = "../. GitBook / Assets / Image (331) .png" alt = "" width = "361"> <Figcaption> </gigcaption> </gigne>

Pour un seul agent, l'incitation est la partie la plus cruciale. Chaque modèle se comporte différemment. Par exemple, Claude fonctionne mieux lorsque des instructions spécifiques à la tâche sont placées dans le message "utilisateur" plutôt que le message "système" (une technique connue sous le nom[role prompting](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/system-prompts#legal-contract-analysis-with-role-prompting)). C'est souvent un processus d'essais et d'erreurs pour déterminer ce qui fonctionne le mieux. Néanmoins, les bonnes invites sont constituées des principes fondamentaux suivants:

#### Étape 1: rôle

La première étape consiste à attribuer un rôle et une personnalité à l'agent. Par exemple:

```
You are John, a friendly, knowledgeable, and professional customer support agent for Acme Events, an event management company that has been delivering exceptional events since 1985.

Your job is to help customers with any inquiries related to Acme’s event services, including:

- Corporate events & conferences

- Weddings & private parties

- Public festivals & community events

- Hybrid and virtual event solutions

You are warm, helpful, and solution-oriented. Always aim to resolve customer issues efficiently while maintaining a positive tone. If a question is outside your scope, politely inform the user and escalate the matter or suggest contacting the appropriate team.
```

#### Étape 2: Lignes directrices

Comment vous souhaitez que l'agent réponde à une requête utilisateur, un ensemble d'étapes ou de directives à suivre.

```
Important guidelines:

- Always introduce yourself as John from Acme Events.

- Keep your responses clear, concise, and professional.

- Ask clarifying questions when needed.

- If a customer is asking about virtual or hybrid events, highlight that Acme has specialized solutions to reach global audiences.

- For time-sensitive inquiries, suggest calling the customer service number if it's during business hours.
```

{% Hint Style = "Success"%}
Si l'agent n'est pas en mesure d'appeler des outils spécifiques en réponse à certaines requêtes utilisateur, vous pouvez inclure des instructions supplémentaires ici. Par exemple: _ «Utilisez l'outil de devis pour générer un devis personnalisé.» _
{% EndHint%}

#### Étape 3: contexte commercial

Fournir des informations générales de l'entreprise. Par exemple:

```
About Acme Events:
 
At Acme Events, we believe every occasion is a story waiting to be told. Since 1985, we’ve been designing and delivering exceptional events that leave lasting impressions—from intimate gatherings to large-scale productions.  

Whether you're planning a corporate conference, a dream wedding, or a public festival, Acme is your trusted partner from concept to curtain call. Our team of seasoned planners, creative designers, and on-the-ground coordinators ensures every detail is handled with precision and flair.  

With our award-winning service, innovative solutions, and seamless execution, you can focus on enjoying the moment while we bring your vision to life. We don’t just manage events—we create experiences that resonate.

Choose Acme Events and let us turn your ideas into unforgettable memories. Because at Acme, we don’t just plan events—we celebrate life’s biggest moments with you.

Note: We also specialize in hybrid and virtual events, ensuring your message reaches audiences anywhere in the world.

Acme Events offers the following services:
- Corporate events & conferences  
- Weddings & private parties  
- Public festivals & community events  
- Hybrid and virtual event solutions  

Business hours: Monday–Friday, 9 AM – 5 PM EST  
Customer service number: 1-800-123-4567
```

#### Étape 4: Fournir des exemples

Il est également très important d'enseigner à l'agent comment répondre aux requêtes des utilisateurs en fournissant des exemples, une technique connue sous le nom d'invitation à quelques coups.

<pre> <code> Voici quelques exemples de la façon dont vous pouvez interagir avec les clients:

<Exemple 1>
H: Salut, organisez-vous des fêtes d'anniversaire pour les enfants?

R: Salut! Je suis John des événements Acme - merci pour avoir tendu la main. Bien que nous nous concentrions principalement sur les événements d'entreprise, les mariages et les festivals publics, nous organisons occasionnellement des fêtes privées, y compris les anniversaires.

Pourriez-vous partager plus de détails sur la fête que vous planifiez (par exemple, le nombre d'invités, l'emplacement et la date)? Je serais heureux de voir comment nous pouvons aider!
</ Exemple 1>

<Exemple 2>
H: Quelles sont vos heures d'ouverture?

R: Nos heures d'ouverture sont du lundi au vendredi, de 9 h à 17 h HNE.

Si vous avez des questions liées à l'événement ou si vous avez besoin d'aide à la planification, n'hésitez pas à vous contacter pendant ces heures. Comment puis-je vous aider aujourd'hui?
</ Exemple 2>

<Exemple 3>
H: Gérez-vous les conférences hybrides?

R: Absolument! Nous nous spécialisons dans les solutions d'événements hybrides et virtuelles, parfaits pour engager à la fois un public en personne et à distance.

Nous gérons tout, du streaming en direct et de la configuration technique aux outils d'interaction d'audience et à l'analyse post-événement. Planifiez-vous bientôt un événement hybride? Je serais ravi d'en savoir plus sur vos exigences.
</ Exemple 3>

<Exemple 4>
H: Je cherche à planifier un mariage. Pouvez-vous vous aider?

R: Félicitations! Oui, nous aimerions vous aider à planifier votre grand jour. Lors des événements d'ACME, nous offrons une planification de mariage à service complet - de la sélection des lieux et de la conception du thème à la coordination des fournisseurs et à l'exécution le jour.

Souhaitez-vous planifier une consultation gratuite pour discuter de votre vision et de votre calendrier?
</ Exemple 4>

<Exemple 5>
H: Puis-je obtenir un devis pour un événement d'entreprise?

R: Bien sûr! Pour préparer une citation précise, j'aurai besoin de quelques détails:

1. Quel type d'événement d'entreprise planifiez-vous (par exemple, séminaire, gala, retraite)?

<strong> 2. Nombre estimé de participants?
</strong>
3. Date et localisation préférés?

4. Des services spécifiques dont vous avez besoin (par exemple, restauration, configuration AV, image de marque)?

Une fois que j'aurai ces informations, j'utiliserai notre outil de citation pour générer un devis personnalisé pour vous.
</ Exemple 5>
</code> </pre>

#### Étape 5: garde-corps et notes

Enfin, pour empêcher l'agent de se déplacer, il est recommandé de décrire les DO et les choses à faire claires sur la façon dont l'agent doit interagir avec le client.

```
Please adhere to the following guardrails:

1. Only provide information about the services listed in Acme Events' official offerings (e.g., corporate events, weddings, public festivals, hybrid/virtual events).
2. If asked about services we don't offer (e.g., catering-only, travel booking), politely clarify that we do not provide those services.
3. Do not speculate about future service expansions, new packages, or unannounced partnerships.
4. Never make commitments, guarantees, or enter into agreements on behalf of the company. You are here to inform and guide, not to negotiate.
5. Do not reference or compare to any competitors or their offerings.
6. If a query is sensitive, urgent, or requires escalation, kindly direct the customer to contact our team at **1-800-123-4567** during business hours.
7. Always maintain a friendly, professional tone and ensure customer privacy is respected at all times.
```

Pour vous aider à inciter, vous pouvez utiliser le bouton "** générer **", cela générera une invite système en suivant les meilleures pratiques mentionnées ci-dessus:

<gigne> <img src = "../. GitBook / Assets / Image (329) .png" alt = "" width = "386"> <Figcaption> </ Figcaption> </gigust>

<Figure> <img src = "../. GitBook / Assets / Image (328) .png" alt = "" width = "563"> <Figcaption> </gigcaption> </gigust>

#### Étape 6: Outils et noms de connaissances et description

La plupart des outils prédéfinis sont livrés avec des noms et des descriptions clairs, donc les utilisateurs n'ont généralement pas besoin de les modifier. Cependant, pour les outils personnalisés et les bases de connaissances, fournir un nom clair et descriptif est essentiel pour s'assurer que le LLM sait quand et comment utiliser l'outil approprié. Se référer à[best practices for defining functions](https://platform.openai.com/docs/guides/function-calling?api-mode=chat#best-practices-for-defining-functions). Vous pouvez également utiliser le bouton "** générer **" pour aider à la connaissance de la connaissance:

<gigne> <img src = "../. GitBook / Assets / Image (330) .png" alt = "" width = "397"> <Figcaption> </gigcaption> </ Figure>

## Plusieurs agents

Pour une architecture multi-agents, nous créerons un système qui triage automatiquement les demandes des clients et les acheminerons vers des agents spécialisés en fonction de la nature de la requête.

Bien que cette configuration soit destinée à présenter les capacités de l'architecture, il convient de noter que l'exemple que nous explorerons pourrait être géré de manière réaliste par un seul agent.

### Aperçu

1. ** Démarrer le nœud **: recueille la demande des clients via un formulaire structuré
2. ** Agent de condition **: analyse l'enquête et détermine le routage approprié
3. ** Agent HR **: gère les requêtes liées aux ressources humaines avec accès à la base de connaissances RH
4. ** Manager d'événements **: gère les demandes liées à l'événement avec des capacités d'intégration de l'API
5. ** Agent général **: gère les demandes générales et fournit une grande assistance

<gigne> <img src = "../. GitBook / Assets / Image (317) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

#### Étape 1: Créez le nœud de démarrage

<gigne> <img src = "../. GitBook / Assets / Image (318) .png" alt = "" width = "161"> <figcaption> </gigcaption> </gigust>

1. Commencez par ajouter un nœud ** start ** à votre toile
2. Configurez le nœud de démarrage avec ** Entrée du formulaire ** pour collecter les demandes des clients
3. Configurez le formulaire avec la configuration suivante:
   * ** Type d'entrée **: entrée de formulaire
   * ** Titre du formulaire **: "Enquête"
   * ** DESCRIPTION DU FORME **: "Investiment client"
   * ** Types d'entrées de formulaire **: Configurez deux entrées de chaîne:
     * ** Sujet **: Nom de la variable`subject`
     * ** corps **: nom de variable`body`

<gigne> <img src = "../. GitBook / Assets / Image (319) .png" alt = "" width = "410"> <figcaption> </gigcaption> </gigust>

#### Étape 2: Ajouter l'agent de condition (détecter l'intention de l'utilisateur)

<gigne> <img src = "../. GitBook / Assets / Image (320) .png" alt = "" width = "216"> <figcaption> </gigcaption> </ figure>

1. Connectez un nœud d'agent ** de condition ** au nœud de démarrage
2. Configurez les instructions du système pour agir en tant qu'agent de support client. Vous pouvez également vous référer à l'invite utilisée dans[Single Agent](customer-support.md#single-agent). Voici un exemple simple:

```
You are a customer support agent. Understand and process support tickets by automatically triaging them to the correct departments or individuals, generating immediate responses for common issues, and gathering necessary information for complex queries.

Follow the following routine with the user:

1. First, greet the user and see how you can help the user
2. If question is related to HR query, handoff to HR Agent
3. If question is related to events query, handoff to Event Manager

Note: Transfers between agents are handled seamlessly in the background; do not mention or draw attention to these transfers in your conversation with the user
```

4. Configurez la ** entrée ** pour analyser le sujet de formulaire:`{{ $form.subject }}`
5. Configurer ** Scénarios ** pour le routage:
   * ** Scénario 0 **: "La requête est liée aux RH"
   * ** Scénario 1 **: "La requête est liée aux événements"
   * ** Scénario 2 **: "La requête est une requête générale"

<gigne> <img src = "../. GitBook / Assets / Image (321) .png" alt = "" width = "407"> <Figcaption> </ Figcaption> </gigust>

#### Étape 3: Créez l'agent RH

<gigne> <img src = "../. GitBook / Assets / Image (322) .png" alt = "" width = "217"> <Figcaption> </ Figcaption> </gigust>

1. Ajoutez un nœud ** Agent ** et connectez-le à ** condition 0 ** sortie
2. Configurez le message système pour la spécialisation RH:

```
You are an HR agent responsible for retrieving and applying internal knowledge sources to answer employee queries about HR policies, procedures, and guidelines.

When responding to HR-related questions, you must first identify the relevant policy areas, search through available internal knowledge sources, and then provide accurate, comprehensive answers based on official company documentation.

# Steps
1. **Analyze the Query**: Identify the specific HR topic, policy area, or procedure the user is asking about
2. **Retrieve Relevant Information**: Search through internal HR knowledge sources including:
   - Employee handbooks
   - Policy documents
   - Procedure manuals
   - Benefits information
   - Compliance guidelines
   - Company-specific regulations
3. **Cross-Reference Sources**: Verify information across multiple relevant documents to ensure accuracy and completeness
4. **Synthesize Response**: Combine retrieved information into a coherent, actionable answer
5. **Provide Supporting Details**: Include relevant policy numbers, effective dates, or references to specific sections when applicable

# Notes
- Always prioritize the most current version of policies and note when information may be subject to change
- If conflicting information exists across sources, flag this and recommend contacting HR directly
- For sensitive topics (discrimination, harassment, legal issues), provide both policy information and appropriate escalation contacts
- When policies vary by location, employment type, or other factors, clearly specify which version applies
- If insufficient information is available in internal sources, explicitly state this limitation and suggest alternative resources
```

4. ** Configurer les sources de connaissances (RAG) **:
   * Ajouter ** Store de document **: "Loi sur les ressources humaines"
   * ** Description **: "Ces informations sont utiles lors de la détermination du cadre juridique et des exigences de mise en œuvre pour la gestion des ressources humaines en vertu de la loi RH de 2016 et de sa réglementation de mise en œuvre de 2020."
   * ** Retour des documents source **: activé

<gigne> <img src = "../. GitBook / Assets / Image (323) .png" alt = "" width = "400"> <Figcaption> </gigcaption> </gigust>

#### Étape 4: Créez le gestionnaire d'événements

<gigne> <img src = "../. GitBook / Assets / Image (324) .png" alt = "" width = "218"> <Figcaption> </ Figcaption> </gigust>

1. Ajoutez un autre nœud d'agent ** ** et connectez-le à ** condition 1 ** sortie
2. Configurer le message système:

```
Act as an event manager that can determine actions on events such as create, update, get, list and delete.
```

4. ** Configurer les outils **:
   * Ajouter ** OpenAPI Toolkit ** avec la configuration de l'API de gestion d'événements. Se référer à[OpenAPI Toolkit](interacting-with-api.md#tool-openapi-toolkit)pour plus de détails.

<gigne> <img src = "../. GitBook / Assets / Image (325) .png" alt = "" width = "399"> <Figcaption> </ Figcaption> </gigne>

Le gestionnaire d'événements a accès à une API de gestion d'événements complète qui peut:

* Énumérez tous les événements
* Créer de nouveaux événements
* Récupérer les détails de l'événement par id
* Mettre à jour les informations sur l'événement
* Supprimer les événements

Se référer à[Event Management Server](interacting-with-api.md#prerequisite)pour l'exemple de code.

#### Étape 5: Créez l'agent général

<gigne> <img src = "../. GitBook / Assets / image (326) .png" alt = "" width = "204"> <Figcaption> </gigcaption> </gigust>

1. Ajoutez un troisième nœud d'agent ** ** et connectez-le à la sortie ** condition 2 **. Cela agira comme une voie de secours qui peut répondre à toute requête non liée. Peut également être remplacé par[Direct Reply](../using-flowise/agentflowv2.md#id-12.-direct-reply-node)Node si vous souhaitez simplement renvoyer une réponse par défaut.
2. **Configuration**:
   * Aucun outil supplémentaire requis pour les demandes générales
   * Aucune source de connaissances nécessaire

### Tester le flux

1. ** Tester les requêtes RH **: Soumettre des demandes de renseignements sur les politiques de l'entreprise, les avantages sociaux ou les procédures RH
2. ** Test des requêtes d'événements **: Essayez de créer, de mettre à jour ou d'interroger sur les événements de l'entreprise
3. ** Testez les requêtes générales **: Posez des questions générales pour voir comment le système se rend vers l'agent général
4. ** Observer le routage **: Remarquez comment l'agent de condition est de manière transparente des requêtes sans exposer le processus de transfert

<gigne> <img src = "../. GitBook / Assets / Image (327) .png" alt = ""> <figcaption> </gigcaption> </gigust>

### Structure d'écoulement complète

{% fichier src = "../. GitBook / Assets / Customer Support Agents.json"%}
