Here’s a **README.md** you can drop into the root of your `pr_reviewer` project. It follows common conventions and documents every step from setup to running the MCP-powered PR review server:

# PR Reviewer (MCP Demo Project)

A Model Context Protocol (MCP)–powered server that connects Claude Desktop, GitHub, and Notion to automate pull-request reviews.  
It fetches PR diffs from GitHub, uses Claude to analyze code changes, and publishes review summaries to Notion.

---

## Table of Contents

- [Features](#features)  
- [Prerequisites](#prerequisites)  
- [Installation](#installation)  
- [Configuration](#configuration)  
- [Project Structure](#project-structure)  
- [Usage](#usage)  
- [Environment Variables](#environment-variables)  
- [Extending & Contributing](#extending--contributing)  
- [License](#license)  

---

## Features

- **Standardized AI integration** via Anthropic’s Model Context Protocol  
- **GitHub PR fetching**: retrieves PR metadata and file diffs  
- **Automated code analysis**: uses Claude Desktop as the LLM client  
- **Notion publishing**: creates a Notion page for each review  
- **Modular & transport-flexible**: works over stdio, WebSockets, HTTP-SSE, UNIX sockets  

---

## Prerequisites

- **Python 3.10+**  
- **uv** package manager (recommended)  
- A GitHub account with a Personal Access Token  
- A Notion workspace with an Internal Integration  

---

## Installation

1. **Install `uv`** (Mac/Linux):  
   ```bash
   curl -LsSf https://astral.sh/uv/install.sh | sh

Windows (PowerShell):

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

2. **Initialize your project**:

   ```bash
   uv init pr_reviewer
   cd pr_reviewer
   ```

3. **Create & activate a virtual environment**:

   * macOS/Linux:

     ```bash
     uv venv
     source .venv/bin/activate
     ```
   * Windows:

     ```powershell
     .venv\Scripts\activate
     ```

4. **Install dependencies**:

   ```bash
   uv add "mcp[cli]" requests python-dotenv notion-client
   # or, if you prefer pip:
   pip install mcp[cli] requests python-dotenv notion-client
   ```

5. **(Optional)** pin dependency versions in `requirements.txt` and install:

   ```text
   requests>=2.31.0
   python-dotenv>=1.0.0
   mcp[cli]>=1.4.0
   notion-client>=2.3.0
   ```

   ```bash
   pip install -r requirements.txt
   ```

---

## Configuration

Create a `.env` file in the project root with the following keys:

```dotenv
GITHUB_TOKEN=your_github_token
NOTION_API_KEY=your_notion_api_key
NOTION_PAGE_ID=your_notion_page_id
```

* **GITHUB\_TOKEN**: a classic Personal Access Token with `repo` and `repo_hook` scopes.
* **NOTION\_API\_KEY** & **NOTION\_PAGE\_ID**: from your Notion internal integration.

---

## Project Structure

```text
pr_reviewer/
├── .env
├── github_integration.py     # Fetches PR metadata & diffs
├── pr_analyzer.py            # MCP server: tools & run loop
├── requirements.txt          # (optional) pinned deps
├── README.md                 # ← this file
└── .venv/                    # virtual environment
```

---

## Usage

Once configured:

```bash
# Activate your venv if not already active
source .venv/bin/activate

# Start the MCP server
python pr_analyzer.py
```

Then, from **Claude Desktop** (or any MCP client), invoke the registered tools:

1. **Fetch a PR**

   ```json
   { "tool": "fetch_pr", "args": ["owner", "repo", 42] }
   ```
2. **Create a Notion page**

   ```json
   { "tool": "create_notion_page", "args": ["PR-42 Review", "Here’s the summary…"] }
   ```

The server will stream results over stdio (or your chosen transport) back to the LLM client.

---

## Environment Variables

| Variable         | Description                                         |
| ---------------- | --------------------------------------------------- |
| `GITHUB_TOKEN`   | GitHub Personal Access Token (`repo`, `repo_hook`)  |
| `NOTION_API_KEY` | Notion integration secret                           |
| `NOTION_PAGE_ID` | ID of the page or database where reviews are posted |

---

## Extending & Contributing

* **Add new MCP tools** by decorating methods with `@self.mcp.tool()` in `pr_analyzer.py`.
* **Support other transports** (WebSockets, HTTP-SSE) via `self.mcp.run(transport="…")`.
* **Pull requests** are welcome! Please follow standard Python coding conventions and update this README as needed.

---

## License

This project is released under the MIT License. See [LICENSE](LICENSE) for details.

```

Feel free to tweak sections (e.g. examples, badges, license) to match your preferences. Let me know if you need anything else!
::contentReference[oaicite:0]{index=0}
```
