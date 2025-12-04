---
description: Learn how to use Zendesk with Flowise Document Stores
---

# Zendesk

***

Pulling information from Zendesk is a common use case for document stores. This focuses on pulling information from a Zendesk Knowledge store as opposed to from the ticketing system.

## Setup

Make sure that you have the information that you will need from Zendesk including:

- Zendesk Domain
- API Key
- Brand ID
- Locale

## Steps

### 1. Create a new document store
Following the normal process of creating a document store, name it something that makes sense for your use case. Or select an existing one.
<figure><img src="../.gitbook/assets/datastore-zendesk/add-loader.png" alt=""><figcaption></figcaption></figure>

### 2. Select Zendesk as the document store type
Add an initial or a new document loader. Then select Zendesk as the document loader type.
<figure><img src="../.gitbook/assets/datastore-zendesk/select-zendesk.png" alt=""><figcaption></figcaption></figure>

### 3. Enter the information from above
<figure><img src="../.gitbook/assets/datastore-zendesk/zendesk-form-fields.png" alt=""><figcaption></figcaption></figure>

* Give it a name
* Enter the api key is the connection credential.
* Enter the zendesk domain. An example is given below, but you may have a non-zendesk domain. That's fine.
* Enter the brand_id. If you don't have a brand_id, you can leave it blank
* Enter the locale as a comma separated list of locales. For example, en-us, en-gb, en-ca. You can choose to create a document store for each locale or a single document store for all locales.
* The characters per token can help you to adjust the number of tokens that are used per document. Entering a larger number will create larger documents.
### 4. Save the document store
Save and preview. Adjust any of the form fields as needed.