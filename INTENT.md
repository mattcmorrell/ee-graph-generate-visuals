# INTENT: Generative Visuals

## Goal

Test whether AI can generate usable, trustworthy visuals from scratch — no component library, no pre-built renderers, just data in, visual out.

This is the Tier 3 question from the strategic vision: when no pre-built component fits, can the AI create something good enough? And where's the boundary between "primitives are better" and "generation is necessary"?

## Current Direction

Minimal test harness. User types a question, AI queries the graph, generates complete HTML/CSS for a single visual. Results display in a feed for comparison.

## What's Done

- Project seeded with server, graph layer, single-page frontend
- System prompt constrains AI to generate self-contained HTML with inline styles
- 6 suggestion chips covering a range of question types
- SSE streaming for status updates during generation

## Rejected Approaches

_None yet._

## Open Questions

- Does giving the AI a base CSS theme improve consistency, or does it count as "providing a component library"?
- Should we test with different models (gpt-4o vs claude) to see if generation quality varies?
- How do we systematically compare these results against Experiment 2's primitive-based outputs?

## Next Steps

1. Install dependencies and test with the 6 built-in suggestion questions
2. Evaluate: are the generated visuals readable, accurate, well-designed?
3. Try harder questions that no pre-built component would handle
4. Try the same easy questions that Experiment 2 handles — compare quality
5. Document findings: where does generation work, where does it fail?
