# Generative AI vs. Agentic AI

## 1. Executive Summary

**Generative AI (GenAI)** is primarily designed to **generate content** such as text, images, code, audio, or video in response to a prompt.

**Agentic AI** goes a step further: it uses AI models to **pursue a goal, reason about what needs to be done, plan multiple steps, use tools, take actions, observe results, and adapt** with limited human intervention.

A simple way to remember the difference:

> **Generative AI creates. Agentic AI acts.**

However, this is a simplification. Agentic AI commonly uses generative AI models such as LLMs as its reasoning or decision-making component.

---

# 2. What is Generative AI?

## Definition

Generative AI is a class of AI models that learns patterns from existing data and generates new, synthetic content based on those patterns.

According to **NIST**, generative AI can generate content including:

* Text
* Images
* Video
* Audio
* Software/code
* Other digital content

Examples include:

* ChatGPT
* Claude
* Gemini
* GitHub Copilot
* Image-generation models
* AI music and video generation systems

### Simple example

You ask:

> "Write a 500-word marketing email for a new smartphone."

A generative AI system might:

1. Understand the prompt
2. Generate the email
3. Return the result

The AI is primarily **producing an output**.

It normally doesn't need to independently decide what other actions should happen afterward.

---

# 3. How Generative AI Works

A simplified GenAI workflow looks like this:

```text
User
  |
  v
Prompt
  |
  v
AI Model / LLM
  |
  v
Generated Output
  |
  v
User
```

For example:

```text
"Summarize this report"
        |
        v
      LLM
        |
        v
"Here is a 5-point summary..."
```

The human generally decides:

* What to ask
* When to ask
* What to do with the answer
* What happens next

---

# 4. Common Generative AI Use Cases

| Use Case            | Example                              |
| ------------------- | ------------------------------------ |
| Content creation    | Write blogs, emails, advertisements  |
| Summarization       | Summarize reports or meetings        |
| Translation         | Translate documents                  |
| Coding              | Generate functions or SQL queries    |
| Image generation    | Create marketing images              |
| Data analysis       | Explain patterns in a dataset        |
| Customer support    | Draft responses                      |
| Education           | Explain difficult concepts           |
| Research            | Summarize and synthesize information |
| Document processing | Extract and rewrite information      |

### Example

A customer-support employee receives:

> "My order hasn't arrived and I want a refund."

GenAI can generate:

> "I'm sorry your order has not arrived. I'll be happy to help you with the refund process..."

But a purely generative system may stop there.

A human still has to:

* Check the order
* Verify eligibility
* Process the refund
* Update the customer

---

# 5. What is Agentic AI?

## Definition

Agentic AI refers to AI systems that can pursue a goal with some degree of autonomy.

IBM describes agentic AI as systems capable of autonomously planning and performing tasks on behalf of users or other systems. These systems can break complex goals into smaller tasks and use tools to interact with external systems.

The important concepts are:

* **Goal-oriented behavior**
* **Planning**
* **Reasoning**
* **Tool use**
* **Memory/context**
* **Decision-making**
* **Action**
* **Feedback**
* **Adaptation**
* **Autonomy**

Agentic AI therefore isn't simply another type of model.

It is better understood as a **system or architecture built around AI models**.

---

# 6. Generative AI vs. Agentic AI

The easiest comparison is:

```text
GENERATIVE AI

Human
  |
  | "Write an email"
  v
AI
  |
  | Generates email
  v
Human
```

Versus:

```text
AGENTIC AI

Human
  |
  | "Resolve this customer issue"
  v
AI Agent
  |
  +--> Understand problem
  |
  +--> Check customer record
  |
  +--> Check order
  |
  +--> Determine eligibility
  |
  +--> Process refund
  |
  +--> Update CRM
  |
  +--> Notify customer
  |
  v
Goal completed
```

The major difference is therefore **action and autonomy**.

---

# 7. Key Differences

| Dimension         | Generative AI                      | Agentic AI                                  |
| ----------------- | ---------------------------------- | ------------------------------------------- |
| Primary purpose   | Generate content                   | Achieve a goal                              |
| Typical behavior  | Responds to prompts                | Plans and executes                          |
| Autonomy          | Low to moderate                    | Moderate to high                            |
| Planning          | Usually limited                    | Core capability                             |
| Tool usage        | Optional                           | Usually important                           |
| External actions  | Usually none                       | Common                                      |
| Memory            | Often conversational/contextual    | Often persistent or task-oriented           |
| Workflow          | Usually human-driven               | AI can drive workflow                       |
| Decision-making   | Generates recommendations          | Can make decisions within constraints       |
| Feedback loops    | Limited                            | Common                                      |
| Multi-step tasks  | Possible but usually user-directed | Core use case                               |
| Human involvement | Usually frequent                   | Can be reduced                              |
| Risk              | Primarily incorrect output         | Incorrect output **plus incorrect actions** |
| Example           | "Write a refund email"             | "Process eligible refunds"                  |

