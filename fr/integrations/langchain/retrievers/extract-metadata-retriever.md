# Extraire les métadonnées Retriever

Ce retriever est conçu pour extraire automatiquement les mots clés de la requête. La sortie JSON extraite est utilisée comme filtre de métadonnées pour le magasin vectoriel.

Par exemple, lorsque nous posons une question: "Quel est le profit pour Apple", LLM donnera une sortie de`{source: "apple"}`, et cela sera transmis au filtre des métadonnées de Vectore Store.

<gigne> <img src = "../../../. GitBook / Assets / Image (5) (6) .png" alt = ""> <figcaption> </gigcaption> </gigust>
