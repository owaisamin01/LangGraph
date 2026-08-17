# LangChain vs LangGraph

> A practical guide to understanding LangChain, LangGraph, when to use each, how they work together, and what production systems look like.

# LangChain vs LangGraph

A practical guide to understanding LangChain, LangGraph, when to use each, how they work together, and what production systems look like.

## Short Answer

LangChain is the higher-level framework for quickly building LLM applications and agents.

LangGraph is the lower-level orchestration/runtime layer for building complex, stateful, controllable agent workflows.

They are complementary rather than competing technologies. LangChain's current agent architecture is built on top of LangGraph.

## 1. What is LangChain?

LangChain
 is an open-source framework for building applications powered by large language models.

It provides abstractions and integrations for:

Chat models
LLMs
Tools
Structured output
Agents
Retrieval/RAG
Embeddings
Vector stores
Middleware
Model providers
Tool calling
Simple mental model

Think of LangChain as a toolbox:

                    Your AI Application
                           |
              +------------+------------+
              |            |            |
            Model        Tools         RAG
              |            |            |
           GPT/Claude   APIs/DBs     Vector DB
              |
           LangChain


LangChain makes it easier to connect these components together.

## 2. What is LangGraph?

LangGraph
 is a low-level orchestration framework and runtime for building long-running, stateful agents and workflows.

Instead of thinking primarily in terms of a simple chain:

Input
  ↓
LLM
  ↓
Tool
  ↓
LLM
  ↓
Output


you can explicitly define a graph:

                    ┌──────────────┐
                    │    START     │
                    └──────┬───────┘
                           ↓
                    ┌──────────────┐
                    │   Planner    │
                    └──────┬───────┘
                           ↓
                    ┌──────────────┐
                    │  Researcher  │
                    └──────┬───────┘
                           ↓
                    ┌──────────────┐
                    │   Reviewer   │
                    └──────┬───────┘
                           ↓
                    ┌──────┴───────┐
                    │              │
                 Good?           Bad?
                    │              │
                    ↓              ↓
                  END          Research Again


This gives you explicit control over:

State
Nodes
Edges
Conditional routing
Loops
Retries
Parallel execution
Human approval
Memory
Checkpoints
Multi-agent systems
Long-running tasks

LangGraph's persistence system
 can save graph state at execution steps, enabling memory, human-in-the-loop workflows, recovery, and debugging.

## 3. The Most Important Difference

The easiest way to remember the distinction is:

| LangChain | LangGraph |
|---|---|
| Higher-level | Lower-level |
| Faster to start | More control |
| Prebuilt agent architecture | Custom agent architecture |
| Model/tool integrations | Workflow/agent orchestration |
| Good for standard agents | Good for complex agents |
| Less code | More explicit code |
| Easier learning curve | Steeper learning curve |
| Standard agent loop | Custom graph |
| Quick prototypes | Complex production workflows |

LangChain's current create_agent
 API provides a production-ready agent implementation, while LangGraph provides lower-level graph primitives.

## 4. LangChain Agent Architecture

A basic agent often looks like:

              User
                |
                ↓
          ┌───────────┐
          │    LLM    │
          └─────┬─────┘
                |
          Need a tool?
           /        \
         Yes         No
          |           |
          ↓           ↓
       Tool call    Response
          |
          ↓
          LLM
          |
          ↓
       Response


For example:

User question
     ↓
LLM
     ↓
"I need weather data"
     ↓
weather_tool()
     ↓
LLM
     ↓
Response


With LangChain, you can create this type of agent without manually implementing every state transition.

## 5. LangGraph Architecture

With LangGraph, you can explicitly define the workflow.

For example:

START
  |
  ↓
Classify Request
  |
  +------ Question ------→ RAG Search
  |
  +------ Coding --------→ Code Agent
  |
  +------ Billing --------→ Billing Agent
                              |
                              ↓
                         Human Approval
                              |
                         +----+----+
                         |         |
                       Approve    Reject
                         |         |
                         ↓         ↓
                       Execute    END


This becomes extremely useful when your application has business logic around the AI.

## 6. LangChain and LangGraph Are Not Alternatives

You don't necessarily have to choose:

LangChain OR LangGraph


You can use:

             Your Application
                    |
              LangGraph
                    |
        +-----------+-----------+
        |                       |
   LangChain Agent          Custom Nodes
        |                       |
   Model + Tools          Business Logic
        |
      LLM


