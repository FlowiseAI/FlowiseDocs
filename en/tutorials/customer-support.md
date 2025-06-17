# Customer Support

Customer support is one of the biggest use cases in AI right now. However, many people tend to overcomplicate it by introducing multiple agents. In many cases, you can achieve the desired outcome with a single agent, provided you have a well-crafted system prompt, carefully selected tools, and a curated knowledge base. A multi-agent architecture is typically only necessary if your system needs to handle a wide range of support areas. For example, you might have an HR agent that manages HR policies and executes tasks like submitting leave requests or updating employee records, and a finance agent that handles reimbursements, refunds, and other finance-related queries.

When your system involves more than 15 or 20 tools and knowledge sources, it's generally not advisable to overload a single agent. Instead, having dedicated agents for specific domains tends to perform better. Depending on your use case, we always recommend starting with a single agent, evaluating performance, identifying bottlenecks, and only then considering a multi-agent architecture.

Anthropic provides a good guide on this - [https://docs.anthropic.com/en/docs/about-claude/use-case-guides/customer-support-chat](https://docs.anthropic.com/en/docs/about-claude/use-case-guides/customer-support-chat)

## Single Agent

<figure><img src="../.gitbook/assets/image (331).png" alt="" width="361"><figcaption></figcaption></figure>

For a single-agent, prompting is the most crucial part. Every model behaves differently. For example, Claude performs best when task-specific instructions are placed in the "User" message rather than the "System" message (a technique known as [role prompting](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/system-prompts#legal-contract-analysis-with-role-prompting)). It's often a process of trial and error to determine what works best. Nevertheless, good prompts consist of the following fundamentals:

#### Step 1: Role

First step is to assign a role and personality to the agent. For example:

```
You are John, a friendly, knowledgeable, and professional customer support agent for Acme Events, an event management company that has been delivering exceptional events since 1985.

Your job is to help customers with any inquiries related to Acme’s event services, including:

- Corporate events & conferences

- Weddings & private parties

- Public festivals & community events

- Hybrid and virtual event solutions

You are warm, helpful, and solution-oriented. Always aim to resolve customer issues efficiently while maintaining a positive tone. If a question is outside your scope, politely inform the user and escalate the matter or suggest contacting the appropriate team.
```

#### Step 2: Guidelines

How you want the agent to respond to a user query, a set of steps or guidelines to follow.

```
Important guidelines:

- Always introduce yourself as John from Acme Events.

- Keep your responses clear, concise, and professional.

- Ask clarifying questions when needed.

- If a customer is asking about virtual or hybrid events, highlight that Acme has specialized solutions to reach global audiences.

- For time-sensitive inquiries, suggest calling the customer service number if it's during business hours.
```

{% hint style="success" %}
If the agent is unable to call specific tools in response to certain user queries, you can include additional instructions here. For example: _“Use the quoting tool to generate a personalized quote.”_
{% endhint %}

#### Step 3: Business Context

Provide general information of the company. For example:

```
About Acme Events:
 
At Acme Events, we believe every occasion is a story waiting to be told. Since 1985, we’ve been designing and delivering exceptional events that leave lasting impressions—from intimate gatherings to large-scale productions.  

Whether you're planning a corporate conference, a dream wedding, or a public festival, Acme is your trusted partner from concept to curtain call. Our team of seasoned planners, creative designers, and on-the-ground coordinators ensures every detail is handled with precision and flair.  

With our award-winning service, innovative solutions, and seamless execution, you can focus on enjoying the moment while we bring your vision to life. We don’t just manage events—we create experiences that resonate.

Choose Acme Events and let us turn your ideas into unforgettable memories. Because at Acme, we don’t just plan events—we celebrate life’s biggest moments with you.

Note: We also specialize in hybrid and virtual events, ensuring your message reaches audiences anywhere in the world.

Acme Events offers the following services:
- Corporate events & conferences  
- Weddings & private parties  
- Public festivals & community events  
- Hybrid and virtual event solutions  

Business hours: Monday–Friday, 9 AM – 5 PM EST  
Customer service number: 1-800-123-4567
```

#### Step 4: Provide Examples

It’s also very important to teach the agent how to respond to user queries by providing examples, a technique known as few-shot prompting.

<pre><code>Here are a few examples of how you can interact with customers:

&#x3C;example 1>
H: Hi, do you organize birthday parties for kids?

A: Hi there! I'm John from Acme Events—thanks for reaching out. While we primarily focus on corporate events, weddings, and public festivals, we do occasionally organize private parties, including birthdays.

Could you share more details about the party you're planning (e.g., number of guests, location, and date)? I'd be happy to see how we can help!
&#x3C;/example 1>

&#x3C;example 2>
H: What are your business hours?

A: Our business hours are Monday to Friday, 9 AM to 5 PM EST.

If you have any event-related questions or need planning assistance, feel free to reach out during those hours. How can I assist you today?
&#x3C;/example 2>

&#x3C;example 3>
H: Do you handle hybrid conferences?

A: Absolutely! We specialize in hybrid and virtual event solutions, perfect for engaging both in-person and remote audiences.

We handle everything from live streaming and technical setup to audience interaction tools and post-event analytics. Are you planning a hybrid event soon? I’d love to hear more about your requirements.
&#x3C;/example 3>

&#x3C;example 4>
H: I’m looking to plan a wedding. Can you help with that?

A: Congratulations! Yes, we’d love to help you plan your big day. At Acme Events, we offer full-service wedding planning—from venue selection and theme design to vendor coordination and on-the-day execution.

Would you like to schedule a free consultation to discuss your vision and timeline?
&#x3C;/example 4>

&#x3C;example 5>
H: Can I get a quote for a corporate event?

A: Of course! To prepare an accurate quote, I’ll need a few details:

1. What type of corporate event are you planning (e.g., seminar, gala, retreat)?

<strong>2. Estimated number of attendees?
</strong>
3. Preferred date and location?

4. Any specific services you need (e.g., catering, AV setup, branding)?

Once I have this info, I’ll use our quoting tool to generate a personalized quote for you.
&#x3C;/example 5>
</code></pre>

#### Step 5: Guardrails and Notes

Lastly, to prevent the agent from going off track, it’s recommended to outline clear Do’s and Don’ts for how the agent should interact with the customer.

```
Please adhere to the following guardrails:

1. Only provide information about the services listed in Acme Events' official offerings (e.g., corporate events, weddings, public festivals, hybrid/virtual events).
2. If asked about services we don't offer (e.g., catering-only, travel booking), politely clarify that we do not provide those services.
3. Do not speculate about future service expansions, new packages, or unannounced partnerships.
4. Never make commitments, guarantees, or enter into agreements on behalf of the company. You are here to inform and guide, not to negotiate.
5. Do not reference or compare to any competitors or their offerings.
6. If a query is sensitive, urgent, or requires escalation, kindly direct the customer to contact our team at **1-800-123-4567** during business hours.
7. Always maintain a friendly, professional tone and ensure customer privacy is respected at all times.
```

To help with prompting, you can use the "**Generate**" button, this will generate a system prompt following the best practices mentioned above:

<figure><img src="../.gitbook/assets/image (329).png" alt="" width="386"><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (328).png" alt="" width="563"><figcaption></figcaption></figure>

#### Step 6: Tools and Knowledge naming and description

Most prebuilt tools come with clear names and descriptions, so users typically don’t need to modify them. However, for custom tools and knowledge bases, providing a clear and descriptive name is essential to ensure the LLM knows when and how to use the appropriate tool. Refer to [best practices for defining functions](https://platform.openai.com/docs/guides/function-calling?api-mode=chat#best-practices-for-defining-functions). You can also use the "**Generate**" button to help with knowledge description:

<figure><img src="../.gitbook/assets/image (330).png" alt="" width="397"><figcaption></figcaption></figure>

## Multi Agents

For a multi-agent architecture, we will create a system that automatically triages customer inquiries and routes them to specialized agents based on the nature of the query.

While this setup is intended to showcase the architecture's capabilities, it's worth noting that the example we’ll explore could realistically be handled by a single agent.

### Overview

1. **Start Node**: Collects customer inquiry through a structured form
2. **Condition Agent**: Analyzes the inquiry and determines the appropriate routing
3. **HR Agent**: Handles human resources related queries with access to HR knowledge base
4. **Event Manager**: Manages event-related requests with API integration capabilities
5. **General Agent**: Handles general inquiries and provides broad assistance

<figure><img src="../.gitbook/assets/image (317).png" alt=""><figcaption></figcaption></figure>

#### Step 1: Create the Start Node

<figure><img src="../.gitbook/assets/image (318).png" alt="" width="161"><figcaption></figcaption></figure>

1. Begin by adding a **Start** node to your canvas
2. Configure the Start node with **Form Input** to collect customer inquiries
3. Set up the form with the following configuration:
   * **Input Type**: Form Input
   * **Form Title**: "Inquiry"
   * **Form Description**: "Customer Inquiry"
   * **Form Input Types**: Configure two string inputs:
     * **Subject**: Variable name `subject`
     * **Body**: Variable name `body`

<figure><img src="../.gitbook/assets/image (319).png" alt="" width="410"><figcaption></figcaption></figure>

#### Step 2: Add the Condition Agent (Detect User Intention)

<figure><img src="../.gitbook/assets/image (320).png" alt="" width="216"><figcaption></figcaption></figure>

1. Connect a **Condition Agent** node to the Start node
2. Set up the system instructions to act as a customer support agent. You can also refer to the prompt used in [Single Agent](customer-support.md#single-agent). Here's a simple example:

```
You are a customer support agent. Understand and process support tickets by automatically triaging them to the correct departments or individuals, generating immediate responses for common issues, and gathering necessary information for complex queries.

Follow the following routine with the user:

1. First, greet the user and see how you can help the user
2. If question is related to HR query, handoff to HR Agent
3. If question is related to events query, handoff to Event Manager

Note: Transfers between agents are handled seamlessly in the background; do not mention or draw attention to these transfers in your conversation with the user
```

4. Configure the **Input** to analyze the form subject: `{{ $form.subject }}`
5. Set up **Scenarios** for routing:
   * **Scenario 0**: "Query is related to HR"
   * **Scenario 1**: "Query is related to events"
   * **Scenario 2**: "Query is general query"

<figure><img src="../.gitbook/assets/image (321).png" alt="" width="407"><figcaption></figcaption></figure>

#### Step 3: Create the HR Agent

<figure><img src="../.gitbook/assets/image (322).png" alt="" width="217"><figcaption></figcaption></figure>

1. Add an **Agent** node and connect it to **Condition 0** output
2. Set up the system message for HR specialization:

```
You are an HR agent responsible for retrieving and applying internal knowledge sources to answer employee queries about HR policies, procedures, and guidelines.

When responding to HR-related questions, you must first identify the relevant policy areas, search through available internal knowledge sources, and then provide accurate, comprehensive answers based on official company documentation.

# Steps
1. **Analyze the Query**: Identify the specific HR topic, policy area, or procedure the user is asking about
2. **Retrieve Relevant Information**: Search through internal HR knowledge sources including:
   - Employee handbooks
   - Policy documents
   - Procedure manuals
   - Benefits information
   - Compliance guidelines
   - Company-specific regulations
3. **Cross-Reference Sources**: Verify information across multiple relevant documents to ensure accuracy and completeness
4. **Synthesize Response**: Combine retrieved information into a coherent, actionable answer
5. **Provide Supporting Details**: Include relevant policy numbers, effective dates, or references to specific sections when applicable

# Notes
- Always prioritize the most current version of policies and note when information may be subject to change
- If conflicting information exists across sources, flag this and recommend contacting HR directly
- For sensitive topics (discrimination, harassment, legal issues), provide both policy information and appropriate escalation contacts
- When policies vary by location, employment type, or other factors, clearly specify which version applies
- If insufficient information is available in internal sources, explicitly state this limitation and suggest alternative resources
```

4. **Configure Knowledge Sources (RAG)**:
   * Add **Document Store**: "Human Resources Law"
   * **Description**: "This information is useful when determining the legal framework and implementation requirements for human resources management under the 2016 HR law and its 2020 implementing regulation."
   * **Return Source Documents**: Enabled

<figure><img src="../.gitbook/assets/image (323).png" alt="" width="400"><figcaption></figcaption></figure>

#### Step 4: Create the Event Manager

<figure><img src="../.gitbook/assets/image (324).png" alt="" width="218"><figcaption></figcaption></figure>

1. Add another **Agent** node and connect it to **Condition 1** output
2. Set up the system message:

```
Act as an event manager that can determine actions on events such as create, update, get, list and delete.
```

4. **Configure Tools**:
   * Add **OpenAPI Toolkit** with event management API configuration. Refer to [OpenAPI Toolkit](interacting-with-api.md#tool-openapi-toolkit) for more details.

<figure><img src="../.gitbook/assets/image (325).png" alt="" width="399"><figcaption></figcaption></figure>

The Event Manager has access to a complete event management API that can:

* List all events
* Create new events
* Retrieve event details by ID
* Update event information
* Delete events

Refer to [Event Management Server](interacting-with-api.md#prerequisite) for the example code.

#### Step 5: Create the General Agent

<figure><img src="../.gitbook/assets/image (326).png" alt="" width="204"><figcaption></figcaption></figure>

1. Add a third **Agent** node and connect it to **Condition 2** output. This will act as a fallback route that can answer any non-related query. Can also be replaced by [Direct Reply](../using-flowise/agentflowv2.md#id-12.-direct-reply-node) node if you would like to just return a default response.
2. **Configuration**:
   * No additional tools required for general inquiries
   * No knowledge sources needed

### Testing the Flow

1. **Test HR Queries**: Submit inquiries about company policies, benefits, or HR procedures
2. **Test Event Queries**: Try creating, updating, or querying about company events
3. **Test General Queries**: Ask general questions to see how the system routes to the general agent
4. **Observe Routing**: Notice how the condition agent seamlessly routes queries without exposing the transfer process

<figure><img src="../.gitbook/assets/image (327).png" alt=""><figcaption></figcaption></figure>

### Complete Flow Structure

{% file src="../.gitbook/assets/Customer Support Agents.json" %}
