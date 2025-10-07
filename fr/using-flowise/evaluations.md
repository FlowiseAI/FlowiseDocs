# Évaluations

{% hint style = "info"%}
Les évaluations ne sont disponibles que pour le cloud et le plan d'entreprise
{% EndHint%}

Les évaluations vous aident à surveiller et à comprendre les performances de votre application ChatFlow / AgentFlow. Au niveau élevé, une évaluation est un processus qui prend un ensemble d'entrées et de sorties correspondantes de votre ChatFlow / AgentFlow, et génère des scores. Ces scores peuvent être dérivés en comparant les sorties aux résultats de référence, tels que par correspondance de cordes, une comparaison numérique ou même en tirant parti d'un LLM en tant que juge. Ces évaluations sont effectuées à l'aide de ensembles de données et d'évaluateurs.

## Ensembles de données

Les ensembles de données sont les entrées qui seront utilisées pour exécuter votre ChatFlow / AgentFlow, ainsi que les sorties correspondantes pour la comparaison. L'utilisateur peut ajouter manuellement l'entrée et la sortie prévue ou télécharger un fichier CSV avec 2 colonnes: entrée et sortie.

<gigne> <img src = "../. GitBook / Assets / Image (3) (3) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

| Entrée | Sortie |
| --------------------------------- | ---------------------------- |
| Quelle est la capitale du Royaume-Uni | La capitale du Royaume-Uni est Londres |
| Combien de jours y a-t-il dans un an | Il y a 365 jours par an |

## Évaluateurs

Les évaluateurs sont comme des tests unitaires. Au cours d'une évaluation, les entrées des ensembles de données sont exécutées sur les flux sélectionnés et les sorties sont évaluées à l'aide d'évaluateurs sélectionnés. Il existe 3 types d'évaluateurs:

* ** basé sur le texte **: Vérification basée sur les chaînes:
  * Contient tout
  * Contient tout
  * Ne contient aucun
  * Ne contient pas tout
  * Commence par
  * Ne commence pas par

<gigne> <img src = "../. GitBook / Assets / Image (6) (2) .png" alt = ""> <Figcaption> </ Figcaption> </gigne>

* ** Basée numérique: ** Type de nombres Vérification:
  * Jetons totaux
  * Jetons rapides
  * Jetons d'achèvement
  * Latence API
  * Latence LLM
  * Latence de chatflow
  * LAFENCE DE LA FLOW Agent (à venir)
  * Longueur de caractères de sortie

<gigne> <img src = "../. GitBook / Assets / Image (7) (2) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

* ** LLM basé sur LLM **: Utilisation d'un autre LLM pour noter la sortie
  * Hallucination
  * Correction

<gigne> <img src = "../. GitBook / Assets / Image (9) (2) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

## Évaluations

Maintenant que nous avons préparé des ensembles de données et des évaluateurs, nous pouvons commencer à exécuter une évaluation.

1.) Sélectionnez DataSet et ChatFlow pour évaluer. Vous pouvez sélectionner plusieurs ensembles de données et chatflows. En utilisant l'exemple ci-dessous, toutes les entrées de DataSet1 seront exécutées contre 2 ChatFlows. Étant donné que DataSet1 a 2 entrées, un total de 4 sorties seront produites et évaluées.

<gigne> <img src = "../. GitBook / Assets / Image (10) (2) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

2.) Sélectionnez les évaluateurs. Seuls des évaluateurs basés sur des chaînes et basés sur des chaînes sont disponibles pour être sélectionnés à ce stade.

<gigne> <img src = "../. GitBook / Assets / Image (11) (2) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

3.) (Facultatif) Sélectionnez l'évaluateur basé sur LLM. Démarrer l'évaluation:

<gigne> <img src = "../. GitBook / Assets / Image (12) (2) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

4.) Attendez que l'évaluation soit terminée:

<gigne> <img src = "../. GitBook / Assets / Image (13) (2) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

5.) Une fois l'évaluation terminée, cliquez sur l'icône du graphique sur le côté droit pour afficher les détails:

<gigne> <img src = "../. GitBook / Assets / Image (14) (2) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

Les 3 graphiques ci-dessus montrent le résumé de l'évaluation:

* Tarif de réussite / échouer
* Invite moyen et jetons d'achèvement utilisés
* Latence moyenne de la demande

Le tableau sous les graphiques montre les détails de chaque exécution.

<gigne> <img src = "../. GitBook / Assets / Image (15) (2) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

<gigne> <img src = "../. GitBook / Assets / Image (16) (2) .png" alt = "" width = "355"> <Figcaption> </ Figcaption> </gigne>

### Réévaluer une réévaluation

Lorsque les flux utilisés sur l'évaluation ont été mis à jour / modifiés, un message d'avertissement sera affiché:

<gigne> <img src = "../. GitBook / Assets / Image (17) (2) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

Vous pouvez réexaminer la même évaluation en utilisant le bouton d'évaluation à nouveau dans le coin supérieur droit. Vous pourrez voir les différentes versions:

<gigne> <img src = "../. GitBook / Assets / Image (18) (2) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

Vous pouvez également afficher et comparer les résultats de différentes versions:

<gigne> <img src = "../. GitBook / Assets / Image (19) (2) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

## Tutoriel vidéo

{% embed url = "https://youtu.be/kgutthmkgfg?si=3rlplep_0ti0p6uv&t=486"%}
