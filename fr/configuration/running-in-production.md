# Exécution en Production

## Mode

Lorsque vous exécutez en production, nous vous recommandons vivement d'utiliser le mode [Queue](running-flowise-using-queue.md) avec les paramètres suivants :

* 2 serveurs principaux avec équilibrage de charge, chacun commençant avec 4vCPU 8 Go de RAM
* 4 travailleurs, chacun commençant avec 4vCPU 8 Go de RAM

Vous pouvez configurer l'auto-scaling en fonction du trafic et du volume.

## Base de données

Par défaut, Flowise utilisera SQLite comme base de données. Cependant, lorsqu'il est exécuté à grande échelle, il est recommandé d'utiliser PostgresQL.

## Stockage

Actuellement, Flowise ne prend en charge que [AWS S3](https://aws.amazon.com/s3/) avec un plan pour prendre en charge d'autres fournisseurs de stockage d'objets. Cela permettra de stocker des fichiers et des journaux sur S3, au lieu d'un chemin de fichier local. Consultez [#for-storage](environment-variables.md#for-storage "mention")

## Chiffrement

Flowise utilise une clé de chiffrement pour chiffrer/déchiffrer les identifiants que vous utilisez, tels que les clés API OpenAI. Il est recommandé d'utiliser [AWS Secret Manager](https://aws.amazon.com/secrets-manager/) en production pour un meilleur contrôle de la sécurité et une rotation des clés. Consultez [#for-credentials](environment-variables.md#for-credentials "mention")

## Limite de Taux

Lorsqu'il est déployé dans le cloud/en local, il est probable que les instances soient derrière un proxy/un équilibreur de charge. L'adresse IP de la requête pourrait être celle de l'équilibreur de charge/proxy inverse, rendant le limiteur de taux effectivement global et bloquant toutes les requêtes une fois la limite atteinte ou `undefined`. Définir le bon `NUMBER_OF_PROXIES` peut résoudre le problème. Consultez [#rate-limit-setup](rate-limit.md#rate-limit-setup "mention")

## Test de Charge

Artillery peut être utilisé pour tester la charge de votre application Flowise déployée. Un exemple de script peut être trouvé [ici](https://github.com/FlowiseAI/Flowise/blob/main/artillery-load-test.yml).