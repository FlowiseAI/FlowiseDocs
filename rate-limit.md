# Rate Limit

When you share your chatflow to public with no API authorization through API or embedded chat, anybody can access the flow. To prevent spamming, you can set the rate limit on your chatflow.

<figure><img src=".gitbook/assets/image (1) (1) (1).png" alt="" width="462"><figcaption></figcaption></figure>

* **Message Limit per Duration**: How many messages can be received in a specific duration. Ex: 20&#x20;
* **Duration in Seconds**: The specified duration. Ex: 60
* **Limit Message**: What message to return when the limit is exceeded. Ex: Quota Exceeded

Using the example above, that means only 20 messages are allowed to be received in 60 seconds. The rate limitation is tracked by IP-address. If you have deployed Flowise on cloud service, you'll have to set `NUMBER_OF_PROXIES` env variable.

## Rate Limit Setup Guide

1. **Host Flowise:** Begin by hosting the Flowise service.
2. **Set Environment Variable:** Create an environment variable named `NUMBER_OF_PROXIES` and set its value to `1` within your hosting environment.
3. **Check IP Address:** To verify the IP address, access the following URL: `{{hosted_url}}/api/v1/ip`. You can do this either by entering the URL into your web browser or by making an API request.
4. **Compare IP Addresses:** After making the request, compare the IP address returned to your current IP address. You can find your current IP address by visiting either of these websites:
   * [http://ip.nfriedly.com/](http://ip.nfriedly.com/)
   * [https://api.ipify.org/](https://api.ipify.org/)
5. **Adjust Proxies:** If the IP address returned does not match your current IP address, increase the number of proxies by 1. Repeat this process until the IP address matches your own.
