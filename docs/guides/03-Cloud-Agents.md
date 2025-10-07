# Cloud Agents

## Overview

A **Cloud Agent** is an agent deployed to the Agentic Atlas cloud infrastructure by importing or uploading your local agent project or creating your own agent. In this workflow you either use **Agent Maker** (prompt-based scaffolding) or **Upload a ZIP** of your agent — that’s it. The platform builds and runs the agent in the cloud for you.

Cloud agents are ideal for:
- Quick deployments of local agents to a managed runtime  
- Production or shared team agents that need uptime and scalability  
- Running agents that require cloud compute (GPUs, larger memory, persistent networking)

---

## Key Concepts

- **Import & Upload:** You can import your local agent directly into the platform and deploy it. No complex containerization required.
- **Agent Maker:** Use a natural-language prompt to auto-generate an agent scaffold — great for prototyping or generating an agent from a brief description.
- **Upload ZIP:** Zip your agent project (code, requirements, and metadata) and upload it to the platform; the platform extracts, builds, and runs it.
- **Secrets & Env:** Each cloud agent still uses an `AGENT_JWT_TOKEN` for authentication. For cloud deployments, if you are using the agent maker it will be automatically generated so there's no need to upload an .env file.


---

## Quick Steps — Two Ways to Deploy

### Option A — Agent Maker (Prompt → Agent)

1. Go to **Dashboard → Agent Maker**.  
2. Enter a clear prompt describing the agent’s goal, capabilities, and any required prompts or integrations (examples of desired behavior, APIs to call, etc.).  
3. Review and edit the generated scaffold if needed (the platform gives you a preview/editor).  
4. Configure runtime settings (Python version, resource tier)  
5. Deploy. The platform builds and runs the agent in its cloud infra, it will register and install the agent. Monitor logs and health from the agent can be turned on too via prompt.

### Option B — Upload a ZIP (Your Local Agent → Cloud)

1. Prepare your local agent directory. Typical files:
   - `agent.py` (or your entrypoint)  
   - `requirements.txt`  
   - `Procfile` or `start.sh` (optional; specify start command)  
   - `agent.yaml` or `manifest.json` (optional metadata: name, entrypoint, runtime)  
   - `.env` (local only — **remove secrets before zipping**)  
2. Zip the folder:

```bash
zip -r my-agent.zip my-agent-folder/
```
3. In the platform dashboard: **Dashboard → Agents → Upload Agent → Choose ZIP**, then upload `my-agent.zip`.  
4. Configure runtime (instance type, region, scaling) and deploy. The platform will run your agent in the cloud infra and surface logs, metrics, and health checks.

---

## Recommended Practices

- **Secrets:** Use the platform secret manager for `AGENT_JWT_TOKEN` and other credentials.  
- **No secrets in ZIPs:** Remove `.env` or any files containing secrets before creating the ZIP.  
- **Single responsibility:** Keep one agent per upload for simpler versioning and monitoring.  
- **Logs & Monitoring:** Connect platform logs to your observability tools or use the built-in dashboard.  
- **Versioning:** Tag uploads or use the platform’s versioning to enable rollbacks.  
- **Testing:** Test locally with a local router and your `AGENT_JWT_TOKEN` before uploading.

---

## Minimal Example Manifest (optional)

You can include a short `agent.yaml` to hint entrypoint/runtime to the platform:

```yaml
name: my-cloud-agent
entrypoint: agent.py
runtime: python:3.11
start_cmd: python agent.py
```

---

## Example Workflow (Zip Upload)

```mermaid
sequenceDiagram
    participant Dev
    participant Platform
    participant CloudAgentRuntime
    participant Router

    Dev->>Platform: Upload my-agent.zip (or use Agent Maker with a prompt)
    Platform->>Platform: Build and configure runtime
    Platform-->>CloudAgentRuntime: Start agent with assigned AGENT_JWT_TOKEN (or user token)
    CloudAgentRuntime->>Router: Connect using AGENT_JWT_TOKEN
    Router->>Platform: Validate token & register agent
    Platform-->>Dev: Deploy complete; logs & health available in dashboard
```

---

## TL;DR

- To deploy a cloud agent, **import or upload** your local agent to the platform.  
- Use **Agent Maker** for prompt-based scaffolding or **Upload ZIP** for your existing project.  
- That’s it — the platform handles building, running, and monitoring the agent in the cloud.
