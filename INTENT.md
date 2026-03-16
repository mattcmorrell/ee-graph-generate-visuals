# INTENT: Generative Visuals

## Goal

Test whether AI can generate usable, trustworthy visuals from scratch — no component library, no pre-built renderers, just data in, visual out.

This is the Tier 3 question from the strategic vision: when no pre-built component fits, can the AI create something good enough? And where's the boundary between "primitives are better" and "generation is necessary"?

## Current Direction

Minimal test harness. User types a question, AI queries the graph, generates complete HTML/CSS for a single visual. Results display in a feed for comparison. System prompt includes atomic design patterns for consistent micro-level rendering while leaving layout fully generative.

## What's Done

- Project seeded with server, graph layer, single-page frontend
- System prompt constrains AI to generate self-contained HTML with inline styles
- 6 suggestion chips covering a range of question types
- SSE streaming for status updates during generation
- Dark/light mode toggle with theme passed to AI so generated visuals respect current mode
- Upgraded to gpt-5.4 (flagship model)
- Added `query_people` tool for bulk filtering/aggregation by location, status, level, role, department
- Added `analyze_people` tool for workload analysis (projects, reports, mentees, skills per person)
- Raised tool call limit from 5 to 15 for complex multi-hop questions
- Added no-placeholder rule — AI must use real data or explain what it can't answer
- **Added atomic design patterns to system prompt** — 6 locked-down micro-patterns (person lockup, stat block, section block, tag/chip, data row, bar/proportion) plus layout principles. This measurably improved visual consistency and quality.
- **Click-to-expand drill-down** — person lockups in generated visuals are clickable. Clicking fetches direct reports via a lightweight `/api/reports` endpoint and renders them inline (no AI round-trip). Click again to collapse. Deduplicates against people already shown in the AI-generated card.

## Key Findings

### Core hypothesis: VALIDATED
AI can generate good, usable single-card visuals from scratch. It picks appropriate visual formats for different question types and uses real graph data accurately when given the right tools.

### Surprise finding: atomic patterns are the unlock
The boundary isn't "primitives for common cases, generation for the long tail" as originally framed. The real insight is that **atomic design tokens + generative layouts** outperforms both Experiment 2's full-component approach and Experiment 3's pure-generation approach.

- The AI is excellent at deciding *what* to show and *how* to arrange it (layout, composition, information selection)
- The AI is inconsistent at micro-level rendering of common data types (person references, stat formatting, section hierarchy)
- Tiny locked-down patterns (person lockup, stat block, section, tag, data row, bar) fix the inconsistency without constraining layout creativity
- This is a synthesis of Experiments 2 and 3 that KNOWLEDGE.md didn't anticipate

### Tool gaps matter
The AI generates placeholder mockups when it can't get the data it needs. The fix is better tools, not better prompting. Added `query_people` (bulk filter/aggregate) and `analyze_people` (workload metrics) to handle aggregate and multi-hop questions.

### Three layers, not two
Atomic patterns solved micro-level consistency (every person lockup looks the same). But **macro-level layout is still unpredictable** — the same org chart query produces a nested indented tree one run, and a division-head + 2-column grid + span-of-control bar chart the next. Both are good, but different every time.

This reveals three distinct layers of consistency:
1. **Atomic patterns** (solved) — person lockup, stat block, tag, bar. Micro-level rendering of data types.
2. **Layout archetypes** (unsolved) — structural recipes for common question types. "An org chart uses nested indented rows." "A comparison uses side-by-side columns." Bigger than an atom, smaller than a full pre-built card.
3. **Pure generation** (working) — for the long tail where no archetype fits.

Layer 2 is the open question. It could be solved with prompt-level structural guidance (like we did for atomic patterns), or it could be the argument that Experiment 2's primitives earn their keep — not as full cards, but as layout archetypes.

### Dark mode needs explicit guidance
Telling the AI "dark mode" isn't enough. It needs specific background color ranges for inner sections to maintain contrast against the card background.

## Rejected Approaches

- **Pure free-form generation with no design guidance** — produces visuals that are structurally fine but visually inconsistent. Person names as plain text, stats in random formats, sections that blend into card backgrounds. Atomic patterns solved this.
- **Placeholder/mockup fallback** — the AI would generate mockup wireframes when it couldn't query the data. Fixed by adding a no-placeholder rule and better tools.

## Open Questions

- How do the atomic-pattern-enhanced results compare side-by-side with Experiment 2's primitives on the same questions?
- Does this finding change the architecture of the combined system? (Progressive disclosure + atomic patterns + generative layouts, rather than progressive disclosure + pre-built primitives + generative fallback)
- Would non-developer users perceive the atomic-pattern visuals as trustworthy and professional?
- Could the atomic patterns be extracted into a shared design token system that both Experiments 2 and 3 use?
- **Layer 2 — layout archetypes:** Should structural recipes for common question types (org chart = nested tree, comparison = side-by-side columns) be defined in the prompt, or is this where actual primitives/components earn their keep? This is the gap between atomic patterns and full pre-built cards.

## Next Steps

1. Run side-by-side comparison: same 5 questions through Experiment 2 primitives and Experiment 3 generative (with atomic patterns)
2. Get a non-developer to evaluate the generated visuals (KNOWLEDGE.md requires this for conclusive)
3. Update KNOWLEDGE.md experiment framework with the atomic pattern finding
4. Consider whether this changes the combined architecture vision