LangChain agents are built on LangGraph. You can therefore start with a high-level LangChain agent and move to custom LangGraph orchestration when you need more control.

## 7. When Should You Use LangChain?

Use LangChain first when your application is relatively straightforward.

### Good LangChain use cases
## 1. Simple chatbot
User
 ↓
LLM
 ↓
Answer


You probably don't need LangGraph.

## 2. RAG application
Question
   ↓
Retriever
   ↓
Documents
   ↓
LLM
   ↓
Answer


LangChain provides integrations for retrieval components, models, embeddings, and vector stores.

## 3. Simple tool-using agent
User
 ↓
Agent
 ↓
Calculator
 ↓
Agent
 ↓
Answer


If the default agent loop is sufficient, LangChain is usually the simpler choice.

## 4. Prototype

For example:

Can an AI agent search our database and answer employee questions?

Start with LangChain.

Don't immediately build a large LangGraph architecture if you don't know whether the product works.

## 8. When Should You Use LangGraph?

Use LangGraph when your application needs explicit orchestration and state management.

Strong signals that you need LangGraph:

Complex branching
Loops
Multiple agents
Human approval
Long-running processes
Persistent state
Checkpointing
Retry/resume behavior
Complex business rules
Deterministic + AI steps
Multiple specialized agents
Fine-grained execution control

The official LangGraph documentation is particularly relevant when you need customized workflows, long-running processes, persistence, or human oversight.

## 9. Example: Customer Support Agent

A simple version:

Customer
   ↓
LLM
   ↓
Answer


LangChain may be enough.

But imagine the actual workflow:

Customer
   ↓
Classify Issue
   |
   +----------+-----------+
   |          |           |
 Billing    Technical   Refund
   |          |           |
   ↓          ↓           ↓
Billing     Tech        Refund
Agent       Agent       Agent
   |          |           |
   +----------+-----------+
              |
              ↓
        Risk Assessment
              |
         High risk?
          /      \
        Yes       No
         |         |
         ↓         ↓
Human Review    Execute
         |         |
         +----+----+
              |
              ↓
           Response


This is an excellent LangGraph problem because the application has routing, specialized agents, business rules, and human approval.

## 10. Example: Research Agent

Imagine you want to build a research assistant.

It needs to:

Understand the question
Search the web
Search internal documents
Compare sources
Identify missing information
Search again
Generate an answer
Review the answer
Retry if quality is poor

Graph:

                 START
                   |
                   ↓
             Understand Query
                   |
                   ↓
             Search Sources
                   |
                   ↓
            Collect Evidence
                   |
                   ↓
             Generate Draft
                   |
                   ↓
              Evaluate
              /       \
          Good         Bad
           |            |
           ↓            ↓
          END       Search Again


LangGraph is a natural fit because the workflow contains loops and conditional routing.

## 11. Example: Software Engineering Agent

Consider an AI coding agent:

User Request
     |
     ↓
   Planner
     |
     ↓
 Code Generator
     |
     ↓
 Run Tests
     |
     ↓
 Tests Pass?
    /      \
  Yes       No
   |         |
   ↓         ↓
 Review    Debug
   |         |
   |         └──────→ Run Tests
   ↓
 Human Approval
   |
   ↓
 Merge


This is much more complicated than:

prompt → LLM → response


LangGraph lets you represent the lifecycle explicitly.

## 12. LangGraph's Biggest Feature: State

One of the most important concepts in LangGraph is state.

For example:

state = {
    "user_request": "...",
    "research_results": [],
    "draft": "...",
    "review_score": 0,
    "retry_count": 2,
    "approved": False
}


Different nodes can read and modify this state:

             State
               |
      +--------+--------+
      |        |        |
   Planner  Researcher Reviewer
      |        |        |
      +--------+--------+
               |
             State


The graph can then decide what to do next based on the current state.

## 13. Persistence

Suppose an agent is performing a long-running task and the process gets interrupted.

A stateless workflow might have to start again.

With LangGraph persistence:

Step 1 ✓
Step 2 ✓
Step 3 ✓
Step 4 ✓
     ↓
  CRASH
     ↓
  Resume
     ↓
Step 5
Step 6
...


LangGraph checkpoints allow execution state to be persisted and later resumed.

This is particularly valuable for production agents.

See the official LangGraph Persistence documentation
.

