# AI Agent Router

**AI-Agent-Router** is a demonstration of a multi‑agent conversational router that uses a large language model to classify user messages and route them to specialized agents for a contextual response.  It shows how to orchestrate multiple LLM-powered components in a single workflow using LangGraph.

## Problem statement

When building chatbots or AI assistants with multiple skills (e.g., therapy vs. logic), it is hard to route each user request to the appropriate agent.  Without routing, a generic model may answer inappropriately or inefficiently.

This project demonstrates how to classify user messages into different categories (emotional vs. logical) and invoke the corresponding agent to generate a better response.

## Main features

- **Message classification**: uses an LLM to determine whether a user message is emotional or logical.
- **Specialized agents**: defines separate agents (therapist and logic) to handle different kinds of queries.
- **Graph orchestration**: uses LangGraph to build a state graph that routes messages based on classification.
- **Extensible architecture**: easy to add more agents and classification categories.

## Architecture / Workflow

1. **Load models** – A chat model is loaded through `init_chat_model` (e.g., Anthropic Claude) using your API key.
2. **Define message classifier** – A Pydantic model describes the classification result.
3. **Build the routing graph** – The router uses LangGraph's `StateGraph` to set up nodes for classification and different agents, with edges based on classification results.
4. **Process a message** – The `chat_once` function takes a message, runs the graph, and returns the agent’s response.

For more details, see the comments in `main.py`.

## Tech stack

- **Python**
- **LangChain / LangGraph** – for building LLM workflows and the state graph.
- **Anthropic Claude** (or any chat model) via `langchain[anthropic]`
- **Pydantic** – type safety and structured outputs.
- **dotenv** – for environment variable management.

## Installation

Clone the repository and install dependencies:

```bash
git clone https://github.com/tahamohmadf19-dev/AI-Agent-Router1.git
cd AI-Agent-Router1
python -m venv venv
source venv/bin/activate  # On Windows use `venv\Scripts\activate`
pip install -r requirements.txt  # or use pip install -e . to install from pyproject
```

> **Note:** This project uses Python 3.10 or newer.  The `pyproject.toml` lists the required packages.

## Environment variables

Create a `.env` file in the project root with your API keys:

```
ANTHROPIC_API_KEY=sk-...
```

You can adjust the model name or provider in `main.py` (e.g., `anthropic:claude-sonnet-4-5`).

## How to run

After installing dependencies and setting environment variables, you can test the router by starting a Python session:

```python
from main import chat_once
response = chat_once("I feel very stressed. What should I do?")
print(response)
```

The function will classify the message and route it to the appropriate agent.

## Usage example

Here is a sample interaction:

```
>>> chat_once("I'm worried about my future and need some advice.")
# Therapist agent responds with empathetic guidance.
>>> chat_once("What is the capital of France?")
# Logical agent returns a factual answer: "The capital of France is Paris."
```

## Future improvements

- Add more classification categories and agents (e.g., coding assistant, math tutor).
- Integrate a vector database and retrieval‑augmented generation to ground responses in your documents.
- Build a Streamlit or FastAPI interface for interactive chat.
- Persist conversation histories in a secure database.

## License

This project is provided for educational purposes under the MIT license.