IBM similarly describes agentic AI as building on generative AI techniques and using LLMs to apply generated outputs toward specific goals.

---

# 8. The Relationship Between Them

Agentic AI does **not necessarily replace** Generative AI.

Instead:

```text
                    AI
                     |
          +----------+----------+
          |                     |
      Generative AI        Agentic AI
          |                     |
       Creates              Acts
          |                     |
       Content             Achieves goals
                                |
                         Often uses GenAI
```

A typical agent architecture might look like:

```text
                    USER GOAL
                       |
                       v
                +--------------+
                |   AI Agent   |
                +--------------+
                       |
              +--------+--------+
              |        |        |
              v        v        v
            LLM      Memory    Planning
              |                 |
              +--------+--------+
                       |
                       v
                     Tools
                       |
        +--------------+--------------+
        |              |              |
        v              v              v
      CRM/API       Database       Browser
        |              |              |
        +--------------+--------------+
                       |
                       v
                    Actions
                       |
                       v
                  Goal achieved
```

So an LLM may be the **brain**, while tools, memory, APIs, orchestration, permissions, and feedback mechanisms turn it into an **agentic system**.

---

# 9. Examples of Generative AI

## Example 1: Marketing

### User

> Create five advertising headlines for a new electric car.

### GenAI

Generates:

```text
1. Drive the Future.
2. Electric Power. Zero Compromise.
3. Meet Your Next Generation Drive.
4. Charge Less. Experience More.
5. The Future Is Already Here.
```

The AI has created content.

---

## Example 2: Software Development

A developer asks:

> "Write a Python function that validates an email address."

The AI generates the code.

The developer decides:

* Whether to use it
* Where to put it
* Whether to test it
* Whether to deploy it

That's primarily **Generative AI**.

---

## Example 3: Document Analysis

You upload a 100-page contract and ask:

> "Summarize the termination clauses."

The AI analyzes the document and produces a summary.

Again, primarily **Generative AI**.

---

# 10. Examples of Agentic AI

## Example 1: Travel Agent

You tell an AI:

> "Plan a 5-day trip to Japan under ₹2 lakh."

An agent could potentially:

1. Search flights
2. Compare prices
3. Search hotels
4. Check availability
5. Build an itinerary
6. Calculate the total cost
7. Adjust the plan if the budget is exceeded
8. Present options
9. With appropriate authorization, make bookings

This is agentic because the system is working toward a **goal across multiple steps**.

---

## Example 2: Software Engineering Agent

Goal:

> "Fix the failing authentication tests."

An agent could:

```text
Understand issue
      ↓
Inspect repository
      ↓
Find failing tests
      ↓
Inspect relevant code
      ↓
Develop a fix
      ↓
Run tests
      ↓
Analyze failures
      ↓
Modify implementation
      ↓
Run tests again
      ↓
Create pull request
```

This is significantly more agentic than simply:

> "Write code that fixes authentication."

---

## Example 3: Customer-Service Agent

Customer says:

> "My flight was cancelled. Please help me get another flight."

An agent could:

```text
Identify customer
      ↓
Find reservation
      ↓
Check cancellation
      ↓
Find alternative flights
      ↓
Compare options
      ↓
Check customer preferences
      ↓
Offer alternatives
      ↓
Receive approval
      ↓
Rebook flight
      ↓
Update reservation
      ↓
Send confirmation
```

This involves reasoning, tools, state, and actions.

---

# 11. Real-World Case Study: Generative AI — AlphaCode

A good example of generative AI being used for sophisticated code generation is **AlphaCode**, developed by Google DeepMind.

AlphaCode used transformer-based language models to generate computer programs for competitive programming problems.

DeepMind reported that AlphaCode reached approximately the level of the median human competitor in the evaluated Codeforces competitions, ranking around the top 54% of participants.

The important point is that the system was primarily focused on **generating solutions/code**.

### Why this is a GenAI example

```text
Problem description
       |
       v
   AI model
       |
       v
Generate programs
       |
       v
Filter/test solutions
       |
       v
Best candidate
```

