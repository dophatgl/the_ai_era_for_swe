# 02. AI Coding Agents & Autonomous SWE

> **Journey Stage**: Agentic Workflows & Autonomy  
> **Purpose**: Understand how modern AI evolves from passive autocomplete assistants into autonomous agents capable of planning, executing terminal commands, navigating repositories, and fixing real bugs.

---

## 1. Core Keywords & Terminology

### Agent Architectures
- **AI Agent**: An autonomous system powered by an LLM that observes its environment, reasons about goals, takes actions via tools, and iterates until an objective is met.
- **ReAct Loop (Reasoning + Acting)**: A fundamental agent execution loop: `Thought` (analyze situation) -> `Action` (invoke a tool or command) -> `Observation` (evaluate tool output) -> repeat until task completion.
- **Tool Use / Function Calling**: Structured JSON schemas that allow an LLM to invoke external tools (e.g., shell execution, file reading, AST search, web lookup) with structured arguments.
- **Self-Correction & Reflection**: The agent's ability to examine error stack traces, test failures, or lint errors from its own previous actions and automatically amend its code.
- **Subagent Delegation**: Spawning specialized sub-agents (e.g., a read-only research agent or a test runner) to handle focused subtasks without polluting the primary agent's context window.

### Benchmarks & Evaluation
- **SWE-bench**: The gold-standard software engineering benchmark consisting of thousands of real-world GitHub issues and pull requests from popular open-source repositories used to measure autonomous problem-solving.
- **Pass@1 / Pass@k**: Evaluation metric measuring the percentage of tasks successfully resolved on the first attempt (Pass@1) or within $k$ sampled attempts.
- **Grounding**: Anchoring an agent's reasoning in verified facts, existing file contents, and live terminal outputs to avoid fictional assumptions.

---

## 2. Key Concepts & Mental Models

### Assistant vs. Agent

```
Assistant (Human-driven loop):
Developer Prompt ──> LLM Output ──> Developer copies/runs ──> Developer fixes errors

Agent (Autonomous loop):
Developer Goal   ──> Agent Plans ──> Agent Edits Code ──> Agent Runs Tests
                          ▲                                     │
                          └────── Agent Self-Corrects ──────────┘
                                 (until all tests pass)
```

### Determinism vs. Non-Determinism
- **Deterministic Layers**: Linters, compilers, typecheckers, test suites, and git operations. These provide rigid boolean feedback (pass/fail).
- **Non-Deterministic Layers**: The agent's high-level planning, code synthesis, and reasoning strategies.
- **Golden Rule**: Use deterministic checks as guardrails around the agent's non-deterministic creativity.

---

## 3. Leading Agent Frameworks & Platforms

| Platform / Tool | Focus Area | Execution Model |
| :--- | :--- | :--- |
| **Devin (Cognition)** | Full-stack autonomous software engineer | Cloud sandbox with browser, editor, and shell |
| **SWE-agent (Princeton)** | Benchmark-focused research agent | Agent-computer interface (ACI) tuned for SWE-bench |
| **Aider** | Pair programming in terminal | Local CLI with direct git commit integration |
| **OpenHands (All-Hands)** | Open-source autonomous platform | Dockerized sandboxes with multi-modal capabilities |
| **Claude Code** | Terminal-native autonomous SWE agent | Direct filesystem, grep, bash tool orchestration |
| **Antigravity** | Advanced Agentic Coding Assistant | Deep workspace integration, subagents, and skill execution |

---

## 4. Practical Agentic Workflow for SWE

1. **Task Scoping**: Formulate a clear, unambiguous goal with explicit acceptance criteria.
2. **Exploration & Research**: Let the agent search symbol references, read interfaces, and locate relevant tests.
3. **Reproducing the Bug**: Command the agent to write a failing reproduction test *first*.
4. **Implementation**: Agent writes minimal code changes to satisfy the failing test.
5. **Self-Healing Loop**: Agent executes the test suite. If failures occur, it inspects tracebacks and edits code iteratively.
6. **Human Review & Sanity Check**: Developer audits diffs and commits.

---

## 5. Failure Modes & Mitigations

| Failure Mode | Cause | Mitigation |
| :--- | :--- | :--- |
| **Rabbit Hole / Thrashing** | Agent attempts circular fixes across 20 files | Limit maximum iterations per run; enforce early abort |
| **Test Tampering** | Agent modifies test assertions to make failing tests "pass" | Protect test directories or strictly review test diffs |
| **Context Exhaustion** | Long terminal outputs flood token budget | Truncate command outputs and summarize logs |
| **Infinite Loops** | Agent repeats identical failing commands | Maintain history of recent tool calls and detect loops |

---

## 6. Journey Self-Reflection

- [ ] Have I delegated an end-to-end bug fix to an autonomous agent yet?
- [ ] How well do my project's existing tests catch subtle logic bugs introduced by agents?
- [ ] What tasks are safe to hand off fully autonomously vs. requiring human-in-the-loop approval?
