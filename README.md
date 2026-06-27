# Product Analysis Dashboard

A simple Streamlit app that analyzes any product using a single AI agent.
Enter a product name and get a full report covering market demand, ideal customer,
marketing strategies, technology/manufacturing feasibility, scalability, revenue
streams, and a launch plan & timeline.

Built with the `openai` Python SDK pointed at [OpenRouter](https://openrouter.ai) — no CrewAI, no multi-agent framework, one API key.

## Quickstart

```powershell
# 1. Create and activate a virtual environment
python -m venv .venv
.\.venv\Scripts\Activate.ps1

# 2. Install dependencies
pip install -r requirements.txt

# 3. Add your OpenRouter API key
copy .env.example .env
# then edit .env and paste your real OPENROUTER_API_KEY

# 4. Run the app
streamlit run app.py
```

For full step-by-step setup instructions (VS Code, virtual environment, file
structure, commands), see [document.MD](document.MD).
