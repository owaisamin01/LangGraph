# Building Effective Agents — Anthropic

> **Reference article:** https://www.anthropic.com/engineering/building-effective-agents  
> **Published:** December 19, 2024  
> **Main idea:** Don't make an AI system unnecessarily complicated. Start simple, measure its performance, and introduce workflows or autonomous agents only when they provide a real benefit.

---

# 1. What is this article about?

Anthropic's article explains **how to build effective AI agents and agentic systems**.

The most important lesson is:

> **A good AI system is not necessarily the most complicated AI system.**

You might be tempted to build:

```text
User
  ↓
Agent 1
  ↓
Agent 2
  ↓
Agent 3
  ↓
Agent 4
  ↓
Agent 5
```

But this can make the system:

- expensive
- slow
- difficult to debug
- difficult to test
- difficult to maintain
- more likely to make mistakes

Anthropic recommends starting with the **simplest architecture that can solve the problem**, then increasing complexity only when necessary.

---

# 2. Workflow vs Agent

Anthropic makes an important distinction between **workflows** and **agents**.

## Workflow

A workflow has a predefined path.

```text
Step 1 → Step 2 → Step 3 → Step 4
```

The developer decides what happens at each stage.

Example:

```text
PDF
 ↓
OCR
 ↓
Extract information
 ↓
Validate information
 ↓
Generate report
```

The system already knows what should happen next.

---

## Agent

An agent has more freedom.

Instead of telling the LLM exactly what to do, you give it:

```text
Goal
+
Tools
+
Environment
```

The LLM decides:

```text
What should I do next?
↓
Which tool should I use?
↓
Did the tool give me the expected result?
↓
What should I do now?
```

Example:

```text
User:
"Find why this application is failing and fix it."

Agent:

Inspect project
    ↓
Run tests
    ↓
Read error
    ↓
Inspect relevant file
    ↓
Modify code
    ↓
Run tests again
    ↓
Tests fail
    ↓
Analyze failure
    ↓
Modify code again
    ↓
Run tests
    ↓
Tests pass
    ↓
Finish
```

The exact number of steps was not necessarily known beforehand.

---

# 3. Workflow vs Agent — Simple Comparison

| Feature | Workflow | Agent |
|---|---|---|
| Path | Predetermined | Dynamic |
| Decision maker | Developer/code | LLM |
| Number of steps | Usually known | May be unknown |
| Control | High | Lower |
| Predictability | High | Lower |
| Flexibility | Lower | High |
| Cost | Usually lower | Usually higher |
| Best for | Structured problems | Open-ended problems |

### Simple analogy

A workflow is like:

> **Following a recipe.**

An agent is like:

> **A chef deciding how to prepare the meal based on the ingredients and problems encountered.**

---

# 4. Start Simple

This is probably the most important lesson.

Use:

```text
Simple solution
      ↓
Evaluate
      ↓
Is it good enough?
   ↙       ↘
 Yes        No
 ↓           ↓
Stop       Add complexity
```

Don't immediately build a multi-agent system.

Start with:

```text
Prompt
  ↓
LLM
  ↓
Answer
```

Then perhaps:

```text
Prompt
  ↓
LLM + Retrieval
  ↓
Answer
```

Then:

```text
Prompt
  ↓
LLM + Tools
  ↓
Answer
```

Only after that should you consider:

```text
Workflow
```

or:

```text
Autonomous Agent
```

---

# 5. Why Not Always Use Agents?

Agents are powerful, but they have costs.

## Cost

An agent may make many LLM calls:

```text
LLM call 1
LLM call 2
LLM call 3
LLM call 4
LLM call 5
...
```

Instead of:

```text
LLM call 1
```

Therefore, an agent can be much more expensive.

## Latency

Suppose one LLM call takes 2 seconds.

A simple system:

```text
2 seconds
```

An agent that performs many sequential calls can take substantially longer.

## Error accumulation

Imagine:

