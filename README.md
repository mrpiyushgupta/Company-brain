WorkCity: The Living Company Brain

Graph Hacks (WeMakeDevs x FalkorDB) | Track 3: Company Brain

WorkCity connects a company's people, projects, systems, decisions, documents, incidents, tickets and chat threads in one FalkorDB graph. Ask a question like "Who knows the payment service best?" and the answer is built from real graph connections, with the reasoning shown next to it.

The demo company is a fictional one called NovaWorks.

Features
Ask WorkCity: ask plain-English questions. Each answer comes from Cypher queries on FalkorDB, and the answer shows the steps taken.
City View: the company as a city. Districts are departments, buildings are projects, and you can filter by status (Active, Delayed, Planning). Click a project or a person for details.
Graph Explorer: pick any entity, see its connections, expand its neighborhood, and find the path between two entities.
Decision trace: follows SUPERSEDES links to show which decision is current, who made it, and which document records it.
Document ingestion: add a document through the API; references to known entities (such as decision, incident or system IDs) are linked into the graph.
Sessions: questions and answers are stored in the graph per session.
Works offline and without an LLM: a deterministic agent answers from graph queries. An optional LLM can be added through .env.
Quick start

Requirements: Docker Desktop (running) and nothing else.

bash
docker compose up --build

Then open http://localhost:8000

The first build takes a few minutes.
Wait until the logs show Graph seeded successfully. The graph is seeded automatically the first time.
Everything is served from one address: the React app and the API both live on port 8000.

Stop it: press Ctrl + C, then run:

bash
docker compose down        # keeps the graph data
docker compose down -v     # also deletes the graph data (it re-seeds next time)

If port 8000 is busy: change "8000:8000" to "8080:8000" in docker-compose.yml and open http://localhost:8080.

Try these questions

In the Ask WorkCity tab:

Who knows the payment service best?
Why was Project Atlas delayed?
Which decision supersedes DEC-202?
What could break if core-db goes down?
Who is the most central employee in the company?
What changed in the last 30 days?
How it works
question
parameterized Cypher
rows
answer + trace + sources
BrowserReact app
FastAPI/api/v1
Graph tools
FalkorDB
The question goes to the assistant in the FastAPI backend.
The assistant picks one or more graph tools and runs them on FalkorDB.
The answer text, evidence path and sources are built from the rows that come back.
The UI shows the answer together with how it was found.
Graph tools
Tool	What it does
find_entity	Finds an entity by id, name or title
neighbors	Returns the connections of an entity (1 or 2 hops)
shortest_path	Finds how two entities are connected
who_knows	Ranks people for a system or project using weighted paths (owns, resolved incidents, authored documents, project work, chat mentions)
decision_trace	Follows a decision to its maker, documents, and what supersedes it
blast_radius	Finds what depends on a system
central_nodes	Finds the most connected nodes
search_documents	Searches documents, tickets and chat threads
recent_changes	Lists recently created or updated items
Graph model

Nodes: Employee, Department, Skill, Project, System, Document, Decision, Incident, Ticket, ChatThread, Session, Question, Answer

Relationships (examples): BELONGS_TO, REPORTS_TO, WORKED_ON, OWNS, HAS_SKILL, USES, DEPENDS_ON, AFFECTED, RESOLVED_BY, DOCUMENTED_IN, AUTHORED, MADE, COLLABORATES_WITH, ASSIGNED_TO, PARTICIPATED_IN, MENTIONS, SUPERSEDES

Optional: use an LLM

The app works without any LLM. To try one (for example a local model through Ollama or LM Studio), copy .env.example to .env and set:

env
LLM_BASE_URL=http://host.docker.internal:11434/v1
LLM_MODEL=qwen2.5:7b
LLM_API_KEY=

Any OpenAI-compatible endpoint works. If the LLM is unavailable, the app falls back to the built-in agent.

Configuration
Variable	Default	Meaning
FALKORDB_HOST	falkordb	FalkorDB host (set by Docker Compose)
FALKORDB_PORT	6379	FalkorDB port
GRAPH_NAME	workcity_novaworks	Name of the graph
LLM_BASE_URL / LLM_MODEL / LLM_API_KEY	empty	Optional LLM settings
Project structure
.
├── backend/            FastAPI app, graph tools, assistant, seed data loader
│   ├── app/
│   └── tests/
├── frontend/           React + Vite + TypeScript + Tailwind
├── data/               seed_data.json and sample documents
├── Dockerfile          builds the frontend, then serves it from FastAPI
└── docker-compose.yml  app + FalkorDB
Main API endpoints (under /api/v1)

/health, /city, /employees, /projects, /systems, /documents, /decisions, /incidents, /graph/neighborhood/{id}, /graph/path, /assistant/ask, /sessions/{id}, /ingest/document, /ingest/json

Run without Docker (development)

You need Python 3.12+, Node 20+, and a FalkorDB instance on localhost:6379.

bash
# FalkorDB
docker run -p 6379:6379 -d falkordb/falkordb:latest

# Backend
cd backend
pip install -r requirements.txt
FALKORDB_HOST=localhost uvicorn app.main:app --port 8000

# Frontend (in a second terminal)
cd frontend
npm install
npm run dev
Tests
bash
cd backend
pytest -q

The tests need a running FalkorDB instance.

Built with

FalkorDB, FastAPI, React, Vite, TypeScript, Tailwind CSS, Docker Compose

