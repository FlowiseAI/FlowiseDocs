---
description: Learn how to use Multi-Agents in Flowise
---

# Multi-Agents

This guide intends to provide an introduction of the multi-agent AI system architecture within Flowise, detailing its components, operational constraints, and workflow.

## Concept

Analogous to a team of domain experts collaborating on a complex project, a multi-agent system uses the principle of specialization within artificial intelligence.&#x20;

This multi-agent AI system utilizes a hierarchical, sequential workflow, maximizing efficiency and specialization.

### 1. System Architecture

We can define a multi-agent AI architecture as a scalable system capable of handling complex projects by breaking them down into manageable sub-tasks.

In Flowise, a multi-agent system comprises two primary nodes or agent types and a user, interacting in a hierarchical graph to process requests and deliver a targeted outcome:

1. **User:** The user acts as the **system's starting point**, providing the initial input or request. While a multi-agent system can be designed to handle a wide range of requests, it's important that these user requests align with the system's intended purpose. Any request falling outside this scope can lead to inaccurate results, unexpected loops, or even system errors. Therefore, user interactions, while flexible, should always align with the system's core functionalities for optimal performance.
2. **Supervisor AI:** The Supervisor acts as the **system's orchestrator**, overseeing the entire workflow. It analyzes user requests, decomposes them into a sequence of sub-tasks, assigns these sub-tasks to the specialized worker agents, aggregates the results, and ultimately presents the processed output back to the user.
3. **Worker AI Team:** This team consists of specialized AI agents, or Workers, each instructed - via prompt messages - to handle a specific task within the workflow. These Workers operate independently, receiving instructions and data from the Supervisor, **executing their specialized functions**, using tools as needed, and returning the results to the Supervisor.

<figure><img src="../../.gitbook/assets/mas01.png" alt=""><figcaption></figcaption></figure>

### 2. Operational Constraints

To maintain order and simplicity, this multi-agent system operates under two important constraints:

* **One task at a time:** The Supervisor is intentionally designed to focus on a single task at a time. It waits for the active Worker to complete its task and return the results before it analyzes the next step and delegates the subsequent task. This ensures each step is completed successfully before moving on, preventing overcomplexity.
* **One Supervisor per flow:** While it's theoretically possible to implement a set of nested multi-agent systems to form a more sophisticated hierarchical structure for highly complex workflows, what LangChain defines as "[Hierarchical Agent Teams](https://github.com/langchain-ai/langgraph/blob/main/examples/multi\_agent/hierarchical\_agent\_teams.ipynb)", with a top-level supervisor and mid-level supervisors managing teams of workers, Flowise's multi-agent systems currently operate with a single Supervisor.

{% hint style="info" %}
These two constraints are important when **planning your application's workflow**. If you try to design a workflow where the Supervisor needs to delegate multiple tasks simultaneously, in parallel, the system won't be able to handle it and you'll encounter an error.

**Note:** If your desired system behavior requires parallel processing, consider using the more flexible **Sequential-Agent architecture**, which supports this capability.
{% endhint %}

## The Supervisor

The Supervisor, as the agent governing the overall workflow and responsible for delegating tasks to the appropriate Worker, requires a set of components to function correctly:

* **Chat Model capable of function calling** to manage the complexities of task decomposition, delegation, and result aggregation.
* **Agent Memory (optional)**: While the Supervisor can function without Agent Memory, this module can significantly enhance workflows that require access to past Supervisor states. This **State preservation** could allow the Supervisor to resume the job from a specific point or leverage past data for improved decision-making.

<figure><img src="../../.gitbook/assets/mas07.png" alt=""><figcaption></figcaption></figure>

### Supervisor Prompt

By default, the Supervisor Prompt is worded in a way that instructs the Supervisor to analyze user requests, decompose them into a sequence of sub-tasks, and assign these sub-tasks to the specialized worker agents.

While the Supervisor Prompt is customizable to fit specific application needs, it always requires the following two key elements:

* **The {team\_members} Variable:** This variable is crucial for the Supervisor's understanding of the available workforce. It provides the Supervisor with list of Worker names and, importantly, a description of each Worker's capabilities. This allows the Supervisor to diligently delegate tasks to the most appropriate Worker based on their expertise.
* **The "FINISH" Keyword:** This keyword serves as a signal within the Supervisor Prompt. It indicates when the Supervisor should consider the task complete and present the final output to the user. Without a clear "FINISH" directive, the Supervisor might continue delegating tasks unnecessarily or fail to deliver a coherent and finalized result to the user. It signals that all necessary sub-tasks have been executed and the user's request has been fulfilled.

<figure><img src="../../.gitbook/assets/mas06.png" alt="" width="375"><figcaption></figcaption></figure>

{% hint style="info" %}
It's important to understand that the Supervisor plays a distinct role from Workers. Unlike Workers, which can be tailored with highly specific instructions, the **Supervisor operates most effectively with general directives, which allow it to plan and delegate tasks as it deems appropriate.** If you're new to multi-agent systems, we recommend sticking with the default Supervisor prompt
{% endhint %}

### 2. Understanding `Recursion Limit` in Supervisor node:

This parameter restricts the maximum depth of nested function calls within our application. In our current context, **it limits how many times the Supervisor can trigger itself within a single workflow execution**. This is important for preventing unbounded recursion and ensuring resources are used efficiently.

