# Deployment

Flowise can be deployed to any cloud services. Here's few commonly used cloud providers:

* [Render](render.md)
* [Railway](railway.md)
* [Replit](replit.md)
* [Hugging Face](hugging-face.md)

Cloud providers like AWS, Azure, GCP, DigitalOcean are slightly more technical to set it up:

* [AWS](aws.md)
* [Azure](azure.md)
* [DigitalOcean](digital-ocean.md)
* [GCP](gcp.md)
* [Kubernetes using Helm](https://artifacthub.io/packages/helm/cowboysysop/flowise)

## Rate Limit Setup Guide

1. Host Flowise.
2. Add `NUMBER_OF_PROXIES` as environment name and `1` as the environment value into your host's environment.
3. Check IP address using `{{hosted_url}}/api/v1/ip` via enter the URL into browser or API request.
4. If returned IP address from API request does not match your IP address (which you can get by going to http://ip.nfriedly.com/ or https://api.ipify.org/).
5. Increase the number of proxies by 1 until it matches your IP address.
