# Chargeur de fichiers S3

Amazon S3 (Simple Storage Service) est un service de stockage d'objets offrant une évolutivité de pointe, une disponibilité des données, une sécurité et des performances. Ce module fournit des fonctionnalités complètes pour charger et traiter les fichiers stockés dans des seaux S3.

Ce module fournit un chargeur de document S3 sophistiqué qui peut:
- Chargez des fichiers à partir de seaux S3 à l'aide d'identification AWS
- Prise en charge de plusieurs formats de fichiers (PDF, DOCX, CSV, Excel, PowerPoint, Fichiers texte)
- Processus de fichiers à l'aide de chargeurs intégrés ou non structurés.io API
- Gérer le texte et les fichiers binaires
- Personnaliser l'extraction des métadonnées

## Entrées

### Paramètres requis
- ** seau **: le nom du seau S3
- ** Clé d'objet **: l'identifiant unique de l'objet dans le seau S3
- ** Région **: Région AWS où se trouve le seau (par défaut: US-East-1)

### Options de traitement
- ** Méthode de traitement des fichiers **: Choisissez entre:
  - Intégrés de chargeurs: utilisez des processeurs de format de fichiers natifs
  - Non structuré: utilisez une API non structurée.io pour un traitement avancé
- ** Splitter de texte ** (facultatif): séparateur de texte pour le traitement intégré
- ** Métadonnées supplémentaires ** (Facultatif): objet JSON avec métadonnées supplémentaires
- ** omettre les clés de métadonnées ** (facultative): clés pour omettre des métadonnées

### Options non structurées.io
- ** URL API non structurée **: point de terminaison pour API non structuré.io
- ** Clé API non structurée ** (facultatif): clé API pour l'authentification
- ** Stratégie **: Stratégie de traitement (Hi_res, Fast, OCR_ONLY, AUTO)
- ** Encodage **: Méthode de codage de texte (par défaut: UTF-8)
- ** Sauter les types de tables inférieurs **: Types de documents pour sauter l'extraction de la table

## Sorties

- ** Document **: tableau d'objets de document contenant des métadonnées et un conceptent
- ** Texte **: chaîne concaténée du conceptent de documents

## Caractéristiques
- Intégration AWS S3
- Prise en charge du format de fichier multiple
- Traitement intégré et non structuré.io
- Régions AWS configurables
- Manipulation flexible des métadonnées
- Traitement de fichiers binaires
- Gestion temporaire des fichiers
- Détection de type mime

## Types de fichiers pris en charge
- Documents PDF
- Microsoft Word (DOCX)
- Microsoft Excel
- Microsoft PowerPoint
- Fichiers CSV
- Fichiers texte
- Et plus par non structuré.io

## Notes
- Nécessite des informations d'identification AWS (facultative si vous utilisez des rôles IAM)
- Certains types de fichiers peuvent nécessiter des méthodes de traitement spécifiques
- L'API non structurée.io nécessite une configuration et des informations d'identification distinctes
- Les fichiers temporaires sont créés et gérés automatiquement
- Gestion des erreurs pour les types de fichiers non pris en charge

## Configuration non structurée

Vous pouvez soit utiliser l'API hébergé ou exécuter localement via Docker.

* [Hosted API](https://unstructured-io.github.io/unstructured/api.html)
* Docker:`docker run -p 8000:8000 -d --rm --name unstructured-api quay.io/unstructured-io/unstructured-api:latest --port 8000 --host 0.0.0.0`

## Configuration du chargeur de fichiers S3

1 \ \. Faites glisser et déposez le chargeur de fichiers S3 sur Canvas:

<gigne> <img src = "../../../. GitBook / Assets / Image (71) .png" alt = "" width = "234"> <Figcaption> </gigcaption> </ figure>

2 \. Indemnité AWS: créez un nouvel diplôme pour votre compte AWS. Vous aurez besoin de l'accès et de la clé secrète. N'oubliez pas d'accorder la politique du seau S3 au compte associé. Vous pouvez vous référer au guide politique[here](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/AuroraMySQL.Integrating.Authorizing.IAM.S3CreatePolicy.html).

<gigne> <img src = "../../../. GitBook / Assets / Image (72) .png" alt = "" width = "551"> <Figcaption> </ Figcaption> </ Figure>

3. Bodet: Connectez-vous à votre console AWS et accédez à S3. Obtenez le nom de votre seau:

<gigne> <img src = "../../../. GitBook / Assets / Image (73) .png" alt = ""> <Figcaption> </gigcaption> </gigne>

4. Clé: cliquez sur l'objet que vous souhaitez utiliser et obtenez le nom de la clé:

<gigne> <img src = "../../../. GitBook / Assets / Image (75) .png" alt = "" width = "228"> <Figcaption> </ Figcaption> </ Figure>

5. URL de l'API non structurée: Selon la façon dont vous utilisez non structuré, que ce soit via une API ou Docker hébergée, modifiez le paramètre URL de l'API non structuré. Si vous utilisez une API hébergée, vous aurez également besoin de la touche API.
6. Vous pouvez ensuite commencer à discuter avec votre fichier depuis S3. Vous n'avez pas à spécifier le séparateur de texte pour la réduction du document car il est géré automatiquement par non-structuré.

<gigne> <img src = "../../../. GitBook / Assets / Screly-1698767992182.png" alt = ""> <Figcaption> </ FigCaption> </gigust>