```text
Step 1 → 90% correct
Step 2 → 90% correct
Step 3 → 90% correct
Step 4 → 90% correct
```

Errors can accumulate across steps.

Therefore:

```text
More steps ≠ automatically better
```

---

# 6. Augmented LLM

Anthropic describes the **augmented LLM** as a basic building block of agentic systems.

A normal LLM:

```text
User
 ↓
LLM
 ↓
Answer
```

An augmented LLM:

```text
             ┌───────────┐
             │ Retrieval │
             └─────┬─────┘
                   │
User → LLM ←───────┤
       ↑           │
       │      ┌────┴────┐
       │      │  Tools  │
       │      └─────────┘
       │
       └──── Memory
```

It can be enhanced with:

- Retrieval
- Tools
- Memory

---

# 7. Retrieval

Retrieval allows the model to obtain information that is not directly available in its context.

Example:

```text
User:
"What does our company refund policy say?"

        ↓

Search company documents

        ↓

Relevant policy retrieved

        ↓

LLM

        ↓

Answer
```

This is the basic idea behind RAG.

---

# 8. Tools

Tools allow an LLM to interact with the outside world.

Without tools:

```text
LLM
 ↓
Text
```

With tools:

```text
LLM
 ↓
Choose tool
 ↓
Tool executes
 ↓
Result
 ↓
LLM
```

Examples:

```text
search_web()
read_file()
write_file()
run_python()
query_database()
send_email()
create_ticket()
run_tests()
```

Tools change the LLM from something that only generates text into something that can perform actions.

---

# 9. Memory

Memory allows an AI system to retain useful information.

```text
Conversation
    ↓
Important information
    ↓
Memory
    ↓
Future interaction
```

Memory becomes especially useful for long-running tasks and agents.

---

# 10. Pattern 1 — Prompt Chaining

**Prompt chaining** means dividing a task into a sequence of LLM calls.

```text
LLM 1
 ↓
LLM 2
 ↓
LLM 3
 ↓
Final Output
```

Each step processes the previous step's output.

---

## Example — Writing an Article

Instead of:

```text
"Write a perfect technical article."
```

Use:

```text
Step 1:
Generate outline

        ↓

Step 2:
Check outline

        ↓

Step 3:
Write article

        ↓

Step 4:
Review article
```

Each LLM call has a smaller responsibility.

---

# 11. Why Prompt Chaining Works

One huge task can be difficult:

```text
Write a complete high-quality article
```

Breaking it down:

```text
Create outline
```

then:

```text
Check outline
```

then:

```text
Write article
```

makes each individual task easier.

> **Make each individual LLM task simpler.**

---

# 12. Adding a Gate

A programmatic check can be placed between steps.

```text
Generate outline
      ↓
Check outline
      ↓
Is outline valid?
   ↙       ↘
 No        Yes
 ↓          ↓
Fix       Write article
```

The system doesn't blindly pass bad output to the next stage.

---

# 13. Pattern 2 — Routing

Routing means:

> **Understand what type of request was received, then send it to the appropriate specialist.**

Architecture:

```text
                 ┌── Technical Agent
                 │
User → Router ───┼── Refund Agent
                 │
                 └── General Agent
```

---

## Example — Customer Support

Customer:

> "I want my money back."

Router:

```text
Request classification
        ↓
Refund request
        ↓
Refund workflow
```

Another customer:

> "The application crashes when I open it."

Router:

```text
Request classification
        ↓
Technical problem
        ↓
Technical support workflow
```

---

# 14. Why Routing is Useful

Instead of one giant prompt:

```text
Handle refunds.
Handle technical issues.
Answer general questions.
Handle complaints.
Handle account problems.
...
```

you can give each category its own workflow.

Routing can also select models:

```text
              ┌── Easy → Small/Cheap Model
User → Router ┤
              └── Hard → Powerful Model
```

This can reduce cost while maintaining quality.

---

# 15. Pattern 3 — Parallelization

