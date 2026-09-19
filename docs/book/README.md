# Book and code navigation

Use this file to move from a book chapter to the code that implements it.
The book explains the ideas; the links below identify the runnable examples and
the PicoAgents implementation to read when you need to understand how an idea
works.

Start with Chapter 4. It is the point where the book becomes an implementation
guide. Read the four small programs in [`code_along/`](../../code_along/) in
order, then use the chapter map below to follow a feature into the full
framework.

For section-by-section maps from the book to current source, tests, and
examples, use these chapter guides:

- [`CH4.md`](CH4.md) — agent fundamentals
- [`CH5.md`](CH5.md) — computer-use agents
- [`CH6.md`](CH6.md) — deterministic workflows
- [`CH7.md`](CH7.md) — autonomous orchestration
- [`CH8.md`](CH8.md) — web experiences
- [`CH9.md`](CH9.md) — framework comparison
- [`CH10.md`](CH10.md) — evaluation
- [`CH11.md`](CH11.md) — optimization
- [`CH12.md`](CH12.md) — MCP and distributed protocols
- [`CH13.md`](CH13.md) — responsible operation
- [`CH14.md`](CH14.md) — YC-analysis application
- [`CH15.md`](CH15.md) — SWE agent application

## Book files

- [`book.md`](book.md) is the searchable Markdown conversion. Its `--- end of
  page=N ---` markers preserve the original PDF page boundaries.
- [`book.pdf`](book.pdf) is the original book.
- [`images/`](images/) contains the 53 PNG figures extracted during conversion.

The Markdown is also indexed in the repository's OpenAI vector store. See
[`AGENTS.md`](../../AGENTS.md) for the search instructions and vector-store ID.

## Structure

Read the repository from small mechanism to complete system:

| Directory | Purpose |
| --- | --- |
| [`code_along/`](../../code_along/) | Teaches the mechanism. Four small, standalone Chapter 4 programs progressively build the agent loop, tools, memory, and streaming. |
| [`examples/`](../../examples/) | Demonstrates practical uses. Chapter-oriented programs show agents, workflows, orchestration, evaluation, optimization, MCP, and applications. |
| [`picoagents/src/`](../../picoagents/src/) | Contains the complete framework: middleware, persistence, approval, observability, serialization, MCP, and other production concerns. |

## Use the repository without following the book

The repository is easier to learn by dependency than by chapter number. Read a
test first, trace the implementation, run an example, change one behavior, and
then add or update a test.

| Step | Read and run | Action |
| --- | --- | --- |
| 1. Contracts | [`messages.py`](../../picoagents/src/picoagents/messages.py), [`types.py`](../../picoagents/src/picoagents/types.py), and [`context.py`](../../picoagents/src/picoagents/context.py) | Learn the state that every subsystem exchanges. |
| 2. One agent | [`test_agent_basic.py`](../../picoagents/tests/test_agent_basic.py) → [`agents/_base.py`](../../picoagents/src/picoagents/agents/_base.py) → [`agents/_agent.py`](../../picoagents/src/picoagents/agents/_agent.py) | Start with the mock-client tests; trace `run()` and `run_stream()` before using a live model. |
| 3. Capabilities | [`test_tools.py`](../../picoagents/tests/test_tools.py), [`tools/_base.py`](../../picoagents/src/picoagents/tools/_base.py), and [`memory/_base.py`](../../picoagents/src/picoagents/memory/_base.py) | Add tools, memory, approval, and middleware one at a time. |
| 4. Deterministic systems | [`workflow` tests](../../picoagents/tests/workflow/), [`sequential.py`](../../examples/workflows/sequential.py), and [`conditional.py`](../../examples/workflows/conditional.py) | Use workflows when the control flow is known. Learn checkpoints before parallelism. |
| 5. Autonomous coordination | [`test_orchestrator.py`](../../picoagents/tests/test_orchestrator.py), [`orchestration/`](../../picoagents/src/picoagents/orchestration/), and [`round-robin.py`](../../examples/orchestration/round-robin.py) | Learn termination before AI-driven selection or planning. |
| 6. Evidence and improvement | [`evaluation examples`](../../examples/evaluation/), [`eval/`](../../picoagents/src/picoagents/eval/), then [`optimization examples`](../../examples/optimization/) | Establish an evaluation set before changing prompts, tools, or configuration. |
| 7. Integrations | MCP, computer use, persistence, OpenTelemetry, and Web UI modules | Add integrations only when the core system requires them. |
| 8. Complete applications | [`YC analysis`](../../examples/workflows/yc_analysis/) and [`SWE agent`](../../examples/agents/swe_agent/) | Reverse-engineer complete systems using the earlier layers. |

