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

1. **Host Flowise:** Begin by hosting the Flowise service.

2. **Set Environment Variable:** Create an environment variable named `NUMBER_OF_PROXIES` and set its value to `1` within your hosting environment.

3. **Check IP Address:** To verify the IP address, access the following URL: `{{hosted_url}}/api/v1/ip`. You can do this either by entering the URL into your web browser or by making an API request.

4. **Compare IP Addresses:** After making the request, compare the IP address returned to your current IP address. You can find your current IP address by visiting either of these websites:
   - [http://ip.nfriedly.com/](http://ip.nfriedly.com/)
   - [https://api.ipify.org/](https://api.ipify.org/)

5. **Adjust Proxies:** If the IP address returned does not match your current IP address, increase the number of proxies by 1. Repeat this process until the IP address matches your own.
