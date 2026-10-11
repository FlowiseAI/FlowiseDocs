---
description: 了解如何在铁路上部署 Flowise
---

# 铁路

***

1. 单击以下预建的[模板](https://railway.app/template/pn4G8S?referralCode=WVNPD9)
2. 单击“立即部署”

<figure><img src="../../.gitbook/assets/image (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

3. 更改为您的首选存储库名称并单击部署

<figure><img src="../../.gitbook/assets/image (2) (1) (2) (1).png" alt="" width="375"><figcaption></figcaption></figure>

4. 如果成功，您应该能够看到已部署的 URL

<figure><img src="../../.gitbook/assets/image (2) (2).png" alt=""><figcaption></figcaption></figure>

5. 要添加授权，请导航到“变量”选项卡并添加：

* FLOWISE\_用户名
* FLOWISE\_PASSWORD

<figure><img src="../../.gitbook/assets/image (15) (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

6. 您可以配置环境变量列表。请参阅[environment-variables.md](../environment-variables.md "mention")

就是这样！您现在已在 Railway 上部署了 Flowise [🎉](https://emojipedia.org/party-popper/)[🎉](https://emojipedia.org/party-popper/)

## 持久卷

在 Railway 上运行的服务的默认文件系统是短暂的。 Flowise 数据不会在部署和重新启动时保留。要解决此问题，我们可以使用[Railway Volume](https://docs.railway.app/reference/volumes)。

为了简化步骤，我们有一个安装了卷的铁路模板：[https://railway.app/template/nEGbjR](https://railway.app/template/nEGbjR)

只需单击“部署”并填写环境变量，如下所示：

* DATABASE\_PATH - `/opt/railway/.flowise`
* APIKEY\_PATH - `/opt/railway/.flowise`
* LOG\_PATH - `/opt/railway/.flowise/logs`
* SECRETKEY\_PATH - `/opt/railway/.flowise`
* BLOB\_STORAGE\_PATH - `/opt/railway/.flowise/storage`

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="420"><figcaption></figcaption></figure>

现在尝试创建一个流程并将其保存在 Flowise 中。然后尝试重新启动服务或重新部署，您应该仍然能够看到之前保存的流程。
