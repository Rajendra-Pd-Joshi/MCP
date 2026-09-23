# MCP — Model Context Protocol from First Principles

> A first-principles learning repository for understanding **MCP (Model Context Protocol)** from the ground up — starting with **why**, then **what**, and finally **how**.

## Why this repository?

Large Language Models are powerful at generating and reasoning over information, but an LLM by itself does not automatically have access to the systems where useful information and actions live.

A real application may need an AI assistant to:

- read files
- query a database
- call an API
- inspect project information
- retrieve knowledge from external systems
- perform controlled actions in software tools

A common early approach is to build a separate custom integration for every model and every application.

That creates an integration problem:

```
Model A ──> Tool 1
Model A ──> Tool 2
Model A ──> Tool 3

Model B ──> Tool 1
Model B ──> Tool 2
Model B ──> Tool 3
```

As the number of models, clients, and tools grows, the number of connections grows with it.

MCP introduces a common protocol for connecting AI applications to external **tools, resources, and prompts**.

The goal of this repository is not to memorize an MCP framework or copy a template. The goal is to understand the underlying problem deeply enough that the implementation makes sense.

---

## What is MCP?

**MCP stands for Model Context Protocol.**

At a high level, MCP is a standardized protocol that allows an **MCP host/client** to communicate with an **MCP server** that exposes capabilities and context to an AI application.

A simplified mental model is:

```
┌──────────────────────┐
│      AI Host         │
│  Chat app / Agent    │
└──────────┬───────────┘
           │
           │ MCP
           ▼
┌──────────────────────┐
│     MCP Server       │
│                      │
│  Tools               │
│  Resources            │
│  Prompts             │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ External Systems     │
│ Files / DB / APIs    │
│ Services / Apps      │
└──────────────────────┘
```

The important idea is that the model does not need a completely different integration architecture for every external system. The MCP layer provides a common way to describe and interact with capabilities.

---

## Learning philosophy

This repository follows a **first-principles approach**.

Instead of starting with:

> "Here is an MCP server. Copy this code."

we start with:

> "What problem are we trying to solve?"

Then:

1. **Why** do AI applications need a protocol like MCP?
2. **What** exactly is being standardized?
3. **Who** are the participants?
4. **How** does communication happen?
5. **How** are tools, resources, and prompts represented?
6. **How** do we build an MCP server?
7. **How** does a client discover and call capabilities?
8. **How** does MCP fit into agentic AI systems?
9. **How** should MCP systems be secured and deployed?

---

# Course Roadmap

## 01 — The Problem Before MCP

Start with the problem rather than the protocol.

Topics:

- What an LLM can and cannot do by itself
- The difference between reasoning and access
- Why external tools are required
- Function calling and tool calling
- The integration problem
- Why direct model-to-every-tool integrations become difficult to scale
- Separation between an AI application and the systems it needs to access

### Core question

**Why do we need a protocol at all?**

---

## 02 — From Function Calling to MCP

Before MCP, many systems already supported function/tool calling.

This section builds the bridge:

```
LLM
 ↓
Function Calling
 ↓
Tool Execution
 ↓
External System
```

Then asks:

**What happens when many different AI clients need many different tools?**

Topics:

- Tool schemas
- Input validation
- Tool discovery
- Tool execution
- Tool result handling
- Integration fragmentation
- Standardization

---

## 03 — MCP Architecture

Understand the roles involved in an MCP system.

Topics:

- MCP Host
- MCP Client
- MCP Server
- Server-side capabilities
- Connection lifecycle
- Requests and responses
- Notifications
- Separation of responsibilities

Conceptually:

```
              ┌─────────────────────┐
              │        Host         │
              │  AI application     │
              └──────────┬──────────┘
                         │
                    MCP Client
                         │
                    MCP protocol
                         │
                         ▼
              ┌─────────────────────┐
              │     MCP Server      │
              │                     │
              │  Tools              │
              │  Resources          │
              │  Prompts            │
              └─────────────────────┘
```

---

## 04 — Understanding the Protocol

Now move from architecture to protocol mechanics.

Topics:

- Protocol messages
- Request / response model
- Message identifiers
- Method names
- Parameters
- Results
- Errors
- Notifications
- Capability negotiation
- Initialization and lifecycle

