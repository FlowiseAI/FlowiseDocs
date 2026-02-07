# Mémoire MEM0

[Mem0](https://github.com/mem0ai/mem0)(prononcé "mem-zéro") améliore les assistants et agents de l'IA avec une couche de mémoire intelligente, permettant des interactions AI personnalisées. Il se souvient des préférences des utilisateurs, s'adapte aux besoins individuels et s'améliore en continu avec le temps. Cela le rend idéal pour des applications telles que les chatbots de support client, les assistants d'IA et les agents d'IA autonomes.

MEM0 propose une suite complète de fonctionnalités de gestion de la mémoire, permettant une intégration transparente dans diverses applications axées sur l'IA.

---

## Utilisation de MEM0 avec Flowise

Suivez ces étapes pour intégrer MEM0 à Flowise:

### 1. Configuration de Flowise

1. Ouvrez l'application Flowise et créez une nouvelle toile, ou sélectionnez un modèle sur le marché Flowise.
2. Dans cet exemple, nous utilisons le modèle ** de chaîne de conversation **.
3. Remplacez la mémoire de tampon par défaut ** ** par ** MEM0 MEMER **.

<gigne> <img src = "../../../. GitBook / Assets / Mem0 / Flowise-Flow.png" alt = "Flowise Memory Intégration"> <Figcaption> Intégration Flowise avec MEM0 </ Figcaption> </ Figure>

### 2. Obtenez votre clé API MEM0

1. Accédez à la[Mem0 API Key dashboard](https://app.mem0.ai/dashboard/api-keys).
2. Générer ou copier votre clé API MEM0 existante.

<gigne> <img src = "../../../. GitBook / Assets / mem0 / api-key.png" alt = "clés de l'API MEM0"> <Figcaption> Récupère la clé API de MEM0 </ Figcaption> </gigust>

### 3. Configurer les informations d'identification MEM0 en flux

1. Entrez la clé de l'API ** MEM0 ** dans la section des informations d'identification MEM0.

<gigne> <img src = "../../../. gitbook / actifs / mem0 / creds.png" alt = "MEM0 Idementials"> <Figcaption> Configurer les informations d'identification de l'API </figcaption> </pignon>

### 4. Enregistrer et tester le chatflow

1. Enregistrez votre configuration Flowise.
2. Exécutez un chat de test et stockez des informations.

<gigne> <img src = "../../../. GitBook / Assets / Mem0 / Flowise-Chat-1.png" alt = "Flowise Test Chat"> <Figcaption> Test Memory Storage </gigcaption> </gistre>

### 5. Vérifiez les souvenirs stockés dans le tableau de bord MEM0

1. Visiter le[Mem0 Dashboard](https://app.mem0.ai/dashboard/requests)pour examiner les souvenirs stockés.

<gigne> <img src = "../../../. GitBook / Assets / mem0 / flowise-memory.png" alt = "mem0 stocké de souvenirs"> <figcaption> examiner les souvenirs stockés </gigcaption> </ figure>

### 6. Valider la rétention de la mémoire

1. Effacer l'historique de chat dans Flowise.
2. Posez une question basée sur les informations précédemment stockées pour confirmer la rétention.

<gigne> <img src = "../../../. GitBook / Assets / Mem0 / Flowise-Chat-2.png" alt = "Tester Memory Retention"> <Figcaption> Confirmer la persistance de la mémoire </ Figcaption> </stigne>

---

## Paramètres supplémentaires

MEM0 offre diverses options de personnalisation:

<gigne> <img src = "../../../. GitBook / Assets / Mem0 / Settings.png" alt = "MEM0 Settings"> <Figcaption> Options de configuration MEM0 </ FigCaption> </GUFFICE>

1. ** Mode de recherche uniquement **: Active la récupération de la mémoire sans créer de nouvelles souvenirs. L'historique du chat reste jusqu'à ce qu'il soit éliminé manuellement.
2. ** Entités MEM0 **: Utiliser des identifiants tels que`user_id`, `run_id`, `app_id`, et`agent_id`Pour le contrôle de la mémoire granulaire.
3. ** ID du projet **: Attribuez un stockage de mémoire à un projet spécifique. Gérer les projets via[Mem0 Projects](https://app.mem0.ai/settings/projects/overview).
4. ** ID de l'organisation **: attribuer un stockage de mémoire à une organisation spécifique. Gérer les organisations via[Mem0 Organizations](https://app.mem0.ai/settings/organizations/overview).

---

## Configurations de plate-forme MEM0

Des configurations supplémentaires sont disponibles sous[Mem0 Project Settings](https://app.mem0.ai/dashboard/project-settings):

1. ** Instructions personnalisées **: Définissez les instructions au niveau du projet pour affiner l'extraction de la mémoire. Exemple: Extraire uniquement les détails académiques.
2. ** Date d'expiration **: Définissez une période d'expiration pour les souvenirs stockés, permettant l'élimination automatique des données si nécessaire.

<gigne> <img src = "../../../. GitBook / Assets / mem0 / mem0-settings.png" alt = "Paramètres du projet MEM0"> <Figcaption> Personnaliser les paramètres de niveau de projet </gigcaption> </gigust>

---

## Configuration des informations d'identification MEM0 en flux

Pour ajouter des informations d'identification en flux:

1. Accédez aux paramètres des informations d'identification.
2. Ajoutez une nouvelle entrée d'identification pour MEM0.
3. Collez votre[Mem0 API Key](https://app.mem0.ai/dashboard/api-keys)Dans le champ de clé de l'API.

<gigne> <img src = "../../../. GitBook / Assets / mem0 / api-Key.png" alt = "Adding API Key in Flowise"> <Figcaption> Entrée de la touche API dans Flowise </ Figcaption> </ Figure>

---

Avec ces configurations, votre configuration fluide s'intégrera de manière transparente à MEM0, offrant une rétention de mémoire améliorée et des interactions IA personnalisées.

