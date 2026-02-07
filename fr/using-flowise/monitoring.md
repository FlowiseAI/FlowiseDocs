# Surveillance

Flowise a un support natif pour Prometheus avec Grafana et Opentelémétrie. Cependant, seules des mesures de haut niveau telles que les demandes d'API, les dénombrements de flux / prédictions sont suivis. Référer[here](https://github.com/FlowiseAI/Flowise/blob/main/packages/server/src/Interface.Metrics.ts#L13)Pour les listes de contre-métriques. Pour plus de détails, l'observabilité du nœud par nœud, nous vous recommandons d'utiliser[Analytic](broken-reference).

## Prométhée

[Prometheus](https://prometheus.io/)est une solution de surveillance et d'alerte open source.

Avant de configurer Prometheus, configurez les variables Env suivantes dans Flowise:

```properties
ENABLE_METRICS=true
METRICS_PROVIDER=prometheus
METRICS_INCLUDE_NODE_METRICS=true
```

Une fois ProTheus installé, exécutez-le à l'aide d'un fichier de configuration. Flowise fournit un fichier de configuration par défaut qui peut être trouvé[here](https://github.com/FlowiseAI/Flowise/blob/main/metrics/prometheus/prometheus.config.yml).

N'oubliez pas d'avoir une instance fluide également en cours d'exécution. Vous pouvez ouvrir le navigateur et naviguer vers le port 9090. Dans le tableau de bord, vous devriez pouvoir voir le point de terminaison métrique -`/api/v1/metrics`est maintenant en direct.

<gigne> <img src = "../. GitBook / Assets / Image (178) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

Par défaut,`/api/v1/metrics`est disponible pour Prometheus pour tirer les mesures.

<gigne> <img src = "../. GitBook / Assets / Image (177) .png" alt = "" width = "563"> <Figcaption> </gigcaption> </ Figure>

## Grafana

Prométhée recueille des mesures riches et fournit un langage de requête puissant; Grafana transforme les mesures en visualisations significatives.

Grafana peut être installé de diverses manières. Reportez-vous au[guide](https://grafana.com/docs/grafana/latest/setup-grafana/installation/).

Grafana expose par défaut le port 9091:

<Figure> <img src = "../. GitBook / Assets / Image (179) .png" alt = ""> <figcaption> </gigcaption> </gigust>

Sur la barre gauche, cliquez sur Ajouter une nouvelle connexion et sélectionnez Prometheus:

<gigne> <img src = "../. GitBook / Assets / Image (180) .png" alt = ""> <Figcaption> </gigcaption> </gigne>

Puisque notre Prometheus sert au port 9090:

<gigne> <img src = "../. GitBook / Assets / Image (181) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

Faites défiler vers le bas et testez la connexion:

<gigne> <img src = "../. GitBook / Assets / Image (182) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

Prenez note de l'ID de source de données indiqué dans la barre d'outils, nous en aurons besoin pour créer des tableaux de bord:

<gigne> <img src = "../. GitBook / Assets / Image (184) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

Maintenant que la connexion est ajoutée avec succès, nous pouvons commencer à ajouter un tableau de bord. Dans la barre gauche, cliquez sur Tableaux de bord et créez un tableau de bord.

Flowise fournit 2 tableaux de bord de modèle:

* [grafana.dashboard.app.json.txt](https://github.com/FlowiseAI/Flowise/blob/main/metrics/grafana/grafana.dashboard.app.json.txt): Les mesures API telles que le nombre de ChatFlows / Agentflows, le nombre de prédictions, les outils, l'assistant, les vecteurs renversés, etc.
* [grafana.dashboard.server.json.txt](https://github.com/FlowiseAI/Flowise/blob/main/metrics/grafana/grafana.dashboard.server.json.txt): métriques de l'instance Node.js flowise telle que le tas, le processeur, l'utilisation de la RAM

Si vous utilisez des modèles ci-dessus, trouvez et remplacez toutes`cds4j1ybfuhogb`avec l'ID de source de données que vous avez créé et enregistré plus tôt.

<gigne> <img src = "../. GitBook / Assets / Image (183) .png" alt = ""> <figcaption> </gigcaption> </gigust>

Vous pouvez également choisir d'importer d'abord, puis modifier le JSON plus tard:

<gigne> <img src = "../. GitBook / Assets / Image (185) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

Maintenant, essayez d'effectuer des actions sur le flux, vous devriez pouvoir voir les métriques affichées:

<gigne> <img src = "../. GitBook / Assets / Image (186) .png" alt = ""> <Figcaption> </gigcaption> </gigne>

<gigne> <img src = "../. GitBook / Assets / Image (187) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

## OpenTelemetry

[OpenTelemetry](https://opentelemetry.io/)est un cadre open source pour créer et gérer les données de télémétrie. Pour activer Otel, configurez les variables Env suivantes dans Flowise:

```properties
ENABLE_METRICS=true
METRICS_PROVIDER=open_telemetry
METRICS_INCLUDE_NODE_METRICS=true
METRICS_OPEN_TELEMETRY_METRIC_ENDPOINT=http://localhost:4318/v1/metrics
METRICS_OPEN_TELEMETRY_PROTOCOL=http # http | grpc | proto (default is http)
METRICS_OPEN_TELEMETRY_DEBUG=true
```

Ensuite, nous avons besoin d'un collecteur d'OpenTelemetry pour recevoir, traiter et exporter des données de télémétrie. Flowise fournit un[docker compose file](https://github.com/FlowiseAI/Flowise/blob/main/metrics/otel/compose.yaml)qui peut être utilisé pour démarrer le conteneur collecteur.

```bash
cd Flowise
cd metrics && cd otel
docker compose up -d
```

Le collectionneur utilisera le[otel.config.yml](https://github.com/FlowiseAI/Flowise/blob/main/metrics/otel/otel.config.yml)Fichier sous le même répertoire pour les configurations. Actuellement seulement[Datadog](https://www.datadoghq.com/)et prometheus sont soutenus, référer à[Open Telemetry](https://opentelemetry.io/)Documentation pour configurer différents outils APM tels que Zipkin, Jeaver, New Relic, Splunk et autres.

Assurez-vous de remplacer par la touche API nécessaire pour les exportateurs dans le fichier YML.
