# Gmail

## Créer des informations d'identification dans Flowise

1. Ajouter un nouvel diplôme Gmail OAuth2
2. Entrez un nom pour les informations d'identification.
3. Copiez l'URL de redirection OAuth.
4. Notez que les champs suivants doivent être remplis:
   * ID client
   * Secret client

<gigne> <img src = "../../../. GitBook / Assets / Image (255) .png" alt = "" width = "437"> <Figcaption> </ Figcaption> </ Figure>

## Créer / utiliser Google Project

1. Connectez-vous à votre[**Google Cloud**](https://console.cloud.google.com/)compte.
2. Se diriger vers[**Google Cloud Console > APIs & Services**](https://console.cloud.google.com/apis/credentials), et sélectionnez le projet que vous souhaitez utiliser dans la liste déroulante en haut à gauche (ou créez un nouveau projet et sélectionnez-le).
3. Configurez l'écran de consentement ** OAuth ** Si vous n'en avez pas confiuré auparavant.

<gigne> <img src = "../../../. GitBook / Assets / Image (256) .png" alt = "" width = "563"> <Figcaption> </ Figcaption> </ Figure>

4. Accédez à ** des informations d'identification **, puis cliquez sur ** + Créer des informations d'identification> ID client OAuth **.

<gigne> <img src = "../../../. GitBook / Assets / Image (257) .png" alt = "" width = "563"> <Figcaption> </ Figcaption> </ Figure>

5. Dans le ** Type d'application ** Dropdown, sélectionnez ** Application Web **.
6. Sous ** URIS REDIRECT AMÉCIRISÉ **, cliquez sur ** + Ajouter URI ** et collez l'URL de redirection OAuth copiée plus tôt.
7. Cliquez sur ** Créer **.

<gigne> <img src = "../../../. GitBook / Assets / Image (258) .png" alt = "" width = "407"> <Figcaption> </gigcaption> </gigust>

8. Copiez l'ID client et le secret du client:

<gigne> <img src = "../../../. GitBook / Assets / Image (259) .png" alt = "" width = "489"> <Figcaption> </ Figcaption> </gigne>

9. Dans ** APPATIVE API & SERVICES **, cliquez sur ** + Activer les API et Services **.
10. Recherchez et activez l'API ** Gmail **.

<gigne> <img src = "../../../. GitBook / Assets / Image (260) .png" alt = "" width = "538"> <Figcaption> </ Figcaption> </ Figure>

11. Retour à ** Indementiels **, cliquez sur les informations d'identification nouvellement créées sous ** OAuth 2.0 Client IDS **, et sur la page de détail, vous trouverez le ** Client ID ** et ** Client Secret **.

## Terminer la configuration en flux

1. Remplissez toutes les valeurs copiées plus tôt. Puis cliquez sur "** Authenticiate **":

<gigne> <img src = "../../../. GitBook / Assets / Image (262) .png" alt = "" width = "433"> <Figcaption> </ Figcaption> </ Figure>

2. Une fenêtre de connexion Google apparaîtra:

<gigne> <img src = "../../../. GitBook / Assets / Image (261) .png" alt = "" width = "448"> <Figcaption> </ Figcaption> </ Figure>

3. Accorder les autorisations:

<gigne> <img src = "../../../. GitBook / Assets / Image (263) .png" alt = "" width = "373"> <Figcaption> </ Figcaption> </ Figure>

4. La fenêtre pop-up sera fermée automatiquement et les informations d'identification seront enregistrées et prêtes à être utilisées.

## Utiliser comme outil d'agent

Plusieurs actions peuvent être sélectionnées pour permettre à l'agent de choisir intelligemment celui approprié. \
Les paramètres peuvent être laissés vides pour permettre à l'agent de déterminer les valeurs par elle-même. Cependant, si l'utilisateur fournit des valeurs, ceux-ci remplaceront les choix de l'agent.

<gigne> <img src = "../../../. GitBook / Assets / Image (264) .png" alt = ""> <Figcaption> </gigcaption> </gigne>

## Utiliser comme nœud d'outil

Il peut également être utilisé comme nœud d'outil dans un scénario de flux de travail déterminé. Par exemple, la récupération d'une liste de messages projets avant de passer à l'étape suivante. \
Dans ce mode, ** Les arguments d'entrée de l'outil doivent être explicitement définis et remplis de valeurs **. \
Contrairement au[**Use as Agent Tool**](gmail.md#use-as-agent-tool)Option, il n'y a pas d'agent pour déterminer automatiquement les entrées. L'utilisateur doit remplir manuellement les champs, soit en entrant des valeurs fixes, soit en utilisant des variables enfermées dans des supports doubles bouclés`{{ }}`.

<gigne> <img src = "../../../. GitBook / Assets / Image (265) .png" alt = ""> <Figcaption> </gigcaption> </gigne>
