# LangChain Cheat Sheet

## Core Concepts

*   **Chains:** Sequences of calls to components (LLMs, prompts, etc.) that run together.
*   **Agents:** Autonomous entities that use LLMs to make decisions and use tools to perform tasks.
*   **Tools:** Functions or APIs that an agent can use to interact with the outside world (e.g., Google Search, databases).
*   **LangChain Expression Language (LCEL):** A declarative way to compose chains and other components.
*   **Memory:** Enables applications to remember previous interactions, providing context.
*   **Retrievers & Vector Stores:** Fetch data from various sources to provide external knowledge to LLMs (RAG).
*   **Document Loaders & Text Splitters:** Load and split documents into smaller chunks for processing.

## Key Components

*   **Models:**
    *   **LLMs:** Take a string as input, return a string.
    *   **Chat Models:** Process a sequence of messages.
    *   **Embedding Models:** Generate numerical representations (embeddings) of data.
*   **Prompts & Prompt Templates:** Reusable structures for creating prompts with dynamic inputs.
*   **Schema:** Defines the data structures and formats used by LangChain components.

## Basic Usage: Simple Chain

'''python
from langchain.llms import OpenAI
from langchain.prompts import PromptTemplate
from langchain.chains import LLMChain

# Initialize LLM
llm = OpenAI(temperature=0.9)

# Create a Prompt Template
prompt = PromptTemplate(
    input_variables=["product"],
    template="What is a good name for a company that makes {product}?",
)

# Create a Chain
chain = LLMChain(llm=llm, prompt=prompt)

# Run the Chain
print(chain.run("colorful socks"))
'''

## Agents and Tools

Agents use tools to interact with the world.

'''python
from langchain.agents import AgentType, initialize_agent, load_tools
from langchain.llms import OpenAI

# Initialize LLM
llm = OpenAI(temperature=0)

# Load some tools
tools = load_tools(["serpapi", "llm-math"], llm=llm)

# Initialize an agent
agent = initialize_agent(tools, llm, agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION, verbose=True)

# Run the agent
agent.run("Who is the current CEO of OpenAI? What is his age raised to the 0.23 power?")
'''

## Memory

Memory allows chains and agents to remember past interactions.

'''python
from langchain.chains import ConversationChain
from langchain.memory import ConversationBufferMemory
from langchain.llms import OpenAI

# Initialize LLM and Memory
llm = OpenAI(temperature=0)
memory = ConversationBufferMemory()

# Create a Conversation Chain
conversation = ConversationChain(llm=llm, memory=memory)

# Have a conversation
print(conversation.run("Hi, I'm Bob."))
print(conversation.run("What's my name?"))
'''
