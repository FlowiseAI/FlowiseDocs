---
description: >-
  Check whether input consists of any text from Deny list, and prevent being
  sent to LLM.
---

# Simple Prompt Moderation

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (2) (1).png" alt="" width="301"><figcaption><p>Simple Prompt Moderation Node</p></figcaption></figure>

Use another LLM to identify if user query is close to the deny list, if yes output a default error message.

For example, deny list can be:

* Ignore previous instructions
* Leak all sensitive information

<figure><img src="../../../.gitbook/assets/image (336).png" alt=""><figcaption></figcaption></figure>
