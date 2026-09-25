# Restaurant Recommendation System

An AI-powered restaurant and recipe recommendation platform. It combines a multimodal vector search index (text + food images) with a multi-agent LangGraph workflow to generate personalized dining recommendations from natural-language requests.

## Features

- **Conversational recommendations** — describe what you're craving and get personalized restaurant and recipe suggestions
- **Multi-agent reasoning** — dedicated agents for trend analysis, cuisine/style matching, and nutrition fit, synthesized into a final recommendation
- **Multimodal search** — restaurants and recipes are indexed by text embeddings and food image embeddings for cross-modal retrieval
- **Restaurant management** — add, update, and remove restaurant records through the UI or API
- **REST API** — a documented set of endpoints for chat and data management, usable from any client

## Tech Stack

- **Backend:** FastAPI, LangGraph, LangChain, Groq (LLM inference)
- **Retrieval:** ChromaDB, Sentence-Transformers (text), CLIP (images)
- **Frontend:** HTML, CSS, JavaScript
- **Data validation:** Pydantic

## Getting Started

### Prerequisites

- Python 3.11
- A [Groq API key](https://console.groq.com)

### Installation

```bash
python -m venv .venv
.venv/Scripts/pip install -e .
.venv/Scripts/pip install -r requirements.txt
cp .env.example .env
```

Add your `GROQ_API_KEY` to `.env`.

### Running the app

Build the vector search index (one-time, or whenever the dataset changes):

```bash
.venv/Scripts/python -m retrieval.index_builder
```

Start the server:

```bash
.venv/Scripts/uvicorn api.main:app --host 0.0.0.0 --port 8000
```

On Windows, `run.bat` does both steps for you.

Open **http://localhost:8000** in a browser.

## Project Structure

```text
src/
  api/          FastAPI application and routes
  agents/       LangGraph multi-agent recommendation workflow
  chatbot/      conversational logic shared by the API and a Gradio prototype
  data/         dataset management and CLI
  llm/          Groq client and prompt templates
  mcp_app/      MCP server/client/host implementation
  retrieval/    embeddings, vector index, similarity search
  schemas/      Pydantic data models
frontend/       web client (HTML/CSS/JS)
data/           restaurant, recipe, and review datasets
tests/          unit and integration tests
```

## API Reference

| Method | Endpoint                  | Description                       |
|--------|----------------------------|------------------------------------|
| GET    | `/api/health`              | Health check                      |
| GET    | `/api/sample-prompts`      | Example chat prompts              |
| POST   | `/api/chat`                | Send a message, get a recommendation |
| GET    | `/api/restaurants`         | List restaurants                  |
| GET    | `/api/restaurants/{id}`    | Get a restaurant                  |
| POST   | `/api/restaurants`         | Add a restaurant                  |
| PUT    | `/api/restaurants/{id}`    | Update a restaurant               |
| DELETE | `/api/restaurants/{id}`    | Remove a restaurant               |
| GET    | `/api/recipes`             | List recipes                      |

## Deployment

The app is a standard Python web service and runs anywhere that can install dependencies and run a process from an entry point:

- **Build:** `pip install -e . && pip install -r requirements.txt && python -m retrieval.index_builder`
- **Start:** `uvicorn api.main:app --host 0.0.0.0 --port $PORT`
- **Environment:** `GROQ_API_KEY`

A `Procfile` and `runtime.txt` are included for platforms that read them.

## Testing

```bash
.venv/Scripts/pytest tests/ -v
```
