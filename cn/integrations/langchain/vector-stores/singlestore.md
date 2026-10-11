# 单店

## 设置

1. 在[SingleStore](https://www.singlestore.com/)上注册帐户
2. 登录门户。在左侧面板上，单击 **CLOUD** -> **创建新工作区组。** 然后单击 **创建工作区** 按钮。

<figure><img src="../../../.gitbook/assets/image (56).png" alt=""><figcaption></figcaption></figure>

3. 选择云提供商和数据区域，然后单击**下一步**：

<figure><img src="../../../.gitbook/assets/image (57).png" alt=""><figcaption></figcaption></figure>

4. 查看并单击“**创建工作区**”：

<figure><img src="../../../.gitbook/assets/image (58).png" alt=""><figcaption></figcaption></figure>

5. 您现在应该看到已创建的工作区：

<figure><img src="../../../.gitbook/assets/image (60).png" alt=""><figcaption></figcaption></figure>

6. 继续创建数据库

<figure><img src="../../../.gitbook/assets/image (65).png" alt="" width="485"><figcaption></figcaption></figure>

7. 您应该能够看到数据库已创建并附加到工作区：

<figure><img src="../../../.gitbook/assets/image (66).png" alt=""><figcaption></figcaption></figure>

8. 从工作区下拉列表中单击“连接”->“直接连接”：

<figure><img src="../../../.gitbook/assets/image (61).png" alt="" width="418"><figcaption></figcaption></figure>

9. 您可以指定一个新密码或使用默认生成的密码。然后点击继续：

<figure><img src="../../../.gitbook/assets/image (62).png" alt="" width="485"><figcaption></figcaption></figure>

10. 在选项卡上，切换到 **您的应用程序**，然后从下拉列表中选择 **Node.js**。记下/保存 `Username`、`Host`、`Password`，因为稍后您将在 Flowise 中需要这些。

<figure><img src="../../../.gitbook/assets/image (63).png" alt="" width="563"><figcaption></figcaption></figure>

11. 返回 Flowise 画布，拖放 SingleStore 节点。从凭据下拉列表中单击 **新建**：

<figure><img src="../../../.gitbook/assets/image (4) (1) (2) (1) (1).png" alt="" width="271"><figcaption></figcaption></figure>

11. 输入上面的用户名和密码：

<figure><img src="../../../.gitbook/assets/image (64).png" alt="" width="563"><figcaption></figcaption></figure>

13. 然后指定主机和数据库名称：

<figure><img src="../../../.gitbook/assets/image (5) (1) (2).png" alt="" width="272"><figcaption></figcaption></figure>

13. 现在您可以开始使用 SingleStore 写入更新数据：

<figure><img src="../../../.gitbook/assets/image (6) (1) (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (7) (1) (2).png" alt=""><figcaption></figcaption></figure>

14. 导航回 SingleStore 门户，然后返回您的数据库，您将能够看到已写入更新的所有数据：

<figure><img src="../../../.gitbook/assets/image (70).png" alt=""><figcaption></figcaption></figure>