The objective is to understand what is happening **on the wire**, not just what a library hides.

---

## 05 — Transport and Connections

Explore how MCP communication is carried between participants.

Topics:

- Transport concept
- Local process communication
- Standard input / output based connections
- Streamable HTTP
- Connection lifecycle
- When local and remote MCP servers make sense

The focus is understanding the difference between:

```
Protocol
   ≠
Transport
```

MCP defines how participants communicate conceptually; the transport determines how those messages move between processes or systems.

---

## 06 — MCP Primitives

Understand the major capability types exposed by MCP servers.

### Tools

Tools represent actions that an AI application can invoke.

Examples:

- Search a database
- Call an API
- Create a record
- Run a calculation
- Search a codebase

### Resources

Resources represent contextual information that can be read or retrieved.

Examples:

- Documents
- Files
- Database information
- Application state
- Other structured context

### Prompts

Prompts provide reusable prompt templates or interaction patterns.

The key lesson is understanding the difference between:

```
Tool     → "Do something"
Resource → "Give me context"
Prompt   → "Use this interaction template"
```

---

## 07 — Build Your First MCP Server

Move from theory to implementation.

A minimal learning progression should look like:

```
hello_world_server
        ↓
simple_tool
        ↓
tool_with_arguments
        ↓
multiple_tools
        ↓
resources
        ↓
prompts
```

Possible examples:

- `add(a, b)`
- `get_current_time()`
- `search_notes(query)`
- `read_file(path)`

The code should stay intentionally small so the protocol behavior remains visible.

---

## 08 — Build an MCP Client

Understand the other side of the connection.

Topics:

- Connecting to a server
- Initialization
- Discovering capabilities
- Listing tools
- Reading resources
- Calling tools
- Handling results
- Handling errors

Conceptually:

```
Client
  │
  ├── initialize
  │
  ├── list tools
  │
  ├── inspect schema
  │
  └── call tool
          │
          ▼
       Server
```

---

## 09 — MCP + LLM

Once the protocol is understood independently, connect it to an LLM.

A useful conceptual pipeline is:

```
User
 ↓
LLM
 ↓
Decides a tool is needed
 ↓
MCP Client
 ↓
MCP Server
 ↓
External System
 ↓
Tool Result
 ↓
LLM
 ↓
Final Response
```

Topics:

- Tool selection by the model
- Tool schemas and model reasoning
- Tool results
- Multi-step tool use
- Agent loops
- Failure handling
- Context boundaries

---

## 10 — MCP + Agentic AI

Connect MCP to the broader agent architecture.

Topics:

- Agents
- Tool-using agents
- Planning and execution
- State
- Memory
- MCP as a capability layer
- MCP with LangChain
- MCP with LangGraph
- Multi-tool agents
- Human-in-the-loop workflows

A useful conceptual separation:

```
Agent
  ↓
Reasoning / Planning
  ↓
MCP Client
  ↓
Capability
  ↓
MCP Server
  ↓
External World
```

---

## 11 — Designing Good MCP Tools

Having an MCP server that works is different from having a well-designed MCP server.

Topics:

- Small and focused tools
- Clear tool descriptions
- Explicit input schemas
- Predictable outputs
- Validation
- Idempotency where appropriate
- Error messages
- Safe defaults
- Tool naming
- Avoiding unnecessarily broad tools

Compare:

```
run_everything(data)
```

with:

```
search_customer(customer_id)
update_customer_email(customer_id, email)
create_invoice(customer_id, items)
```

The second style exposes clearer capabilities and boundaries.

---

## 12 — Security

MCP systems can connect AI applications to real systems, so security is a fundamental part of the architecture.

Topics:

- Authentication
- Authorization
- Least privilege
- Input validation
- Sensitive information
- Secrets management
- Tool abuse
- Prompt injection
- Untrusted tool outputs
- Sandboxing
- Audit logging
- Permission boundaries

Important principle:

> A tool being callable by an AI system does not mean it should be unrestricted.

---

## 13 — Debugging MCP

Learn to understand failures systematically.

Topics:

