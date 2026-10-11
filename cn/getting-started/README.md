# 开始吧

***

## 云

自托管需要更多的技术技能来设置实例、备份数据库和维护更新。如果您没有管理服务器的经验，而只想使用网络应用程序，我们建议使用[Flowise Cloud](https://cloud.flowiseai.com)。

## 快速入门

{% hint style="info" %}
先决条件：确保计算机上已安装 [NodeJS](https://nodejs.org/en/download)。支持节点 `v18.15.0` 或 `v20` 及以上。
{% endhint %}

使用 NPM 在本地安装 Flowise。

1.安装Flowise：

```bash
npm install -g flowise
```

您还可以安装特定版本。请参阅可用的[版本](https://www.npmjs.com/package/flowise?activeTab=versions)。

```
npm install -g flowise@x.x.x
```

2.启动Flowise：

```bash
npx flowise start
```

3. 打开：[http://localhost:3000](http://localhost:3000)

***

## 码头工人

使用 Docker 部署 Flowise 有两种方法。首先，git克隆项目：[https://github.com/FlowiseAI/Flowise](https://github.com/FlowiseAI/Flowise)

### Docker 组合

1. 转到项目根目录下的 `docker folder`
2. 复制 `.env.example` 文件并将其粘贴为另一个名为 `.env` 的文件
3. 运行：

```bash
docker compose up -d
```

4. 打开：[http://localhost:3000](http://localhost:3000)
5. 您可以通过运行以下命令来关闭容器：

```bash
docker compose stop
```

### Docker 镜像

1. 构建镜像：

```bash
docker build --no-cache -t flowise .
```

2.运行镜像：

```bash
docker run -d --name flowise -p 3000:3000 flowise
```

3. 停止图像：

```bash
docker stop flowise
```

***

## 对于开发者

Flowise 在一个单一存储库中有 4 个不同的模块：

* **服务器**：服务 API 逻辑的节点后端
* **UI**：反应前端
* **组件**：集成组件
* **Api 文档**：Flowise API 的 Swagger 规范

### 先决条件

安装[PNPM](https://pnpm.io/installation)。

```bash
npm i -g pnpm
```

### 设置 1

使用 PNPM 进行简单设置：

1. 克隆存储库

```bash
git clone https://github.com/FlowiseAI/Flowise.git
```

2.进入存储库文件夹

```bash
cd Flowise
```

3.安装所有模块的所有依赖项：

```bash
pnpm install
```

4. 构建代码：

```bash
pnpm build
```

在 [http://localhost:3000](http://localhost:3000) 启动应用

```bash
pnpm start
```

### 设置 2

项目贡献者的逐步设置：

1. 分叉官方 [Flowise Github 存储库](https://github.com/FlowiseAI/Flowise)
2. 克隆您的分叉存储库
3. 创建新分支，请参阅[guide](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-and-deleting-branches-within-your-repository)。命名约定：
* 对于功能分支：`feature/<Your New Feature>`
* 对于错误修复分支：`bugfix/<Your New Bugfix>`。
4.切换到刚刚创建的分支
5.进入存储库文件夹：

```bash
cd Flowise
```

6.安装所有模块的所有依赖项：

```bash
pnpm install
```

7. 构建代码：

```bash
pnpm build
```

8. 通过 [http://localhost:3000](http://localhost:3000) 启动应用程序

```bash
pnpm start
```

9. 对于开发构建：

* 创建`.env`文件并在`packages/ui`中指定`PORT`（参考`.env.example`）
* 创建`.env`文件并在`packages/server`中指定`PORT`（参考`.env.example`）

```bash
pnpm dev
```

* 在 `packages/ui` 或 `packages/server` 中所做的任何更改都将反映在 [http://localhost:8080](http://localhost:8080/) 中
* 对于 `packages/components` 中所做的更改，您需要再次构建才能获取更改
* 完成所有更改后，运行：

    ```bash
    pnpm build
    ```

    和

    ```bash
    pnpm start
    ```

    确保生产中一切正常。

***

## 对于企业

在启动应用程序之前，企业用户需要填写 `.env` 文件中的企业参数值。请参阅 `.env.example` 了解所需的更改。

请联系 support@flowiseai.com 以获取以下环境变量的值：

```
LICENSE_URL
FLOWISE_EE_LICENSE_KEY
```

***

## 了解更多

在本视频教程中，Leon 介绍了 Flowise，并解释了如何在本地计算机上进行设置。

{% embed url="https://youtu.be/nqAK_L66sIQ" %}

## 社区指南

* [介绍\[实用\]使用Flowise / LangChain构建LLM应用程序](https://volcano-ice-cd6.notion.site/Introduction-to-Practical-Building-LLM-Applications-with-Flowise-LangChain-03d6d75bfd20495d96dfdae964bea5a5)
* [Flowise / LangChainによるLLMアプurikeーション构筑\[実践\]入门](https://volcano-ice-cd6.notion.site/Flowise-LangChain-LLM-e106bb0f7e2241379aad8fa428ee064a)
