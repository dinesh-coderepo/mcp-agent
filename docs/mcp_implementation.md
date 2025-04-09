# MCP Implementation in Python

This document provides an in-depth explanation of the MCP (Model Context Protocol) implementation in Python, including examples and usage instructions.

## Overview

The MCP implementation in Python is spread across multiple files, including `src/mcp_agent/agents/agent.py`, `src/mcp_agent/app.py`, and `src/mcp_agent/mcp/mcp_aggregator.py`. Each of these files contains classes and methods that are essential for the functioning of the MCP agent.

## Agent Class

The `Agent` class is defined in `src/mcp_agent/agents/agent.py`. It represents an entity that interacts with MCP servers. The class includes methods for initialization, attaching LLMs, and requesting human input.

### Initialization

The `Agent` class is initialized with a name, instruction, server names, functions, connection persistence, human input callback, and context. The initialization method sets up the agent with these parameters.

```python
def __init__(
    self,
    name: str,
    instruction: str | Callable[[Dict], str] = "You are a helpful agent.",
    server_names: List[str] = None,
    functions: List[Callable] = None,
    connection_persistence: bool = True,
    human_input_callback: HumanInputCallback = None,
    context: Optional["Context"] = None,
    **kwargs,
):
    # Initialization code
```

### Attaching LLMs

The `attach_llm` method creates an LLM instance for the agent. It takes a callable that constructs an `AugmentedLLM` or its subclass.

```python
async def attach_llm(self, llm_factory: Callable[..., LLM]) -> LLM:
    # Method code
```

### Requesting Human Input

The `request_human_input` method requests input from a human user and pauses the workflow until input is received.

```python
async def request_human_input(
    self,
    request: HumanInputRequest,
) -> str:
    # Method code
```

## MCPApp Class

The `MCPApp` class is defined in `src/mcp_agent/app.py`. It manages global state and can host workflows. The class includes methods for initialization, cleanup, and running the application.

### Initialization

The `initialize` method initializes the application.

```python
async def initialize(self):
    # Method code
```

### Cleanup

The `cleanup` method cleans up application resources.

```python
async def cleanup(self):
    # Method code
```

### Running the Application

The `run` method runs the application using a context manager.

```python
@asynccontextmanager
async def run(self):
    # Method code
```

## MCPAggregator Class

The `MCPAggregator` class is defined in `src/mcp_agent/mcp/mcp_aggregator.py`. It aggregates multiple MCP servers and provides methods to list tools, call tools, and list prompts.

### Listing Tools

The `list_tools` method lists all tools available to the agent, optionally filtered by server name.

```python
async def list_tools(self, server_name: str | None = None) -> ListToolsResult:
    # Method code
```

### Calling Tools

The `call_tool` method calls a namespaced tool with the provided arguments.

```python
async def call_tool(
    self, name: str, arguments: dict | None = None
) -> CallToolResult:
    # Method code
```

### Listing Prompts

The `list_prompts` method lists all prompts available to the agent, optionally filtered by server name.

```python
async def list_prompts(self, server_name: str | None = None) -> ListPromptsResult:
    # Method code
```

## Examples and Usage Instructions

### Example 1: Basic Agent Initialization

This example demonstrates how to initialize an `Agent` with a name, instruction, and server names.

```python
from mcp_agent.agents.agent import Agent

agent = Agent(
    name="example_agent",
    instruction="You are a helpful agent.",
    server_names=["server1", "server2"]
)
```

### Example 2: Attaching an LLM to an Agent

This example demonstrates how to attach an LLM to an `Agent`.

```python
from mcp_agent.workflows.llm.augmented_llm_openai import OpenAIAugmentedLLM

llm = await agent.attach_llm(OpenAIAugmentedLLM)
```

### Example 3: Requesting Human Input

This example demonstrates how to request human input from an `Agent`.

```python
from mcp_agent.human_input.types import HumanInputRequest

request = HumanInputRequest(prompt="Please provide input:")
response = await agent.request_human_input(request)
```

### Example 4: Running the MCPApp

This example demonstrates how to run the `MCPApp` using a context manager.

```python
from mcp_agent.app import MCPApp

app = MCPApp(name="example_app")

async with app.run() as running_app:
    # Application code
```

### Example 5: Using MCPAggregator

This example demonstrates how to use the `MCPAggregator` to list tools and call a tool.

```python
from mcp_agent.mcp.mcp_aggregator import MCPAggregator

aggregator = await MCPAggregator.create(server_names=["server1", "server2"])

tools = await aggregator.list_tools()
result = await aggregator.call_tool(name="server1-tool1", arguments={"arg1": "value1"})
```

## Conclusion

The MCP implementation in Python provides a robust framework for interacting with MCP servers, managing global state, and hosting workflows. By using the `Agent`, `MCPApp`, and `MCPAggregator` classes, developers can create powerful and flexible MCP agents with ease.
