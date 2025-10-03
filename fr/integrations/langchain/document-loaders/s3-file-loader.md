# S3 File Loader

Amazon S3 (Simple Storage Service) is an object storage service offering industry-leading scalability, data availability, security, and performance. This module provides comprehensive functionality to load and process files stored in S3 buckets.

This module provides a sophisticated S3 document loader that can:
- Load files from S3 buckets using AWS credentials
- Support multiple file formats (PDF, DOCX, CSV, Excel, PowerPoint, text files)
- Process files using built-in loaders or Unstructured.io API
- Handle text and binary files
- Customize metadata extraction

## Inputs

### Required Parameters
- **Bucket**: The name of the S3 bucket
- **Object Key**: The unique identifier of the object in the S3 bucket
- **Region**: AWS region where the bucket is located (default: us-east-1)

### Processing Options
- **File Processing Method**: Choose between:
  - Built In Loaders: Use native file format processors
  - Unstructured: Use Unstructured.io API for advanced processing
- **Text Splitter** (optional): Text splitter for built-in processing
- **Additional Metadata** (optional): JSON object with additional metadata
- **Omit Metadata Keys** (optional): Keys to omit from metadata

### Unstructured.io Options
- **Unstructured API URL**: Endpoint for Unstructured.io API
- **Unstructured API KEY** (optional): API key for authentication
- **Strategy**: Processing strategy (hi_res, fast, ocr_only, auto)
- **Encoding**: Text encoding method (default: utf-8)
- **Skip Infer Table Types**: Document types to skip table extraction

## Outputs

- **Document**: Array of document objects containing metadata and pageContent
- **Text**: Concatenated string from pageContent of documents

## Features
- AWS S3 integration
- Multiple file format support
- Built-in and Unstructured.io processing
- Configurable AWS regions
- Flexible metadata handling
- Binary file processing
- Temporary file management
- MIME type detection

## Supported File Types
- PDF documents
- Microsoft Word (DOCX)
- Microsoft Excel
- Microsoft PowerPoint
- CSV files
- Text files
- And more through Unstructured.io

## Notes
- Requires AWS credentials (optional if using IAM roles)
- Some file types may require specific processing methods
- Unstructured.io API requires separate setup and credentials
- Temporary files are created and managed automatically
- Error handling for unsupported file types

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