The core capability is **generation of software solutions**.

### Lesson

Generative AI can do much more than write emails.

It can generate:

* Code
* Designs
* Content
* Solutions
* Summaries
* Synthetic data
* Other digital artifacts

---

# 12. Real-World Case Study: Generative AI — Klarna

Klarna deployed an AI assistant using OpenAI technology for customer service and shopping-related tasks.

According to OpenAI's published case study, within its first month the assistant handled approximately **2.3 million conversations**, representing about two-thirds of Klarna's customer-service chats. OpenAI reported that it performed work equivalent to around 700 full-time agents, reduced repeat inquiries by 25%, and reduced average resolution time from 11 minutes to less than 2 minutes.

This illustrates how GenAI can be integrated into a business workflow rather than simply being a standalone chatbot.

### Important distinction

Some modern customer-service systems like this can also have **agentic characteristics** when they independently perform actions such as refunds, returns, or account operations.

Therefore, the boundary is not always binary.

A production AI system can contain both:

```text
Generative AI
      +
Tools
      +
Business rules
      +
Workflow
      +
Autonomy
      =
Agentic system
```

---

# 13. Real-World Case Study: Agentic AI — Salesforce

Salesforce deployed Agentforce on its website to answer questions and help capture leads.

According to Salesforce, its website agent:

* Answers product and pricing questions
* Uses information from thousands of pages and product records
* Captures lead information
* Creates qualified leads in Sales Cloud
* Transfers conversations to sales representatives when necessary

Salesforce reported more than **100,000 conversations** since launch and more than **30,000 leads**, with a reported 40% reduction in time to qualify opportunities year over year.

This is a good example of agentic behavior because the system isn't merely generating answers.

It can:

```text
Question
   ↓
Understand intent
   ↓
Retrieve information
   ↓
Answer
   ↓
Determine sales intent
   ↓
Capture information
   ↓
Create lead
   ↓
Route to salesperson
```

The AI is participating in an **end-to-end workflow**.

---

# 14. Real-World Case Study: Agentic AI — Engine

Travel company Engine uses an AI agent called **Eva** for customer service.

According to Salesforce's case study, Eva manages more than 50% of customer cases end-to-end, including tasks such as rescheduling reservations and recommending accommodations based on customer preferences. Salesforce says this has reduced handling time and saved millions annually.

The important difference is that Eva is not simply writing:

> "Your reservation can be changed."

It can actually participate in the process of **changing the reservation**.

That's the transition from:

> **Answering → Doing**

---

# 15. Real-World Case Study: Agentic AI — Commerzbank

Commerzbank built an AI agent called **Ava** using Microsoft's Azure AI Foundry Agent Service.

Microsoft reports that Ava handles more than **30,000 customer conversations per month** and resolves approximately **75% of requests autonomously**, while incorporating security, compliance, persistent memory, and agentic orchestration.

This is particularly interesting because financial services require:

* Security
* Compliance
* Auditability
* Controlled access
* Human escalation
* Reliable data

It demonstrates why agentic AI requires more than simply putting an LLM behind a chatbot.

---

# 16. GenAI vs. Agentic AI: A Practical Example

Imagine an employee says:

> "Find all invoices from last month that are overdue and prepare follow-up emails."

### Generative AI approach

The AI might:

1. Analyze an uploaded invoice spreadsheet.
2. Identify overdue invoices.
3. Generate email drafts.

The human then:

```text
Review
  ↓
Copy
  ↓
Send
```

### Agentic AI approach

An agent could:

```text
Access invoice system
       ↓
Retrieve invoices
       ↓
Identify overdue invoices
       ↓
Check customer information
       ↓
Determine appropriate message
       ↓
Generate email
       ↓
Apply business rules
       ↓
Send email
       ↓
Record activity in CRM
       ↓
Report results
```

The key difference is:

> **GenAI helps you perform the task. Agentic AI can perform more of the task.**

---

# 17. When Should You Use Generative AI?

Use **Generative AI** when the main requirement is to **create, transform, analyze, or explain information**.

### Good use cases

Use GenAI for:

* Writing
* Summarization
* Translation
* Brainstorming
* Content creation
* Code generation
* Document analysis
* Research assistance
* Data interpretation
* Creating presentations
* Creating marketing material
* Drafting customer responses

### Choose GenAI when:

```text
Input → AI → Output
```

is sufficient.

For example:

> "Summarize this 50-page report."

You probably don't need an autonomous agent.

---

# 18. When Should You Use Agentic AI?

