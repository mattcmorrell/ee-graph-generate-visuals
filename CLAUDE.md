# Generative Visuals — EE Graph Experiment

> **Before starting work, read `../KNOWLEDGE.md`** for the three-experiment research framework. This project is **Experiment 3: Generative Visuals**. The question is whether AI can generate good visuals from scratch with no component library.

## The Rule

**No component library. No pre-built renderers. No templates.** The AI gets graph data and a div. It generates the complete HTML/CSS for every visual from scratch. If you find yourself building reusable components or a renderer dispatch, you're doing Experiment 2, not Experiment 3. Stop and switch projects.

## What This Is

A minimal test harness: user asks a question, server queries the graph, AI generates a self-contained HTML visual, page renders it. Each generated visual is displayed in a feed so you can compare results across different questions.

## Tech Stack

- **Server:** Node.js + Express, port 3458
- **LLM:** OpenAI SDK (gpt-4o) + dotenv
- **Frontend:** Single HTML page, no JS framework, no component system
- **Data:** EE graph fetched from GitHub Pages at startup

## Data Source

Acme Co Employee Experience Graph (~648 nodes, ~3,104 edges).

- Nodes: `https://mattcmorrell.github.io/ee-graph/data/nodes.json`
- Edges: `https://mattcmorrell.github.io/ee-graph/data/edges.json`

## Key Files

- `server.js` — Express server, graph loading, query tools, `/api/generate` SSE endpoint, system prompt
- `public/index.html` — Input, suggestion chips, result feed

## How to Evaluate Results

For each generated visual, ask:
1. **Readable?** Can you understand it in 5 seconds?
2. **Accurate?** Does it use real data from the graph, no hallucinated names or numbers?
3. **Well-designed?** Is the layout, typography, and use of space decent — or does it look like AI slop?
4. **Appropriate?** Did the AI pick a good visual format for this type of question?
5. **Compared to a primitive?** For questions that Experiment 2's components could also answer, how does this compare?
