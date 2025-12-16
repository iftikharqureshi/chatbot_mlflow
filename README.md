# Chatbot with MLflow Tracking

Guide for reviewing the notebook-based chatbot that streams conversations to MLflow.

## Overview
- Simple console chatbot built in `chatbot.ipynb` using OpenAI Chat Completions (`gpt-3.5-turbo`).
- Tracks latency, token counts, and model params in MLflow for each turn.
- Adds minimal terminal styling with Colorama for readable user/assistant prompts.

## Key Components
- `generate_text(conversation, max_tokens)`: calls OpenAI, times the request, counts tokens with `tiktoken`, and logs metrics/params to MLflow.
- Conversation loop: runs inside an `mlflow.start_run()`, appends user/assistant messages, and exits on `exit/quit/q/e`.
- Helpers: `print_user_input` / `print_ai_output` colorize prompts; `count_tokens` wraps `tiktoken`.

## Prerequisites
- Python 3.9+ with `openai`, `mlflow`, `tiktoken`, `colorama`, `ipykernel`.
- OpenAI API key in `OPENAI_API_BOOK_KEY`.
- Running MLflow tracking server (defaults to `http://localhost:5000`); adjust `MLFLOW_URI` in the notebook if needed.

## Quickstart
1) Create env and install deps:
```bash
python -m venv .venv
source .venv/bin/activate
pip install openai mlflow tiktoken colorama ipykernel
```
2) Start or point to an MLflow server (e.g., `mlflow server --backend-store-uri sqlite:///mlflow.db --default-artifact-root ./mlruns --host 0.0.0.0 --port 5000`).
3) Launch Jupyter and open `chatbot.ipynb`:
```bash
jupyter notebook chatbot.ipynb
```
4) Run cells top-to-bottom. At the prompt, type messages; leave with `exit`, `quit`, `q`, or `e`.

## Notes for Reviewers
- Check the `generate_text` cell to see logging details and configurable model knobs (temperature, top_p, penalties, max tokens).
- MLflow experiment defaults to `GenAI_Week9`; runs and metrics appear there automatically via `mlflow.autolog()` plus manual logging.
- The notebook assumes a local MLflow server; if unavailable, either start one or switch to `mlflow.set_tracking_uri("file:./mlruns")` for local-only tracking.
