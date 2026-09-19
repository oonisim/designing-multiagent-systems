# Chapter 4: current code guide

Chapter 4 explains the architecture, but its listings are not a runnable,
versioned implementation. Use this guide to read the current code by topic.
For every row, read the test first, trace the implementation, then run the
example only after configuring its provider credentials.

The current framework is larger than the book's teaching sketches. Read the
smallest linked source first; do not begin with the full `Agent` class.

## Actual framework source

These are the current implementation files behind Chapter 4. The later
sections explain which test and example to pair with each file.

| Topic | Actual code file |
| --- | --- |
| Agent contract and configuration | [`picoagents/src/picoagents/agents/_base.py`](../../picoagents/src/picoagents/agents/_base.py) |
| Agent execution loop | [`picoagents/src/picoagents/agents/_agent.py`](../../picoagents/src/picoagents/agents/_agent.py) |
| Messages, responses, and events | [`messages.py`](../../picoagents/src/picoagents/messages.py) and [`types.py`](../../picoagents/src/picoagents/types.py) |
| Conversation and approval state | [`context.py`](../../picoagents/src/picoagents/context.py) |
| Cancellation | [`_cancellation_token.py`](../../picoagents/src/picoagents/_cancellation_token.py) |
| Model-client contract and OpenAI adapter | [`llm/_base.py`](../../picoagents/src/picoagents/llm/_base.py) and [`llm/_openai.py`](../../picoagents/src/picoagents/llm/_openai.py) |
| Tool contract and function wrapper | [`tools/_base.py`](../../picoagents/src/picoagents/tools/_base.py) |
| Application-managed and agent-managed memory | [`memory/_base.py`](../../picoagents/src/picoagents/memory/_base.py) and [`tools/_memory_tool.py`](../../picoagents/src/picoagents/tools/_memory_tool.py) |
| Middleware and telemetry | [`_middleware.py`](../../picoagents/src/picoagents/_middleware.py) and [`_otel.py`](../../picoagents/src/picoagents/_otel.py) |
| Agent composition | [`agents/_agent_as_tool.py`](../../picoagents/src/picoagents/agents/_agent_as_tool.py) |
| Context compaction and loop hooks | [`compaction.py`](../../picoagents/src/picoagents/compaction.py) and [`_hooks.py`](../../picoagents/src/picoagents/_hooks.py) |

## 4.1 Design principles

Read [`ch04_v1_agent.py`](../../code_along/ch04_v1_agent.py) for the smallest
agent loop. Then read [`messages.py`](../../picoagents/src/picoagents/messages.py),
[`types.py`](../../picoagents/src/picoagents/types.py), and
[`context.py`](../../picoagents/src/picoagents/context.py). They define the
messages, responses, events, and state that the framework exchanges.

## 4.2 Agent execution loop

Start with [`test_agent_basic.py`](../../picoagents/tests/test_agent_basic.py).
It uses a mock model client, so it exposes the contract without an API call.
Then trace [`Agent.run()`](../../picoagents/src/picoagents/agents/_agent.py) and
[`Agent.run_stream()`](../../picoagents/src/picoagents/agents/_agent.py).

### 4.2.1 Streaming events and real-time updates

Read the event types in [`types.py`](../../picoagents/src/picoagents/types.py),
then the `run_stream()` implementation in
[`_agent.py`](../../picoagents/src/picoagents/agents/_agent.py). The runnable
conceptual version is [`ch04_v4_streaming.py`](../../code_along/ch04_v4_streaming.py).

### 4.2.2 BaseAgent

Read [`BaseAgent`](../../picoagents/src/picoagents/agents/_base.py) in this
order: constructor, `_process_tools()`, `_get_tools_for_llm()`, `run()`, and
`run_stream()`. It is the current counterpart to Listings 4.3 and 4.4.
[`Agent`](../../picoagents/src/picoagents/agents/_agent.py) is the concrete
subclass that implements those two execution methods.

## 4.3 Task cancellation

Read [`CancellationToken`](../../picoagents/src/picoagents/_cancellation_token.py)
and [`test_cancellation_token.py`](../../picoagents/tests/test_cancellation_token.py),
then follow the `cancellation_token` argument through
[`Agent.run()`](../../picoagents/src/picoagents/agents/_agent.py) and
`run_stream()`.

## 4.4 Model clients

Read [`BaseChatCompletionClient`](../../picoagents/src/picoagents/llm/_base.py)
before [`OpenAIChatCompletionClient`](../../picoagents/src/picoagents/llm/_openai.py).
The base class defines provider-neutral `create()` and `create_stream()`;
the OpenAI class converts PicoAgents messages and tools to provider requests.
Use [`test_model_clients.py`](../../picoagents/tests/test_model_clients.py) to
see the expected behavior.

### 4.4.1 Agent-model integration

Return to [`Agent.run_stream()`](../../picoagents/src/picoagents/agents/_agent.py).
This is where the agent constructs messages, calls the model client, receives
a response, and decides whether a tool loop is needed.

## 4.5 Structured output

