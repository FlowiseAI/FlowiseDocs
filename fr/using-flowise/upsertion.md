# Augmentation

Upsert fait référence au processus de téléchargement et de traitement des documents dans les magasins vectoriels, formant les bases des systèmes de génération augmentée (RAG) de récupération.

Il existe deux façons fondamentales de renverser les données dans Vector Store:

* [Document Stores (Recommended)](document-stores.md)
* ChatFlow Upsert

Nous vous recommandons fortement d'utiliser des magasins de documents car il fournit une interface unifiée pour aider avec les pipelines de chiffon - récupérer des données à partir de différentes sources, la stratégie de section, la mise en œuvre de la base de données vectorielle, la synchronisation avec les données mises à jour.

Dans ce guide, nous allons couvrir une autre méthode - ChatFlow Upsert. Il s'agit d'une méthode plus ancienne avant les magasins de documents.

Pour plus de détails, voir le[Vector Upsert Endpoint API Reference](../api-reference/vector-upsert.md).

## Comprendre le processus de mise en service

ChatFlow vous permet de créer un flux qui peut effectuer à la fois le processus de requête à la hausse et de requête RAG, les deux peuvent être exécutés de manière identique.

<gigne> <img src = "../. GitBook / Assets / UD_01.png" alt = ""> <Figcaption> <p> Upsert vs Rag </p> </gigcaption> </gigne>

## Installation

Pour qu'un processus Upsert fonctionne, nous aurions besoin de créer un ** Flow userting ** avec 5 nœuds différents:

1. Chargeur de documents
2. Séparateur de texte
3. Modèle d'incorporation
4. Magasin vectoriel
5. Record Manager (facultatif)

Tous les éléments ont été couverts par[Document Stores](document-stores.md), reportez-vous là pour plus de détails.

Une fois que le flux est correctement configuré, il y aura un bouton vert en haut à droite qui permet à l'utilisateur de démarrer le processus UPSERT.

<gigne> <img src = "../. GitBook / Assets / Picture1.png" alt = ""> <Figcaption> </gigcaption> </gigust>

<gigne> <img src = "../. GitBook / Assets / Image (1) (1) (1) (1) (1) .png" alt = "" width = "563"> <figCaption> </gigcaption> </gigu

Le processus Upsert peut également être effectué via l'API:

<gigne> <img src = "../. GitBook / Assets / Image (2) (1) (1) (1) (1) .png" alt = "" width = "563"> <figCaption> </gigcaption> </gigu

## URL de base et authentification

