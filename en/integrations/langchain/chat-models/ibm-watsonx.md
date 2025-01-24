# IBM Watsonx

## Prerequisite

1. Register an account on [IBM Watsonx](https://www.ibm.com/watsonx)
2. Create a new project:

<figure><img src="../../../.gitbook/assets/image (238).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (239).png" alt=""><figcaption></figcaption></figure>

3. After project has been created, back to the main dashboard, and click **Explore foundation models**:

<figure><img src="../../../.gitbook/assets/image (240).png" alt=""><figcaption></figcaption></figure>

4. Choose the model you would like to use and open in Prompt Lab:

<figure><img src="../../../.gitbook/assets/image (241).png" alt=""><figcaption></figcaption></figure>

5. From the top right corner, click on View Code:

<figure><img src="../../../.gitbook/assets/image (242).png" alt=""><figcaption></figcaption></figure>

6. Take note on the `model_id` and `version` parameter. In this case, it is `ibm/granite-3-8b-instruct,` and the version is `2023-05-29`.
7. Click the navigation bar from the left side, and click Developer access

<figure><img src="../../../.gitbook/assets/image (243).png" alt="" width="308"><figcaption></figcaption></figure>

8. Take note on the `watsonx.ai URL`, `Project ID` and create a new API key from IBM Cloud Console.
9. By now, you should have the following information:
   * Watsonx.ai URL
   * Project ID
   * API Key
   * Model's version
   * Model's ID

## Setup

1. **Chat Models** > drag **ChatIBMWatsonx** node

<figure><img src="../../../.gitbook/assets/image (244).png" alt="" width="306"><figcaption></figcaption></figure>

2. Fill in the Model with the Model ID earlier. Create New Credential and fill in all the details.

<figure><img src="../../../.gitbook/assets/image (245).png" alt="" width="419"><figcaption></figcaption></figure>

2. Voila [🎉](https://emojipedia.org/party-popper/), you can now use **ChatIBMWatsonx node** in Flowise!

<figure><img src="../../../.gitbook/assets/image (246).png" alt=""><figcaption></figcaption></figure>