[`code_along/`](../../code_along/) is an optional conceptual warm-up, not the
canonical implementation. [`course/samples/`](../../course/samples/) is a
separate collection of framework-specific tutorials and is not part of this
PicoAgents path.

## What to take from Chapters 1–3

These chapters supply the decision framework, rather than a codebase to study.

| Chapter | Keep this idea | Why it matters before writing code |
| --- | --- | --- |
| 1. Understanding Multi-Agent Systems | Use multiple agents only when a task has separable expertise, long-lived state, or coordination needs. | More agents add cost, latency, and failure modes; a single agent or normal program is often enough. |
| 2. Multi-Agent Patterns | Choose explicit workflows when you know the control flow; choose autonomous orchestration when the next action must be decided at runtime. | This is the design decision that separates the Chapter 6 workflow engine from the Chapter 7 orchestrators. |
| 3. UX Principles for Multi-Agent Systems | Show capability, progress, cost, provenance, and a way to interrupt work. | An agent that acts invisibly or cannot be stopped is difficult to trust and operate. |

## Code map: Chapters 4 onward

The book's listings are teaching snapshots. The framework has evolved, so use
the small examples to learn the mechanism and the linked `picoagents/` source
to see the complete implementation.

| Chapter | Objective | Start with | Then inspect |
| --- | --- | --- | --- |
| 4. Building Your First Agent | Build an async agent that calls a model and tools, keeps context and memory, streams events, and can be controlled. | [`ch04_v1_agent.py`](../../code_along/ch04_v1_agent.py) → [`ch04_v2_tools.py`](../../code_along/ch04_v2_tools.py) → [`ch04_v3_memory.py`](../../code_along/ch04_v3_memory.py) → [`ch04_v4_streaming.py`](../../code_along/ch04_v4_streaming.py) | Full loop: [`_agent.py`](../../picoagents/src/picoagents/agents/_agent.py); interfaces: [`_base.py`](../../picoagents/src/picoagents/agents/_base.py); examples: [`examples/agents/`](../../examples/agents/), [`examples/memory/`](../../examples/memory/), [`examples/tools/approval_example.py`](../../examples/tools/approval_example.py), and [`examples/otel/`](../../examples/otel/) |
| 5. Building Computer Use Agents | Let an agent perceive and operate a browser or GUI. | [`computer_use.py`](../../examples/agents/computer_use.py) | [`_computer_use/`](../../picoagents/src/picoagents/agents/_computer_use/) |
| 6. Building Multi-Agent Workflows | Build deterministic graphs of typed steps, branches, parallel work, checkpoints, and streamed progress. | [`sequential.py`](../../examples/workflows/sequential.py), [`conditional.py`](../../examples/workflows/conditional.py), and [`checkpoint_example.py`](../../examples/workflows/checkpoint_example.py) | [`workflow/`](../../picoagents/src/picoagents/workflow/) and its tests in [`picoagents/tests/workflow/`](../../picoagents/tests/workflow/) |
| 7. Building Autonomous Multi-Agent Orchestration | Coordinate agents whose next speaker or plan is selected at runtime. | [`round-robin.py`](../../examples/orchestration/round-robin.py), [`ai-driven.py`](../../examples/orchestration/ai-driven.py), and [`plan-based.py`](../../examples/orchestration/plan-based.py) | [`orchestration/`](../../picoagents/src/picoagents/orchestration/) and [`termination/`](../../picoagents/src/picoagents/termination/) |
| 8. Building Modern Web Experiences for Agent Applications | Build a backend that executes agents and a streaming UI that makes work visible and interruptible. | [`examples/app/`](../../examples/app/) | FastAPI server: [`webui/_server.py`](../../picoagents/src/picoagents/webui/_server.py); React client: [`webui/frontend/`](../../picoagents/src/picoagents/webui/frontend/) |
| 9. Multi-Agent Frameworks | Compare framework choices against the same agent, workflow, and orchestration patterns. | [`examples/frameworks/`](../../examples/frameworks/) | The PicoAgents reference implementation: [`picoagents/src/picoagents/`](../../picoagents/src/picoagents/) |
| 10. Evaluating Multi-Agent Systems | Define datasets, targets, judges, metrics, and repeatable evaluation runs. | [`examples/evaluation/`](../../examples/evaluation/) | [`eval/`](../../picoagents/src/picoagents/eval/) and [`test_eval.py`](../../picoagents/tests/test_eval.py) |
| 11. Optimizing Multi-Agent Systems | Improve instructions and configurations from evaluation evidence, while testing generalization. | [`examples/optimization/`](../../examples/optimization/) | [`optim/`](../../picoagents/src/picoagents/optim/) and [`test_optim.py`](../../picoagents/tests/test_optim.py) |
| 12. Protocols for Distributed Agents | Connect agents to external tools and services through MCP; understand the protocol boundary. | [`examples/mcp/`](../../examples/mcp/) | MCP client and tool integration: [`tools/_mcp/`](../../picoagents/src/picoagents/tools/_mcp/); playground: [`webui/mcp/`](../../picoagents/src/picoagents/webui/mcp/) |
| 13. Ethics and Responsible AI for Multi-Agent Systems | Identify risks created when agents can act: unsafe tools, sensitive data, uncontrolled cost, and weak observability. | [`approval_example.py`](../../examples/tools/approval_example.py) and [`middleware.py`](../../examples/agents/middleware.py) | Approval and context types: [`context.py`](../../picoagents/src/picoagents/context.py); enforcement hooks: [`_middleware.py`](../../picoagents/src/picoagents/_middleware.py) |
| 14. Answering Business Questions from Unstructured Data | Build a production workflow that loads data, filters cheaply, applies structured analysis, and produces insights. | [`examples/workflows/yc_analysis/`](../../examples/workflows/yc_analysis/) | [`workflow.py`](../../examples/workflows/yc_analysis/workflow.py), [`steps.py`](../../examples/workflows/yc_analysis/steps.py), and [`models.py`](../../examples/workflows/yc_analysis/models.py) |
| 15. Building a Software Engineering Agent | Assemble coding tools, workspace handling, memory, verification, and review into an agent application. | [`examples/agents/swe_agent/`](../../examples/agents/swe_agent/) | [`agent.py`](../../examples/agents/swe_agent/agent.py) and built-in [`_coding_tools.py`](../../picoagents/src/picoagents/tools/_coding_tools.py) |