## 14. Human-in-the-Loop

Consider a financial agent.

The AI can prepare a transaction but should not execute it automatically above a certain threshold.

User
 ↓
Agent
 ↓
Prepare Transaction
 ↓
Risk Check
 ↓
Amount > $10,000?
    /       \
  Yes        No
   |          |
   ↓          ↓
Human       Execute
Approval
   |
 +---+---+
 |       |
Yes      No
 |       |
 ↓       ↓
Execute  Reject


LangGraph can pause execution, allow a human to inspect or modify state, and then resume the workflow.

## 15. Multi-Agent Systems

LangChain can create agents.

LangGraph can orchestrate them.

For example:

                    Supervisor
                        |
          +-------------+-------------+
          |             |             |
          ↓             ↓             ↓
       Research       Coding        Finance
        Agent         Agent         Agent
          |             |             |
          +-------------+-------------+
                        |
                        ↓
                    Reviewer
                        |
                        ↓
                      User


You can also create hierarchical systems:

                Main Agent
                    |
          ┌─────────┴─────────┐
          ↓                   ↓
    Research Manager     Engineering Manager
       /      \             /       \
      ↓        ↓           ↓         ↓
   Search   RAG Agent   Coder     Tester


LangGraph's graph model is particularly useful for these architectures.

## 16. Workflow vs Agent

This distinction is important.

Workflow

A workflow has mostly predetermined execution paths:

Input
 ↓
Step A
 ↓
Step B
 ↓
Step C
 ↓
Output


Example:

Invoice
 ↓
Extract data
 ↓
Validate
 ↓
Calculate tax
 ↓
Generate report

Agent

An agent dynamically decides what to do:

             ┌───────────┐
             │    LLM    │
             └─────┬─────┘
                   ↓
             Decide Action
             /     |      \
           Tool A Tool B  Tool C
             \      |      /
              \     |     /
                 LLM
                  |
              Decide Again


LangGraph supports both workflows and agents. The official Workflows and Agents documentation
 explains this distinction in detail.

## 17. A Useful Decision Tree
                    Start
                      |
                      ↓
             Is it just an LLM?
                /          \
              Yes           No
               |             |
          Use model      Need tools?
                           /      \
                         No        Yes
                         |          |
                       Simple    Need custom
                       LangChain  control?
                                  /     \
                                No       Yes
                                |         |
                           LangChain   LangGraph

Choose LangChain if:
Simple application
RAG
Chatbot
Standard tool-calling agent
Prototype
Standard agent loop
Minimum code is preferred
Choose LangGraph if:
Multiple agents
Complex workflow
Branching
Loops
Human approval
Long-running tasks
Persistent state
Checkpoints
Retry/resume
Fine-grained control
Deterministic + agentic workflows
## 18. Can You Start With LangChain and Move to LangGraph?

Yes. This is often a good approach.

Start:

LangChain
   ↓
create_agent()
   ↓
Prototype


Then complexity grows:

LangChain Agent
       ↓
Need custom routing?
       ↓
Need state?
       ↓
Need human approval?
       ↓
Need multiple agents?
       ↓
Need persistence?
       ↓
Move orchestration into LangGraph


You don't have to throw away the LangChain components.

For example:

                 LangGraph
                     |
        +------------+------------+
        |                         |
 LangChain Agent            Custom Node
        |                         |
      Tools                  Business Logic
        |
      Model

## 19. Production Case Studies
### Klarna

Klarna built its AI Assistant using LangGraph and LangSmith.

The assistant handles tasks including:

Customer payments
Refunds
Payment escalations
Customer support

LangChain's published case study reports significant reductions in resolution time and substantial automation of repetitive support tasks.

The architecture conceptually resembles:

Customer
 ↓
Intent Detection
 ↓
Route
 ├── Payment
 ├── Refund
 ├── Account
 └── Escalation
       ↓
   Specialized Logic
       ↓
   Risk / Control
       ↓
    Response


This is an orchestrated business process rather than simply a chatbot.

*Source: Klarna + LangChain case study*

### Uber

Uber's Developer Platform team has used LangGraph for large-scale code migration and unit-test generation.

Conceptually:

Codebase
   ↓
Migration Planner
   ↓
Code Analysis
   ↓
Code Transformation
   ↓
Generate Tests
   ↓
Run Tests
   ↓
Passed?
  /   \
Yes    No
 |      |
 ↓      ↓
