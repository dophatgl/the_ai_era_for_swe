# 03. Context Engineering & Model Context Protocol (MCP)

> **Journey Stage**: Information Retrieval & Tool Standardisation  
> **Purpose**: Master how codebases, documentation, and external systems are indexed, filtered, and fed into LLMs to deliver accurate, hallucination-free code generation.

---

## 1. Core Keywords & Terminology

### Context Engineering
- **Context Window Management**: The deliberate selection, ranking, and compression of information supplied to an LLM to maximize relevance while remaining well below token limits.
- **Lost in the Middle Effect**: The empirical phenomenon where LLMs recall information placed at the very beginning or end of a prompt with high fidelity, while performance degrades on details buried in the middle of long contexts.
- **Context Compaction / Pruning**: Stripping unnecessary boilerplate, comments, or uncalled functions from the prompt to optimize signal-to-noise ratio.
- **Repo Map**: A condensed structural representation of a codebase (file trees, class hierarchies, public method signatures) generated via ASTs to give LLMs global awareness without loading all raw files.

### Model Context Protocol (MCP)
- **Model Context Protocol (MCP)**: An open standard created by Anthropic that standardizes how AI applications connect to external data sources and tools (similar to a USB-C standard for LLMs).
- **MCP Host**: The application initiating requests (e.g., Cursor, Claude Desktop, Antigravity IDE).
- **MCP Client**: The protocol connector maintaining a 1:1 connection with an MCP server.
- **MCP Server**: A lightweight program that exposes specific capabilities through three primitives:
  1. **Resources**: Read-only data files, database schemas, or logs.
  2. **Tools**: Executable functions (e.g., query database, execute git command).
  3. **Prompts**: Pre-configured prompt templates with embedded context.

### Code Indexing & Search
- **AST (Abstract Syntax Tree)**: A hierarchical tree representation of source code structure parsed by engines like `tree-sitter`, enabling language-aware semantic queries (e.g., finding all classes implementing an interface).
- **Lexical Search (BM25 / ripgrep)**: Fast, exact keyword and regex-based matching across files.
- **Semantic Code Search (Embeddings)**: Vectorizing code snippets based on conceptual meaning, allowing developers to query *"authentication middleware"* and match `verify_jwt_token.py` without exact string matches.
- **Hybrid Search**: Combining keyword search (exact identifier matches) with dense vector search (conceptual meaning) via Reciprocal Rank Fusion (RRF).

---

## 2. Key Concepts & Architecture

### The Context Pyramid

```
                ┌─────────────────────────┐
                │   Active File & Cursor  │  <-- Highest Attention / Priority
                ├─────────────────────────┤
                │   Direct Dependencies   │  <-- Imported Types & Signatures
                ├─────────────────────────┤
                │   Repo Map / AST Graph  │  <-- Structural Skeleton
                ├─────────────────────────┤
                │   Semantic Search (RAG) │  <-- Relevant Code Chunks
                ├─────────────────────────┤
                │   Project Rules & Spec  │  <-- .cursorrules / CLAUDE.md
                └─────────────────────────┘
```

### The Power of Project Rules
Modern AI tools support project-level instruction files that persist context across conversations:
- `.cursorrules` / `.cursor/rules/*.mdc` (Cursor)
- `CLAUDE.md` (Claude Code)
- `.windsurfrules` (Windsurf)
- Agent Custom Skills / Rules (Antigravity)

**Best Practice Content for Rule Files:**
- Build and test commands (e.g., `pnpm test`, `cargo check`)
- Coding style conventions (e.g., "Use functional components with hooks, never class components")
- File naming and project directory structure
- Forbidden patterns (e.g., "Never use `any` in TypeScript; never use raw SQL queries without parameterized bindings")

---

## 3. Practical Example: Anatomy of an MCP Integration

```
┌──────────────┐          JSON-RPC          ┌───────────────────────┐
│              │ <────────────────────────> │ MCP Server: Postgres  │
│              │     (list_tables, query)   └───────────────────────┘
│   AI Agent   │
│  (MCP Host)  │          JSON-RPC          ┌───────────────────────┐
│              │ <────────────────────────> │ MCP Server: GitHub    │
│              │   (create_pr, read_issue)  └───────────────────────┘
└──────────────┘
```

When an agent needs to debug a database migration:
1. It calls `mcp__postgres__list_tables` to inspect schemas.
2. It calls `mcp__postgres__describe_table` to get foreign keys.
3. It cross-references with local ORM definitions without hallucinating column names.

---

## 4. Journey Self-Reflection

- [ ] Does my repository have a clear rule file (`CLAUDE.md`, `.cursorrules`, etc.)?
- [ ] Have I tested an MCP server to connect my AI assistant to our local development database or issue tracker?
- [ ] Am I polluting prompts with huge file dumps when a targeted symbol search would be 10x more effective?
