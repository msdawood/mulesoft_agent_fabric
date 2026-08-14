# MuleSoft Agent Fabric: Architecture & Learning Notes

A comprehensive repository capturing architectural patterns, core concepts, and development workflows for **MuleSoft Agent Fabric**. This repo tracks my learning journey as I build multi-vendor AI agent networks on the Anypoint Platform.

---

## 1. Core Architecture Concepts

MuleSoft Agent Fabric splits AI implementation into a distinct **Control Plane** (cloud-managed governance) and a **Runtime Plane** (execution).

* **Agent Network:** The overall project wrapper (the domain boundary). It functions like a virtual business department (e.g., HR, Finance) and contains multiple specialized agents, brokers, LLM profiles, and data tools.
* **Agent Broker:** The "brain" and traffic controller of the network. It sits at the edge, parses natural language, manages the execution graph, and orchestrates tasks.
* **Model Context Protocol (MCP) Servers:** Open-standard bridges that translate abstract AI logic into concrete enterprise data system actions.

### API-Led Connectivity Mapping
Agent Fabric maps directly onto MuleSoft’s classic 3-tier API-Led Architecture:

**API layers**
| Your component | Agent Fabric equivalent | What matches | What's different / new |
| :--- | :--- | :--- | :--- |
|XAPI	|Agent Broker	|Both are the consumer-facing entry point. They receive requests and route them downstream.	|Broker uses LLM-based intent routing — no fixed rule config needed. It plans dynamically, not schema-driven.|
|PAPI	|Agent Broker + YAML agent-network	|Both orchestrate a flow across multiple services, coordinating step execution.	|Agent Fabric uses a declarative YAML spec rather than a programmatic flow config. The broker can re-plan at runtime.|
|SAPI	|AI Connectors / MCP tools	|Both abstract system-level connectivity — databases, APIs, cloud services.	|MCP tools expose capabilities as a protocol, not just endpoints. Any LLM can discover and call them without bespoke adapters.|

**Infrastructure and observability**
| Your component | Agent Fabric equivalent | What matches | What's different / new |
| :--- | :--- | :--- | :--- |
|AWS Serverless runtime |	Anypoint / CloudHub	| Both provide a managed runtime for flow/agent execution.	| Agent Fabric also supports hybrid deployment. Your AWS serverless agents can still be registered via Agent Scanners.|
|Custom logs + CloudWatch |	Agent Visualizer + Anypoint Monitoring |	Both capture latency, errors, and request traces. | Agent Visualizer adds a live visual map of agent interactions — not just logs, but topology and decision paths.|
|Flow config (JSON/YAML) |	agent-network.yaml |	Both use a declarative config to define what executes and in what order. | Agent Fabric's YAML is platform-agnostic. It decouples the agent definition from the runtime — any agent from any platform can be referenced.|

---

## 2. Orchestration & Guided Determinism

Rather than letting an LLM randomly guess the next step, Agent Fabric uses **Guided Determinism** via **AgentScript** (an open-source domain language shared with Salesforce Agentforce). 

* **Probabilistic Nodes (`|`):** Steps that utilize LLM reasoning to parse moods, extract messy text variables, or interpret intent.
* **Deterministic Nodes (`->`):** Rule-based logic loops, mathematical calculations, and strict API calls that execute under 100% predictable conditions.

---

## 3. Local Development Environment Setup

To develop Agent Networks locally using **Anypoint Code Builder (ACB)** inside VS Code, the local machine must be configured with explicit underlying technical dependencies.
Create a MuleSoft Anypoint Platform account.


### Step-by-Step Installation Order

1. **Install VS Code:** At least VS Code 1.80+ recommended.
2. **Install Git:** Required by ACB background scripts to clone asset templates from Anypoint Exchange.
3. **Install Node.js (v20+ LTS):** Vital to host the background `@mulesoft/mcp-server` that powers the **MuleSoft Vibes** AI development assistant.
4. **Verify System Paths:** Ensure both `git` and `npm` are accessible globally via your system environment variables.
5. **Install VS Code Extension Pack:** Add the *Anypoint* extension pack (contains 8 extensions) **after** the dependencies above are fully installed.
6. **Authenticate Cloud Platform:** Connect ACB to an active Anypoint Platform account to map your project's `exchange.json` to an active Organization ID.

### Environment Verification Screenshots

*Place your environment setup screenshots below to reference working configurations:*

#### VS Code Extension & Profile Login

#### Live Terminal Check (`node -v` and `git --version`)

---

## 4. Enterprise CI/CD & Multi-Environment Monitoring

Agent Fabric abstracts complex python container management by integrating natively into Java-based enterprise deployment lifecycles.

