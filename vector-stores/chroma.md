# Chroma

## Prerequisite

1. Download & Install [Docker ](https://www.docker.com/)and [Git](https://git-scm.com/)
2.  Clone [Chroma's repository](https://github.com/chroma-core/chroma) with your terminal

    ```git
    git clone https://github.com/chroma-core/chroma.git
    ```

    <figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>
3.  Change directory path to your cloned Chroma

    ```powershell
    cd chroma
    ```

    <figure><img src="../.gitbook/assets/image (38) (1) (1).png" alt=""><figcaption></figcaption></figure>
4.  Run docker compose to build up Chroma image and container

    ```powershell
    docker-compose up -d --build
    ```

    <figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

## Setup

1. Input Collection Name into **Chroma Upsert Document** node or **Chroma Load Existing Index** node. (e.g. elon-musk)\
   ![](<../.gitbook/assets/image (37) (1).png>)![](<../.gitbook/assets/image (31).png>)
2. **Document** can be connect with any node under [**Document Loader**](../document-loaders.md) category
3. **Embeddings** can be connect with any node under [**Embeddings** ](../embeddings/)category

## Resources

* [LangChain JS Chroma](https://js.langchain.com/docs/modules/indexes/vector\_stores/integrations/chroma)
* [Chroma Getting Started](https://docs.trychroma.com/getting-started)
