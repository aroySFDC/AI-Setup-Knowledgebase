# CLAUDE.md

Behavioral guidelines to reduce common LLM coding mistakes. Merge with project-specific instructions as needed.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

---

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, and clarifying questions come before implementation rather than after mistakes.

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository is a guide for setting up a Python project with AI-assisted agentic workflow development. It contains a minimal FastAPI server that proxies chat requests to a local Ollama LLM instance.

## Structure

- `fastapi-ollama/` — FastAPI server module
  - `server.py` — FastAPI app with two endpoints: `GET /` (health check) and `POST /chat` (proxies to Ollama gemma4 model)
- `requirements.txt` — Full dependency list (generated via `pip freeze`)
- `README.md` — Setup instructions for Python venv, Docker Ollama, and Claude CLI integration

## Development Commands

### Environment Setup
```bash
python -m venv .venv
.venv\Scripts\activate  # Windows
pip install openai
pip freeze > requirements.txt
```

### Running the FastAPI Server
```bash
uvicorn fastapi-ollama.server:app --reload
```

### Running Ollama (Docker)
```bash
docker run -d --gpus=all -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama
docker exec -it ollama ollama pull gemma4:latest
```

## Key Implementation Notes

- The server connects to Ollama at `http://localhost:11434`
- Default model is `gemma4:latest` — change in `server.py` if using a different model
- The `POST /chat` endpoint expects a JSON body with a `message` field
- For extended context, create a custom Ollama model with `PARAMETER num_ctx 65000`
