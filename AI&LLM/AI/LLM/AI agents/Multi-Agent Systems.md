# Multi-Agent Systems

# 🧠 **Multi-Agent Systems (MAS)** – *Smolagents Summary Notes*

---

## ⚙️ **Definition**

A **Multi-Agent System (MAS)** is a system where **multiple specialized agents collaborate** to perform complex tasks.

Each agent focuses on a specific skill, and a **Manager (or Orchestrator)** coordinates them.

This approach improves:

- **Modularity** – each agent has its own role
- **Scalability** – tasks can be distributed
- **Robustness** – one agent’s failure doesn’t stop the entire system

---

## 🤖 **In Smolagents**

Smolagents framework allows different agents to:

- Generate Python code
- Use external tools (e.g., search engines, web scrapers)
- Perform reasoning and analysis
- Visualize data

### 🧩 Example agents:

| Agent | Role |
| --- | --- |
| **Manager Agent** | Delegates and coordinates tasks |
| **Code Interpreter Agent** | Executes Python code |
| **Web Search Agent** | Searches information on the web |
| **Image Generation Agent** | Creates visuals (optional) |

---

## 🧠 **How It Works**

1. **Manager Agent (Orchestrator)** — decides who does what.
2. **Sub-Agents** — perform specialized subtasks:
    - e.g., search, analyze, compute, visualize.
3. **Communication** — results are passed back to the Manager for combination.

---

## 🌐 **Example Scenario**

> 🦇 Alfred wants to find Batman filming locations and nearby supercar factories.
> 

### Task:

Find all Batman filming locations, calculate flight times from Gotham (40.7128° N, 74.0060° W), and plot them on a world map.

---

## 🧮 **Supporting Tool Example**

A helper tool to calculate **cargo plane travel time** using haversine distance formula.

```python
def calculate_cargo_travel_time(origin_coords, destination_coords, cruising_speed_kmh=750.0):
    # Returns estimated flight time between two locations

```

- Converts coordinates → radians
- Uses Earth’s radius (6371 km)
- Adds 10% for non-direct routes
- Adds 1 hour for takeoff/landing
- Returns time in **hours**

---

## 🧰 **Agent Setup (Smolagents)**

### Step 1️⃣: **Model Setup**

```python
model = InferenceClientModel("Qwen/Qwen2.5-Coder-32B-Instruct", provider="together")

```

### Step 2️⃣: **Create Tools**

```python
tools = [GoogleSearchTool("serper"), VisitWebpageTool(), calculate_cargo_travel_time]

```

### Step 3️⃣: **Define Task**

```python
task = """Find Batman filming locations and supercar factories..."""

```

### Step 4️⃣: **Create Agent**

```python
agent = CodeAgent(model=model, tools=tools, additional_authorized_imports=["pandas"], max_steps=20)
result = agent.run(task)

```

---

## 📊 **Sample Output**

| Location | Travel Time (hours) |
| --- | --- |
| Glasgow, UK | 8.60 |
| London, UK | 9.17 |
| Chicago, USA | 2.68 |
| Jodhpur, India | 18.34 |
| Hong Kong | 19.99 |
| McLaren, UK | 9.13 |

---

## 🧩 **Adding Planning (Better Results)**

Agents can plan steps using:

```python
agent.planning_interval = 4

```

This helps it think ahead, search multiple queries, and refine answers.

---

## ⚡ **Improving System with Multiple Agents**

When tasks grow complex → **split them into two agents**:

1. **Web Agent:**
    - Searches and gathers info.
    - Tools: `GoogleSearchTool`, `VisitWebpageTool`, `calculate_cargo_travel_time`.
2. **Manager Agent:**
    - Analyzes results, plots data, and prepares final reports.
    - Tools: `geopandas`, `plotly`, `shapely`, etc.
    - Manages the Web Agent.

---

### 🧠 **Why Split Agents?**

✅ Each agent focuses on one job → faster and more accurate.

✅ Reduces memory & token use → cheaper and efficient.

✅ Easier debugging → track errors per agent.

---

## 🗺️ **Visualization Example (Plotly)**

Manager agent uses **Plotly** to plot map points based on travel time:

```python
import plotly.express as px
fig = px.scatter_map(df, lat="lat", lon="lon", color="travel_time", text="location",
    color_continuous_scale=px.colors.sequential.Magma, zoom=1)
fig.write_image("saved_map.png")
final_answer(fig)

```

---

## ✅ **Final Output**

- A **world map** showing:
    - Batman filming locations
    - Supercar factories
    - Color variation by travel time (shorter = brighter)

---

## 📘 **Summary Table**

| Concept | Description |
| --- | --- |
| **Multi-Agent System** | Multiple AI agents working together |
| **Manager Agent** | Main coordinator |
| **Sub-Agent (Web Agent)** | Performs web-related subtasks |
| **Tools** | Functions used by agents (search, calculate, plot, etc.) |
| **Advantages** | Modular, scalable, efficient, cost-effective |
| **Visualization** | Plot results using `plotly` |
| **Framework** | Smolagents |

---

## 📚 **Resources**

- [**Multi-Agent Systems**](https://huggingface.co/docs/smolagents/main/en/examples/multiagents) – Overview of multi-agent systems.
- [**What is Agentic RAG?**](https://weaviate.io/blog/what-is-agentic-rag) – Introduction to Agentic RAG.
- [**Multi-Agent RAG System 🤖🤝🤖 Recipe**](https://huggingface.co/learn/cookbook/multiagent_rag_system) – Step-by-step guide to building a multi-agent RAG system.