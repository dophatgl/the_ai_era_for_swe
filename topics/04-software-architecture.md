# 04. Software Architecture & Agent-Friendly Design

> **Journey Stage**: System Design & Architecture  
> **Purpose**: Understand how software design patterns and architectural decisions must adapt so that human engineers and autonomous agents can collaborate seamlessly and reliably.

---

## 1. Core Keywords & Terminology

### Agent Ergonomics
- **Agent-Friendly Codebase**: A codebase structured to minimize ambiguity, optimize searchability for automated tools, and provide immediate deterministic feedback upon changes.
- **Cognitive Scope / Chunkability**: Organizing code into small, self-contained modules whose full context can fit comfortably inside an LLM's active attention window (typically < 300 lines per module).
- **Explicit Schema Enforcement**: Using declarative data schemas (e.g., Pydantic, Zod, Protobuf, JSON Schema) rather than untyped dictionaries or objects, giving agents unambiguous validation boundaries.
- **Contract-First Design**: Defining API specifications (e.g., OpenAPI / Swagger, gRPC proto files) before writing implementations. Agents excel when coding against strict contracts.

### Architectural Patterns
- **Hexagonal Architecture (Ports and Adapters)**: Decoupling core business logic from databases, UI, and external AI services. This makes it trivial for an agent to mock dependencies and write unit tests.
- **Declarative vs. Imperative Scaffolding**: Expressing infrastructure, database schemas, and workflows declaratively (e.g., Terraform, SQL migrations, Kubernetes manifests) so agents can generate and validate plans deterministically.
- **Deterministic Boundaries**: Isolating non-deterministic AI features (LLM calls) behind strict, deterministic runtime validation pipelines with retry and fallback mechanics.

---

## 2. Key Principles for Agent-Friendly Architectures

### 1. Strict Typing Eliminates 80% of Hallucinations
Untyped dynamic languages (plain JavaScript, Python without type hints) force the LLM to guess property names and return types:
```typescript
// ❌ Poor for Agents: Ambiguous parameters and returns
function processUser(data) {
  return data.items.map(x => x.val * 2);
}

// ✅ Agent-Friendly: Unambiguous interfaces and types
interface UserPayload {
  items: Array<{ id: string; value: number }>;
}

function processUser(payload: UserPayload): number[] {
  return payload.items.map(item => item.value * 2);
}
```

### 2. High Cohesion & Single Responsibility
When a single file spans 3,000 lines and handles authentication, billing, database writes, and email notifications:
- The agent consumes massive tokens just to read it.
- Edits in one section accidentally break another section.
- Smaller files with clear imports enable precision diffs.

### 3. Self-Documenting Directory Structure
Organize directories by feature domain rather than purely technical layer when possible:
```
src/
├── features/
│   ├── auth/
│   │   ├── auth.service.ts
│   │   ├── auth.controller.ts
│   │   ├── auth.schema.ts
│   │   └── auth.spec.ts
│   └── billing/
│       ├── billing.service.ts
│       └── billing.spec.ts
```
An agent working on `auth` can pinpoint everything it needs within a single subfolder.

---

## 3. Monorepo vs. Polyrepo in the AI Era

| Dimension | Monorepo (e.g., Turborepo, Nx) | Polyrepo (Multi-repo) |
| :--- | :--- | :--- |
| **Agent Cross-Boundary Awareness** | **High**: Agent can read API client and backend service in the same workspace | **Low**: Agent often lacks visibility into external repo schemas |
| **Refactoring Ease** | High: Multi-file refactors across frontend and backend in one prompt | Requires cross-repo coordination and multiple PRs |
| **Context Window Load** | Higher risk of noise; requires good repo mapping | Lower noise per repository, but isolated |
| **Verdict for AI Era** | **Preferred** for small-to-medium teams building end-to-end features with agents | Viable with shared schema packages / MCP servers |

---

## 4. Designing Systems with Embedded AI Components

When building applications that themselves call LLMs:

```
┌──────────────────┐
│   User Request   │
└────────┬─────────┘
         ▼
┌──────────────────┐
│ Input Sanitizer  │ <── Regex, Guardrails, Schema Validator
└────────┬─────────┘
         ▼
┌──────────────────┐
│ LLM Engine Call  │ <── Non-deterministic generation
└────────┬─────────┘
         ▼
┌──────────────────┐
│ Output Validator │ <── Pydantic / Zod parse (Retry on schema error)
└────────┬─────────┘
         ▼
┌──────────────────┐
│  Core App Logic  │ <── Pure deterministic business code
└──────────────────┘
```

---

## 5. Journey Self-Reflection

- [ ] Does my codebase have strong typing enabled across all new modules?
- [ ] Are functions and files small enough to fit comfortably in an LLM context window?
- [ ] Would an AI agent reading my repository structure be able to find where a specific API route is implemented in under 10 seconds?