Review  Fix
          |
          └──────→ Run Tests


The important feature is not simply the LLM; it is the orchestration surrounding it.

*Source: LangGraph production case studies*

### LinkedIn

LinkedIn has used LangGraph for AI systems including an AI recruiter and SQL Bot.

A SQL-oriented workflow can look like:

Employee Question
       ↓
   SQL Planner
       ↓
 Schema Search
       ↓
 SQL Generator
       ↓
 SQL Validator
       ↓
 Execute Query
       ↓
 Error?
    /    \
  Yes     No
   |       |
 Fix      Format
   |       |
   └──→────┘
       ↓
    Response


*Source: LangChain production agents*

### Replit

Replit's coding agent is another example of a complex agent architecture.

The system involves concepts such as:

Multi-agent architecture
Human-in-the-loop
Package installation
File creation
Software development tasks
Iterative execution

Conceptually:

User
 ↓
Planning
 ↓
Code
 ↓
Install dependency
 ↓
Modify files
 ↓
Run application
 ↓
Observe result
 ↓
Fix
 ↓
Repeat
 ↓
Human/User


*Source: LangGraph production examples*

### Elastic

Elastic initially used LangChain for its AI assistant and later moved toward LangGraph as the system became more complex.

This demonstrates an important engineering lesson:

Start simple, but don't force a simple abstraction onto a complex workflow.

*Source: LangGraph production agents*

### AppFolio

AppFolio built a domain-specific copilot using LangGraph.

This is an example of a vertical/domain-specific agent rather than a general-purpose chatbot.

*Source: LangGraph production examples*

## 20. What Does the Technology Stack Look Like?

A modern architecture might look like:

                    User
                      |
                      ↓
                Application
                      |
                      ↓
                 LangGraph
               Orchestration
                      |
        +-------------+-------------+
        |             |             |
        ↓             ↓             ↓
    LangChain       Tools         Memory
        |
   +----+----+
   |         |
 Model     Retrieval
   |         |
GPT/Claude  Vector DB


And around the system:

                ┌─────────────────┐
                │    LangSmith    │
                │ Tracing / Evals │
                └────────┬────────┘
                         |
                         ↓
Application → LangGraph → LangChain → Models/Tools


LangGraph can be used without LangChain, but LangChain components are commonly used with it for models and tools.

## 21. LangChain vs LangGraph vs LangSmith

These three are often confused.

| Technology | Main purpose |
| LangChain | Build LLM applications and agents |
| LangGraph | Orchestrate complex/stateful agents |
| LangSmith | Trace, evaluate, debug, and monitor AI applications |

Think:

LangChain
    ↓
BUILD

LangGraph
    ↓
ORCHESTRATE

LangSmith
    ↓
OBSERVE + EVALUATE

## 22. What Should a Beginner Learn First?

Recommended progression:

## 1. LLM fundamentals
       ↓
## 2. Prompting
       ↓
## 3. Tool calling
       ↓
## 4. Structured output
       ↓
## 5. RAG
       ↓
## 6. LangChain
       ↓
## 7. Agents
       ↓
## 8. LangGraph
       ↓
## 9. Memory / persistence
       ↓
## 10. Human-in-the-loop
       ↓
## 11. Multi-agent systems
       ↓
## 12. Evaluation + observability


Don't start with multi-agent LangGraph systems.

First understand:

LLM
 ↓
Tool
 ↓
Agent
 ↓
Workflow
 ↓
State
 ↓
Graph

## 23. Recommended Learning Path
Level 1 — Understand LLM Applications

Learn:

Chat models
Prompts
Structured output
Tool calling
Embeddings
RAG

Start with the official LangChain documentation
.

Level 2 — Build a Simple Agent

Learn:

LLM
 +
Tools
 +
Agent loop


Then work through the LangChain Agents documentation
.

Level 3 — Learn LangGraph

Study:

State
Nodes
Edges
Conditional edges
Loops
Persistence
Interrupts
Memory

Start with the official LangGraph overview
.

Level 4 — Learn Workflows and Agents

Read the LangGraph Workflows and Agents guide
.

It explains the distinction between predetermined workflows and dynamically acting agents.

Level 5 — Learn Persistence

Study LangGraph Persistence
.

Focus on:

Checkpoints
Memory
Human-in-the-loop
Fault tolerance
Time-travel debugging
Level 6 — Study Production Systems

