---
description: 了解如何在 Flowise 中构建代理系统
---

# 智能体流程

## Introducing Agentic Systems in Flowise

Flowise 的 智能体流程 部分提供了一个平台，用于构建基于代理的系统，该系统可以与外部工具和数据源交互。

目前，Flowise 提供两种设计这些系统的方法：[**多智能体**](#user-content-fn-1)[^1] 和 [**顺序智能体**](#user-content-fn-2)[^2]。这些方法提供不同级别的控制和复杂性，使您可以选择最适合您需求的方法。

<figure><img src="../../.gitbook/assets/agentflow.png" alt=""><figcaption><p>流动 APP</p></figcaption></figure>

{% hint style="success" %}
本文档将探讨顺序智能体和多智能体方法，解释它们的功能以及如何使用它们来构建不同类型的会话工作流。
{% endhint %}

[^1]: **Multi-Agents**, built on top of the Sequential Agent architecture, simplify the process of building and managing teams of agents by pre-configuring core elements and providing a higher-level abstraction.

[^2]: **Sequential Agents** provide developers with direct access to the underlying workflow structure, enabling granular control over every step of the conversation flow and offering maximum flexibility for building highly customized conversational applications.
