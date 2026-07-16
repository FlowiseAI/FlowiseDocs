# 与 API 交互

几乎所有 Web 应用程序都依赖于 RESTful API。使 LLM 能够与它们交互，扩展了其实用性。

本教程展示了如何使用 LLM 通过工具调用来进行 API 调用。

## 先决条件 - 示例事件管理服务器

我们将使用一个简单的事件管理服务器，并展示如何与其交互。

```javascript
const express = require('express');
const { v4: uuidv4 } = require('uuid');

const app = express();
const PORT = process.env.PORT || 5566;

// Middleware
app.use(express.json());

// Fake database - in-memory storage
let events = [
  {
    id: '1',
    name: 'Tech Conference 2024',
    date: '2024-06-15T09:00:00Z',
    location: 'San Francisco, CA'
  },
  {
    id: '2',
    name: 'Music Festival',
    date: '2024-07-20T18:00:00Z',
    location: 'Austin, TX'
  },
  {
    id: '3',
    name: 'Art Exhibition Opening',
    date: '2024-05-10T14:00:00Z',
    location: 'New York, NY'
  },
  {
    id: '4',
    name: 'Startup Networking Event',
    date: '2024-08-05T17:30:00Z',
    location: 'Seattle, WA'
  },
  {
    id: '5',
    name: 'Food & Wine Tasting',
    date: '2024-09-12T19:00:00Z',
    location: 'Napa Valley, CA'
  }
];

// Helper function to validate event data
const validateEvent = (eventData) => {
  const required = ['name', 'date', 'location'];
  const missing = required.filter(field => !eventData[field]);
  
  if (missing.length > 0) {
    return { valid: false, message: `Missing required fields: ${missing.join(', ')}` };
  }
  
  // Basic date validation
  const dateRegex = /^\d{4}-\d{2}-\d{2}T\d{2}:\d{2}:\d{2}(\.\d{3})?Z?$/;
  if (!dateRegex.test(eventData.date)) {
    return { valid: false, message: 'Date must be in ISO 8601 format (YYYY-MM-DDTHH:mm:ssZ)' };
  }
  
  return { valid: true };
};

// GET /events - List all events
app.get('/events', (req, res) => {
  res.status(200).json(events);
});

// POST /events - Create a new event
app.post('/events', (req, res) => {
  const validation = validateEvent(req.body);
  
  if (!validation.valid) {
    return res.status(400).json({ error: validation.message });
  }
  
  const newEvent = {
    id: req.body.id || uuidv4(),
    name: req.body.name,
    date: req.body.date,
    location: req.body.location
  };
  
  events.push(newEvent);
  res.status(201).json(newEvent);
});

// GET /events/{id} - Retrieve an event by ID
app.get('/events/:id', (req, res) => {
  const event = events.find(e => e.id === req.params.id);
  
  if (!event) {
    return res.status(404).json({ error: 'Event not found' });
  }
  
  res.status(200).json(event);
});

// DELETE /events/{id} - Delete an event by ID
app.delete('/events/:id', (req, res) => {
  const eventIndex = events.findIndex(e => e.id === req.params.id);
  
  if (eventIndex === -1) {
    return res.status(404).json({ error: 'Event not found' });
  }
  
  events.splice(eventIndex, 1);
  res.status(204).send();
});

// PATCH /events/{id} - Update an event's details by ID
app.patch('/events/:id', (req, res) => {
  const eventIndex = events.findIndex(e => e.id === req.params.id);
  
  if (eventIndex === -1) {
    return res.status(404).json({ error: 'Event not found' });
  }
  
  const validation = validateEvent(req.body);
  
  if (!validation.valid) {
    return res.status(400).json({ error: validation.message });
  }
  
  // Update the event
  events[eventIndex] = {
    ...events[eventIndex],
    name: req.body.name,
    date: req.body.date,
    location: req.body.location
  };
  
  res.status(200).json(events[eventIndex]);
});

// Error handling middleware
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ error: 'Something went wrong!' });
});

// 404 handler
app.use((req, res) => {
  res.status(404).json({ error: 'Endpoint not found' });
});

// Start the server
app.listen(PORT, () => {
  console.log(`Event Management API server is running on port ${PORT}`);
  console.log(`Available endpoints:`);
  console.log(`  GET    /events      - List all events`);
  console.log(`  POST   /events      - Create a new event`);
  console.log(`  GET    /events/{id} - Get event by ID`);
  console.log(`  PATCH  /events/{id} - Update event by ID`);
  console.log(`  DELETE /events/{id} - Delete event by ID`);
});

module.exports = app; 
```

***

## 请求工具

有 4 个请求工具可以使用。这允许 LLM 在必要时调用 GET、POST、PUT、DELETE 工具。

### 步骤1：添加起始节点

开始节点是流程的入口点

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="324"><figcaption></figcaption></figure>

### 步骤2：添加代理节点

接下来，添加一个 Agent 节点。在此设置中，代理配置有四个主要工具：GET、POST、PUT 和 DELETE。每个工具都设置为执行特定类型的 API 请求。

