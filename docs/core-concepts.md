---
title: Core Concepts
---

# Core Concepts

Understanding a few core concepts will help you unlock the full power of `agentic atlas`.

### Agent

An `Agent` is the fundamental building block. It's an autonomous entity that can perform tasks, communicate, and make decisions. Each agent runs in an isolated environment, can be assigned a unique ID, and is secured with a JWT token. You give it a name, a description (its "prompt" or "constitution"), and logic for how it handles messages.

### Agent Maker

The Agent Maker is the developer toolset for creating, configuring, and registering agents. It generates starter code, registers agent metadata, and assigns the JWT token required for authentication.

### Cloud Agents 
Cloud Agents are deployed to managed infrastructure for persistent, scalable execution. They can handle long-running processes, scale automatically, and integrate with external APIs or event streams.

Example: A fraud_detector cloud agent continuously analyzes transactions in real time and flags anomalies.

### Message

Agents communicate using `Message` objects. A message is a simple data structure that contains the content (text), the sender, and the recipient. Our protocol ensures these messages can be reliably sent between agents, whether they are running on the same machine or across the internet.

### Backend

The `Backend` is the engine that powers the agents. It manages the agent lifecycle, message queuing, and state. When you use the `run()` function, you are starting a simple, local backend. For more complex applications, you can run a persistent backend server that manages multiple agents.

### Protocol (A2A)

The Agent-to-Agent (A2A) protocol is the set of rules that governs how agents communicate. It defines how messages are formatted, addressed, and routed. You don't usually need to interact with the protocol directly, but it's what enables the powerful multi-agent systems you can build with `agentic atlas`.

### MCP

The MCP Server (Multi-Channel Processing Server) orchestrates agent execution and communication. It handles routing of messages, monitoring, logging, error handling, and resource management. Agents remain loosely coupled while working together as part of complex workflows.

Example: When a document_parser agent finishes extracting data, the MCP Server triggers a compliance_checker agent to validate it.

### Agent Flows

Agent Flows define the sequence and logic of multiple agents working together. They support sequential, parallel, and conditional execution, and can include automated or human-in-the-loop steps.
In Agentic Atlas we use drag and drop diagrams to make it easier for the user to create these flows.
We implement a central manager/orchestrator invokes specialized sub‑agents as tools and retains control of the conversation.

Example Flow: Speaking + Translation Agents. Imagine a flow with two agents: Speaker Agent – generates a message in English.Translator Agent – translates the message into Spanish.


### Marketplace

The Marketplace is a platform for discovering, publishing, and reusing agents and flows. It supports versioning, ratings, and metadata, making it easy to integrate community or team-built agents into your own workflows.

Example: A currency_converter agent published in the Marketplace can be integrated into multiple travel or finance workflows without rewriting logic.