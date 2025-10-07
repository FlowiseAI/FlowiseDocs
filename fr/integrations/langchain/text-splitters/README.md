---
description: LangChain Text Splitter Nodes
---

# Séparateurs de texte

***

** Lorsque vous souhaitez faire face à de longs morceaux de texte, il est nécessaire de diviser ce texte en morceaux. ** \
Aussi simple que cela puisse paraître, il y a beaucoup de complexité potentielle ici. Idéalement, vous voulez garder les morceaux de texte sémantiquement liés. Ce que signifie «sémantiquement lié» pourrait dépendre du type de texte. Ce cahier présente plusieurs façons de le faire.

** À un niveau élevé, les séparateurs de texte fonctionnent comme suit: **

1. Divisez le texte en petits morceaux sémantiquement significatifs (souvent des phrases).
2. Commencez à combiner ces petits morceaux en un morceau plus grand jusqu'à ce que vous atteigniez une certaine taille (mesurée par une fonction).
3. Une fois que vous avez atteint cette taille, faites de ce morceau son propre texte, puis commencez à créer un nouveau morceau de texte avec un peu de chevauchement (pour garder le contexte entre des morceaux).

** Cela signifie qu'il y a deux axes différents le long desquels vous pouvez personnaliser votre séparateur de texte: **

1. Comment le texte est divisé
2. Comment la taille du morceau est mesurée

### Nœuds de séparateur de texte:

* [Character Text Splitter](character-text-splitter.md)
* [Code Text Splitter](code-text-splitter.md)
* [Html-To-Markdown Text Splitter](html-to-markdown-text-splitter.md)
* [Markdown Text Splitter](markdown-text-splitter.md)
* [Recursive Character Text Splitter](recursive-character-text-splitter.md)
* [Token Text Splitter](token-text-splitter.md)
