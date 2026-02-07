---
description: LangChain Record Manager Nodes
---

# Gestionnaires de dossiers

***

Les gestionnaires d'enregistrement gardent une trace de vos documents indexés, empêchant les intégres du vecteur dupliqués dans[Vector Store](vector-stores/).

Lorsque des morceaux de document augmentent, chaque morceau sera haché en utilisant[SHA-1](https://github.com/emn178/js-sha1)algorithme. Ces hachages seront stockés dans Record Manager. S'il y a un hachage existant, le processus d'incorporation et de mise en service sera ignoré.

Dans certains cas, vous voudrez peut-être supprimer les documents existants dérivés des mêmes sources que les nouveaux documents indexés. Pour cela, il existe 3 modes de nettoyage pour le gestionnaire de disques:

{% Tabs%}
{% tab title = "incrémentiel"%}
Lorsque vous augmentez plusieurs documents et que vous souhaitez empêcher la suppression des documents existants qui ne font pas partie du processus de mise en service actuel, utilisez le mode de nettoyage ** ** **.

1. Ayons un gestionnaire de disques avec`Incremental`Nettoyage et`source`En tant que clé sourceid

<div align = "Left"> <Figure> <img src = "../../. Gitbook / Assets / Image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2) .png" Alt = "" width = "264"> <Figcaption> </Figcaption> src = "../../. Gitbook / Assets / Image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2) .png" alt = "" width = "410"> <figcaption> </gigcaption> </figne> </v>

2. Et avoir les 2 documents suivants:

| Texte | Métadonnées |
| ---- | ---------------- |
| Cat |`{source:"cat"}` |
| Chien |`{source:"dog"}` |

<div align = "Left"> <Figure> <img src = "../../. GitBook / Assets / Image (11) (1) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "202"> <figcaption> </figcaption> </ figure> <figure <img src = "../. (10) (1) (1) (1) (1) (1) (1) (2) .png "alt =" "width =" 563 "> <figcaption> </gigcaption> </gigust> </div>

<div align = "Left"> <Figure> <img src = "../../. GitBook / Assets / Image (2) (1) (1) (1) (1) (2) .png" alt = "" width = "231"> <figcaption> </ figcaption> </ Figure> <Figure> <img src = "..//. (1) (1) (1) (1) (1) (2) .png "alt =" "width =" 563 "> <Figcaption> </gigcaption> </gigust> </div>

3. Après un upsert, nous verrons 2 documents qui sont mis en place:

<gigne> <img src = "../../. GitBook / Assets / Image (9) (1) (1) (1) (1) (2) .png" alt = "" width = "433"> <Figcaption> </gigcaption> </gigu

4. Maintenant, si nous supprimons le document ** Dog ** et mettons à jour ** Cat ** à ** Cats **, nous verrons maintenant ce qui suit:

<gigne> <img src = "../../. GitBook / Assets / Image (13) (2) (2) .png" alt = "" width = "425"> <Figcaption> </ Figcaption> </gigne>

* Le document original ** Cat ** est supprimé
* Un nouveau document avec ** Cats ** est ajouté
* ** Dog ** Le document est laissé intact
* Les incorporations vectorielles restantes dans le magasin vectoriel sont ** chats ** et ** chien **

<gigne> <img src = "../../. GitBook / Assets / Image (15) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "448"> <Figcaption> </ FigCaption> </ Figure>
{% endtab%}

{% tab title = "full"%}
Lorsque vous augmentez plusieurs documents, ** le mode de nettoyage complet ** supprimera automatiquement tous les intérêts vectoriels qui ne font pas partie du processus de mise en service actuel.

1. Ayons un gestionnaire de disques avec`Full`Nettoyage. Nous n'avons pas besoin d'avoir une clé sourceid pour le mode de nettoyage complet.

<div align = "Left"> <Figure> <img src = "../../. Gitbook / Assets / Image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2) .png" Alt = "" width = "264"> <Figcaption> </Figcaption> src = "../../. GitBook / Assets / Image (17) (1) (1) (1) (2) .png" alt = "" width = "407"> <Figcaption> </ FigCaption> </gigust> </div>

2. Et avoir les 2 documents suivants:

| Texte | Métadonnées |
| ---- | ---------------- |
| Cat |`{source:"cat"}` |
| Chien |`{source:"dog"}` |

<div align = "Left"> <Figure> <img src = "../../. GitBook / Assets / Image (11) (1) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "202"> <figcaption> </figcaption> </ figure> <figure <img src = "../. (10) (1) (1) (1) (1) (1) (1) (2) .png "alt =" "width =" 563 "> <figcaption> </gigcaption> </gigust> </div>

<div align = "Left"> <Figure> <img src = "../../. GitBook / Assets / Image (2) (1) (1) (1) (1) (2) .png" alt = "" width = "231"> <figcaption> </ figcaption> </ Figure> <Figure> <img src = "..//. (1) (1) (1) (1) (1) (2) .png "alt =" "width =" 563 "> <Figcaption> </gigcaption> </gigust> </div>

3. Après un upsert, nous verrons 2 documents qui sont mis en place:

<gigne> <img src = "../../. GitBook / Assets / Image (9) (1) (1) (1) (1) (2) .png" alt = "" width = "433"> <Figcaption> </gigcaption> </gigu

4. Maintenant, si nous supprimons le document ** Dog ** et mettons à jour ** Cat ** à ** Cats **, nous verrons maintenant ce qui suit:

<gigne> <img src = "../../. GitBook / Assets / Image (18) (1) (1) (1) (2) .png" alt = "" width = "430"> <Figcaption> </ Figcaption> </ Figure>

* Le document original ** Cat ** est supprimé
* Un nouveau document avec ** Cats ** est ajouté
* ** Dog ** Le document est supprimé
* Les incorporations vectorielles restantes dans le magasin vectoriel sont juste ** Cats **

<gigne> <img src = "../../. GitBook / Assets / Image (19) (1) (1) (1) .png" alt = "" width = "527"> <Figcaption> </ Figcaption> </gigu
{% endtab%}

{% Tab Title = "None"%}
Aucun nettoyage ne sera effectué
{% endtab%}
{% endtabs%}

Les nœuds actuels disponibles des gestionnaires d'enregistrement sont:

* Sqlite
* Mysql
* Postgresql

## Ressources

* [LangChain Indexing - How it works](https://js.langchain.com/docs/how_to/indexing/#how-it-works)
