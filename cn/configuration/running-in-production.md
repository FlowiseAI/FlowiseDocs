# 在生产环境中运行

## 模式

在生产环境中运行时，我们强烈建议使用 [Queue](running-flowise-using-queue.md) 模式并进行以下设置：

* 2 个带负载均衡的主服务器，每台从 4vCPU 8GB RAM 起
* 4个worker，每个从4vCPU 8GB RAM开始

您可以根据流量和容量配置自动缩放。

## 数据库

默认情况下，Flowise 将使用 SQLite 作为数据库。但是，当大规模运行时，建议使用 PostgresQL。

## 存储

目前 Flowise 仅支持 [AWS S3](https://aws.amazon.com/s3/)，并计划支持更多 Blob 存储提供商。这将允许文件和日志存储在 S3 上，而不是本地文件路径上。请参阅[#for-storage](environment-variables.md#for-storage "mention")

## 加密

Flowise 使用加密密钥来加密/解密您使用的凭据，例如 OpenAI API 密钥。建议在生产中使用 [AWS Secret Manager](https://aws.amazon.com/secrets-manager/)，以实现更好的安全控制和密钥轮换。请参阅[#for-credentials](environment-variables.md#for-credentials "mention")

## 速率限制

当部署到云/本地时，实例很可能位于代理/负载均衡器后面。请求的 IP 地址可能是负载均衡器/反向代理的 IP，使速率限制器实际上成为全局速率限制器，并在达到限制或 `undefined` 时阻止所有请求。设置正确的 `NUMBER_OF_PROXIES` 可以解决该问题。请参阅[#rate-limit-setup](rate-limit.md#rate-limit-setup "mention")

## 负载测试

Artillery 可用于对您部署的 Flowise 应用程序进行负载测试。示例脚本可在[此处](https://github.com/FlowiseAI/Flowise/blob/main/artillery-load-test.yml)找到。

## 安全

请参阅[#安全配置](environment-variables.md#security-configuration "mention")