Parallelization means doing independent tasks at the same time.

Instead of:

```text
Task A
 ↓
Task B
 ↓
Task C
```

do:

```text
       ┌── Task A ──┐
       │            │
Input ─┼── Task B ──┼→ Combine
       │            │
       └── Task C ──┘
```

Anthropic discusses two important forms:

1. Sectioning
2. Voting

---

# 16. Parallelization — Sectioning

Sectioning means dividing a large task into independent pieces.

Example:

```text
                    ┌── Security analysis
                    │
Code → Parallel ────┼── Performance analysis
                    │
                    └── Quality analysis
                           ↓
                       Combine
```

Each reviewer focuses on a specific concern.

---

# 17. Example — Code Review

Instead of asking one LLM to check everything:

```text
Check security
Check performance
Check bugs
Check style
```

run:

```text
             ┌── Security reviewer
             │
Code ────────┼── Performance reviewer
             │
             ├── Bug reviewer
             │
             └── Style reviewer
                    ↓
                Aggregator
                    ↓
              Final review
```

---

# 18. Parallelization — Voting

Voting means asking multiple model calls to solve the same problem independently.

```text
             ┌── Model 1 → Answer A
             │
Question ────┼── Model 2 → Answer B
             │
             └── Model 3 → Answer A
                         ↓
                    Majority vote
                         ↓
                       Answer A
```

Example:

```text
Reviewer 1 → Vulnerable
Reviewer 2 → Vulnerable
Reviewer 3 → Safe
Reviewer 4 → Vulnerable
Reviewer 5 → Vulnerable
```

Voting:

```text
4 / 5 → Vulnerable
```

Therefore:

```text
Flag for review
```

---

# 19. Pattern 4 — Orchestrator-Workers

This pattern is more dynamic.

The key idea:

> **The orchestrator decides what workers are needed.**

Architecture:

```text
                 ┌── Worker 1
                 │
User → Orchestrator ── Worker 2
                 │
                 ├── Worker 3
                 │
                 └── Worker 4
                         ↓
                    Synthesize
```

The workers are not necessarily known beforehand.

---

# 20. Parallelization vs Orchestrator-Workers

### Parallelization

The tasks are already known:

```text
Security
Performance
Style
```

So:

```text
Input
 ↓
A + B + C
 ↓
Combine
```

### Orchestrator-Workers

The tasks are not known beforehand:

```text
Input
 ↓
Orchestrator
 ↓
"Which tasks do I need?"
 ↓
Worker A
Worker B
Worker C
...
 ↓
Combine
```

The orchestrator dynamically creates the work.

---

# 21. Example — Coding Agent

User:

> "Add authentication to this application."

The orchestrator inspects the project and decides:

```text
Need to modify:

auth.py
 ↓
Worker 1

routes.py
 ↓
Worker 2

database.py
 ↓
Worker 3

tests/
 ↓
Worker 4
```

The number and type of workers depend on the actual repository.

---

# 22. Example — Research Agent

User:

> "Research the impact of post-quantum cryptography on TLS."

Orchestrator:

```text
Need research on:

NIST standards
     ↓
Worker 1

TLS 1.3
     ↓
Worker 2

Migration challenges
     ↓
Worker 3

Performance impact
     ↓
Worker 4

Security implications
     ↓
Worker 5
```

Then:

```text
Worker results
      ↓
Orchestrator
      ↓
Synthesize
      ↓
Research report
```

---

# 23. Pattern 5 — Evaluator-Optimizer

This pattern introduces a feedback loop.

```text
Generator
    ↓
Output
    ↓
Evaluator
    ↓
Feedback
    ↓
Generator
    ↓
Improved output
```

It is essentially:

> **Write → Review → Improve → Review → Improve**

---

# 24. Example — Writing

Generator:

```text
Draft 1
```

Evaluator:

```text
Problems:
- Introduction is weak
- Missing examples
- Explanation is too technical
```

