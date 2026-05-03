# Hawk-Eye Agent — AI-Powered Internal Documentation Assistant 🦅

An intelligent AI agent built with **CrewAI** that reads your company's internal documentation and answers user queries accurately — eliminating the need to manually search through hundreds of pages.

---

## 📌 Problem Statement

Every company maintains internal documentation — often spanning hundreds of pages covering platforms, tools, processes, and guidelines. Searching through them manually is time-consuming and inefficient.

This project solves that by deploying an **AI agent** that can:
- 📄 **Summarize** large internal documents instantly
- 🔍 **Answer queries** accurately based on the actual documentation
- 🧠 **Train and respond** like a domain-specific assistant tailored to your internal knowledge base

> **Note on Practice Docs:** Since using confidential company documentation for testing is not safe, you can generate **fake documentation** using an LLM prompt. For example, a fake observability platform called `Vision` (which uses Victoria Metrics instead of TSDB, making it better than Prometheus) was used here. This gives you realistic, structured content without any risk.

---

## 🏗️ Architecture Overview

```
Internal PDF Document
        |
        ▼
  CrewAI Framework
  (Breaks task into smaller units)
        |
        ▼
  AI Agent (hawk-eye-agent)
  + Custom Tool (vision_expert_tool)
        |
        ▼
  LLM Model (configured via .env)
        |
        ▼
  Answers your queries ✅
```

---

## 🛠️ Tech Stack

| Component | Purpose |
|-----------|---------|
| **CrewAI** | Orchestrates the AI agent and breaks tasks into manageable units |
| **Python** | Core language for scripting the agent |
| **LLM Model** | The language model that powers the agent's reasoning (configured in `.env`) |
| **Custom Tool** | `vision_expert_tool` — reads and processes the documentation PDF |
| **Virtual Environment** | Isolated Python environment for clean dependency management |

---

## 🧠 What is CrewAI?

**CrewAI** is a beginner-friendly Python framework for building AI agents. It:
- **Breaks down complex tasks** into smaller, simpler units
- Assigns **roles, goals, and tools** to agents
- Coordinates multiple agents to collaborate on a workflow
- Is easy to scaffold and customize via configuration files

Think of it as giving your AI agent a job description, a set of tools, and a team structure — all in one framework.

---

## ⚙️ Prerequisites & Installation

### Step 1 — Create a Virtual Environment
Always use a virtual environment before starting a Python project to keep dependencies isolated:

```bash
# Create the virtual environment
python -m venv crew

# Activate it (Git Bash / Linux / Mac)
source crew/Scripts/activate
```

### Step 2 — Install CrewAI
```bash
pip3 install crewai
```
This installs CrewAI along with all required dependencies automatically.

### Step 3 — Install UV (CrewAI's dependency manager)
```bash
pip install uv
```

---

## 🚀 Step-by-Step Implementation

### Phase 1 — Scaffold the Agent Project

Use the CrewAI CLI to generate the boilerplate project structure:

```bash
crewai create crew hawk-eye-agent
cd hawk-eye-agent
ls -a
```

This creates a structured project with the following key files:

```
hawk-eye-agent/
├── .env                  # Environment variables (LLM model, API keys)
├── config/
│   ├── agents.yaml       # Define your agent's role, goal, and backstory
│   └── tasks.yaml        # Define tasks the agent will perform
├── src/
│   ├── crew.py           # Wires agents, tasks, and tools together
│   └── main.py           # Entry point to run the agent
```

---

### Phase 2 — Configure the Environment

Open and edit the `.env` file to set your LLM model and API credentials:

```bash
nano .env
```

**What to configure in `.env`:**
- Set the **LLM model version** you want to use (e.g., GPT-4, LLaMA, Gemini, etc.)
- Add your **API key** for the chosen model provider
- Any other environment-specific variables

Save and verify:
```bash
cat .env
```

> 🔒 **Security Note:** Never hardcode API keys or model credentials directly in code files. Always use `.env` and add it to `.gitignore` before pushing to GitHub.

---

### Phase 3 — Customize Configuration Files

This is where you tailor the agent to your specific use case. All four files need to reflect your domain — in this example, the observability platform `Vision`:

#### `config/agents.yaml`
Define your agent's identity:
- **Role** — e.g., `Vision Platform Expert`
- **Goal** — e.g., Answer queries about the Vision observability platform
- **Backstory** — Context that shapes the agent's persona and expertise

#### `config/tasks.yaml`
Define what the agent does:
- **Task description** — What question or action to perform
- **Expected output** — What format/content the result should be in
- **Agent assignment** — Which agent handles this task

#### `src/crew.py`
Wires everything together:
- Register your custom tool (e.g., `vision_expert_tool`)
- Link agents with their tasks
- Configure the crew (team of agents)

#### `src/main.py`
The entry point:
- Pass in the user's query as input
- Kick off the crew execution

> 💡 **For your own project:** Replace all references to the custom tool name and domain-specific content with your own platform name, document, and tool. The structure remains identical.

---

### Phase 4 — Add Your Custom Tool

The custom tool (e.g., `vision_expert_tool`) reads the internal documentation PDF and makes its content available to the agent for reasoning.

When building your own version:
- Point the tool to your documentation file (PDF, text, etc.)
- Define what the tool does (e.g., read, chunk, and search content)
- Register it in `crew.py` so the agent can use it during task execution

---