** URL de base **:`http://localhost:3000`(ou votre URL d'instance fluide)

** Point de terminaison **:`POST /api/v1/vector/upsert/:id`

** Authentification **: se référer[Authentication for Flows](../configuration/authorization/chatflow-level.md)

## Méthodes de demande

L'API prend en charge deux méthodes de demande différentes en fonction de votre configuration de chat:

#### 1. Données de formulaire (téléchargement de fichiers)

Utilisé lorsque votre ChatFlow contient des chargeurs de documents avec une capacité de téléchargement de fichiers.

#### 2. Body JSON (pas de téléchargement de fichiers)

Utilisé lorsque votre ChatFlow utilise des chargeurs de documents qui ne nécessitent pas de téléchargements de fichiers (par exemple, les grattoirs Web, les connecteurs de base de données).

{% hint style = "avertissement"%}
Pour remplacer toutes les configurations de nœud telles que les fichiers, les métadonnées, etc., vous devez activer explicitement cette option.
{% EndHint%}

<gigne> <img src = "../. GitBook / Assets / image (3) (1) (1) (1) .png" alt = ""> <figcaption> </gigcaption> </gigust>

### Chargeurs de documents avec téléchargement de fichiers

#### Types de documents pris en charge

| Chargeur de documents | Types de fichiers |
| ----------------- | ---------- |
| Fichier CSV |`.csv`     |
| Docx / Word Fichier |`.docx`    |
| Fichier JSON |`.json`    |
| Fichier de lignes JSON |`.jsonl`   |
| Fichier PDF |`.pdf`     |
| Fichier texte |`.txt`     |
| Fichier Excel |`.xlsx`    |
| Fichier PowerPoint |`.pptx`    |
| Chargeur de fichiers | Multiple |
| Fichier non structuré | Multiple |

{% hint style = "info"%}
** IMPORTANT **: Assurez-vous que le type de fichier correspond à votre configuration de chargeur de document. Pour une flexibilité maximale, envisagez d'utiliser le chargeur de fichiers qui prend en charge plusieurs types de fichiers.
{% EndHint%}

#### Demander le format (données de formulaire)

Lors du téléchargement de fichiers, utilisez`multipart/form-data`Au lieu de JSON:

#### Exemples

{% Tabs%}
{% tab title = "python"%}
```python
import requests
import os

def upsert_document(chatflow_id, file_path, config=None):
    """
    Upsert a single document to a vector store.
    
    Args:
        chatflow_id (str): The chatflow ID configured for vector upserting
        file_path (str): Path to the file to upload
        return_source_docs (bool): Whether to return source documents in response
        config (dict): Optional configuration overrides
    
    Returns:
        dict: API response containing upsert results
    """
    url = f"http://localhost:3000/api/v1/vector/upsert/{chatflow_id}"
    
    # Prepare file data
    files = {
        'files': (os.path.basename(file_path), open(file_path, 'rb'))
    }
    
    # Prepare form data
    data = {}
    
    # Add configuration overrides if provided
    if config:
        data['overrideConfig'] = str(config).replace("'", '"')  # Convert to JSON string
    
    try:
        response = requests.post(url, files=files, data=data)
        response.raise_for_status()
        
        return response.json()
        
    except requests.exceptions.RequestException as e:
        print(f"Upload failed: {e}")
        return None
    finally:
        # Always close the file
        files['files'][1].close()

# Example usage
result = upsert_document(
    chatflow_id="your-chatflow-id",
    file_path="documents/knowledge_base.pdf",
    config={
        "chunkSize": 1000,
        "chunkOverlap": 200
    }
)

if result:
    print(f"Successfully upserted {result.get('numAdded', 0)} chunks")
    if result.get('sourceDocuments'):
        print(f"Source documents: {len(result['sourceDocuments'])}")
else:
    print("Upload failed")
```
{% endtab%}

{% Tab Title = "JavaScript (Browser)"%}
```javascript
class VectorUploader {
    constructor(baseUrl = 'http://localhost:3000') {
        this.baseUrl = baseUrl;
    }
    
    async upsertDocument(chatflowId, file, config = {}) {
        /**
         * Upload a file to vector store from browser
         * @param {string} chatflowId - The chatflow ID
         * @param {File} file - File object from input element
         * @param {Object} config - Optional configuration
         */
        
        const formData = new FormData();
        formData.append('files', file);
        
        if (config.overrideConfig) {
            formData.append('overrideConfig', JSON.stringify(config.overrideConfig));
        }
        
        try {
            const response = await fetch(`${this.baseUrl}/api/v1/vector/upsert/${chatflowId}`, {
                method: 'POST',
                body: formData
            });
            
            if (!response.ok) {
                throw new Error(`HTTP error! status: ${response.status}`);
            }
            
            const result = await response.json();
            return result;
            
        } catch (error) {
            console.error('Upload failed:', error);
            throw error;
        }
    }
    
  
}

// Example usage in browser
const uploader = new VectorUploader();

// Single file upload
document.getElementById('fileInput').addEventListener('change', async function(e) {
    const file = e.target.files[0];
    if (file) {
        try {
            const result = await uploader.upsertDocument(
                'your-chatflow-id',
                file,
                {
                    overrideConfig: {
                        chunkSize: 1000,
                        chunkOverlap: 200
                    }
                }
            );
            
            console.log('Upload successful:', result);
            alert(`Successfully processed ${result.numAdded || 0} chunks`);
            
        } catch (error) {
            console.error('Upload failed:', error);
            alert('Upload failed: ' + error.message);
        }
    }
});
```
{% endtab%}

{% tab title = "javascript (node.js)"%}
```javascript
const fs = require('fs');
const path = require('path');
const FormData = require('form-data');
const fetch = require('node-fetch');

class NodeVectorUploader {
    constructor(baseUrl = 'http://localhost:3000') {
        this.baseUrl = baseUrl;
    }
    
    async upsertDocument(chatflowId, filePath, config = {}) {
        /**
         * Upload a file to vector store from Node.js
         * @param {string} chatflowId - The chatflow ID
         * @param {string} filePath - Path to the file
         * @param {Object} config - Optional configuration
         */
        
        if (!fs.existsSync(filePath)) {
            throw new Error(`File not found: ${filePath}`);
        }
        
        const formData = new FormData();
        const fileStream = fs.createReadStream(filePath);
        
        formData.append('files', fileStream, {
            filename: path.basename(filePath),
            contentType: this.getMimeType(filePath)
        });
        
        if (config.overrideConfig) {
            formData.append('overrideConfig', JSON.stringify(config.overrideConfig));
        }
        
        try {
            const response = await fetch(`${this.baseUrl}/api/v1/vector/upsert/${chatflowId}`, {
                method: 'POST',
                body: formData,
                headers: formData.getHeaders()
            });
            
            if (!response.ok) {
                const errorText = await response.text();
                throw new Error(`HTTP ${response.status}: ${errorText}`);
            }
            
            return await response.json();
            
        } catch (error) {
            console.error('Upload failed:', error);
            throw error;
        }
    }

    getMimeType(filePath) {
        const ext = path.extname(filePath).toLowerCase();
        const mimeTypes = {
            '.pdf': 'application/pdf',
            '.txt': 'text/plain',
            '.docx': 'application/vnd.openxmlformats-officedocument.wordprocessingml.document',
            '.csv': 'text/csv',
            '.json': 'application/json'
        };
        return mimeTypes[ext] || 'application/octet-stream';
    }
}

// Example usage
async function main() {
    const uploader = new NodeVectorUploader();
    
    try {
        // Single file upload
        const result = await uploader.upsertDocument(
            'your-chatflow-id',
            './documents/manual.pdf',
            {
                overrideConfig: {
                    chunkSize: 1200,
                    chunkOverlap: 100
                }
            }
        );
        
        console.log('Single file upload result:', result); 
    } catch (error) {
        console.error('Process failed:', error);
    }
}

// Run if this file is executed directly
if (require.main === module) {
    main();
}

module.exports = { NodeVectorUploader };
```
{% endtab%}

{% tab title = "curl"%}
```bash
# Basic file upload with cURL
curl -X POST "http://localhost:3000/api/v1/vector/upsert/your-chatflow-id" \
  -F "files=@documents/knowledge_base.pdf"

# File upload with configuration override
curl -X POST "http://localhost:3000/api/v1/vector/upsert/your-chatflow-id" \
  -F "files=@documents/manual.pdf" \
  -F 'overrideConfig={"chunkSize": 1000, "chunkOverlap": 200}'

# Upload with custom headers for authentication (if configured)
curl -X POST "http://localhost:3000/api/v1/vector/upsert/your-chatflow-id" \
  -H "Authorization: Bearer your-api-token" \
  -F "files=@documents/faq.txt" \
  -F 'overrideConfig={"chunkSize": 800, "chunkOverlap": 150}'
```
{% endtab%}
{% endtabs%}

### Chargeurs de documents sans téléchargement de fichiers

Pour les chargeurs de documents qui ne nécessitent pas de téléchargements de fichiers (par exemple, les grabyers Web, les connecteurs de base de données, les intégrations API), utilisez le format JSON similaire à l'API de prédiction.

#### Exemples

{% Tabs%}
{% tab title = "python"%}
```python
import requests
from typing import Dict, Any, Optional

def upsert(chatflow_id: str, config: Optional[Dict[str, Any]] = None) -> Optional[Dict[str, Any]]:
    """
    Trigger vector upserting for chatflows that don't require file uploads.
    
    Args:
        chatflow_id: The chatflow ID configured for vector upserting
        config: Optional configuration overrides
    
    Returns:
        API response containing upsert results
    """
    url = f"http://localhost:3000/api/v1/vector/upsert/{chatflow_id}"
    
    payload = {
        "overrideConfig": config
    }
    
    headers = {
        "Content-Type": "application/json"
    }
    
    try:
        response = requests.post(url, json=payload, headers=headers, timeout=300)
        response.raise_for_status()
        
        return response.json()
        
    except requests.exceptions.RequestException as e:
        print(f"Upsert failed: {e}")
        return None

result = upsert(
    chatflow_id="chatflow-id",
    config={
        "chunkSize": 800,
        "chunkOverlap": 100,
    }
)

if result:
    print(f"Upsert completed: {result.get('numAdded', 0)} chunks added")
```
{% endtab%}

{% tab title = "javascript"%}
```javascript
class NoFileUploader {
    constructor(baseUrl = 'http://localhost:3000') {
        this.baseUrl = baseUrl;
    }
    
    async upsertWithoutFiles(chatflowId, config = {}) {
        /**
         * Trigger vector upserting for flows that don't need file uploads
         * @param {string} chatflowId - The chatflow ID
         * @param {Object} config - Configuration overrides
         */
        
        const payload = {
            overrideConfig: config
        };
        
        try {
            const response = await fetch(`${this.baseUrl}/api/v1/vector/upsert/${chatflowId}`, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify(payload)
            });
            
            if (!response.ok) {
                throw new Error(`HTTP error! status: ${response.status}`);
            }
            
            return await response.json();
            
        } catch (error) {
            console.error('Upsert failed:', error);
            throw error;
        }
    }
    
    async scheduledUpsert(chatflowId, interval = 3600000) {
        /**
         * Set up scheduled upserting for dynamic content sources
         * @param {string} chatflowId - The chatflow ID
         * @param {number} interval - Interval in milliseconds (default: 1 hour)
         */
        
        console.log(`Starting scheduled upsert every ${interval/1000} seconds`);
        
        const performUpsert = async () => {
            try {
                console.log('Performing scheduled upsert...');
                
                const result = await this.upsertWithoutFiles(chatflowId, {
                    addMetadata: {
                        scheduledUpdate: true,
                        timestamp: new Date().toISOString()
                    }
                });
                
                console.log(`Scheduled upsert completed: ${result.numAdded || 0} chunks processed`);
                
            } catch (error) {
                console.error('Scheduled upsert failed:', error);
            }
        };
        
        // Perform initial upsert
        await performUpsert();
        
        // Set up recurring upserts
        return setInterval(performUpsert, interval);
    }
}

// Example usage
const uploader = new NoFileUploader();

async function performUpsert() {
    try {
        const result = await uploader.upsertWithoutFiles(
            'chatflow-id',
            {
                chunkSize: 800,
                chunkOverlap: 100
            }
        );
        
        console.log('Upsert result:', result);
        
    } catch (error) {
        console.error('Upsert failed:', error);
    }
}

// One time upsert
await performUpsert();

// Set up scheduled updates (every 30 minutes)
const schedulerHandle = await uploader.scheduledUpsert(
    'dynamic-content-chatflow-id',
    30 * 60 * 1000
);

// To stop scheduled updates later:
// clearInterval(schedulerHandle);
```
{% endtab%}

{% tab title = "curl"%}
```bash
# Basic upsert with cURL
curl -X POST "http://localhost:3000/api/v1/vector/upsert/your-chatflow-id" \
  -H "Content-Type: application/json"

# Upsert with configuration override
curl -X POST "http://localhost:3000/api/v1/vector/upsert/your-chatflow-id" \
  -H "Content-Type: application/json" \
  -d '{
    "overrideConfig": {
      "returnSourceDocuments": true
    }
  }'
  
# Upsert with custom headers for authentication (if configured)
curl -X POST "http://localhost:3000/api/v1/vector/upsert/your-chatflow-id" \
  -H "Authorization: Bearer your-api-token" \
  -H "Content-Type: application/json"
```
{% endtab%}
{% endtabs%}

## Champs de réponse

| Champ | Type | Description |
| ------------ | ------ | ----------------------------------------------------------- |
| `numAdded`| Numéro | Nombre de nouveaux morceaux ajoutés au magasin vectoriel |
| `numDeleted`| Numéro | Nombre de morceaux supprimés (si vous utilisez Record Manager) |
| `numSkipped`| Numéro | Nombre de morceaux sautés (si vous utilisez un gestionnaire d'enregistrements) |
| `numUpdated`| Numéro | Nombre de morceaux existants mis à jour (si vous utilisez un gestionnaire d'enregistrements) |

## Stratégies d'optimisation

### 1. Stratégies de traitement par lots

```python
def intelligent_batch_processing(files: List[str], chatflow_id: str) -> Dict[str, Any]:
    """Process files in optimized batches based on size and type."""
    
    # Group files by size and type
    small_files = []
    large_files = []
    
    for file_path in files:
        file_size = os.path.getsize(file_path)
        if file_size > 5_000_000:  # 5MB
            large_files.append(file_path)
        else:
            small_files.append(file_path)
    
    results = {'successful': [], 'failed': [], 'totalChunks': 0}
    
    # Process large files individually
    for file_path in large_files:
        print(f"Processing large file: {file_path}")
        # Individual processing with custom config
        # ... implementation
    
    # Process small files in batches
    batch_size = 5
    for i in range(0, len(small_files), batch_size):
        batch = small_files[i:i + batch_size]
        print(f"Processing batch of {len(batch)} small files")
        # Batch processing
        # ... implementation
    
    return results
```

### 2. Optimisation des métadonnées

```python
import requests
import os
from datetime import datetime
from typing import Dict, Any

def upsert_with_optimized_metadata(chatflow_id: str, file_path: str, 
                                 department: str = None, category: str = None) -> Dict[str, Any]:
    """
    Upsert document with automatically optimized metadata.
    """
    url = f"http://localhost:3000/api/v1/vector/upsert/{chatflow_id}"
    
    # Generate optimized metadata
    custom_metadata = {
        'department': department or 'general',
        'category': category or 'documentation',
        'indexed_date': datetime.now().strftime('%Y-%m-%d'),
        'version': '1.0'
    }
    
    optimized_metadata = optimize_metadata(file_path, custom_metadata)
    
    # Prepare request
    files = {'files': (os.path.basename(file_path), open(file_path, 'rb'))}
    data = {
        'overrideConfig': str({
            'metadata': optimized_metadata
        }).replace("'", '"')
    }
    
    try:
        response = requests.post(url, files=files, data=data)
        response.raise_for_status()
        return response.json()
    finally:
        files['files'][1].close()

# Example usage with different document types
results = []

# Technical documentation
tech_result = upsert_with_optimized_metadata(
    chatflow_id="tech-docs-chatflow",
    file_path="docs/api_reference.pdf",
    department="engineering",
    category="technical_docs"
)
results.append(tech_result)

# HR policies
hr_result = upsert_with_optimized_metadata(
    chatflow_id="hr-docs-chatflow", 
    file_path="policies/employee_handbook.pdf",
    department="human_resources",
    category="policies"
)
results.append(hr_result)

# Marketing materials
marketing_result = upsert_with_optimized_metadata(
    chatflow_id="marketing-chatflow",
    file_path="marketing/product_brochure.pdf", 
    department="marketing",
    category="promotional"
)
results.append(marketing_result)

for i, result in enumerate(results):
    print(f"Upload {i+1}: {result.get('numAdded', 0)} chunks added")
```

## Dépannage

1. ** le téléchargement de fichiers échoue **
   * Vérifier la compatibilité du format de fichier
   * Vérifiez les limites de taille du fichier
2. ** Traitement de temps mort **
   * Augmenter le délai de demande
   * Cassez les fichiers volumineux en pièces plus petites
   * Optimiser la taille du morceau
3. ** Erreurs du magasin vectoriel **
   * Vérifiez la connectivité du magasin vectoriel
   * Vérifiez la compatibilité des dimensions du modèle d'intégration
