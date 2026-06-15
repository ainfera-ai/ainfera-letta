# ainfera-letta — Letta + Ainfera Routing

**Letta (MemGPT) integration + Ainfera Routing.** Stateful agents with persistent memory blocks — inference routed through Ainfera with signed audit per turn.

Internal dogfood: [Namo](https://ainfera.ai/agents) (EU AI Act research synthesis).

## Quickstart (curl only)

No Letta server required — proves the OpenAI-compat layer Letta calls:

```bash
git clone https://github.com/ainfera-ai/ainfera-letta
cd ainfera-letta
./curl-example.sh   # signup → inference → audit verify
```

> **Tip:** Set `AINFERA_BASE_URL` to override the default `https://api.ainfera.ai` — useful for pointing `curl-example.sh` at a staging or local endpoint (e.g. `AINFERA_BASE_URL=http://localhost:8000 ./curl-example.sh`).

## Quickstart (OpenAI SDK — same transport Letta uses)

```bash
pip install -r requirements.txt
export AINFERA_API_KEY=ainfera_...  # https://app.ainfera.ai/signup
python main.py
```

## Letta server + Ainfera

Point Letta's OpenAI-compatible provider at Ainfera:

```bash
export OPENAI_API_KEY="$AINFERA_API_KEY"
export OPENAI_BASE_URL="https://api.ainfera.ai/v1"
# then start your Letta server / agent per https://docs.letta.com
```

When creating agents via `letta-client`, pass an explicit `llm_config` so the proxy URL is honored:

```python
llm_config={
    "model": "ainfera-inference",
    "model_endpoint_type": "openai",
    "model_endpoint": "https://api.ainfera.ai/v1",
    "context_window": 200000,
}
```

> Letta upstream has known issues propagating `OPENAI_BASE_URL` to all code paths — explicit `model_endpoint` is the reliable pattern.

## What this shows

- One Agent Card across providers (L1)
- Routed inference with per-call budget caps (L3)
- Hash-chained receipts per LLM turn (L4)
- Memory blocks (Letta) + audit chain (Ainfera) — complementary layers

## Other frameworks

- [ainfera-mcp](https://github.com/ainfera-ai/ainfera-mcp) — Claude Desktop + Cursor
- [ainfera-langchain](https://github.com/ainfera-ai/ainfera-langchain) — LangGraph-style chains
- [ainfera-crewai](https://github.com/ainfera-ai/ainfera-crewai) — multi-agent crews
- [ainfera-openai-compatible](https://github.com/ainfera-ai/ainfera-openai-compatible) — universal wedge

Apache 2.0. © Ainfera Inc. 2026.
