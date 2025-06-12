# Interacting with API

Almost all of the internet applications relies on RESTful APIs. Enabling LLM to interact with them expand its practical utility.

This tutorial showcases how LLM can be used to make API calls through tool calling.

### Prerequisite

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

### Request Tools

There are 4 Request tools that can be used. This allows LLM to call the GET, POST, PUT, DELETE tools when necessary.



