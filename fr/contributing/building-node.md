# Construction de Node

### Installer Git

Tout d'abord, installez Git et clonez le dépôt Flowise. Vous pouvez suivre les étapes du guide [Commencer](broken-reference).

### Structure

Flowise sépare chaque intégration de node dans le dossier `packages/components/nodes`. Essayons de créer un outil simple !

### Créer l'outil Calculatrice

Créez un nouveau dossier nommé `Calculator` dans le dossier `packages/components/nodes/tools`. Ensuite, créez un nouveau fichier nommé `Calculator.ts`. À l'intérieur du fichier, nous allons d'abord écrire la classe de base.

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

<table><thead><tr><th width="271">Propriété</th><th>Description</th></tr></thead><tbody><tr><td>label</td><td>Le nom du nœud qui apparaît dans l'interface utilisateur</td></tr><tr><td>name</td><td>Le nom utilisé par le code. Doit être <strong>camelCase</strong></td></tr><tr><td>version</td><td>Version du nœud</td></tr><tr><td>type</td><td>Généralement identique au label. Pour définir quel nœud peut être connecté à ce type spécifique dans l'interface utilisateur</td></tr><tr><td>icon</td><td>Icône du nœud</td></tr><tr><td>category</td><td>Catégorie du nœud</td></tr><tr><td>author</td><td>Créateur du nœud</td></tr><tr><td>description</td><td>Description du nœud</td></tr><tr><td>baseClasses</td><td>Les classes de base du nœud, puisque un nœud peut s'étendre à partir d'un composant de base. Utilisé pour définir quel nœud peut être connecté à ce nœud dans l'interface utilisateur</td></tr></tbody></table>

### Définir la classe

Maintenant que la classe de composant est partiellement terminée, nous pouvons procéder à la définition de la classe Tool réelle, dans ce cas - `Calculator`.

Créez un nouveau fichier dans le même dossier `Calculator`, et nommez-le `core.ts`

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

Retournez au fichier `Calculator.ts`, nous pouvons terminer cela en ajoutant la fonction `async init`. Dans cette fonction, nous allons initialiser la classe Calculator que nous avons créée ci-dessus. Lorsque le flux est exécuté, la fonction `init` dans chaque nœud sera appelée, et la fonction `_call` sera exécutée lorsque le LLM décidera d'appeler cet outil.

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

### Construire et Exécuter

Dans le fichier `.env` à l'intérieur de `packages/server`, créez une nouvelle variable d'environnement :

```javascript
SHOW_COMMUNITY_NODES=true
```

Maintenant, nous pouvons utiliser `pnpm build` et `pnpm start` pour donner vie au composant !

<figure><img src="../.gitbook/assets/image (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>
