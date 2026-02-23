# ms-chatbot Bootstrap Guide (with uv)

This README walks you through creating and initializing an `ms-chatbot` project using `uv`, including:

- Creating the project
- Setting the Python version
- Creating a virtual environment
- Activating and using that virtual environment

---

## 1) Prerequisites

- macOS, Linux, or Windows
- Internet access for first-time package and Python downloads
- A terminal (Bash, Zsh, Fish, PowerShell, or CMD)

---

## 2) Install `uv`

Use one method below.

### macOS / Linux (installer script)

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Restart your terminal, then verify:

```bash
uv --version
```

### macOS (Homebrew)

```bash
brew install uv
uv --version
```

### Windows (PowerShell + winget)

```powershell
winget install --id=astral-sh.uv -e
uv --version
```

---

## 3) Create the `ms-chatbot` project with `uv`

`uv` creates projects with `uv init`:

```bash
uv init ms-chatbot
cd ms-chatbot
```

This generates a starter Python project with `pyproject.toml`.

If you already created a folder manually, initialize in-place:

```bash
mkdir ms-chatbot
cd ms-chatbot
uv init --name ms-chatbot
```

---

## 4) Set the Python version

Install Python (if needed) and pin the project to a specific version.

### Install Python with `uv`

```bash
uv python install 3.12
```

### Pin Python for this project

```bash
uv python pin 3.12
```

This creates a `.python-version` file so tools consistently use Python 3.12 in this project.

Optional: ensure `pyproject.toml` is explicit:

```toml
[project]
name = "ms-chatbot"
version = "0.1.0"
requires-python = ">=3.12,<3.13"
```

---

## 5) Create the virtual environment

Create a local `.venv` using the pinned interpreter:

```bash
uv venv .venv --python 3.12
```

If Python is already pinned, this also works:

```bash
uv venv .venv
```

---

## 6) Set and activate the virtual environment

### Bash / Zsh (macOS/Linux)

```bash
source .venv/bin/activate
```

### Fish

```fish
source .venv/bin/activate.fish
```

### PowerShell (Windows)

```powershell
.venv\Scripts\Activate.ps1
```

### CMD (Windows)

```cmd
.venv\Scripts\activate.bat
```

After activation, confirm:

```bash
python --version
which python
```

On Windows PowerShell, use:

```powershell
python --version
Get-Command python
```

You should see the interpreter path inside `.venv`.

---

## 7) Install dependencies for `ms-chatbot`

Example runtime packages:

```bash
uv add fastapi uvicorn pydantic python-dotenv openai
```

Example development packages:

```bash
uv add --dev pytest ruff mypy
```

Sync dependencies from `pyproject.toml` and lockfile:

```bash
uv sync
```

---

## 8) Run the project

Recommended (no manual activation needed):

```bash
uv run python -m ms_chatbot
```

If your entry file is `src/ms_chatbot/main.py`:

```bash
uv run python -m ms_chatbot.main
```

If using FastAPI + Uvicorn:

```bash
uv run uvicorn ms_chatbot.main:app --reload --host 0.0.0.0 --port 8000
```

---

## 9) Daily `uv` workflow

- Add package: `uv add <package>`
- Remove package: `uv remove <package>`
- Add dev package: `uv add --dev <package>`
- Sync environment: `uv sync`
- Run command in project env: `uv run <command>`
- Recreate venv: `rm -rf .venv && uv venv .venv && uv sync`

---

## 10) Suggested project layout

```text
ms-chatbot/
├─ .python-version
├─ .venv/
├─ pyproject.toml
├─ uv.lock
├─ README.md
└─ src/
   └─ ms_chatbot/
      ├─ __init__.py
      └─ main.py
```

---

## 11) Troubleshooting

### `uv: command not found`

Restart your terminal after install and ensure `uv` is on `PATH`.

### Wrong Python version is used

Run:

```bash
uv python pin 3.12
uv venv .venv --python 3.12
```

Then reactivate the environment.

### Virtual environment does not activate on PowerShell

If scripts are blocked, set execution policy for current user:

```powershell
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```

Then retry:

```powershell
.venv\Scripts\Activate.ps1
```

### Dependencies seem out of sync

Run:

```bash
uv sync
```

If needed, recreate `.venv` and sync again.

---

## 12) Quick start (copy/paste)

```bash
uv init ms-chatbot
cd ms-chatbot
uv python install 3.12
uv python pin 3.12
uv venv .venv --python 3.12
source .venv/bin/activate
uv add fastapi uvicorn pydantic python-dotenv openai
uv add --dev pytest ruff mypy
uv sync
```

You now have an `ms-chatbot` project initialized with `uv`, pinned to Python 3.12, and running inside a dedicated virtual environment.