Use **Agentic AI** when the problem requires:

* Multiple steps
* Planning
* Decision-making
* Tool usage
* Interaction with external systems
* Persistent state/context
* Repeated execution
* Dynamic workflows
* Autonomous actions

A useful test is:

> **Does the AI need to do something after it gives me an answer?**

If yes, agentic architecture may be appropriate.

---

# 19. Decision Framework

Use this simple decision tree:

```text
                    Start
                      |
                      v
          Do you mainly need content?
                      |
             +--------+--------+
            YES                NO
             |                  |
             v                  v
         GENERATIVE AI    Does it need to
                          perform actions?
                               |
                     +---------+---------+
                    NO                  YES
                     |                    |
                     v                    v
                 GENERATIVE AI      Does it require
                                    multiple steps,
                                    tools or decisions?
                                         |
                               +---------+---------+
                              NO                  YES
                               |                    |
                               v                    v
                         Simple automation     AGENTIC AI
                         + GenAI
```

---

# 20. A More Practical Rule

| Your requirement                               | Recommended approach  |
| ---------------------------------------------- | --------------------- |
| "Write something"                              | GenAI                 |
| "Summarize something"                          | GenAI                 |
| "Explain something"                            | GenAI                 |
| "Analyze this document"                        | GenAI                 |
| "Generate code"                                | GenAI                 |
| "Recommend what I should do"                   | GenAI                 |
| "Search several systems and give me an answer" | GenAI + tools / Agent |
| "Complete this multi-step process"             | Agentic AI            |
| "Monitor something and respond to changes"     | Agentic AI            |
| "Make decisions and execute actions"           | Agentic AI            |
| "Operate across multiple applications"         | Agentic AI            |
| "Resolve customer issues end-to-end"           | Agentic AI            |
| "Run a workflow continuously"                  | Agentic AI            |

---

# 21. Don't Use Agentic AI Just Because You Can

Agentic AI introduces additional complexity and risk.

A simple task like:

> "Summarize this PDF."

doesn't need an autonomous agent.

Building an agent for it could introduce:

* More infrastructure
* More latency
* More cost
* More failure modes
* More security concerns
* More testing requirements

In many situations:

```text
Simple problem
     ↓
Use GenAI
```

is better than:

```text
Simple problem
     ↓
Build autonomous agent
     ↓
Add memory
     ↓
Add tools
     ↓
Add orchestration
     ↓
Add monitoring
     ↓
Add permissions
     ↓
Create unnecessary complexity
```

---

# 22. Agentic AI Requires Stronger Guardrails

This is one of the most important differences.

If a GenAI system makes a mistake:

> "The meeting is on Tuesday."

That may simply be an incorrect answer.

If an agent makes a mistake:

> "The meeting is on Tuesday."

and then **books the wrong flight for Tuesday**, the mistake becomes an action.

Therefore, agentic systems need stronger controls around:

* Authentication
* Authorization
* Tool permissions
* Data access
* Human approval
* Monitoring
* Logging
* Rate limits
* Error handling
* Rollback
* Prompt-injection protection
* Sensitive actions

Recent reporting on agentic AI has highlighted concerns around agents escaping testing environments or taking unintended actions, reinforcing the importance of permissions and monitoring.

---

# 23. Human-in-the-Loop

A useful production architecture is:

```text
                  AI Agent
                     |
                     v
               Decide action
                     |
            Is action high-risk?
               /           \
             YES            NO
              |              |
              v              v
        Human approval     Execute
              |              |
              +------->-------+
                     |
                     v
                  Result
```

For example:

### Low-risk

> Update a CRM note.

Can potentially be automated.

### Medium-risk

> Send a customer a routine email.

May require policy-based approval.

### High-risk

> Transfer ₹5,00,000.

Should generally require strong authorization and potentially human approval.

---

# 24. The Evolution

A useful way to understand the evolution is:

```text
Traditional Software
       |
       v
Rules-based Automation
       |
       v
Machine Learning
       |
       v
Generative AI
       |
       v
AI + Tools
       |
       v
AI Agents
       |
       v
Agentic AI
       |
       v
Multi-Agent Systems
```

The boundaries are not rigid, and the terminology is still evolving.

A system doesn't suddenly become "agentic" because it has an LLM.

The important question is **how much autonomy and action the system has**.

---

# 25. GenAI + Agentic AI Together

In practice, the strongest architecture may use both.

For example, an insurance claims system could look like:

