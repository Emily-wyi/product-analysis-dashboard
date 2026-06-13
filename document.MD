# Product Analysis Dashboard — Full Setup Guide

This guide walks you through everything from zero: creating a virtual environment in
VS Code, building the file structure, installing dependencies, and running the app.
Commands are for **Windows PowerShell** (the VS Code default terminal on Windows).

---

## 1. Prerequisites

- **Python 3.9+** installed — check with:
  ```powershell
  python --version
  ```
- **VS Code** installed.
- An **OpenAI API key** — get one at https://platform.openai.com/api-keys

---

## 2. Open the project folder in VS Code

1. Create (or choose) a folder, e.g. `product-analysis-dashboard`.
2. In VS Code: **File → Open Folder…** and select it.
3. Open a terminal: **Terminal → New Terminal** (it opens in the project folder).

---

## 3. Create and activate a virtual environment

### What is a Virtual Environment? (Simple Explanation)

Think of a virtual environment like a **separate container or sandbox** on your computer, specifically for this project. Here's why it matters:

- **Project Isolation:** Different Python projects can need different versions of tools/libraries. A virtual environment keeps them separate so they don't interfere with each other. It's like having separate toolboxes for different workshops.
- **Clean Uninstall:** When you're done with a project, you just delete the `.venv` folder. Everything related to that project is gone — no mess left behind on your computer.
- **Reproducibility:** Other people (or you on another computer) can set up the exact same environment by running one command.

### Creating the Virtual Environment with VS Code (Easiest Method)

1. In VS Code, press **Ctrl + Shift + P** (Windows/Linux) or **Cmd + Shift + P** (macOS).
2. Start typing **"Python: Create Environment"** and select it from the dropdown.
3. Select **"Venv"** as the environment type.
4. Choose your Python interpreter version from the list (usually the newest one is fine).
5. VS Code will automatically create a `.venv` folder in your project and activate it.

Your terminal prompt should now start with `(.venv)` — that means the virtual environment is active.

### Alternative: Using Command Line

If the VS Code method doesn't work, you can create it manually:

```powershell
# Create the virtual environment (folder named .venv)
python -m venv .venv

# Activate it
.\.venv\Scripts\Activate.ps1
```

> **If activation is blocked** with a "running scripts is disabled" error, run this once, then activate again:
> ```powershell
> Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
> ```

> **Tip:** After activating, press `Ctrl+Shift+P` → **Python: Select Interpreter** → pick the one inside `.venv` so VS Code always uses it.

---

## 4. Create the file structure

The project is just a few files in one folder:

```
product-analysis-dashboard/
├── .venv/              # virtual environment (created above)
├── app.py              # the Streamlit app + single OpenAI agent
├── requirements.txt    # Python dependencies list
├── .env.example        # template for your API key (safe to share)
├── .env                # your real API key (NEVER share this)
├── .gitignore          # files git should ignore (don't upload)
├── README.md           # short overview
└── document.MD         # this guide
```

To create the empty files from the terminal (then paste in the contents):

```powershell
New-Item app.py
New-Item requirements.txt
New-Item .env.example
New-Item .gitignore
New-Item README.md
New-Item document.MD
```

**Contents for each file:**

**requirements.txt**
```
openai
streamlit
python-dotenv
```

**.env.example**
```
OPENAI_API_KEY=sk-your-key-here
```

**.gitignore**
```
.venv/
.env
__pycache__/
*.pyc
```

(`app.py`, `README.md`, and this `document.MD` are already filled in for you in this repo.)

---

## 5. Install dependencies

### What Does `pip install -r requirements.txt` Do? (Simple Explanation)

**pip** is Python's package manager — think of it like an **app store for code libraries**. Instead of building everything from scratch, Python developers share pre-built tools that anyone can download and use.

**requirements.txt** is a **shopping list** of tools your project needs. When you run `pip install -r requirements.txt`, it's like saying: *"Go to the app store, download every tool listed in this file, and install them for me."*

In our case, we need:
- **openai** — lets your Python code talk to OpenAI's AI models
- **streamlit** — lets you build a pretty web dashboard without knowing web design
- **python-dotenv** — safely loads your API key from the `.env` file

To install them:

```powershell
pip install -r requirements.txt
```

You'll see a bunch of text scrolling — that's pip downloading and installing each library and all the pieces they depend on. This might take a minute or two.

---

## 6. Set your OpenAI API key

### What is an API Key? (Simple Explanation)

An **API key** is like a **password that proves to OpenAI that you're allowed to use their AI**. It's unique to you. Anyone with your key can use your account and spend your money, so **never share it**.

### How to set it up:

```powershell
# Copy the template
copy .env.example .env
```

Open `.env` in VS Code and replace the placeholder with your real key:

```
OPENAI_API_KEY=sk-...your-actual-key...
```

> `.env` is listed in `.gitignore`, so your key is never accidentally pushed to GitHub.

---

## 7. Understanding .gitignore (Simple Explanation)

**Git** is a system that tracks changes to your code and lets you upload it to GitHub. But some files should **never** be uploaded:

- **.env** — contains your secret API key (never share!)
- **.venv/** — the virtual environment folder is huge and specific to your computer
- **__pycache__/** — temporary files Python creates automatically
- **\*.pyc** — compiled Python files you don't need

**.gitignore** is a **blocklist** that tells Git: *"If you see these files or folders, ignore them. Don't track them. Don't upload them."*

When you add files to GitHub, Git checks `.gitignore` first:
- Files NOT in the list → uploaded to GitHub ✅
- Files IN the list → ignored, stays on your computer only ✅

This keeps your code clean and your secrets safe.

---

## 8. Run the app

```powershell
streamlit run app.py
```

Streamlit will print a local URL (usually http://localhost:8501) and open your browser automatically.

---

## 9. Use it

1. Type a product name (e.g. `smart water bottle`).
2. Click **Analyze Product**.
3. Wait a few seconds — the analysis report appears in an expandable section.

To stop the app, press `Ctrl+C` in the terminal.

---

## 10. Push to GitHub (Reference)

The repo was published with the GitHub CLI:

```powershell
git init
git add .
git commit -m "Initial commit: product analysis dashboard"
gh repo create product-analysis-dashboard --public --source=. --remote=origin --push
```

---

## 11. Troubleshooting

| Problem | Solution |
|---------|----------|
| ".venv is not recognized" | Make sure you activated the environment: `.\.venv\Scripts\Activate.ps1` |
| "pip command not found" | Python isn't installed or not in your PATH. Reinstall Python. |
| "ModuleNotFoundError" when running the app | Run `pip install -r requirements.txt` again. |
| "Script disabled" error during activation | Run `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned` |
| API key doesn't work | Check that `.env` has your real key and the file is saved. |

---

## How it works (in one paragraph)

`app.py` loads your API key from `.env`, then `analyze_product()` makes a **single**
`client.chat.completions.create` call to OpenAI (`gpt-4o-mini`) with a system prompt that
frames the model as a senior product/business analyst and a user prompt asking for a full
product analysis report. The returned markdown is shown in the Streamlit dashboard. No
CrewAI, no multi-agent orchestration — one model, one call.
