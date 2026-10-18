# WorkCity: The Living Company Brain

**Graph Hacks (WeMakeDevs x FalkorDB) | Track 3: Company Brain**

WorkCity connects a company's people, projects, systems, decisions and documents in one FalkorDB graph. Ask "Who knows the payment service best?" and get answers from real connections, with proof. Explore the company as a city or as a graph.

Built with FastAPI, React and FalkorDB. The demo company is a fictional one called NovaWorks.

---

## How to run it

**You need:** Docker Desktop, installed and **running**, plus an internet connection for the first build. Nothing else.

**Step 1. Download the project**
On this page click **Code > Download ZIP**, or download the file `company brain.zip` from the file list.

**Step 2. Unzip it**
Right-click the zip file and choose **Extract All** (Windows) or double-click it (Mac).
If the zip file is inside another zip, unzip that one too.

**Step 3. Open the folder in a terminal**
Open the unzipped folder that contains the file `docker-compose.yml` (it also has `Dockerfile`, `backend` and `frontend`).

- **Windows:** click the address bar at the top of the folder, type `cmd`, and press Enter.
- **Mac:** right-click the folder and choose **New Terminal at Folder**.

**Step 4. Paste this command and press Enter**

```
docker compose up --build
```

The first run takes a few minutes. Wait until you see these lines in the terminal:

```
Graph seeded successfully.
Application startup complete.
```

**Step 5. Open the app**
Go to **http://localhost:8000** in your browser.

If the page looks empty, wait 10 seconds and refresh.

---

## Stop it

Press `Ctrl + C` in the terminal, then run:

```
docker compose down
```

---

## If it does not open

| Problem | Fix |
|---|---|
| Docker error or "cannot connect to the Docker daemon" | Start Docker Desktop, wait until it says it is running, and run the command again. |
| `no configuration file provided: not found` | You are in the wrong folder. Open the folder that contains `docker-compose.yml`. |
| Port 8000 is busy | Open `docker-compose.yml`, change `"8000:8000"` to `"8080:8000"`, run the command again, and open http://localhost:8080. |
| Page is empty or stuck | Run `docker compose down -v`, then `docker compose up --build` again. |

---

## What to try

In the **Ask WorkCity** tab, click or type:

1. Who knows the payment service best?
2. Why was Project Atlas delayed?
3. Which decision supersedes DEC-202?
4. What could break if core-db goes down?
5. Who is the most central employee in the company?

Then open **City View** to browse departments and projects, and **Graph Explorer** to see the connections between people, systems, decisions and documents.

---

## How it works

1. You ask a question in the React app.
2. The FastAPI backend picks graph tools and runs Cypher queries on FalkorDB.
3. The answer is built from the rows that come back, and the app shows how it was found.

Graph tools: `find_entity`, `neighbors`, `shortest_path`, `who_knows`, `decision_trace`, `blast_radius`, `central_nodes`, `search_documents`, `recent_changes`.

---

## Built with

FalkorDB, FastAPI, React, Vite, TypeScript, Tailwind CSS, Docker Compose
