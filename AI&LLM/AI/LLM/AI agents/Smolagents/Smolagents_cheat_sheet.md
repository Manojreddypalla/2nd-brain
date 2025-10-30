# Smolagents Cheat Sheet

## What is Smolagents?

Smolagents is a minimalist AI agent library from Hugging Face for creating powerful AI agents with minimal code. It focuses on "Code Agents" that write and execute Python code to perform tasks.

### Key Features

*   **Code Agents**: Agents write and execute Python code, increasing efficiency.
*   **LLM-Agnostic**: Supports various LLMs (Hugging Face Hub, OpenAI, Anthropic, local models).
*   **Tool-Agnostic**: Use custom tools, tools from MCP servers, or LangChain.
*   **Hub Integrations**: Integrates with the Hugging Face Hub for sharing agents and tools.
*   **Security**: Supports sandboxed environments (E2B, Modal, Docker, Pyodide+Deno).
*   **CLI Tools**: `smolagent` and `webagent` for quick agent execution.

### Core Concepts

*   **Agent**: An AI model that interacts with its environment to achieve a goal.
*   **Tools**: Functions an LLM can use. In Smolagents, tools are classes wrapping a function with metadata (`name`, `description`, `inputs`, `output_type`).
    *   **Custom Tools**: Easily created by inheriting from `smolagents.Tool`.
    *   **Tool Collections**: Group multiple tools using `ToolCollection`.

### Basic Usage

1.  **Installation**:
    ```bash
    pip install smolagents huggingface_hub
    ```

2.  **Import**:
    ```python
    from smolagents import CodeAgent, WebSearchTool, InferenceClientModel
    ```

3.  **Instantiate a model**:
    ```python
    model = InferenceClientModel() # Hugging Face Inference API
    # Or for OpenAI:
    # from smolagents import OpenAIModel
    # model = OpenAIModel(model_id="gpt-4")
    ```

4.  **Create an agent**:
    ```python
    agent = CodeAgent(tools=[WebSearchTool()], model=model, stream_outputs=True)
    ```

5.  **Run the agent**:
    ```python
    agent.run("Your question here")
    ```

### Advantages

*   **Minimal Abstractions**: Simple to use with a small codebase.
*   **Efficiency**: Reduces API calls.
*   **Flexibility**: Supports various LLMs and custom tools.
*   **Production Ready**: Suitable for building production systems.
