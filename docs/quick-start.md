---
title: Quick Start
---

# Quick Start: Your First Agent in 5 Minutes

This guide will get you from zero to a running AI agent in just a few minutes. We'll create a simple "Echo Agent" that replies with whatever you send it. This is the "Hello, World!" of agent building.
 
## TL;DR (Quick Commands)

```bash
# 1. Install
pip install genai-cli

# 2. Authenticate
genai signup -u <new_username>       # first-time users
genai login -u <your_username> -p <your_password>

# 3. Create Agent
genai register_agent --name my_cool_agent --description "This agent does something awesome"

# 4. Set Up Environment
cd agents/my_cool_agent
uv venv && uv sync                   # or: python3 -m venv venv

# 5. Run Agent
genai run_agents
# or
python my_cool_agent.py
```

---

## Step 1: Install GenAI CLI

```bash
pip install genai-cli
```

---

## Step 2: Authenticate

If you already have a GenAI account:  

```bash
genai login -u <your_username> -p <your_password>
```

For first-time users:  

```bash
genai signup -u <new_username>
```

Your JWT token will be saved in `~/.genai/credentials` and used for agent-related operations.  

> **Note:** You must be logged in to create or run agents.  

---

## Step 3: Create an Agent

Register a new agent:  

```bash
genai register_agent --name my_cool_agent --description "This agent does something awesome"
```

This will:
- Register metadata in the backend  
- Create a Python file in `agents/`  
- Assign a JWT to the agent (stored in the file)  

> **Do not modify the JWT** — it is required to validate your agent.  

---

## Step 4: Set Up the Agent Environment

1. Move into the agent directory:  

   ```bash
   cd agents/my_cool_agent
   ```

2. Create and sync a virtual environment:  

   ```bash
   uv venv
   uv sync
   ```

   Alternatively:  

   ```bash
   python3 -m venv venv
   ```

---

## Step 5: Run Your Agent

Agents are isolated Python files with their own dependencies.  
To run all agents:  

```bash
genai run_agents
```

Or run a single agent directly:  

```bash
python my_cool_agent.py
```

> If no virtual environment is found, GenAI will fallback to the parent folder or return an error.  

---

## Step 6 (Optional): Register an Agent via API

You can also register agents manually using the API.  

1. Open the docs at `http://localhost:8000/docs`.  
2. Log in or sign up.  
3. Use the `/api/agents/register` endpoint.  

Example response:  

```json
{
  "id": "uuid-here",
  "name": "my_cool_agent",
  "description": "does something great",
  "jwt": "your-agent-jwt"
}
```

4. Add the JWT to your agent, each agent should have its own key:  

   ```python
   session = GenAISession(jwt_token="your-agent-jwt")
   ```

5. Generate the agent file:  

   ```bash
   genai generate_agent --id uuid-here
   ```

---

## Step 7: Agent Template Example

A typical agent file looks like this:

```python
import asyncio
from typing import Annotated
from genai_session.session import GenAISession
from genai_session.utils.context import GenAIContext

AGENT_JWT = "your-agent-jwt"
session = GenAISession(jwt_token=AGENT_JWT)

@session.bind(name="my_cool_agent", description="does something great")
async def my_cool_agent(
    agent_context: GenAIContext,
    test_arg: Annotated[str, "Test argument"]
):
    return "Hello, World!"

async def main():
    print(f"Agent with token '{AGENT_JWT}' started")
    await session.process_events()

if __name__ == "__main__":
    asyncio.run(main())
```

---

You’re ready! Your agent can now be developed, customized, and run inside its own environment.  
