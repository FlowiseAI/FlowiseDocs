# Feuilles Google

<gigne> <img src = "../../../. GitBook / Assets / Image (283) .png" alt = "" width = "234"> <Figcaption> </ Figcaption> </ Figure>

Google Sheets est une application de feuille de calcul basée sur le Web. Ce module fournit des fonctionnalités pour charger et traiter les données à partir des documents Google Sheets, en prenant en charge diverses options de formatage de données et la sélection des feuilles.

Ce module fournit un chargeur de documents Google Sheets sophistiqué qui peut:

* Chargez les données de plusieurs feuilles de calcul
* Sélectionnez des feuilles et des gammes spécifiques
* Gérer les valeurs formatées et non formatées
* Processus des formules et calculs
* Personnaliser la manipulation de l'en-tête
* Traiter le contenu avec des séparateurs de texte
* Gérer l'authentification OAuth2

## Entrées

### Paramètres requis

* ** Connectez les informations d'identification **: Google Sheets OAuth2 Informations d'identification. Se référer à[#Google Sheets](../tools/google-sheets.md)
* ** Sélectionnez le feuille de calcul **: Choisissez des feuilles de calcul (s) dans votre Google Drive

### Paramètres facultatifs

* ** Noms de feuilles **: Liste des noms de feuilles séparées par des virgules à charger
* ** Plage **: Plage spécifique à charger (par exemple, A1: E10)
* ** Inclure les en-têtes **: s'il faut inclure la première ligne comme en-têtes (par défaut: true)
* ** Option de rendu de valeur **: comment les valeurs doivent être représentées:
  * Valeur formatée: comme indiqué dans l'interface utilisateur
  * Valeur non formatée: valeurs brutes
  * Formule: formules originales
* ** Splitter du texte **: un séparateur de texte pour traiter le contenu extrait
* ** Metadata supplémentaires **: objet JSON avec métadonnées supplémentaires
* ** omettre les clés de métadonnées **: Liste des clés de métadonnées séparées par des virgules pour omettre

## Sorties

* ** Document **: tableau d'objets de document contenant des métadonnées et un conceptent
* ** Texte **: chaîne concaténée du conceptent de documents

## Caractéristiques

* Authentification OAuth2
* Prise en charge de la feuille de calcul multiple
* Sélection de feuilles
* Spécification de plage
* Manipulation en tête
* Options de formatage de valeur
* Support de division de texte
* Personnalisation des métadonnées

## Options de rendu de valeur

### Valeur formatée

* Valeurs affichées dans l'interface utilisateur
* Comprend le formatage
* Nombres avec des décimales / devises
* Dates au format spécifié

### Valeur non formatée

* Valeurs de cellules brutes
* Nombres sans formatage
* Dates comme numéros de série
* Booléen comme vrai / faux

### Formule

* Formules originales
* Références cellulaires
* Fonctions
* Calculs

## Structure de document

Chaque document contient:

* ** PageContent **: Contenu de la feuille formatée
* ** Metadata **:
  * SpreadsheetID: ID Google Sheets
  * nom de calcul: nom de document
  * Nom de feuille: nom de la feuille
  * Plage: plage sélectionnée
  * En-têtes: en-têtes de colonne (si inclus)
  * LastModified: Dernière date de modification
  * URL: lien de vue Web
  * Métadonnées personnalisées supplémentaires

## Notes

* Nécessite une authentification OAuth2
* Les manches limites de taux
* Prend en charge de grandes feuilles de calcul
* Traitement économe en mémoire
* Gestion des erreurs pour les gammes non valides
* Rafraîchissement automatique des jetons
* Accès aux données en temps réel