#### 工具 1：GET（检索事件）

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="335"><figcaption></figcaption></figure>

* **目的：** 从 API 获取事件列表或特定事件。
* **配置输入：**
  * **URL:** `http://localhost:5566/events`
  * **名称：** `get_events`
  * **描述：** 描述何时使用此工具。例如：`Use this when you need to get events`
  * **标头：** （可选）添加任何必需的 HTTP 标头。
  * **查询参数架构：** API 的 JSON 架构，允许 LLM 了解 URL 结构，即要生成的路径和查询参数。例如：

      ```json
      {
        "id": {
          "type": "string",
          "in": "path",
          "description": "ID of the item to get. /:id"
        },
        "limit": {
          "type": "string",
          "in": "query",
          "description": "Limit the number of items to get. ?limit=10"
        }
      }
      ```

#### 工具 2：POST（创建事件）

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="335"><figcaption></figcaption></figure>

* **目的：** 在系统中创建一个新事件。
* **配置输入：**
  * **URL:** `http://localhost:5566/events`
  * **名称：** `create_event`
  * **描述：** `Use this when you want to create a new event.`
  * **标头：** （可选）添加任何必需的 HTTP 标头。
  * **Body**：硬编码的主体对象，将覆盖 LLM 生成的主体
  * **主体架构：** API 请求主体的 JSON 架构，允许 LLM 了解如何自动生成正确的 JSON 主体。例如：

      ```json
      {
        "name": {
          "type": "string",
          "required": true,
          "description": "Name of the event"
        },
        "date": {
          "type": "string",
          "required": true,
          "description": "Date of the event"
        },
        "location": {
          "type": "string",
          "required": true,
          "description": "Location of the event"
        }
      }
      ```

#### 工具 3：PUT（更新事件）

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="335"><figcaption></figcaption></figure>

* **目的：** 更新现有活动的详细信息。
* **配置输入：**
  * **URL:** `http://localhost:5566/events`
  * **名称：** `update_event`
  * **描述：** `Use this when you want to update an event.`
  * **标头：** （可选）添加任何必需的 HTTP 标头。
  * **Body**：硬编码的主体对象，将覆盖 LLM 生成的主体
  * **主体架构：** API 请求主体的 JSON 架构，允许 LLM 了解如何自动生成正确的 JSON 主体。例如：

      ```json
      {
        "name": {
          "type": "string",
          "required": true,
          "description": "Name of the event"
        },
        "date": {
          "type": "string",
          "required": true,
          "description": "Date of the event"
        },
        "location": {
          "type": "string",
          "required": true,
          "description": "Location of the event"
        }
      }
      ```

#### 工具 4：DELETE（删除事件）

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="335"><figcaption></figcaption></figure>

* **目的：** 从系统中删除事件。
* **配置输入：**
  * **URL:** `http://localhost:5566/events`
  * **名称：** `delete_event`
  * **描述：** `Use this when you need to delete an event.`
  * **标头：** （可选）添加任何必需的 HTTP 标头。
  * **查询参数架构：** API 的 JSON 架构，允许 LLM 了解 URL 结构，即要生成的路径和查询参数。例如：

      ```json
      {
        "id": {
          "type": "string",
          "required": true,
          "in": "path",
          "description": "ID of the item to delete. /:id"
        }
      }
      ```

### 代理如何使用这些工具

* 代理可以根据用户的请求或流程的逻辑动态选择使用哪个工具。
* 每个工具都映射到特定的 HTTP 方法和端点，并具有明确定义的输入模式。
* 代理利用 LLM 解释用户输入，填写所需参数，并进行适当的 API 调用。

当然！以下是您的流程的一些**示例交互**，包括示例用户查询和每个查询的预期行为，映射到相应的工具（GET、POST、PUT、DELETE）：

### 交互示例

#### 1. 检索事件 (GET)

**查询示例：**

>“显示所有即将发生的事件。”

**预期行为：**

* 代理选择 **GET** 工具。
* 它向 `http://localhost:5566/events` 发送 GET 请求。
* 代理向用户返回所有事件的列表。

***

**查询示例：**

>“获取 ID 为 12345 的活动的详细信息。”

**预期行为：**

* 代理选择 **GET** 工具。
* 它向 `http://localhost:5566/events/12345` 发送 GET 请求。
* 代理返回 ID 为 `12345` 的事件的详细信息。

***

#### 2. 创建一个新事件 (POST)

**查询示例：**

> “于 2024 年 7 月 15 日在 Tech Hall 创建一个名为“AI 会议”的新活动。”

**预期行为：**

* 代理选择 **POST** 工具。
* 它向 `http://localhost:5566/events` 发送 POST 请求，请求体如下：

    ```json
    {
      "name": "AI Conference",
      "date": "2024-07-15",
      "location": "Tech Hall"
    }
    ```
* 代理确认事件已创建，并可能返回新事件的详细信息。

***

#### 3. 更新事件 (PUT)

**查询示例：**

> “将2024年7月15日举办的‘AI大会’地点改为‘主礼堂’。”

**预期行为：**