Read the LangGraph case studies
.

Also explore LangChain customer stories
.

## 24. Best Official Learning Resources
LangChain
LangChain Overview
LangChain Agents
LangGraph
LangGraph Overview
Workflows and Agents
Persistence
LangGraph Case Studies
LangGraph Reference
Production / Architecture
Building LangGraph: Designing an Agent Runtime from First Principles
Is LangGraph Used in Production?
Top LangGraph Agents in Production
LangChain/LangGraph 1.0
Video

Building Effective Agents with LangGraph
 is a useful walkthrough of workflows, routing, orchestrator-worker architectures, evaluator-optimizer patterns, and agent loops.

## 25. A Simple Project Progression

If you're learning this for practical development, build these projects in order.

Project 1 — Simple Chatbot
User → LLM → Response


Use: LangChain

Project 2 — RAG Chatbot
Question
 ↓
Retriever
 ↓
Documents
 ↓
LLM
 ↓
Answer


Use: LangChain

Project 3 — Tool-Calling Agent
User
 ↓
Agent
 ↓
Calculator / Search / Database
 ↓
Answer


Use: LangChain

Project 4 — Research Agent
Question
 ↓
Planner
 ↓
Search
 ↓
Analyze
 ↓
Review
 ↓
Search again if necessary
 ↓
Answer


Use: LangGraph

Project 5 — Customer Support Agent
Customer
 ↓
Router
 ├── Billing Agent
 ├── Technical Agent
 └── Refund Agent
        ↓
   Risk Assessment
        ↓
 Human Approval?
        ↓
   Execute/Respond


Use: LangGraph + LangChain

Project 6 — Coding Agent
Requirement
 ↓
Planner
 ↓
Coder
 ↓
Tester
 ↓
Evaluator
 ↓
 ┌─────────────┐
 │ Tests pass? │
 └──────┬──────┘
       / \
     Yes  No
      |    |
    Review Fix
           |
           └──→ Tester


Use: LangGraph + LangChain + evaluation/observability tooling

## 26. The Most Important Mental Model

Don't think:

# LangChain vs LangGraph


Think:

                AI Application
                      |
        +-------------+-------------+
        |                           |
   LangChain                    LangGraph
   "Building blocks"            "Orchestration"
        |                           |
   Models/Tools/RAG             State/Flow
        |                           |
        +-------------+-------------+
                      |
                   LLM Apps


And at the production layer:

                 LangSmith
              /     |      \
          Tracing  Evals  Monitoring
                    |
                    ↓
              AI Application

## 27. Final Recommendation

If you're new to LangChain/LangGraph, don't begin by building a complicated graph.

Start with:

## 1. LangChain
      ↓
## 2. Simple agent
      ↓
## 3. Tools
      ↓
## 4. RAG
      ↓
## 5. Understand agent loops
      ↓
## 6. LangGraph
      ↓
## 7. State
      ↓
## 8. Conditional routing
      ↓
## 9. Persistence
      ↓
## 10. Human-in-the-loop
      ↓
## 11. Multi-agent systems

Rule of thumb

If the LLM is the main thing you're building around, start with LangChain.

If orchestration is the main thing you're building, use LangGraph.

If you need both, use LangChain components inside a LangGraph application.

## 28. One-Line Summary
LangChain = "Give me the building blocks to build an AI application/agent."

LangGraph = "Give me control over how my AI agents and workflows execute."

LangSmith = "Show me what my AI system did and help me evaluate/debug it."

## 29. Production-Ready Learning Path

Focus less on memorizing LangChain syntax and more on these concepts:

LLMs
  ↓
Tool Calling
  ↓
RAG
  ↓
Agents
  ↓
Workflows
  ↓
State
  ↓
Graph Orchestration
  ↓
Persistence
  ↓
Human-in-the-loop
  ↓
Evaluation
  ↓
Observability
  ↓
Production Reliability


That progression will give you a much stronger understanding than learning LangChain and LangGraph APIs in isolation.

### Sources
LangChain Documentation
LangChain Agents
LangGraph Documentation
LangGraph Workflows and Agents
LangGraph Persistence
LangGraph Case Studies
LangGraph Reference
Building LangGraph
LangGraph in Production
Top LangGraph Agents in Production
Klarna Case Study
LangChain/LangGraph 1.0
LangChain Customer Stories
Building Effective Agents with LangGraph