### Phase 5 — Install Dependencies & Run

```bash
# Install all project dependencies via CrewAI
crewai install

# Run the agent
crewai run
```

Once running, the agent accepts your query, processes it against the documentation, and returns a precise answer.

---

## 💬 Interacting with the Agent

After running, you can ask the agent questions like:

- *"What is the Vision platform?"*
- *"How does Vision compare to Prometheus?"*
- *"What metrics storage does Vision use and why is it better?"*
- *"Summarize the logging capabilities of Vision."*

The agent reads the documentation, reasons through it using the LLM, and responds with accurate, context-aware answers.

---

## 📝 Generating Practice Documentation

Since real company docs are confidential, generate fake but realistic documentation for testing using a prompt like:

> *"Generate a fake documentation of an observability platform called 'Vision'. Assume this platform is better than existing platforms like Prometheus because it uses Victoria Metrics instead of TSDB. Add comparisons across traces, metrics, and logs. Output in PDF format covering at least 1000 lines."*

**Suggested topics to include:**
- What is the Vision platform
- Why Vision is the best in observability
- Comparison of Vision vs Prometheus
- Metrics, traces, and logs capabilities
- Architecture and components

This gives you a realistic, structured, risk-free document to develop and test against.

---

## ✅ Summary

| Step | Action |
|------|--------|
| 1 | Create and activate a Python virtual environment |
| 2 | Install CrewAI and UV |
| 3 | Scaffold the agent using `crewai create crew <name>` |
<img width="710" height="257" alt="create1" src="https://github.com/user-attachments/assets/72525b39-889e-4800-8ba8-6ccb792159e9" />

| 4 | Configure `.env` with your LLM model and API keys |
<img width="741" height="380" alt="modify-env" src="https://github.com/user-attachments/assets/f3e5be15-e966-4073-8f3f-2a0f5f7dc42f" />

| 5 | Customize `agents.yaml`, `tasks.yaml`, `crew.py`, `main.py` for your domain |
| 6 | Add your custom document-reading tool |
| 7 | Run `crewai install` then `crewai run` |
| 8 | Query the agent and get documentation-backed answers |

## Sample Introduction
<img width="959" height="449" alt="output1" src="https://github.com/user-attachments/assets/adeff477-e26f-4eaf-8d26-9f2f0ed3c7b0" />

### Queries & Answers
<img width="952" height="367" alt="output2" src="https://github.com/user-attachments/assets/610c2aa3-247c-4642-84fd-596382fec4e4" />

<img width="959" height="437" alt="output3" src="https://github.com/user-attachments/assets/e00e32cd-b8c9-41ed-b288-ac2718b95998" />

<img width="959" height="296" alt="query3" src="https://github.com/user-attachments/assets/da02dd36-cf6d-4738-b4cf-73ebde00763a" />
 ### Query & Answer
 <img width="956" height="334" alt="query4" src="https://github.com/user-attachments/assets/a792693f-7c3d-4506-ae9b-f9c2a71acaf3" />

---

## 💡 Pros of This Project

- **⏱️ Saves Time** — No more manually searching through hundreds of pages; get instant, accurate answers
- **🔒 Keeps Docs Internal** — The agent runs locally or on your own infrastructure; no data leaves your environment
- **🧩 Modular & Extensible** — Swap the document, change the tool, update the LLM — the framework stays the same
- **👶 Beginner Friendly** — CrewAI's scaffolding and configuration-file approach makes it accessible even for those new to AI agents
- **📈 Scalable** — Can be extended to multiple agents handling different documentation domains simultaneously
- **🔁 Reusable Pattern** — The same architecture works for legal docs, HR policies, engineering runbooks, API references, and more

---

## 🌍 Real-World Use Cases

| Industry | Application |
|----------|-------------|
| **DevOps / Platform Teams** | Query internal runbooks and platform docs without opening Confluence |
| **HR & Onboarding** | New employees ask onboarding questions and get instant policy answers |
| **Legal & Compliance** | Query contracts or compliance docs without involving a lawyer for basic questions |
| **Customer Support** | Internal agents answer support staff queries from product documentation |
| **Healthcare** | Medical teams query internal clinical protocols or drug reference docs |
| **Finance** | Teams query internal financial policy documents or audit guidelines |

---

## 🔒 Security Best Practices

- Always store API keys and credentials in `.env` — never in source code
- Add `.env` to your `.gitignore` before pushing to GitHub
- Use fake/synthetic documentation during development and testing
- Restrict the agent's tool access to only the documents it needs

---

## 🏁 Conclusion

The **Hawk-Eye Agent** demonstrates how modern AI frameworks like **CrewAI** can transform static internal documentation into a dynamic, queryable knowledge base. By combining a structured agent framework, a custom document-reading tool, and a powerful LLM backend, this project makes internal knowledge instantly accessible — without exposing sensitive data or requiring expensive enterprise tooling.

Whether you are a DevOps team querying runbooks, an HR team onboarding new employees, or an engineering team navigating complex platform docs, this pattern is adaptable, scalable, and ready for real-world deployment.

---

## 📚 References

- [CrewAI Documentation](https://docs.crewai.com/)
- [CrewAI GitHub](https://github.com/crewAIInc/crewAI)
- [Python Virtual Environments](https://docs.python.org/3/library/venv.html)
- [UV Package Manager](https://github.com/astral-sh/uv)