Generator:

```text
Draft 2
```

Evaluator:

```text
Much better.
Still missing:
- Practical example
```

Generator:

```text
Final draft
```

This works especially well when you have clear evaluation criteria.

---

# 25. Example — Code Generation

Generator:

```text
Generate Python function
```

Evaluator:

```text
Check:
- Correctness
- Edge cases
- Type safety
- Tests
```

Feedback:

```text
Fails when input is empty.
```

Generator:

```text
Fix empty-input handling.
```

Evaluator:

```text
Tests pass.
```

Final:

```text
Accepted
```

Software is particularly useful because it provides objective feedback:

```text
Code
 ↓
Run tests
 ↓
Pass / Fail
```

---

# 26. Pattern 6 — Autonomous Agents

An autonomous agent operates in a loop.

Basic architecture:

```text
              ┌───────────────┐
              │               ↓
User → Agent → Think → Tool → Result
              ↑               │
              └───────────────┘
```

More explicitly:

```text
Goal
 ↓
Agent decides next action
 ↓
Tool call
 ↓
Observe result
 ↓
Reason
 ↓
Choose next action
 ↓
Tool call
 ↓
Observe result
 ↓
...
 ↓
Finish
```

---

# 27. Agent Example — Software Engineer

User:

> "Fix the failing tests."

Agent:

```text
Read repository
       ↓
Run tests
       ↓
Observe failure
       ↓
Inspect relevant code
       ↓
Modify code
       ↓
Run tests
       ↓
Tests fail
       ↓
Analyze new failure
       ↓
Modify code
       ↓
Run tests
       ↓
Tests pass
       ↓
Finish
```

The exact sequence is decided dynamically.

---

# 28. Ground Truth

Agents should obtain **ground truth from the environment** during execution.

Don't let the model simply assume:

```text
"I think the code works."
```

Instead:

```text
Agent
 ↓
Run tests
 ↓
Actual result
 ↓
Agent
```

Other examples:

```text
Search → actual search results
Database query → actual database result
API call → actual API response
Code execution → actual output
File operation → actual file state
```

This makes agents more reliable.

---

# 29. Agent Loop

A simple agent loop:

```text
while not finished:

    understand current state

    decide next action

    call tool

    observe result

    update understanding
```

For example:

```text
while tests_are_failing:

    inspect_failure()

    choose_file()

    modify_code()

    run_tests()
```

---

# 30. When Should You Use an Agent?

Agents are appropriate when:

- the problem is open-ended
- you cannot predict the exact number of steps
- the agent needs to make decisions
- tools are required
- feedback from the environment is available
- the task can tolerate some autonomy

Examples:

```text
Coding
Research
Customer support
Computer use
Data analysis
Troubleshooting
```

---

# 31. When Should You NOT Use an Agent?

Don't use an autonomous agent simply because agents are popular.

For example:

```text
PDF
 ↓
OCR
 ↓
Extract values
 ↓
Normalize
 ↓
Validate
 ↓
Generate JSON
```

This is predictable.

A workflow is probably better.

You don't need:

```text
Agent:
"What should I do with the PDF?"
```

The pipeline already knows.

---

# 32. Practical Decision Rule

Use this progression:

```text
             Start
               ↓
       Can one LLM call solve it?
          ↙           ↘
        Yes            No
        ↓               ↓
      Use it      Can fixed steps solve it?
                    ↙       ↘
                  Yes        No
                  ↓           ↓
              Workflow      Agent
```

If a workflow is required:

```text
Fixed sequential tasks
        ↓
Prompt chaining

Different request types
        ↓
Routing

Independent tasks
        ↓
Parallelization

Unknown subtasks
        ↓
Orchestrator-workers

Need iterative improvement
        ↓
Evaluator-optimizer
```

---

# 33. Combining Patterns

These patterns can be combined.

Example:

