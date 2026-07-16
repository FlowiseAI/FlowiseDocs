# 构建节点

### 安装 Git

首先，安装 Git 并克隆 Flowise 存储库。您可以按照[开始](/broken/pages/nuiTj70UthEELOvhLrSb#for-developers)指南中的步骤操作。

### 结构

Flowise 将每个节点集成分离到文件夹 `packages/components/nodes` 下。让我们尝试创建一个简单的工具！

### 创建计算器工具

在 `packages/components/nodes/tools` 文件夹下创建一个名为 `Calculator` 的新文件夹。然后创建一个名为 `Calculator.ts` 的新文件。在文件中，我们首先编写基类。

```javascript
import { INode } from '../../../src/Interface'
import { getBaseClasses } from '../../../src/utils'

class Calculator_Tools implements INode {
    label: string
    name: string
    version: number
    description: string
    type: string
    icon: string
    category: string
    author: string
    baseClasses: string[]

    constructor() {
        this.label = 'Calculator'
        this.name = 'calculator'
        this.version = 1.0
        this.type = 'Calculator'
        this.icon = 'calculator.svg'
        this.category = 'Tools'
        this.author = 'Your Name'
        this.description = 'Perform calculations on response'
        this.baseClasses = [this.type, ...getBaseClasses(Calculator)]
    }
}

module.exports = { nodeClass: Calculator_Tools }
```

每个节点都将实现 `INode` 基类。每个属性含义的细分：

<table><thead><tr><th width="271">财产</th><th>描述</th></tr></thead><tbody><tr><td>标签</td><td>UI 上显示的节点名称</td></tr><tr><td>姓名</td><td>代码使用的名称。必须是 <strong>驼色箱</strong></td></tr><tr><td>版本</td><td>节点版本</td></tr><tr><td>类型</td><td>通常与标签相同。定义哪个节点可以连接到 UI 上的该特定类型</td></tr><tr><td>图标</td><td>节点图标</td></tr><tr><td>类别</td><td>节点类别</td></tr><tr><td>作者</td><td>节点创建者</td></tr><tr><td>描述</td><td>节点说明</td></tr><tr><td>基类</td><td>来自节点的基类，因为节点可以从基组件扩展。用于定义UI上哪个节点可以连接到该节点</td></tr></tbody></table>

### 定义类

现在组件类已部分完成，我们可以继续定义实际的工具类，在本例中为 `Calculator`。

在同一个 `Calculator` 文件夹下创建一个新文件，并命名为 `core.ts`

```javascript
import { Parser } from "expr-eval"
import { Tool } from "@langchain/core/tools"

export class Calculator extends Tool {
    name = "calculator"
    description = `Useful for getting the result of a math expression. The input to this tool should be a valid mathematical expression that could be executed by a simple calculator.`
 
    async _call(input: string) {
        try {
            return Parser.evaluate(input).toString()
        } catch (error) {
            return "I don't know how to do that."
        }
    }
}
```

### 整理

回到 `Calculator.ts` 文件，我们可以通过 `async init` 函数来完成此操作。在此函数中，我们将初始化上面创建的 Calculator 类。当流正在执行时，每个节点中的`init`函数将被调用，并且当LLM决定调用该工具时，将执行`_call`函数。

```javascript
import { INode } from '../../../src/Interface'
import { getBaseClasses } from '../../../src/utils'
import { Calculator } from './core'

class Calculator_Tools implements INode {
    label: string
    name: string
    version: number
    description: string
    type: string
    icon: string
    category: string
    author: string
    baseClasses: string[]

    constructor() {
        this.label = 'Calculator'
        this.name = 'calculator'
        this.version = 1.0
        this.type = 'Calculator'
        this.icon = 'calculator.svg'
        this.category = 'Tools'
        this.author = 'Your Name'
        this.description = 'Perform calculations on response'
        this.baseClasses = [this.type, ...getBaseClasses(Calculator)]
    }
    
 
    async init() {
        return new Calculator()
    }
}

module.exports = { nodeClass: Calculator_Tools }
```

### 构建并运行

在 `packages/server` 内的 `.env` 文件中，创建一个新的环境变量：

```javascript
SHOW_COMMUNITY_NODES=true
```

现在我们可以使用 `pnpm build` 和 `pnpm start` 让组件活跃起来！

<figure><img src="../.gitbook/assets/image (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>
