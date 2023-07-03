# Zapier NLA

## Prerequisite

1. [Log in](https://zapier.com/app/login) or [sign up](https://zapier.com/sign-up) to Zapier
2. [Connect](https://nla.zapier.com/dev/start/) your Zapier account to the NLA dev provider.
3. Your API key will be available on the [credentials page](https://nla.zapier.com/credentials/).

## Setup

### Zapier NLA

1.  Go to [Zapier Actions](https://nla.zapier.com/dev/actions/)


    <figure><img src="../.gitbook/assets/zapier/nla/1.png" alt="" width="563"><figcaption></figcaption></figure>
2.  Click **Add a new action**


    <figure><img src="../.gitbook/assets/zapier/nla/2.png" alt="" width="563"><figcaption></figcaption></figure>
3.  Input `Zoho Mail: Send Email` into Action field then choose or connect new account


    <figure><img src="../.gitbook/assets/zapier/nla/3.png" alt="" width="563"><figcaption></figcaption></figure>
4.  Leave everything as default and click **Enable action**


    <figure><img src="../.gitbook/assets/zapier/nla/4.png" alt="" width="563"><figcaption></figcaption></figure>

### Flowise

1.  Go to **Marketplaces**, scroll to the very bottom and click **Zapier NLA**


    <figure><img src="../.gitbook/assets/zapier/nla/5.png" alt=""><figcaption></figcaption></figure>
2.  Click **Use Template**


    <figure><img src="../.gitbook/assets/zapier/nla/6.png" alt=""><figcaption></figcaption></figure>
3.  Fill in required fields **OpenAI Api Key** and **Zapier NLA API Key**, click **Save Chatflow**


    <figure><img src="../.gitbook/assets/zapier/nla/7.png" alt=""><figcaption></figcaption></figure>
4. Ask MRKL Agent to perform action

```
Send a Zoho email to chungyau.ong@flowiseai.com, body content is Testing Zapier NLA.
```

<figure><img src="../.gitbook/assets/zapier/nla/8.png" alt=""><figcaption></figcaption></figure>
5. Voila [🎉](https://emojipedia.org/party-popper/) you should see the email arrived in your Zoho mailbox

<figure><img src="../.gitbook/assets/zapier/nla/9.png" alt=""><figcaption></figcaption></figure>

## Resources

* [LangChain JS Zapier NLA](https://js.langchain.com/docs/modules/agents/tools/zapier_agent)
* [Zapier NLA](https://nla.zapier.com/start/)