```text
User
 ↓
Router
 ↓
Research request
 ↓
Orchestrator
 ↓
 ┌───────────────┐
 │               │
Worker A       Worker B
 │               │
 ↓               ↓
Research       Research
 │               │
 └───────┬───────┘
         ↓
     Evaluator
         ↓
     Optimizer
         ↓
      Report
```

This is a **hybrid architecture**.

---

# 34. Applying These Ideas to LangGraph

The article is especially useful for understanding LangGraph.

You can map Anthropic's patterns to LangGraph concepts.

## Prompt Chaining

```text
Node A
  ↓
Node B
  ↓
Node C
```

Example:

```text
START
 ↓
extract
 ↓
validate
 ↓
generate
 ↓
END
```

---

# 35. Routing in LangGraph

Use conditional edges:

```text
START
 ↓
classifier
 ↓
 ├── CBC workflow
 ├── LFT workflow
 ├── KFT workflow
 └── Lipid workflow
```

Conceptually:

```python
graph.add_conditional_edges(
    "classifier",
    route_request
)
```

The classifier determines which path to follow.

---

# 36. Parallelization in LangGraph

Multiple nodes can perform independent work:

```text
          ┌── security_check
          │
START ────┼── quality_check
          │
          └── factual_check
                    ↓
                 combine
```

This corresponds closely to Anthropic's **sectioning** pattern.

---

# 37. Orchestrator-Workers in LangGraph

A central node can determine what work needs to be performed.

```text
             Orchestrator
              /    |    \
             /     |     \
        Worker 1 Worker 2 Worker 3
             \     |     /
              \    |    /
               Synthesizer
```

Useful for:

- research agents
- coding agents
- document analysis
- complex planning

---

# 38. Evaluator-Optimizer in LangGraph

You can build a loop:

```text
Generate
   ↓
Evaluate
   ↓
Good?
 ↙    ↘
No     Yes
↓       ↓
Improve END
↓
Evaluate
```

This is a natural LangGraph use case because LangGraph supports graph-based conditional routing and cycles.

---

# 39. Autonomous Agent in LangGraph

A simplified graph:

```text
START
  ↓
Agent
  ↓
Choose tool
  ↓
Tool
  ↓
Agent
  ↓
Choose tool
  ↓
Tool
  ↓
Agent
  ↓
Finish
  ↓
END
```

The important part is that the agent repeatedly interacts with tools based on the current state.

---

# 40. Frameworks and Abstraction

Frameworks can help build agentic systems faster, but they can also introduce abstraction.

For example:

```text
Your code
   ↓
Framework
   ↓
Framework abstraction
   ↓
LLM API
```

If something breaks, you may have difficulty understanding what actually happened.

The important lesson is:

> Understand what is happening underneath the framework.

For LangGraph, learn the fundamentals:

```text
State
 ↓
Nodes
 ↓
Edges
 ↓
Conditional edges
 ↓
Loops
 ↓
Tools
 ↓
Persistence
 ↓
Human intervention
```

Then LangGraph becomes an implementation tool for architectures you already understand.

---

# 41. Tool Design

Tools are extremely important because they are the agent's interface to the outside world.

Example:

```python
search_database(
    patient_id: str,
    test_type: str
)
```

The agent needs to understand:

- what the tool does
- what parameters it expects
- what the parameters mean
- what it returns
- when it should be used
- what errors can occur

Tool definitions should receive the same careful design attention as the main system prompt.

---

# 42. Bad Tool vs Good Tool

### Bad

```text
search()
```

Questions remain:

- What does it search?
- What input does it need?
- What does it return?

### Better

```text
search_medical_documents(
    query: str,
    document_type: str,
    max_results: int
)
```

Description:

```text
Searches approved medical documents for information
relevant to the supplied query. Use this tool when
information is not available in the current context.
```

The model now has a clearer interface.

---

# 43. Tool Design Example

Suppose a coding agent has:

```text
run_tests()
```

A better interface could be:

