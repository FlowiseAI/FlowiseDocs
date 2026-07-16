# IBM Watsonx

## 先决条件

1. 在 [IBM Watsonx](https://www.ibm.com/watsonx) 上注册帐户
2.创建一个新项目：

<figure><img src="../../../.gitbook/assets/image (238).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (239).png" alt=""><figcaption></figcaption></figure>

3. 创建项目后，返回主仪表板，然后单击 **探索基础模型**：

<figure><img src="../../../.gitbook/assets/image (240).png" alt=""><figcaption></figcaption></figure>

4. 选择您要使用的模型并在 Prompt Lab 中打开：

<figure><img src="../../../.gitbook/assets/image (241).png" alt=""><figcaption></figcaption></figure>

5. 单击右上角的“查看代码”：

<figure><img src="../../../.gitbook/assets/image (242).png" alt=""><figcaption></figcaption></figure>

6. 记下 `model_id` 和 `version` 参数。在本例中，它是 `ibm/granite-3-8b-instruct,`，版本是 `2023-05-29`。
7.点击左侧导航栏，点击开发者接入

<figure><img src="../../../.gitbook/assets/image (243).png" alt="" width="308"><figcaption></figcaption></figure>

8. 记下 `watsonx.ai URL`、`Project ID` 并从 IBM Cloud Console 创建新的 API 密钥。
9. 到目前为止，您应该拥有以下信息：
   * Watsonx.ai URL
   * 项目编号
   * API 键
   * 型号版本
   * 型号 ID

## 设置

1. **聊天模型** > 拖动 **ChatIBMWatsonx** 节点

<figure><img src="../../../.gitbook/assets/image (244).png" alt="" width="306"><figcaption></figcaption></figure>

2. 将之前的型号 ID 填写到型号中。创建新凭据并填写所有详细信息。

<figure><img src="../../../.gitbook/assets/image (245).png" alt="" width="419"><figcaption></figcaption></figure>

2.瞧[🎉](https://emojipedia.org/party-popper/)，您现在可以在 Flowise 中使用 **ChatIBMWatsonx 节点**！

<figure><img src="../../../.gitbook/assets/image (246).png" alt=""><figcaption></figcaption></figure>
