# MoJo — Multi-Agent Orchestration System

> **MoJo Score = Output Tokens ÷ Human Input Seconds**
>
> Maximum intelligence from minimum human effort.

A live FastAPI web app that orchestrates three Claude agents **in parallel** on every query,
then synthesizes their outputs into one sharp brief — all in a single web request.

---

## Live Demo

```bash
pip install -r requirements.txt
export ANTHROPIC_API_KEY=sk-...
python app.py
# → http://localhost:8000
```

---

## How It Works

```
User Query
    │
    ├──▶ Research Agent     (claude-haiku)  ─┐
    ├──▶ Strategy Agent     (claude-haiku)  ─┼─▶ Synthesis Agent (claude-haiku) ─▶ MoJo Score
    └──▶ Devil's Advocate   (claude-haiku)  ─┘
```

1. **Three specialized agents fire concurrently** via `asyncio.as_completed` — no sequential waiting.
2. Each agent has a distinct system prompt tuned for its role (research, strategy, critique).
3. Results stream back to the browser via **Server-Sent Events** as each agent finishes.
4. A **synthesis agent** integrates all three perspectives into one actionable brief.
5. **MoJo Score** is displayed: `total_output_tokens / seconds_user_spent_typing`.

---

## My MoJo Setup

| Layer | Tool |
|---|---|
| **Orchestration harness** | Claude Code (agent scaffolding + file ops) |
| **Model** | `claude-haiku-4-5` — fastest Claude, ideal for parallel fan-out |
| **Parallel execution** | Python `asyncio` + `AsyncAnthropic` client |
| **Streaming** | FastAPI `StreamingResponse` + SSE → browser `ReadableStream` |
| **Frontend** | Vanilla JS + CSS (zero build tools, instant iteration) |
| **Workflow** | Spec → Claude Code scaffolds → iterate in browser preview → ship |

### Why this achieves a high MoJo Score

- **Parallel agents** cut wall-clock latency by ~3x vs sequential calls.
- **Haiku model** gives fast, cheap tokens — maximizing output/cost.
- **SSE streaming** means the user sees results as they arrive, not after all agents finish.
- **Claude Code** handled boilerplate instantly, keeping human time near zero.

---

## Stack

- **Backend:** Python, FastAPI, Anthropic SDK (async)
- **Frontend:** HTML/CSS/JS (no framework)
- **AI:** Anthropic Claude (`claude-haiku-4-5`)
- **Transport:** Server-Sent Events (SSE)

---

*Built by Venkata Rahul Murarisetty — venkatarahul107@gmail.com*