```text
run_tests(
    test_path,
    timeout
)
```

Return:

```json
{
  "status": "failed",
  "passed": 18,
  "failed": 2,
  "errors": [...]
}
```

Now the agent can reason about the actual result.

---

# 44. Transparency

Make agent planning and actions understandable.

Conceptually:

```text
Goal
 ↓
Plan
 ↓
Action
 ↓
Observation
 ↓
Next action
```

This makes the system easier to understand and debug.

---

# 45. Guardrails

Agents have more freedom, so safeguards are important.

For example:

```text
Agent
 ↓
Dangerous action?
 ↙       ↘
Yes       No
 ↓         ↓
Human     Execute
approval
```

You might require approval before:

- deleting files
- sending emails
- making financial transactions
- changing production systems
- modifying databases
- deploying code

---

# 46. Stopping Conditions

An agent should not run forever.

Examples:

```text
Maximum iterations = 10
```

or:

```text
Stop if tests pass
```

or:

```text
Stop if goal is achieved
```

or:

```text
Stop if human approval is required
```

Stopping conditions help maintain control over autonomous systems.

---

# 47. Human-in-the-Loop

Not everything should be fully autonomous.

A good architecture can be:

```text
Agent
 ↓
Perform analysis
 ↓
Prepare action
 ↓
Human approval
 ↓
Execute
```

Example:

```text
Agent:
"I found a database migration that will delete 3 columns."

        ↓

Human:
Approve / Reject

        ↓

Agent:
Execute only if approved
```

---

# 48. Customer Support Example

Customer support is a useful application for agents because it naturally involves:

```text
Conversation
+
Information retrieval
+
Tools
+
Actions
+
Clear success criteria
```

Example:

```text
Customer:
"My order hasn't arrived."

Agent
 ↓
Look up order
 ↓
Check shipping status
 ↓
Check expected delivery
 ↓
Respond
```

If required:

```text
Create support ticket
```

or:

```text
Issue refund
```

Tools turn a chatbot into an actual problem-solving system.

---

# 49. Coding Agents

Coding is another excellent use case because it has strong feedback mechanisms.

```text
Write code
 ↓
Run tests
 ↓
Pass / Fail
```

Example:

```text
User:
"Fix issue #123."

Agent
 ↓
Read issue
 ↓
Inspect repository
 ↓
Find relevant files
 ↓
Modify code
 ↓
Run tests
 ↓
Analyze failures
 ↓
Modify code
 ↓
Run tests
 ↓
Pass
```

---

# 50. Why Coding Agents Are a Good Example

Compare two tasks.

### Task A

```text
Convert this JSON to CSV.
```

Known steps:

```text
Read JSON
 ↓
Convert
 ↓
Write CSV
```

A workflow is enough.

### Task B

```text
Find and fix the bug causing these tests to fail.
```

Unknown:

```text
Which file?
How many files?
What is the root cause?
How many changes?
Do tests reveal another problem?
```

An agent is more appropriate.

---

# 51. Evaluation

Don't decide that an agent is good simply because it appears to work.

Instead:

```text
Build
 ↓
Create evaluation dataset
 ↓
Measure
 ↓
Identify failures
 ↓
Modify architecture
 ↓
Measure again
```

Useful metrics include:

```text
Accuracy
Latency
Cost
Tool-call success rate
Failure rate
Human approval rate
```

---

# 52. Don't Build Multi-Agent Systems Just Because You Can

A common beginner mistake is:

```text
Agent 1
Agent 2
Agent 3
Agent 4
Agent 5
Agent 6
```

and assuming:

```text
More agents = Better system
```

This is not necessarily true.

Sometimes:

```text
One well-designed LLM
+
Good prompt
+
Good retrieval
```

can beat:

```text
Six poorly coordinated agents
```

---

# 53. Applying the Article to a Medical Report Generator

For a medical-report pipeline, you generally don't need a fully autonomous agent for deterministic processing.

A workflow can look like:

