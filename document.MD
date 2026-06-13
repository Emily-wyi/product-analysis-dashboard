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

```powershell
# Create the virtual environment (folder named .venv)
python -m venv .venv

# Activate it
.\.venv\Scripts\Activate.ps1
```

Your prompt should now start with `(.venv)`.

> **If activation is blocked** with a "running scripts is disabled" error, run this
> once, then activate again:
> ```powershell
> Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
> ```

> **Tip:** In VS Code press `Ctrl+Shift+P` → **Python: Select Interpreter** → pick the
> one inside `.venv` so the editor uses it too.

---

## 4. Create the file structure

The project is just a few files in one folder:

```
product-analysis-dashboard/
├── .venv/              # virtual environment (created above)
├── app.py              # the Streamlit app + single OpenAI agent
├── requirements.txt    # Python dependencies
├── .env.example        # template for your API key
├── .env                # your real API key (you create this, not committed)
├── .gitignore          # files git should ignore
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

Contents:

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

With the virtual environment active:

```powershell
pip install -r requirements.txt
```

---

## 6. Set your OpenAI API key

```powershell
# Copy the template
copy .env.example .env
```

Open `.env` in VS Code and replace the placeholder with your real key:

```
OPENAI_API_KEY=sk-...your-actual-key...
```

> `.env` is listed in `.gitignore`, so your key is never pushed to GitHub.

---

## 7. Run the app

```powershell
streamlit run app.py
```

Streamlit will print a local URL (usually http://localhost:8501) and open your browser.

---

## 8. Use it

1. Type a product name (e.g. `smart water bottle`).
2. Click **Analyze Product**.
3. Wait a few seconds — the analysis report appears in an expandable section.

To stop the app, press `Ctrl+C` in the terminal.

---

## 9. Push to GitHub (reference)

The repo was published with the GitHub CLI:

```powershell
git init
git add .
git commit -m "Initial commit: product analysis dashboard"
gh repo create product-analysis-dashboard --public --source=. --remote=origin --push
```

---

## How it works (in one paragraph)

`app.py` loads your API key from `.env`, then `analyze_product()` makes a **single**
`client.chat.completions.create` call to OpenAI (`gpt-4o-mini`) with a system prompt that
frames the model as a senior product/business analyst and a user prompt asking for a full
product analysis report. The returned markdown is shown in the Streamlit dashboard. No
CrewAI, no multi-agent orchestration — one model, one call.
