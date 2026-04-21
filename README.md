#Context Compression for AI Agents (Graph Memory Module)

A graph-based context compression system for large language models and AI agents. Conversational context is represented as a dynamic knowledge graph where entities become nodes and relationships become edges. An NLP extractor identifies entities and relations, the graph updates incrementally with merged nodes and weighted edges, and relevant subgraphs are retrieved and linearized as compressed context. Achieves 80–90% compression ratio with demonstrated real-time ratios of 4x–21x across multi-turn sessions, while preserving relational structure critical for multi-hop reasoning.
What you get:

A KnowledgeGraph that stores entities (constraints, cities, events, expenses) and edges between them.
An extractor that runs on every user turn (regex-based) to upsert entities and handle invalidation triggers.
A compressor that emits a compact "ACTIVE MEMORY" summary suitable for small-context models.
Simple metrics (token estimate + compression ratio) and a demo evaluation script.
A CLI chat loop to show the module "in action".

Quickstart
Requires Python 3.10+. Run the CLI demo:
python3 -m scripts.chat_cli
Use a pretrained LLM (OpenAI) for responses:
export OPENAI_API_KEY="..."
python3 -m scripts.chat_cli --llm openai --model gpt-4o-mini
Use Gemini for responses:
export GEMINI_API_KEY="..."
python3 -m scripts.chat_cli --llm gemini --model gemini-2.5-flash
Use Gemini for extraction too (more general than regex rules):
python3 -m scripts.chat_cli --llm gemini --extract gemini --model gemini-2.5-flash --show-context
Single-call mode (Gemini does extraction + reply in one API call per user turn):
python3 -m scripts.chat_cli --llm gemini --extract gemini --model gemini-2.5-flash
Storing your key in a local file (recommended for convenience):

Edit .env and set GEMINI_API_KEY=... (this file is ignored by git)
Or copy .env.example to .env

Run the evaluation demo (baseline vs compressed)
python3 -m scripts.eval_demo
Run evaluation on your own conversation file
python3 -m scripts.eval_demo --file /path/to/conversation.json
Run the provided validation suite (tests A–E from the problem statement)
python3 -m scripts.validation_suite
Project Layout

graph_memory/graph.py — graph store (nodes/edges), invalidation cascade, budget recomputation
graph_memory/extractor.py — entity extraction + invalidation detection (runs every user turn)
graph_memory/compressor.py — builds the compressed graph summary
graph_memory/metrics.py — token estimate + compression ratio
graph_memory/memory.py — orchestration (ingest turn, update graph, build contexts)
scripts/chat_cli.py — runnable chatbot interface (rule-based assistant for demo)
scripts/eval_demo.py — quantitative demo report using data/demo_conversation.json
This is intentionally dependency-light (stdlib-only) so it runs anywhere.
Token counting is an estimate (whitespace/punctuation based). You can plug in a real tokenizer later.