Start with [`structured-output.py`](../../examples/agents/structured-output.py),
then read the `output_format` handling in
[`_openai.py`](../../picoagents/src/picoagents/llm/_openai.py). The example
shows the public Pydantic-model API; the client implements its provider schema.

## 4.6 Tools

Read this section in its internal order:

1. [`BaseTool`](../../picoagents/src/picoagents/tools/_base.py) defines the
   schema, execution, result, and approval contract.
2. [`FunctionTool`](../../picoagents/src/picoagents/tools/_base.py) converts a
   normal Python function into that contract.
3. [`BaseAgent._process_tools()`](../../picoagents/src/picoagents/agents/_base.py)
   normalizes supplied functions and tools.
4. [`Agent` tool execution](../../picoagents/src/picoagents/agents/_agent.py)
   executes model-requested calls and adds results to the conversation.
5. [`test_tools.py`](../../picoagents/tests/test_tools.py) is the behavioral
   specification; [`basic-agent.py`](../../examples/agents/basic-agent.py) is
   the live-model example.

For the book's general-purpose versus task-specific distinction, inspect the
built-in tools in [`_core_tools.py`](../../picoagents/src/picoagents/tools/_core_tools.py)
and compare them with the application functions in `basic-agent.py`.

## 4.7 Memory

First distinguish session context from long-lived memory:

- [`AgentContext`](../../picoagents/src/picoagents/context.py) holds the current
  conversation and tool results.
- [`BaseMemory` and `ListMemory`](../../picoagents/src/picoagents/memory/_base.py)
  define storage and retrieval across work.

Read [`list_memory_example.py`](../../examples/memory/list_memory_example.py)
before [`memory.py`](../../examples/agents/memory.py). The former isolates the
memory contract; the latter integrates memory with an agent.

## 4.8 Agent-managed memory

Read [`MemoryTool`](../../picoagents/src/picoagents/tools/_memory_tool.py) and
[`test_memory_tool.py`](../../picoagents/tests/test_memory_tool.py). Then use
[`memory_tool_example.py`](../../examples/memory/memory_tool_example.py) to see
an agent explicitly read and write file-backed memory. Here the model decides
when to use memory as a tool; in Section 4.7, the application owns memory.

## 4.9 Middleware

Read [`BaseMiddleware`](../../picoagents/src/picoagents/_middleware.py), then
[`MiddlewareChain`](../../picoagents/src/picoagents/_middleware.py), and then
the built-ins in that file: logging, rate limiting, PII redaction, guardrails,
and metrics. Validate the behavior with
[`test_middleware.py`](../../picoagents/tests/test_middleware.py). Finally,
read [`examples/agents/middleware.py`](../../examples/agents/middleware.py).

## 4.10 OpenTelemetry

Read [`OTelMiddleware`](../../picoagents/src/picoagents/_otel.py) after Section
4.9; it is middleware specialized for tracing and metrics. Then run an example
in [`examples/otel/`](../../examples/otel/) and inspect
[`test_otel.py`](../../picoagents/tests/test_otel.py). It is optional until the
basic agent behavior is understood.

## 4.11 Agents as tools

Read [`AgentAsTool`](../../picoagents/src/picoagents/agents/_agent_as_tool.py),
then [`agent_as_tool.py`](../../examples/agents/agent_as_tool.py), then
[`test_agent_as_tool_strategies.py`](../../picoagents/tests/test_agent_as_tool_strategies.py).
The child agent is exposed through the same `BaseTool` interface as a function.

## 4.12 Context engineering

Read [`compaction.py`](../../picoagents/src/picoagents/compaction.py) for
message-reduction strategies and [`_hooks.py`](../../picoagents/src/picoagents/_hooks.py)
for deterministic work before model calls and completion. Then read
[`examples/contextengineering/`](../../examples/contextengineering/) and the
tests [`test_context_compaction.py`](../../picoagents/tests/test_context_compaction.py)
and [`test_hooks.py`](../../picoagents/tests/test_hooks.py).

## 4.13 Humans in the loop

Read [`ApprovalMode` and `BaseTool`](../../picoagents/src/picoagents/tools/_base.py),
then [`ToolApprovalRequest`, `ToolApprovalResponse`, and `AgentContext`](../../picoagents/src/picoagents/context.py).
The behavior is specified by [`test_tool_approval.py`](../../picoagents/tests/test_tool_approval.py)
and demonstrated by [`approval_example.py`](../../examples/tools/approval_example.py).
Read the test before the live example: it makes suspension, response, and
resumption explicit.

## 4.14 Summary: one end-to-end trace

Use this order to trace a complete current agent:

```text
test_agent_basic.py
    → agents/_base.py
    → agents/_agent.py
    → llm/_base.py and llm/_openai.py
    → tools/_base.py
    → context.py and messages.py
```

Only after that trace should you run
[`examples/agents/basic-agent.py`](../../examples/agents/basic-agent.py) with a
live model. Add middleware, memory, context compaction, and approval one at a
time, using their tests as the definition of correct behavior.