```text
                  Customer
                     |
                     v
               AI Agent
                     |
          +----------+----------+
          |          |          |
          v          v          v
        LLM       Claims DB    Policy API
          |          |          |
          +----------+----------+
                     |
                     v
              Analyze claim
                     |
                     v
              Generate summary
                     |
                     v
             Apply claim rules
                     |
                     v
             Request documents
                     |
                     v
            Update claim system
                     |
                     v
            Human review if needed
```

Here:

* **Generative AI** generates explanations, summaries, and communications.
* **Agentic AI** coordinates the workflow and takes permitted actions.

---

# 26. The Most Important Concept

Think of the difference as:

### Generative AI

> **"Tell me what to do."**

### Agentic AI

> **"Achieve this goal."**

For example:

### GenAI

> "Write a customer refund response."

### Agentic AI

> "Resolve this customer's refund request."

The second instruction requires the system to determine **how** to accomplish the objective.

---

# 27. Summary

|                      | Generative AI            | Agentic AI                  |
| -------------------- | ------------------------ | --------------------------- |
| Core idea            | Generate                 | Achieve                     |
| Main output          | Content/information      | Completed task/action       |
| Human role           | Directs the AI           | Sets goal and supervises    |
| Autonomy             | Lower                    | Higher                      |
| Planning             | Limited                  | Important                   |
| Tools                | Optional                 | Usually central             |
| External systems     | Usually limited          | Common                      |
| Multi-step workflows | Limited                  | Core capability             |
| Risk                 | Incorrect content        | Incorrect decisions/actions |
| Best for             | Knowledge & content work | Operational workflows       |

### In one sentence:

> **Generative AI produces an answer; Agentic AI uses AI capabilities, tools, and workflows to pursue and complete a goal.**

---

# 28. Recommended Rule of Thumb

Use **Generative AI first**.

Move toward **Agentic AI** when you can clearly demonstrate that the problem requires:

1. Multiple steps
2. Tool/API access
3. Decisions
4. External actions
5. Dynamic workflows
6. Measurable benefits from autonomy

And keep humans in the loop for high-impact or irreversible actions.

---

# Sources

## Definitions and Technical Background

1. **NIST — Generative Artificial Intelligence Glossary**
   [NIST Generative AI definition](https://csrc.nist.gov/glossary/term/generative_artificial_intelligence?utm_source=chatgpt.com)

2. **IBM — What is Agentic AI?**
   [IBM: What is Agentic AI?](https://www.ibm.com/think/topics/agentic-ai?utm_source=chatgpt.com)

3. **IBM — Guide to Agentic AI Systems**
   [IBM: Agentic AI Systems](https://www.ibm.com/think/architectures/patterns/agentic-ai?utm_source=chatgpt.com)

## Generative AI Case Studies

4. **Google DeepMind — AlphaCode**
   [Google DeepMind: Competitive programming with AlphaCode](https://deepmind.google/blog/competitive-programming-with-alphacode/?utm_source=chatgpt.com)

5. **OpenAI — Klarna AI Assistant**
   [OpenAI: Klarna AI case study](https://openai.com/index/klarna/?utm_source=chatgpt.com)

## Agentic AI Case Studies

6. **Salesforce — Agentforce Website Case Study**
   [Salesforce: Agentforce on Salesforce.com](https://www.salesforce.com/customer-stories/agentforce-for-dot-com-implementation/?utm_source=chatgpt.com)

7. **Salesforce — Engine Agentforce Case Study**
   [Salesforce: Engine and Agentforce](https://www.salesforce.com/customer-stories/engine-agentforce-implementation/?utm_source=chatgpt.com)

8. **Microsoft — Commerzbank Agent Case Study**
   [Microsoft: Commerzbank and Azure AI Foundry Agent Service](https://www.microsoft.com/en/customers/story/25676-commerzbank-ag-azure-ai-foundry-agent-service?utm_source=chatgpt.com)

9. **Microsoft — Air India AI Case Study**
   [Microsoft: Air India and Azure AI](https://www.microsoft.com/en/customers/story/26047-air-india-azure-openai-in-foundry-models?utm_source=chatgpt.com)

---

## Final Takeaway

```text
GENAI
"What can AI create for me?"
             |
             v
       Content / Answer


AGENTIC AI
"What goal can AI accomplish for me?"
             |
             v
       Plan → Reason → Use Tools
             → Act → Observe
             → Adapt → Complete
```

**Generative AI is primarily about generating intelligence/content.
Agentic AI is about applying that intelligence to accomplish tasks.**

The two are complementary rather than competing technologies.
