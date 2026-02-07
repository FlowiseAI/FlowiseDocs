# Microsoft Outlook

## Condition préalable

Une licence Microsoft 365 valide attribuée à l'utilisateur Azure Active Directory. Référer:[https://learn.microsoft.com/en-us/answers/questions/761931/microsoft-graph-api-throws-the-mailbox-is-either-i](https://learn.microsoft.com/en-us/answers/questions/761931/microsoft-graph-api-throws-the-mailbox-is-either-i)

## Créer des informations d'identification dans Flowise

1. Ajouter un nouvel diplôme Microsoft Outlook OAuth2
2. Entrez un nom pour les informations d'identification.
3. Copiez l'URL de redirection OAuth.
4. Notez que les champs suivants doivent être remplis:
   * URL d'autorisation
   * URL du jeton d'accès
   * ID client
   * Secret client

<gigne> <img src = "../../../. GitBook / Assets / Image (175) .png" alt = "" width = "437"> <Figcaption> </ Figcaption> </ Figure>

## Créez une application Azure <a href = "# h_8276f6aa94" id = "h_8276f6aa94"> </a>

1. Connectez-vous à votre existant[Azure](https://login.microsoftonline.com/)compte ou[sign up](https://signup.live.com/signup)Si vous ne vous êtes pas déjà inscrit
2. Recherchez ** les inscriptions d'applications **.
3. Ensuite, enregistrez une nouvelle application Azure dans[app registrations](https://portal.azure.com/#view/Microsoft_AAD_RegisteredApps/CreateApplicationBlade/quickStartType~/null/isMSAApp~/false).

<gigne> <img src = "../../../. GitBook / Assets / Image (192) .png" alt = "" width = "304"> <Figcaption> </ Figcaption> </ Figure>

4. Sous "Redirection URI (Facultatif)", sélectionnez "Web" et collez votre "URL de redirection OAuth" que vous avez copié plus tôt.

<gigne> <img src = "../../../. GitBook / Assets / Image (197) .png" alt = ""> <Figcaption> </gigcaption> </gigne>

5. Après une application créée, accédez à ** Certificats et secrets **> ** Secrets du client ** et cliquez sur le bouton "** Nouveau client Secret **" pour créer un secret client. Copiez le secret à utiliser plus tard.

<gigne> <img src = "../../../. GitBook / Assets / Image (200) .png" alt = ""> <Figcaption> </gigcaption> </gigne>

6. Naviguez vers ** Présentation ** et cliquez sur "** Points de terminaison **". Copiez les points de terminaison pour "** OAuth 2.0 Autorisation Endpoint (V2) **" et "** OAuth 2.0 Token Endpoint (V2) **".

<gigne> <img src = "../../../. GitBook / Assets / Image (202) .png" alt = ""> <Figcaption> </gigcaption> </gigne>

7. Fermez la fenêtre contextuelle des points de terminaison, copiez l'application ** (client) ID **:

<gigne> <img src = "../../../. GitBook / Assets / Image (203) .png" alt = "" width = "563"> <Figcaption> </ Figcaption> </ Figure>

## Terminer la configuration en flux

1. Remplissez toutes les valeurs copiées plus tôt. Puis cliquez sur "** Authenticiate **":

<gigne> <img src = "../../../. GitBook / Assets / Image (204) .png" alt = "" width = "440"> <Figcaption> </ Figcaption> </ Figure>

2. Une fenêtre Microsoft apparaîtra, sélectionnez le compte.

{% hint style = "avertissement"%}
L'utilisateur du compte doit avoir une sous-licence Microsoft 365 valide
{% EndHint%}

<gigne> <img src = "../../../. GitBook / Assets / Image (205) .png" alt = "" width = "459"> <Figcaption> </ Figcaption> </ Figure>

3. Accorder les autorisations requises:

<gigne> <img src = "../../../. GitBook / Assets / Image (206) .png" alt = "" width = "454"> <Figcaption> </ Figcaption> </gigne>

4. Le pop-up se fermera automatiquement et les informations d'identification seront enregistrées par la suite.

## Utiliser comme outil d'agent

Plusieurs actions peuvent être sélectionnées pour permettre à l'agent de choisir intelligemment celui approprié. \
Les paramètres peuvent être laissés vides pour permettre à l'agent de déterminer les valeurs par elle-même. Cependant, si l'utilisateur fournit des valeurs, ceux-ci remplaceront les choix de l'agent.

<gigne> <img src = "../../../. GitBook / Assets / Image (207) .png" alt = ""> <Figcaption> </gigcaption> </gigne>

## Utiliser comme nœud d'outil

Il peut également être utilisé comme nœud d'outil dans un scénario de flux de travail déterminé. Par exemple, la récupération d'une liste des messages Outlook avant de passer à l'étape suivante. \
Dans ce mode, ** Les arguments d'entrée de l'outil doivent être explicitement définis et remplis de valeurs **. \
Contrairement au[**Use as Agent Tool**](microsoft-outlook.md#use-as-agent-tool)Option, il n'y a pas d'agent pour déterminer automatiquement les entrées. L'utilisateur doit remplir manuellement les champs, soit en entrant des valeurs fixes, soit en utilisant des variables enfermées dans des supports doubles bouclés`{{ }}`.

<gigne> <img src = "../../../. GitBook / Assets / Image (208) .png" alt = ""> <Figcaption> </gigcaption> </gigne>

