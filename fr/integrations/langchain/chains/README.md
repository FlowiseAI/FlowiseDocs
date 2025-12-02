---
description: Nœuds de chaîne LangChain
---

# Chaînes

***

Dans le contexte des chatbots et des grands modèles de langage, les "chaînes" désignent généralement des séquences de texte ou de tours de conversation. Ces chaînes sont utilisées pour stocker et gérer l'historique de la conversation et le contexte pour le chatbot ou le modèle de langage. Les chaînes aident le modèle à comprendre la conversation en cours et à fournir des réponses cohérentes et contextuellement pertinentes.

Voici comment fonctionnent les chaînes :

1. **Historique de la conversation** : Lorsque un utilisateur interagit avec un chatbot ou un modèle de langage, la conversation est souvent représentée comme une série de messages texte ou de tours de conversation. Chaque message de l'utilisateur et du modèle est stocké dans l'ordre chronologique pour maintenir le contexte de la conversation.
2. **Entrée et sortie** : Chaque chaîne se compose à la fois de l'entrée de l'utilisateur et de la sortie du modèle. L'entrée de l'utilisateur est généralement appelée la "chaîne d'entrée", tandis que les réponses du modèle sont stockées dans la "chaîne de sortie". Cela permet au modèle de se référer aux messages précédents dans la conversation.
3. **Compréhension contextuelle** : En préservant l'ensemble de l'historique de la conversation dans ces chaînes, le modèle peut comprendre le contexte et se référer à des messages antérieurs pour fournir des réponses cohérentes et contextuellement pertinentes. Cela est crucial pour maintenir une conversation naturelle et significative avec les utilisateurs.
4. **Longueur maximale** : Les chaînes ont une longueur maximale pour gérer l'utilisation de la mémoire et les ressources informatiques. Lorsque une chaîne devient trop longue, des messages plus anciens peuvent être supprimés ou tronqués pour faire de la place pour de nouveaux messages. Cela peut potentiellement entraîner une perte de contexte si des détails importants de la conversation sont supprimés.
5. **Continuation de la conversation** : Dans une interaction en temps réel avec un chatbot ou un modèle de langage, la chaîne d'entrée est continuellement mise à jour avec les nouveaux messages de l'utilisateur, et la chaîne de sortie est mise à jour avec les réponses du modèle. Cela permet au modèle de suivre la conversation en cours et de répondre de manière appropriée.

Les chaînes sont un concept fondamental dans la construction et le maintien des conversations des chatbots et des modèles de langage. Elles garantissent que le modèle a accès au contexte dont il a besoin pour générer des réponses significatives et conscientes du contexte, rendant l'interaction plus engageante et utile pour les utilisateurs.

### Nœuds de chaîne :

* [GET API Chain](get-api-chain.md)
* [OpenAPI Chain](openapi-chain.md)
* [POST API Chain](post-api-chain.md)
* [Conversation Chain](conversation-chain.md)
* [Conversational Retrieval QA Chain](conversational-retrieval-qa-chain.md)
* [LLM Chain](llm-chain.md)
* [Multi Prompt Chain](multi-prompt-chain.md)
* [Multi Retrieval QA Chain](multi-retrieval-qa-chain.md)
* [Retrieval QA Chain](retrieval-qa-chain.md)
* [Sql Database Chain](sql-database-chain.md)
* [Vectara QA Chain](vectara-chain.md)
* [VectorDB QA Chain](vectordb-qa-chain.md)