## Recommended learning path

Build one small system repeatedly. Add one capability at a time, and prove
that it works before adding the next one.

```text
Chapter 4 → Chapter 10 → Chapter 6 → Chapter 7 → Chapter 8
           → Chapter 11 → Chapters 12–13 → Chapters 14–15
```

Treat Chapter 5 as optional until the task genuinely needs browser or GUI
operation.

| Stage | Read | Build | Proof of learning |
| --- | --- | --- | --- |
| Agent foundation | 4 | Complete the four [`code_along/`](../../code_along/) files, then rebuild the same small agent yourself. | It calls a tool, retains context, streams events, and handles a rejected tool call. |
| Evaluation early | 10 | Create a small fixed task set and expected outcomes for the agent. | You can measure regressions instead of judging examples by feel. |
| Deterministic coordination | 6 | Turn the agent into a workflow with typed steps, a branch, and a checkpoint. | You can explain every execution path before running it. |
| Autonomous coordination | 7 | Add a specialized second agent only when runtime delegation is needed. | You can explain selection, termination, shared state, and failure behavior. |
| Product surface | 8 | Put the system behind a small UI or API with streaming and cancellation. | A user can see progress and stop work safely. |
| Improvement loop | 11 | Use failed evaluations to revise instructions, tools, or the workflow. | A held-out evaluation improves, rather than only the tasks used to tune it. |
| Integration and responsibility | 12–13 | Add MCP only for a needed external capability; add approval, logging, and guardrails before granting actions. | Sensitive or destructive actions require approval and leave evidence. |
| Complete systems | 14–15 | Study the YC-analysis workflow and SWE agent after learning the components. | You can identify the agent loop, coordination choice, evaluation method, and safeguards in each system. |

Chapter 10 comes immediately after Chapter 4 because agent behavior is
probabilistic. Without a fixed evaluation set, later changes can look like
progress without producing evidence. Evaluation makes the rest of the book an
engineering practice rather than a collection of patterns.

## Limits of this map

The repository contains no standalone implementation for every discussion in
the book. Chapter 13 is primarily a design and governance chapter; its linked
approval and middleware examples demonstrate the closest enforceable controls.
Chapter 9 compares external frameworks, so its examples may need their own
dependencies. The links in this README describe the current repository, not a
claim that every source file exactly matches a printed listing.