```text
PDF/Image
   ↓
OCR
   ↓
Schema Detection
   ↓
Field Extraction
   ↓
Normalization
   ↓
Validation
   ↓
Confidence Scoring
   ↓
Structured Output
   ↓
Report Generation
```

This is primarily a **workflow**.

---

# 54. Where Agents Could Be Added

Introduce agentic behavior only where decisions become uncertain.

Example:

```text
OCR
 ↓
Extraction
 ↓
Validation
 ↓
Low confidence?
    ↙       ↘
  No         Yes
  ↓           ↓
Continue    Review Agent
                ↓
             Search /
             Analyze /
             Resolve
```

This is much better than turning the entire pipeline into one autonomous agent.

---

# 55. Example Medical Review Agent

Suppose extraction produces:

```text
Hemoglobin = 12.4
Unit = ?
Reference Range = ?
Confidence = 0.61
```

A review agent could:

```text
Inspect OCR
      ↓
Check schema
      ↓
Search known units
      ↓
Compare nearby text
      ↓
Evaluate possibilities
      ↓
Return recommendation
```

The deterministic stages remain deterministic.

This follows the principle:

> **Use agentic behavior where flexibility is actually required.**

---

# 56. A Practical Architecture Ladder

Think of AI systems as a ladder:

```text
Level 1
Simple LLM
    ↓
Level 2
LLM + Retrieval
    ↓
Level 3
LLM + Tools
    ↓
Level 4
Prompt Chaining
    ↓
Level 5
Routing
    ↓
Level 6
Parallelization
    ↓
Level 7
Orchestrator-Workers
    ↓
Level 8
Evaluator-Optimizer
    ↓
Level 9
Autonomous Agent
```

You do not necessarily need to reach Level 9.

---

# 57. Choosing the Architecture

## Fixed sequence

Use:

```text
Prompt Chaining
```

Example:

```text
Extract → Validate → Format
```

## Different categories

Use:

```text
Routing
```

Example:

```text
Billing → Billing Agent
Technical → Technical Agent
General → General Agent
```

## Independent tasks

Use:

```text
Parallelization
```

Example:

```text
Security + Performance + Quality
```

## Unknown subtasks

Use:

```text
Orchestrator-Workers
```

Example:

```text
Complex research
Complex coding
```

## Output needs refinement

Use:

```text
Evaluator-Optimizer
```

Example:

```text
Generate → Review → Improve
```

## Open-ended task

Use:

```text
Autonomous Agent
```

Example:

```text
Investigate and fix a software issue
```

---

# 58. Complete Research Agent Example

User:

> "Research the current state of post-quantum cryptography migration."

### Step 1 — Router

Determine:

```text
Research request
```

### Step 2 — Orchestrator

Break research into:

```text
NIST standards
Industry adoption
TLS migration
Performance impact
Migration challenges
```

### Step 3 — Workers

Run research tasks:

```text
       ┌── NIST
       ├── Industry
       ├── TLS
       ├── Performance
       └── Migration
```

### Step 4 — Synthesis

Combine findings.

### Step 5 — Evaluator

Check:

```text
Are sources sufficient?
Are claims supported?
Are important areas missing?
```

### Step 6 — Optimizer

If something is missing:

```text
Search again
```

### Step 7 — Final report

```text
Executive Summary
 ↓
Current Standards
 ↓
Migration Techniques
 ↓
Challenges
 ↓
Recommendations
 ↓
References
```

This combines:

```text
Routing
+
Orchestrator-workers
+
Parallelization
+
Evaluator-optimizer
```

---

# 59. The Three Core Principles

## 1. Simplicity

Keep the architecture as simple as possible.

```text
Simple → Measure → Improve → Add complexity only if needed
```

## 2. Transparency

Make the agent's planning and actions understandable.

```text
Goal
 ↓
Plan
 ↓
Action
 ↓
Observation
```

## 3. Good Agent-Computer Interface

