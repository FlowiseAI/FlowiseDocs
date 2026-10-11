# 在公司代理后面运行 Flowise

如果您在需要代理的环境中运行 Flowise，例如在组织网络内，您可以将 Flowise 配置为通过您选择的代理路由其所有后端请求。此功能由 `global-agent` 包提供支持。

[https://github.com/gajus/global-agent](https://github.com/gajus/global-agent)

## 配置

在公司代理后面运行 Flowise 需要 2 个环境变量：

|变量|目的|必填|
| -------------------------- | -------------------------------------------------------------------------------- | -------- |
| `GLOBAL_AGENT_HTTP_PROXY` |通过哪里代理所有服务器 HTTP 请求 |是的 |
| `GLOBAL_AGENT_HTTPS_PROXY` |通过哪里代理所有服务器 HTTPS 请求 |没有 |
| `GLOBAL_AGENT_NO_PROXY` |应从代理中排除的 URL 模式。例如。 `*.foo.com,baz.com` |没有 |

## 出站允许列表

对于企业计划，您必须允许多个出站连接以进行许可证检查。请联系 support@flowiseai.com 了解更多信息。
