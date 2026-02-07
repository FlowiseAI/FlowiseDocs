# Affiner

Créez et affinez une réponse en parcourant séquentiellement chaque morceau de texte récupéré.

** Pros **: Bon pour les réponses plus détaillées

** CONS **: appel LLM séparé par nœud (peut être cher)

<gigne> <img src = "../../../. Gitbook / Assets / Image (5) (1) (1) (1) (1) (2) (1) .png" alt = ""> <figCaption> </gigcaption> </ figure>

** Affiner l'invite **

```markup
The original query is as follows: {query}
We have provided an existing answer: {existingAnswer}
We have the opportunity to refine the existing answer (only if needed) with some more context below.
------------
{context}
------------
Given the new context, refine the original answer to better answer the query. If the context isn't useful, return the original answer.
Refined Answer:
```

** Texte du texte QA Invite **

```
Context information is below.
---------------------
{context}
---------------------
Given the context information and not prior knowledge, answer the query.
Query: {query}
Answer:
```
