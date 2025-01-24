# Building Node

### Install Git

First, install Git and clone Flowise repository. You can follow the steps from the [Get Started](../getting-started/#for-developers) guide.

### Structure

Flowise separate every node integration under the folder `packages/components/nodes`. Let's try to create a simple Tool!

### Create Calculator Tool

Create a new folder named `Calculator` under the `packages/components/nodes/tools` folder. Then create a new file named `Calculator.ts`. Inside the file, we will first write the base class.

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

Every node will implements the `INode` base class. Breakdown of what each property means:

<table><thead><tr><th width="271">Property</th><th>Description</th></tr></thead><tbody><tr><td>label</td><td>The name of the node that appears on the UI</td></tr><tr><td>name</td><td>The name that is used by code. Must be <strong>camelCase</strong></td></tr><tr><td>version</td><td>Version of the node</td></tr><tr><td>type</td><td>Usually the same as label. To define which node can be connected to this specific type on UI</td></tr><tr><td>icon</td><td>Icon of the node</td></tr><tr><td>category</td><td>Category of the node</td></tr><tr><td>author</td><td>Creator of the node</td></tr><tr><td>description</td><td>Node description</td></tr><tr><td>baseClasses</td><td>The base classes from the node, since a node can extends from a base component. Used to define which node can be connected to this node on UI</td></tr></tbody></table>

### Define Class

Now the component class is partially finished, we can go ahead to define the actual Tool class, in this case - `Calculator`.

Create a new file under the same `Calculator` folder, and named as `core.ts`

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

### Finishing

Head back to the `Calculator.ts` file, we can finish this up by having the `async init` function. In this function, we will initialize the Calculator class we created above. When the flow is being executed, the `init` function in each node will be called, and the `_call` function will be executed when LLM decides to call this tool.

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

### Build and Run

In the `.env` file inside `packages/server`, create a new env variable:

```javascript
SHOW_COMMUNITY_NODES=true
```

Now we can use `pnpm build` and `pnpm start` to bring the component alive!

<figure><img src="../.gitbook/assets/image (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>
