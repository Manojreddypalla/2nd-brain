# Building Agents That Use Code

This Hugging Face course unit focuses on building agents that use code, specifically within the `smolagents` framework. It highlights the advantages of code agents over JSON-based actions, emphasizing composability, object management, generality, and LLM compatibility. The unit explains how a `CodeAgent` works using the ReAct framework and provides examples of creating agents for tasks like selecting music playlists and preparing menus, including using custom tools and handling Python imports. It also covers sharing agents to the Hugging Face Hub and inspecting agent runs with OpenTelemetry and Langfuse for traceability.

[Source](https://huggingface.co/learn/agents-course/unit2/smolagents/code_agents)

## Running an agent

Running an agent is quite straightforward:

```python
from smolagents import CodeAgent, DuckDuckGoSearchTool, InferenceClientModel

agent = CodeAgent(tools=[DuckDuckGoSearchTool()], model=InferenceClientModel())

agent.run("Search for the best music recommendations for a party at the Wayne's mansion.")
```

## Using a Custom Tool to Prepare the Menu
Alfred Menu
Now that we have selected a playlist, we need to organize the menu for the guests. Again, Alfred can take advantage of smolagents to do so. Here, we use the @tool decorator to define a custom function that acts as a tool. We’ll cover tool creation in more detail later, so for now, we can simply run the code.

As you can see in the example below, we will create a tool using the @tool decorator and include it in the tools list.

```python
from smolagents import CodeAgent, tool, InferenceClientModel

# Tool to suggest a menu based on the occasion

def suggest_menu(occasion: str) -> str:
    """
    Suggests a menu based on the occasion.
    Args:
        occasion (str): The type of occasion for the party. Allowed values are:
                        - "casual": Menu for casual party.
                        - "formal": Menu for formal party.
                        - "superhero": Menu for superhero party.
                        - "custom": Custom menu.
    """
    if occasion == "casual":
        return "Pizza, snacks, and drinks."
    elif occasion == "formal":
        return "3-course dinner with wine and dessert."
    elif occasion == "superhero":
        return "Buffet with high-energy and healthy food."
    else:
        return "Custom menu for the butler."

# Alfred, the butler, preparing the menu for the party
agent = CodeAgent(tools=[suggest_menu], model=InferenceClientModel())

# Preparing the menu for the party
agent.run("Prepare a formal menu for the party.")
```
