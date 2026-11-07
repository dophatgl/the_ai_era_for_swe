# 01. AI-Assisted Coding & Pair Programming

> **Journey Stage**: Foundations & Daily Tooling  
> **Purpose**: Master the interactive tools and prompt techniques that augment daily code authoring, from inline autocomplete to conversational code generation.

---

## 1. Core Keywords & Terminology

### Autocompletion & Generation
- **Ghost Text (Inline Completion)**: Grayed-out predictive code suggestions displayed directly in the editor as you type, accepted with keys like `Tab`.
- **Next Edit Suggestions (NES)**: Predictive model output that anticipates not just the next token, but where your next edit in the file or project will occur (e.g., updating a caller function after editing a signature).
- **Prompt-Driven Development (PDD)**: A development methodology where code is iteratively requested, steered, and refined using structured prompts, specifications, and comments before manual edits are made.
- **In-line Diff / Chat-to-Code**: An interactive UI pattern allowing developers to highlight code, request a transformation in natural language, and preview a side-by-side or inline diff before accepting or rejecting changes.

### Model Mechanics in Coding
- **Context Window**: The maximum token capacity (input + output) a model can hold in active memory at one time. In coding, this limits how much of your codebase the model can simultaneously consider.
- **Speculative Decoding**: An inference optimization technique where a smaller, faster draft model proposes code tokens and a larger model verifies them in parallel, drastically speeding up code autocomplete.
- **Hallucinated APIs**: When a model invents non-existent methods, parameters, or library functions with high syntactic confidence.

---

## 2. Key Concepts & Mental Models

### The "Junior Pair Programmer" Mindset
Treat the AI not as an infallible senior architect, but as an extraordinarily fast, tireless junior developer who has read the entire internet but lacks deep context about your specific business rules:
- **Always verify**: Never commit code you do not understand.
- **Give crisp instructions**: Ambiguous specifications yield generic or incorrect code.
- **Small batch sizes**: Ask for one function, one test, or one refactoring step at a time rather than an entire system in one shot.

### Prompting Patterns for Software Engineers
1. **Role + Context + Constraints + Task**:
   > *"You are a senior TypeScript engineer. We use strict null checks and avoid `any`. Refactor the following function to handle network retry with exponential backoff."*
2. **Few-Shot Examples**:
   Provide 1-2 examples of your codebase's preferred styling, error handling, or schema patterns before asking for the new implementation.
3. **Draft-and-Critique**:
   Ask the model to generate a solution, then ask it: *"Review your implementation for edge cases, memory leaks, or race conditions and propose improvements."*

---

## 3. Tool Ecosystem

| Tool | Category | Key Strength |
| :--- | :--- | :--- |
| **Cursor** | AI-First IDE | Deep codebase indexing, multi-file edits, composer mode |
| **Windsurf** | AI-First IDE | Cascade agent flow, deep contextual awareness |
| **GitHub Copilot** | Extension | Ubiquitous across VS Code / JetBrains, enterprise compliance |
| **Supermaven** | Autocomplete | 1M+ token context window, near-instant autocomplete speed |
| **Continue.dev** | Open Source Extension | Connects any local or remote LLM (Ollama, Claude, OpenAI) to VS Code/JetBrains |
| **Claude Code / Aider** | Terminal Pair Programmer | High agency git-aware coding from CLI |

---

## 4. Practical Workflows

```
┌─────────────────┐     Prompt      ┌──────────────────┐
│  Define Spec /  │ ──────────────> │  AI Generates    │
│  Type Interface │                 │  Implementation  │
└─────────────────┘                 └────────┬─────────┘
         ▲                                   │
         │ Review & Iterate                  ▼
┌────────┴────────┐   Run Tests     ┌──────────────────┐
│ Manual Edits /  │ <────────────── │ Inspect Diff &   │
│ Syntax Checking │                 │ Run Local Checks │
└─────────────────┘                 └──────────────────┘
```

1. **Boilerplate & Scaffolding**: Generate repetitive CRUD operations, DTO mappings, regex patterns, and configuration files.
2. **Exploration & Syntax Translation**: Translate algorithms between languages (e.g., Python prototype to Go production service).
3. **Documentation & Docstrings**: Generate standard JSDoc/Sphinx/GoDoc comments from existing code signatures.

---

## 5. Journey Self-Reflection

- [ ] What percentage of my weekly code is generated vs. manually typed?
- [ ] Am I spending less time typing syntax and more time reviewing logic and design?
- [ ] What are the top 3 prompt mistakes that caused the model to produce incorrect code this week?