- Connection failures
- Initialization failures
- Invalid tool arguments
- Schema mismatches
- Transport problems
- Server errors
- Timeouts
- Authentication problems
- Unexpected tool results
- Logging and observability

Debug from the bottom upward:

```
Network / Transport
        ↓
Protocol
        ↓
Client
        ↓
Server
        ↓
Tool
        ↓
External System
```

---

## 14 — Production MCP

Move from educational examples toward real applications.

Topics:

- Project structure
- Configuration
- Environment variables
- Secrets
- Logging
- Monitoring
- Deployment
- Remote servers
- Scaling
- Reliability
- Versioning
- Backward compatibility
- Operational concerns

---

# Suggested Repository Structure

As the course grows, the repository can evolve toward:

```
MCP/
│
├── README.md
│
├── 01-why-mcp/
│   ├── notes/
│   └── examples/
│
├── 02-function-calling/
│   ├── notes/
│   └── examples/
│
├── 03-mcp-architecture/
│   ├── notes/
│   └── diagrams/
│
├── 04-protocol/
│   ├── notes/
│   └── examples/
│
├── 05-transports/
│   └── examples/
│
├── 06-primitives/
│   ├── tools/
│   ├── resources/
│   └── prompts/
│
├── 07-mcp-server/
│   └── examples/
│
├── 08-mcp-client/
│   └── examples/
│
├── 09-llm-integration/
│   └── examples/
│
├── 10-agentic-ai/
│   └── examples/
│
├── 11-security/
│   └── examples/
│
└── 12-production/
    └── examples/
```

The exact structure can change as implementations are added. The important part is keeping the learning sequence visible.

---

# Core Mental Model

Keep this model in mind throughout the repository:

```
                    AI Application
                          │
                          ▼
                    MCP Client
                          │
                     MCP Protocol
                          │
                          ▼
                    MCP Server
                    /    |    \
                   /     |     \
               Tools  Resources  Prompts
                  │       │         │
                  └───────┴─────────┘
                          │
                          ▼
                  External Systems
```

MCP is fundamentally about creating a standardized boundary between an AI application and the capabilities/context provided by external systems.

---

# MCP Is Not...

MCP should not be confused with several related concepts.

### MCP ≠ LLM

The LLM provides reasoning/generation capabilities.

### MCP ≠ Agent

An agent is an application pattern that can use reasoning, state, tools, and workflows. MCP can provide the capability interface an agent uses.

### MCP ≠ Tool

A tool is one capability exposed through MCP.

### MCP ≠ Database

A database can be an external system behind an MCP server.

### MCP ≠ Framework

MCP is a protocol. Frameworks and SDKs can make implementing that protocol easier.

### MCP ≠ Transport

A transport is how protocol messages are moved between participants.

---

# Learning Goal

By the end of this repository, the learner should be able to explain MCP without relying on memorized definitions.

You should be able to answer:

- Why is MCP needed?
- What problem does it solve?
- What are the participants?
- What is an MCP host?
- What is an MCP client?
- What is an MCP server?
- What are tools, resources, and prompts?
- How does discovery work?
- How is a tool called?
- What happens when a tool fails?
- How does an LLM fit into the architecture?
- How does MCP relate to agents?
- What are the main security boundaries?
- How would you build an MCP server from scratch?

---

# Hands-On Principle

Every major concept should eventually be demonstrated with code.

The preferred teaching cycle is:

```
Problem
  ↓
Concept
  ↓
Small Example
  ↓
Protocol-Level View
  ↓
Implementation
  ↓
Experiment
  ↓
Real-World Use Case
```

This avoids treating MCP as a black box.

---

# Resources

Official specification and documentation:

- [Model Context Protocol](https://modelcontextprotocol.io/)
- [MCP Specification](https://modelcontextprotocol.io/specification)
- [MCP Documentation](https://modelcontextprotocol.io/docs)
- [MCP GitHub Organization](https://github.com/modelcontextprotocol)

---

# Status

🚧 **Learning repository in progress**

The repository is being developed incrementally, beginning from the fundamentals and moving toward real-world MCP and agentic AI applications.

---

## Author

**Rajendra Prasad Joshi**

Learning and building AI systems from first principles.
