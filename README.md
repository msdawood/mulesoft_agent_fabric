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

**The conceptual shift: deterministic flows to probabilistic agent networks**

Our custom iPaaS is a deterministic system a flow config defines a fixed sequence with optional dynamic routing based component "output.checks" or what "Route" component returns. Validate -> Enrich -> Transform -> Route -> Operate. The VETRO chain always runs in a known order, with known adapters, producing known output shapes.
Agent Fabric orchestrator doesn't need to know the execution path upfront. The Agent broker receives intent, reasons about it using an LLM, then dynamically selects which registered agents and tools to invoke. But it requires governance which is where Flex Gateway and Agent Governance come in to compensate for the loss of determinism. 

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
5. **Java 17 (Temurin/Corretto):** Required to run Mule runtimes locally. 
6. **Install VS Code Extension Pack:** Add the *Anypoint* extension pack (contains 8 extensions) **after** the dependencies above are fully installed. This includes Anypoint Code Builder - the IDE where you can create, build, publish and deploy Agent Fabric projects.
7. **Anypoint CLI v4:** Command-line tool for publishing and deploying agent network assets. Install via npm: npm install -g Anypoint-cli-v4
8. **Authenticate Cloud Platform:** Connect ACB to an active Anypoint Platform account to map your project's `exchange.json` to an active Organization ID.

### Environment Verification Screenshots

*Place your environment setup screenshots below to reference working configurations:*

#### VS Code Extension & Profile Login

#### Live Terminal Check (`node -v` and `git --version`)

---

## 4. Agent Network Development

Agent Fabric development.

---

## 5. Publish Agent Network Assets

Publishing Agent Fabric to Exchange.

---

## 6. Setup Agent Network Gateways 

Setup Agent Network Gateways and Gotchas with trial accounts. How to setup correctly in Anypoint Console.

Using Command Palette

```
[INFO]	[PID: 96341]	[Setup agent network gateways]		Setting up agent network gateways...
[INFO]	[PID: 96341]	[Setup agent network gateways]		Setting agent-network-shared-gw in Cloudhub-EU-Central-1...
[INFO]	[PID: 96341]	[Setup agent network gateways]		    BaseError: {
[INFO]	[PID: 96341]	[Setup agent network gateways]		      "errorCode": 4002,
[INFO]	[PID: 96341]	[Setup agent network gateways]		      "errorMessage": "Error while trying to setup agent-network-shared-gw.",
[INFO]	[PID: 96341]	[Setup agent network gateways]		      "cause": {
[INFO]	[PID: 96341]	[Setup agent network gateways]		        "message": "Forbidden \n\n    ›   You don't have permission to access 
[INFO]	[PID: 96341]	[Setup agent network gateways]		    url: /gatewaymanager/api/v1/organizations/22e5c6cd-8fb7-
[INFO]	[PID: 96341]	[Setup agent network gateways]		    2dbf/environments/7bec7d86-97a6-40a0-b715-/gateways \n\n    › 
[INFO]	[PID: 96341]	[Setup agent network gateways]		      status 403 - Forbidden"
[INFO]	[PID: 96341]	[Setup agent network gateways]		      }
[INFO]	[PID: 96341]	[Setup agent network gateways]		    }
```
---

## 7. Deploy Agent Network 

Deploying Agent Network. Provide LLM Key


Using Command Palette
```
[INFO]	[PID: 27346]	[Deploy agent network project]		Using shared space 'Cloudhub-US-East-2' derived from gateway 'test-gateway'.
[INFO]	[PID: 27346]	[Deploy agent network project]		Deploying Agent Network project Transcript Summariser — Declarative version v1 (1.0.0).
[INFO]	[PID: 27346]	[Deploy agent network project]		Deployment for connection: '[LLM] openAiGpt' — version 1.0.0 starting...
[INFO]	[PID: 27346]	[Deploy agent network project]		name:    openaiConnection
[INFO]	[PID: 27346]	[Deploy agent network project]		version: 1.0.0
[INFO]	[PID: 27346]	[Deploy agent network project]		url:     http://test-gateway:8082/22e5c6cd-8fb7-40fa-a666-/openAiGpt/openaiConnection/
[INFO]	[PID: 27346]	[Deploy agent network project]		Deployment for connection: '[LLM] openAiGpt' — version 1.0.0 finished... ✅
[INFO]	[PID: 27346]	[Deploy agent network project]		Deployment for Agent Graph: 'test-agent-graph' — version 1.0.0 starting...
[INFO]	[PID: 27346]	[Deploy agent network project]		Waiting for Agent Graph deployment: 'test-agent-graph' to be ready...
[INFO]	[PID: 27346]	[Deploy agent network project]		Deployment for instance: '[Broker] transcript_summariser' — version 1.0.0 starting...
[INFO]	[PID: 27346]	[Deploy agent network project]		name:    transcript_summariser
[INFO]	[PID: 27346]	[Deploy agent network project]		version: 1.0.0
[INFO]	[PID: 27346]	[Deploy agent network project]		url:     https://test-gateway.usa-e2.cloudhub.io/transcript_summariser/
[INFO]	[PID: 27346]	[Deploy agent network project]		Deployment for instance: '[Broker] transcript_summariser' — version 1.0.0 finished... ✅
[INFO]	[PID: 27346]	[Deploy agent network project]		Deployment for Agent Graph: 'test-agent-graph' — version 1.0.0 finished... ✅
[INFO]	[PID: 27346]	[Deploy agent network project]		Agent network project 'Transcript Summariser — Declarative' was deployed.
```
---

