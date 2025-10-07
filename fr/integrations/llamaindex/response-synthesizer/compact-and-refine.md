# Compact et affiner

Il s'agit de la valeur par défaut lorsqu'aucun synthétiseur de réponse n'est défini en explile.

Compacte l'invite lors de chaque appel LLM en remplissant autant de morceaux de texte qui peuvent s'adapter à la taille invite maximale. S'il y a trop de morceaux pour faire des trucs dans une invite, "Créez et affinez" une réponse en passant par plusieurs invites compactes.

** Pros **: le même que[Refine](refine.md), Bon pour les réponses plus détaillées, et devrait entraîner des appels LLM moins

** CONS **: En raison des multiples appels LLM, peut être coûteux

<gigne> <img src = "../../../. GitBook / Assets / Image (6) (1) (1) (1) (2) .png" alt = ""> <figcaption> </ Figcaption> </gigne>

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
