# Exécuter Flowise derrière un proxy d'entreprise

Si vous exécutez Flowise dans un environnement qui nécessite un proxy, comme au sein d'un réseau organisationnel, vous pouvez configurer Flowise pour acheminer toutes ses requêtes backend via un proxy de votre choix. Cette fonctionnalité est alimentée par le package `global-agent`.

[https://github.com/gajus/global-agent](https://github.com/gajus/global-agent)

## Configuration

Il y a 2 variables d'environnement dont vous aurez besoin pour exécuter Flowise derrière un proxy d'entreprise :

| Variable                   | Objectif                                                                          | Requis   |
| -------------------------- | -------------------------------------------------------------------------------- | -------- |
| `GLOBAL_AGENT_HTTP_PROXY`  | Où acheminer toutes les requêtes HTTP du serveur                                  | Oui      |
| `GLOBAL_AGENT_HTTPS_PROXY` | Où acheminer toutes les requêtes HTTPS du serveur                                 | Non      |
| `GLOBAL_AGENT_NO_PROXY`    | Un motif d'URLs qui doivent être exclues du proxy. Par exemple, `*.foo.com,baz.com` | Non      |

## Liste blanche des connexions sortantes

Pour le plan entreprise, vous devez autoriser plusieurs connexions sortantes pour la vérification de licence. Veuillez contacter support@flowiseai.com pour plus d'informations.