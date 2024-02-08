# S3 File Loader

S3 File Loader allows you to retrieve a file from s3, and use [Unstructured](https://unstructured.io/) to preprocess into a structured Document object that is ready to be converted into vector embeddings. Unstructured is being used to cater for wide range of different file types. Regardless if your file on s3 is PDF, XML, DOCX, CSV, it can be processed by Unstructured. See [here](https://unstructured-io.github.io/unstructured/api.html#supported-file-types) for supported file types.

## Unstructured Setup

You can either use the hosted API or running locally via Docker.

* [Hosted API](https://unstructured-io.github.io/unstructured/api.html)
* Docker: `docker run -p 8000:8000 -d --rm --name unstructured-api quay.io/unstructured-io/unstructured-api:latest --port 8000 --host 0.0.0.0`

## S3 File Loader Setup

1\. Drag and drop S3 file loader onto canvas:

<figure><img src="../../../.gitbook/assets/image (71).png" alt="" width="234"><figcaption></figcaption></figure>

2\. AWS Credential: Create a new credential for your AWS account. You'll need the access and secret key. Remember to grant s3 bucket policy to the associated account. You can refer to the policy guide [here](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/AuroraMySQL.Integrating.Authorizing.IAM.S3CreatePolicy.html).

<figure><img src="../../../.gitbook/assets/image (72).png" alt="" width="551"><figcaption></figcaption></figure>

3. Bucket: Login to your AWS console and navigate to S3. Get your bucket name:&#x20;

<figure><img src="../../../.gitbook/assets/image (73).png" alt=""><figcaption></figcaption></figure>

4. Key: Click on the object you would like to use, and get the Key name:

<figure><img src="../../../.gitbook/assets/image (75).png" alt="" width="228"><figcaption></figcaption></figure>

5. Unstructured API URL: Depending on how you are using Unstructured, whether its through Hosted API or Docker, change the Unstructured API URL parameter. If you are using Hosted API, you'll need the API key as well.
6. You can then start chatting with your file from S3. You don't have to specify the text splitter for chunking down the document because thats handled by Unstructured automatically.

<figure><img src="../../../.gitbook/assets/screely-1698767992182.png" alt=""><figcaption></figcaption></figure>