Tools should be:

- clearly documented
- easy to use
- appropriately structured
- tested
- designed around what the model can reliably produce

---

# 60. The Entire Article in One Diagram

```text
                    AI SYSTEM
                        │
                        ▼
                ┌──────────────┐
                │ Simple LLM   │
                └──────┬───────┘
                       │
              Need more capability?
                       │
                       ▼
              ┌─────────────────┐
              │ Augmented LLM   │
              │ Retrieval       │
              │ Tools           │
              │ Memory          │
              └───────┬─────────┘
                      │
             Need multiple steps?
                      │
                      ▼
                ┌───────────┐
                │ Workflows │
                └─────┬─────┘
                      │
        ┌─────────────┼─────────────────┐
        │             │                 │
        ▼             ▼                 ▼
   Chaining       Routing        Parallelization
        │                               │
        └─────────────┬─────────────────┘
                      ▼
             Orchestrator-Workers
                      │
                      ▼
             Evaluator-Optimizer
                      │
                      ▼
             Need dynamic decisions?
                      │
                      ▼
                 AI Agent
                      │
                      ▼
              Tool → Observe
                 ↑      │
                 └──────┘
```

---

# 61. Final Mental Model

Remember these questions.

### Question 1

**Can one LLM call solve it?**

```text
YES → Use one LLM
```

### Question 2

**Can the task be broken into fixed steps?**

```text
YES → Workflow
```

### Question 3

**Are there different types of inputs?**

```text
YES → Routing
```

### Question 4

**Are there independent tasks?**

```text
YES → Parallelization
```

### Question 5

**Can I not predict what steps will be needed?**

```text
YES → Agent / Orchestrator
```

If the output needs repeated improvement:

```text
Evaluator → Optimizer
```

---

# 62. One-Page Cheat Sheet

| Pattern | Simple Meaning | Best Use |
|---|---|---|
| **Augmented LLM** | LLM + tools/retrieval/memory | Basic AI application |
| **Prompt Chaining** | Step 1 → Step 2 → Step 3 | Fixed sequence |
| **Routing** | Decide which path to use | Different request types |
| **Parallelization** | Multiple tasks simultaneously | Independent tasks |
| **Voting** | Multiple attempts → choose result | Higher confidence |
| **Orchestrator-Workers** | Manager dynamically creates tasks | Complex unpredictable tasks |
| **Evaluator-Optimizer** | Generate → evaluate → improve | Quality refinement |
| **Agent** | LLM decides actions dynamically | Open-ended problems |

---

# 63. Most Important Takeaway

If you remember only one thing from the Anthropic article:

```text
                 Don't start here
                       ↓
                ┌─────────────┐
                │ Autonomous  │
                │    Agent    │
                └─────────────┘
                       ↑
                 Add complexity
                  when needed
                       ↑
              ┌─────────────────┐
              │    Workflow     │
              └─────────────────┘
                       ↑
              ┌─────────────────┐
              │ Augmented LLM   │
              └─────────────────┘
                       ↑
              ┌─────────────────┐
              │    Simple LLM   │
              └─────────────────┘
                       ↑
                     START
```

**Start simple.**

Then:

```text
Measure
  ↓
Find limitations
  ↓
Add the smallest useful pattern
  ↓
Measure again
  ↓
Repeat
```

That is the central engineering philosophy behind **Building Effective Agents**.

---

# 64. Reference

Primary source:

https://www.anthropic.com/engineering/building-effective-agents

The article explains:

- Workflows vs agents
- Augmented LLMs
- Prompt chaining
- Routing
- Parallelization
- Orchestrator-workers
- Evaluator-optimizer
- Autonomous agents
- Tool design
- Agent safety
- Human-in-the-loop
- Evaluation
- Simplicity and transparency

> **Core philosophy:** Build the simplest system that works, evaluate it carefully, and only introduce additional agentic complexity when the improvement is worth the additional cost, latency, and complexity.
