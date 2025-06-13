# Interacting with API

Almost all of the internet applications relies on RESTful APIs. Enabling LLM to interact with them expand its practical utility.

This tutorial showcases how LLM can be used to make API calls through tool calling.

## Prerequisite

We are going to use a simple Event Management server, and showcase how to interact with it.

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

## Request Tools

There are 4 Request tools that can be used. This allows LLM to call the GET, POST, PUT, DELETE tools when necessary.

### Step 1: Add the Start Node

The Start node is the entry point of your flow

Absolutely! Here’s an in-depth explanation for **Step 2: Add the Requests Agent Node**, focusing on each tool’s input and configuration.

<figure><img src="../.gitbook/assets/image.png" alt="" width="324"><figcaption></figcaption></figure>

### Step 2: Add the Agent Node

Next, add an Agent node. In this setup, the agent is configured with four main tools: GET, POST, PUT, and DELETE. Each tool is set up to perform a specific type of API request.

#### Tool 1: GET (Retrieve Events)

<figure><img src="../.gitbook/assets/image (1).png" alt="" width="335"><figcaption></figcaption></figure>

* **Purpose:** Fetch a list of events or a specific event from the API.
* **Configuration Inputs:**
  * **URL:** `http://localhost:5566/events`
  * **Name:** `get_events`
  * **Description:** Describe when to use this tool. For example: `Use this when you need to get events`
  * **Headers:** (Optional) Add any required HTTP headers.
  *   **Query Params Schema:** A JSON schema of the API which allows LLM to know about the URL structure, which path and query parameters to generate. For instance:

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

#### Tool 2: POST (Create Event)

<figure><img src="../.gitbook/assets/image (2).png" alt="" width="335"><figcaption></figcaption></figure>

* **Purpose:** Create a new event in the system.
* **Configuration Inputs:**
  * **URL:** `http://localhost:5566/events`
  * **Name:** `create_event`
  * **Description:** `Use this when you want to create a new event.`
  * **Headers:** (Optional) Add any required HTTP headers.
  * **Body**: Hard-coded body object that will override the body generated by LLM
  *   **Body Schema:** A JSON schema of the API request body which allows LLM to know how to automatically generate the correct JSON body. For instance:

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

#### Tool 3: PUT (Update Event)

<figure><img src="../.gitbook/assets/image (3).png" alt="" width="335"><figcaption></figcaption></figure>

* **Purpose:** Update an existing event’s details.
* **Configuration Inputs:**
  * **URL:** `http://localhost:5566/events`
  * **Name:** `update_event`
  * **Description:** `Use this when you want to update an event.`
  * **Headers:** (Optional) Add any required HTTP headers.
  * **Body**: Hard-coded body object that will override the body generated by LLM
  *   **Body Schema:** A JSON schema of the API request body which allows LLM to know how to automatically generate the correct JSON body. For instance:

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

#### Tool 4: DELETE (Delete Event)

<figure><img src="../.gitbook/assets/image (4).png" alt="" width="335"><figcaption></figcaption></figure>

* **Purpose:** Remove an event from the system.
* **Configuration Inputs:**
  * **URL:** `http://localhost:5566/events`
  * **Name:** `delete_event`
  * **Description:** `Use this when you need to delete an event.`
  * **Headers:** (Optional) Add any required HTTP headers.
  *   **Query Params Schema:** A JSON schema of the API which allows LLM to know about the URL structure, which path and query parameters to generate. For instance:

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

### How the Agent Uses These Tools

* The agent can dynamically select which tool to use based on the user’s request or the flow’s logic.
* Each tool is mapped to a specific HTTP method and endpoint, with clearly defined input schemas.
* The agent leverages the LLM to interpret user input, fill in the required parameters, and make the appropriate API call.

Certainly! Here are some **Example Interactions** for your flow, including sample user queries and the expected behavior for each, mapped to the corresponding tool (GET, POST, PUT, DELETE):

### Example Interactions

#### 1. Retrieve Events (GET)

**Sample Query:**

> "Show me all upcoming events."

**Expected Behavior:**

* The agent selects the **GET** tool.
* It sends a GET request to `http://localhost:5566/events`.
* The agent returns a list of all events to the user.

***

**Sample Query:**

> "Get the details for event with ID 12345."

**Expected Behavior:**

* The agent selects the **GET** tool.
* It sends a GET request to `http://localhost:5566/events/12345`.
* The agent returns the details for the event with ID `12345`.

***

#### 2. Create a New Event (POST)

**Sample Query:**

> "Create a new event called 'AI Conference' on 2024-07-15 at Tech Hall."

**Expected Behavior:**

* The agent selects the **POST** tool.
*   It sends a POST request to `http://localhost:5566/events` with the body:

    ```json
    {
      "name": "AI Conference",
      "date": "2024-07-15",
      "location": "Tech Hall"
    }
    ```
* The agent confirms the event was created and may return the new event’s details.

***

#### 3. Update an Event (PUT)

**Sample Query:**

> "Change the location of the event 'AI Conference' on 2024-07-15 to 'Main Auditorium'."

**Expected Behavior:**

* The agent selects the **PUT** tool.
*   It sends a PUT request to `http://localhost:5566/events` with the updated event details:

    ```json
    {
      "name": "AI Conference",
      "date": "2024-07-15",
      "location": "Main Auditorium"
    }
    ```
* The agent confirms the event was updated.

***

#### 4. Delete an Event (DELETE)

**Sample Query:**

> "Delete the event with ID 12345."

**Expected Behavior:**

* The agent selects the **DELETE** tool.
* It sends a DELETE request to `http://localhost:5566/events/12345`.
* The agent confirms the event was deleted.

***