## 8. Securing Agent 

How to secure Agent.

---

## 9. Testing Agent 

How to test the Agent.
Accessing A2A Card
https://test-gateway.usa-e2.cloudhub.io/transcript_summariser//.well-known/agent-card.json

```
{
  "name": "Transcript Summariser",
  "description": "Summarises any transcript using a declarative generator node.",
  "version": "1.0.0",
  "capabilities": {
    "streaming": false,
    "pushNotifications": false
  },
  "skills": [
    {
      "id": "summarise",
      "name": "Summarise Transcript",
      "description": "Summarises the given transcript.",
      "tags": [
        "summarisation"
      ]
    }
  ],
  "defaultInputModes": [
    "text/plain"
  ],
  "defaultOutputModes": [
    "text/plain"
  ]
}
```

Request
```
{
  "jsonrpc": "2.0",
  "id": "test-001",
  "method": "SendMessage",
  "params": {
    "message": {
      "messageId": "task-001",
      "role": "ROLE_USER",
      "parts": [
        {
          "text": "Summarise this: Today we discussed the quarterly results. Revenue was up 12%. The team highlighted three risks: supply chain delays, currency fluctuation, and hiring shortfalls. Action items: CFO to review hedging strategy, HR to accelerate recruitment."
        }
      ]
    }
  }
}
```

Response
```
{
    "result": {
        "task": {
            "id": "9072872d-2c0c-40f2-9491-78fe37585d95",
            "contextId": "5f05eb40-ed3f-4013-90ef-08e4979bad1a",
            "status": {
                "state": "TASK_STATE_COMPLETED",
                "message": {
                    "messageId": "6793d5a5-8763-4cda-84ab-34fc9a28f4eb",
                    "role": "ROLE_AGENT",
                    "parts": [
                        {
                            "text": "During today's meeting, the quarterly results were reviewed, revealing a 12% increase in revenue. However, the team identified three significant risks: supply chain delays, currency fluctuations, and hiring shortfalls. To address these issues, action items include having the CFO review the hedging strategy and HR accelerating recruitment efforts to mitigate risks moving forward. \n\nKey Takeaways:  \n- Quarterly revenue increased by 12%.  \n- Identified risks include supply chain delays.  \n- Currency fluctuations are a concern for future forecasts.  \n- Hiring shortfalls may impact operational capabilities.  \n- Specific action items assigned to the CFO and HR for risk management.  \n\nAction Items:  \n- CFO to review the hedging strategy.  \n- HR to accelerate recruitment processes."
                        }
                    ]
                },
                "timestamp": "2026-08-21T10:43:58.248125Z"
            },
            "history": [
                {
                    "messageId": "task-001",
                    "contextId": "5f05eb40-ed3f-4013-90ef-08e4979bad1a",
                    "taskId": "9072872d-2c0c-40f2-9491-78fe37585d95",
                    "role": "ROLE_USER",
                    "parts": [
                        {
                            "text": "Summarise this: Today we discussed the quarterly results. Revenue was up 12%. The team highlighted three risks: supply chain delays, currency fluctuation, and hiring shortfalls. Action items: CFO to review hedging strategy, HR to accelerate recruitment."
                        }
                    ]
                }
            ]
        }
    },
    "id": "test-001",
    "jsonrpc": "2.0"
}
```

---

## 10. Wrap up 


---
