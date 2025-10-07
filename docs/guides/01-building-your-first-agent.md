---
title: Build your first Agent
---

# Build your first Agent

As an example, we will create our first agent that translates text, after we set up our environment. Let's do the following steps!

- Register your agent
```bash
genai register_agent --name translate_agent --description "This agent is a powerful bilingual translator"
```

- Set Up Environment

```bash
cd agents/my_cool_agent
uv venv && uv sync                   
# or: 
python3 -m venv venv
```

Here is our translator agent, it uses an openai key, remember to use your own key.

```python
import asyncio
import os
from typing import Any, Annotated

from genai_session.session import GenAISession
from openai import OpenAI

session = GenAISession()

OPENAPI_KEY = os.environ.get("OPENAPI_KEY")

openai_client = OpenAI(
    api_key=OPENAPI_KEY
)


@session.bind(name="get_translation", description="Translate the text into specified language")
async def get_translation(
        agent_context, text: Annotated[str, "Text to translate"],
        language: Annotated[str, "Code of the language to translate to (e.g. 'fr', 'es')"]
) -> dict[str, Any]:
    agent_context.logger.info("Inside get_translation")
    prompt = f"Translate the text into specified language {language}.\n\n{text}"

    response = openai_client.chat.completions.create(
        messages=[
            {
                "role": "user",
                "content": prompt
            }
        ],
        model="gpt-4o-mini"
    )
    translation = response.choices[0].message.content
    return {"translation": translation}


async def main():
    await session.process_events()


if __name__ == "__main__":
    asyncio.run(main())
```

- Run Agent

```bash
genai run_agents
```


## Agent Lifecycle

1. Create agent
2. Test locally  
3. Deploy as Cloud Agent  
4. Orchestrate with MCP Server  
5. Compose Agent Flows  
6. Publish or install via Marketplace  


---

## Agent Template Example

Most agents will follow this template:

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
