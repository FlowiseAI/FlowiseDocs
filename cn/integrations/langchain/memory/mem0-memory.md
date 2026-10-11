# Mem0 内存

[Mem0](https://github.com/mem0ai/mem0)（发音为“mem-zero”）通过智能内存层增强 AI 助手和代理，从而实现个性化的 AI 交互。它会记住用户偏好，适应个人需求，并随着时间的推移不断改进。这使其成为客户支持聊天机器人、人工智能助手和自主AI 智能体等应用的理想选择。

Mem0 提供了一整套内存管理功能，允许无缝集成到各种人工智能驱动的应用程序中。

---

## 将 Mem0 与 Flowise 结合使用

请按照以下步骤将 Mem0 与 Flowise 集成：

### 1. 设置 Flowise

1. 打开 Flowise 应用程序并创建新画布，或从 Flowise 市场选择模板。
2. 在此示例中，我们使用 **Conversation Chain** 模板。
3. 将默认的 **Buffer Memory** 替换为 **Mem0 Memory**。

<figure><img src="../../../.gitbook/assets/mem0/flowise-flow.png" alt="Flowise Memory Integration"><figcaption>Flowise 与 Mem0 集成</figcaption></figure>

### 2. 获取您的 Mem0 API 密钥

1. 导航至 [Mem0 API 关键仪表板](https://app.mem0.ai/dashboard/api-keys)。
2. 生成或复制现有的 Mem0 API 密钥。

<figure><img src="../../../.gitbook/assets/mem0/api-key.png" alt="Mem0 API Key"><figcaption>从 Mem0 检索 API 密钥</figcaption></figure>

### 3. 在 Flowise 中配置 Mem0 凭据

1. 在 Mem0 凭据部分输入 **Mem0 API 密钥**。

<figure><img src="../../../.gitbook/assets/mem0/creds.png" alt="Mem0 Credentials"><figcaption>配置 API 凭据</figcaption></figure>

### 4. 保存并测试聊天流

1. 保存您的 Flowise 配置。
2. 运行测试聊天并存储一些信息。

<figure><img src="../../../.gitbook/assets/mem0/flowise-chat-1.png" alt="Flowise Test Chat"><figcaption>测试内存存储</figcaption></figure>

### 5. 验证 Mem0 仪表板中存储的内存

1. 访问[Mem0 控制板](https://app.mem0.ai/dashboard/requests) 查看存储的内存。

<figure><img src="../../../.gitbook/assets/mem0/flowise-memory.png" alt="Mem0 Stored Memories"><figcaption>回顾储存的记忆</figcaption></figure>

### 6. 验证内存保留

1. 清除 Flowise 中的聊天记录。
2. 根据之前存储的信息提出问题以确认保留。

<figure><img src="../../../.gitbook/assets/mem0/flowise-chat-2.png" alt="Testing Memory Retention"><figcaption>确认内存持久性</figcaption></figure>

---

## 附加设置

Mem0 提供各种定制选项：

<figure><img src="../../../.gitbook/assets/mem0/settings.png" alt="Mem0 Settings"><figcaption>Mem0 配置选项</figcaption></figure>

1. **仅搜索模式**：启用内存检索而不创建新内存。聊天记录会一直保留，直至手动清除。
2. **Mem0 实体**：利用 `user_id`、`run_id`、`app_id` 和 `agent_id` 等标识符进行精细内存控制。
3. **项目 ID**：将内存分配给特定项目。通过[Mem0 项目](https://app.mem0.ai/settings/projects/overview) 管理项目。
4. **组织ID**：将内存存储分配给特定组织。通过[Mem0 组织](https://app.mem0.ai/settings/organizations/overview) 管理组织。

---

## Mem0 平台配置

[Mem0 项目设置](https://app.mem0.ai/dashboard/project-settings) 下提供了其他配置：

1. **自定义指令**：定义项目级指令以细化内存提取。示例：仅提取学术详细信息。
2. **有效期**：设置存储记忆的有效期，以便在必要时自动处理数据。

<figure><img src="../../../.gitbook/assets/mem0/mem0-settings.png" alt="Mem0 Project Settings"><figcaption>自定义项目级别设置</figcaption></figure>

---

## 在 Flowise 中配置 Mem0 凭据

要在 Flowise 中添加凭据：

1. 导航至凭据设置。
2. 为 Mem0 添加新的凭据条目。
3. 将 [Mem0 API 密钥](https://app.mem0.ai/dashboard/api-keys) 粘贴到 API 密钥字段中。

<figure><img src="../../../.gitbook/assets/mem0/api-key.png" alt="Adding API Key in Flowise"><figcaption>在 Flowise 中输入 API</figcaption></figure>

---

通过这些配置，您的 Flowise 设置将与 Mem0 无缝集成，提供增强的内存保留和个性化的 AI 交互。

