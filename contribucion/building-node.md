# Building Node

### Install Git

Primero, instala Git y clona el repositorio de Flowise. Puedes seguir los pasos de la guía [Get Started](../getting-started/#for-developers).

### Structure

Flowise separa cada integración de node bajo la carpeta `packages/components/nodes`. ¡Vamos a crear una simple Tool!

### Create Calculator Tool

Crea una nueva carpeta llamada `Calculator` bajo la carpeta `packages/components/nodes/tools`. Luego crea un nuevo archivo llamado `Calculator.ts`. Dentro del archivo, primero escribiremos la clase base.

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

Cada node implementará la clase base `INode`. Desglose de lo que significa cada propiedad:

<table><thead><tr><th width="271">Property</th><th>Description</th></tr></thead><tbody><tr><td>label</td><td>El nombre del node que aparece en la UI</td></tr><tr><td>name</td><td>El nombre que es usado por el código. Debe estar en <strong>camelCase</strong></td></tr><tr><td>version</td><td>Versión del node</td></tr><tr><td>type</td><td>Usualmente igual que label. Para definir qué node puede conectarse a este tipo específico en la UI</td></tr><tr><td>icon</td><td>Icono del node</td></tr><tr><td>category</td><td>Categoría del node</td></tr><tr><td>author</td><td>Creador del node</td></tr><tr><td>description</td><td>Descripción del node</td></tr><tr><td>baseClasses</td><td>Las clases base del node, ya que un node puede extender de un componente base. Usado para definir qué node puede conectarse a este node en la UI</td></tr></tbody></table>

### Define Class

Ahora que la clase del componente está parcialmente terminada, podemos proceder a definir la clase actual Tool, en este caso - `Calculator`.

Crea un nuevo archivo bajo la misma carpeta `Calculator`, y nómbralo como `core.ts`

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

Vuelve al archivo `Calculator.ts`, podemos terminarlo teniendo la función `async init`. En esta función, inicializaremos la clase Calculator que creamos arriba. Cuando el flujo se está ejecutando, la función `init` en cada node será llamada, y la función `_call` será ejecutada cuando el LLM decida llamar a esta tool.

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

En el archivo `.env` dentro de `packages/server`, crea una nueva variable de entorno:

```javascript
SHOW_COMMUNITY_NODES=true
```

¡Ahora podemos usar `pnpm build` y `pnpm start` para dar vida al componente!

<figure><img src="../.gitbook/assets/image (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>