* 代理选择 **PUT** 工具。
* 它向 `http://localhost:5566/events` 发送 PUT 请求，其中包含更新的事件详细信息：

    ```json
    {
      "name": "AI Conference",
      "date": "2024-07-15",
      "location": "Main Auditorium"
    }
    ```
* 代理确认该事件已更新。

***

#### 4. 删除事件 (DELETE)

**查询示例：**

> “删除 ID 为 12345 的事件。”

**预期行为：**

* 代理选择 **DELETE** 工具。
* 它向 `http://localhost:5566/events/12345` 发送 DELETE 请求。
* 代理确认该事件已被删除。

### 完整流程

{% file src="../.gitbook/assets/Requests Tool Agent.json" %}

***

## OpenAPI 工具包

如果您有几个 API，4 个请求工具会非常有用，但想象一下有数十或数百个 API，这可能会变得难以维护。为了解决这个问题，Flowise 提供了一个 OpenAPI 工具包，它能够接收 OpenAPI YAML 文件，并将每个 API 解析为一个工具。 [OpenAPI 规范 (OAS)](https://swagger.io/specification/) 是一项普遍接受的标准，用于以机器可以读取和解释的格式描述 RESTful API 的详细信息。 

使用事件管理 API，我们可以生成一个 OpenAPI YAML 文件：

```yaml
openapi: 3.0.0
info:
  version: 1.0.0
  title: Event Management API
  description: An API for managing event data

servers:
  - url: http://localhost:5566
    description: Local development server

paths:
  /events:
    get:
      summary: List all events
      operationId: listEvents
      responses:
        '200':
          description: A list of events
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Event'
    
    post:
      summary: Create a new event
      operationId: createEvent
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/EventInput'
      responses:
        '201':
          description: The event was created
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Event'
        '400':
          description: Invalid input
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'

  /events/{id}:
    parameters:
      - name: id
        in: path
        required: true
        schema:
          type: string
        description: The event ID
    
    get:
      summary: Retrieve an event by ID
      operationId: getEventById
      responses:
        '200':
          description: The event
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Event'
        '404':
          description: Event not found
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
    
    patch:
      summary: Update an event's details by ID
      operationId: updateEventDetails
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/EventInput'
      responses:
        '200':
          description: The event's details were updated
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Event'
        '400':
          description: Invalid input
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
        '404':
          description: Event not found
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
    
    delete:
      summary: Delete an event by ID
      operationId: deleteEvent
      responses:
        '204':
          description: The event was deleted
        '404':
          description: Event not found
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'

components:
  schemas:
    Event:
      type: object
      properties:
        id:
          type: string
          description: The unique identifier for the event
        name:
          type: string
          description: The name of the event
        date:
          type: string
          format: date-time
          description: The date and time of the event in ISO 8601 format
        location:
          type: string
          description: The location of the event
      required:
        - name
        - date
        - location
    
    EventInput:
      type: object
      properties:
        name:
          type: string
          description: The name of the event
        date:
          type: string
          format: date-time
          description: The date and time of the event in ISO 8601 format
        location:
          type: string
          description: The location of the event
      required:
        - name
        - date
        - location
    
    Error:
      type: object
      properties:
        error:
          type: string
          description: Error message
```

### 步骤1：添加起始节点

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="319"><figcaption></figcaption></figure>

### 步骤2：添加代理节点

接下来，添加一个 Agent 节点。在此设置中，代理仅配置了 1 个工具 - OpenAPI Toolkit

#### 工具：OpenAPI 工具包

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="332"><figcaption></figcaption></figure>

* **目的：** 从 YAML 文件中获取 API 列表，并将每个 API 转换为工具列表
* **配置输入：**
  * **YAML 文件：** OpenAPI YAML 文件
  * **Return Direct:** 是否直接返回API的响应
  * **标头：** （可选）添加任何必需的 HTTP 标头。
  * **删除空参数：** 从解析的参数中删除所有具有空值的键
  * **自定义代码**：自定义返回响应的方式

### 交互示例：

我们可以使用前面示例中的相同示例查询来测试它：

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

***

## 依次调用 API

从上面的例子中，我们已经看到了Agent如何动态调用工具并与API交互。在某些情况下，可能需要在某些操作之前或之后顺序调用 API。例如，您可以从 CRM 获取客户列表并将其传递给代理。在这种情况下，您可以使用 [HTTP 节点](../using-flowise/agentflowv2.md#id-6.-http-node)。

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

## 最佳实践

* 当您希望代理获取最新信息时，通常会使用与 API 交互。例如，代理可能会检索您的日历可用性、项目状态或其他实时数据。
* 在系统提示词中明确包含当前时间通常会很有帮助。 Flowise 提供了一个名为 `{{current_date_time}}` 的变量，用于检索当前日期和时间。这使得 LLM 能够了解当前时刻，因此，如果您询问今天的空闲时间，模型可以参考正确的日期。否则，它可能依赖于最后的训练截止日期，这将返回过时的信息。例如：

```
You are helpful assistant.

Todays date time is {{current_date_time }}
```
