---
name: guide-creator
description: This skill should be used when the user asks to "create a guide", "build a mega-guide", "write a 100x guide", "create a comprehensive guide", "build a curriculum", "write a multi-part guide", or wants to author large-scale technical reference guides using the diverge-converge method with parallel agents.
---

## Overview

The guide-creator skill produces large-scale technical reference guides (like the 100x Engineer Guide series) using a **diverge-converge** workflow with parallel agents. It orchestrates the full lifecycle: brainstorming the curriculum, designing the spiral structure, dispatching parallel agents to write chapters, converging to weave cross-references, refining with targeted agents, and doing a final consistency pass.

The key insight: 10 agents writing independently for 25 minutes produces more content than one agent writing sequentially for hours — but the convergence step is what turns 10 independent documents into one cohesive guide.

## The Diverge-Converge Method

```
Phase 0: RESEARCH & SOURCE GATHERING
  Ask user for: sample curricula, docs, repos, Slack, Notion, articles
  Research independently: arXiv, Medium, StackOverflow, GitHub, official docs

Phase 1: DESIGN
  Brainstorm curriculum → clarify scope → propose approaches → design spiral structure

Phase 2: DIVERGE ROUND 1 — WRITE (parallel agents)
  Dispatch N agents (typically 8-12), each writing 1-2 Parts
  Each agent gets: full curriculum context, chapter details, spiral connections, format spec
  Agents write independently with no shared state

Phase 3: CONVERGE ROUND 1 — WEAVE (centralized)
  Review all output, fix cross-references, ensure consistent voice
  Verify spiral connections actually match between independently-written chapters
  Fix formatting inconsistencies, update README

Phase 4: GAP ANALYSIS & IDEATION (centralized)
  Identify gaps: thin chapters, missing topics, underserved concepts
  Ideate: suggest new topics, deeper coverage, additional examples, new spiral threads
  Present findings to user: "here's what's missing, here's what I'd add, here's what I'd deepen"
  Get user approval on which gaps to fill and which ideas to pursue

Phase 5: DIVERGE ROUND 2 — ENRICH (targeted agents)
  Dispatch refinement agents for approved gap-fills and enrichments
  Focus on: thin chapters, missing code examples, new topics, deeper coverage
  Each agent gets: existing chapter content + specific enrichment instructions

Phase 6: CONVERGE ROUND 2 — INDEX & COHESION (final pass)
  Final centralized review for quality and polish
  Update all cross-references and spiral connections for new/changed content
  Rebuild the README index: chapter tables, spiral map, reading paths
  Verify cohesion: does the guide read as one voice, one narrative, one spiral?
  Commit and present results
```

## Guide Creation Workflow

### Phase 0: Research & Source Gathering

Before designing the curriculum, gather reference materials. **Ask the user** for:

1. **Sample curricula** — existing courses, guides, syllabi, or outlines they want to draw from (URLs, files, repos)
2. **Documentation** — official docs, internal wikis, Notion pages, Confluence, Google Docs
3. **GitHub repos** — relevant codebases, example projects, starter templates
4. **Slack/Discord channels** — team discussions about the topic (links to channels with relevant history)
5. **Existing content** — blog posts, articles, video courses, conference talks they like

Then **research independently** using web search and web fetch:
- Academic papers and research (arXiv, Google Scholar, Semantic Scholar)
- Industry articles (Medium, dev.to, Hacker News, company engineering blogs)
- StackOverflow top questions and answers for each major topic
- GitHub trending repos and awesome lists for the domain
- YouTube/conference talks from domain experts
- Official documentation for all tools/frameworks/libraries in scope
- Competitor guides and courses (Frontend Masters, Udemy, Coursera, O'Reilly)

Compile research into a **knowledge base** organized by topic area before designing the curriculum. This ensures the chapter content is grounded in real, current information — not just what the model remembers from training.

### Phase 1: Design the Curriculum

Use the brainstorming skill to design the guide structure:

1. **Explore context** — check existing guides, related materials, reference content gathered in Phase 0
2. **Clarify scope** — who is the reader? what do they need to accomplish? what's the depth?
3. **Propose approaches** — present 2-3 structural options with trade-offs
4. **Design the spiral** — define how concepts revisit at increasing depth across Parts
5. **Present full chapter breakdown** — every chapter with title, key concepts, spiral connections
6. **Get approval** — user confirms the structure before any writing begins

The curriculum design should capture:
- **Parts**: logical groupings of chapters (typically 3-7 chapters each)
- **Phases**: macro groupings (e.g., "Get Dangerous" / "Become an Expert")
- **Spiral connections**: how each chapter spirals back to earlier concepts and forward to later ones
- **Language per chapter**: which programming language (TypeScript, Python, etc.)
- **Difficulty progression**: within and across Parts

### Phase 2: Scaffold and Dispatch

1. **Create the directory** and initialize git:
   ```bash
   mkdir -p <guide-name>
   cd <guide-name>
   git init
   ```

2. **Create the folder structure** — one directory per Part, plus appendices and course dirs

3. **Write the README.md** — the master index with chapter tables, spiral map, reading paths

4. **Commit the scaffolding**

5. **Dispatch parallel agents** — one Agent per 1-2 Parts. Each agent's prompt must include:
   - What this guide is and who it's for
   - The chapter format (metadata comment, intro, sections, code examples)
   - Their specific chapter assignments with full details (title, key concepts, spiral connections)
   - Writing style guidelines (tone, line count targets, code language)
   - The within-Part spiral (how their chapters connect to each other)

   **Critical agent prompt requirements:**
   - Self-contained: the agent has ZERO context from the conversation, brief them completely
   - Specific: include every chapter title, every key concept, every spiral connection
   - Format-explicit: show the exact metadata format and chapter structure
   - Code-language-explicit: tell them TypeScript or Python per chapter

6. **Run all agents in background** (`run_in_background: true`)

### Phase 3: Converge Round 1 — Weave

Once all agents complete:

1. **Commit all content** — one big commit with the diverge output
2. **Dispatch convergence agents** (typically 3 in parallel):
   - Agent A: Audit Phase 1 chapters (cross-refs, spiral connections, voice, format)
   - Agent B: Audit Phase 2 chapters (cross-refs, spiral connections, appendices)
   - Agent C: Verify README index, spiral map accuracy, Part READMEs
3. **Cross-reference accuracy** — do "Related Chapters" sections point to chapters that exist with the right content?
4. **Spiral connection verification** — when Ch 14 says "spirals back to Ch 1 (embeddings)", does Ch 1 actually introduce embeddings?
5. **Voice consistency** — all chapters should sound like the same author. Fix tonal mismatches
6. **Format consistency** — every chapter follows the metadata format, section numbering, code style
7. **Update README** — fix chapter descriptions that don't match actual content
8. **Commit the convergence fixes**

### Phase 4: Gap Analysis & Ideation

This is the critical step between convergence and enrichment. **Do not skip this.**

1. **Identify gaps** by auditing the guide for:
   - **Thin chapters** — any chapter significantly below the target line count
   - **Missing topics** — concepts the curriculum promised but chapters didn't deliver
   - **Underserved spiral threads** — concepts that were supposed to appear N times but only appear N-2
   - **Missing code examples** — sections that explain a concept but don't show working code
   - **Dead-end chapters** — chapters that don't connect forward to anything

2. **Ideate new content** — suggest improvements:
   - **New topics** — subjects that emerged during writing but weren't in the original curriculum
   - **Deeper coverage** — sections that deserve their own subsection or even their own chapter
   - **Additional examples** — real-world case studies, production patterns, war stories
   - **New spiral threads** — concepts that could benefit from being revisited across more chapters
   - **Cross-cutting concerns** — patterns that should be consistent across chapters (e.g., error handling, testing approaches)

3. **Present findings to user** in a structured format:
   ```
   ## Gaps Found
   - Ch 22 (Telemetry): only 600 lines, missing Datadog integration code
   - Ch 33 (Skills Internals): no working Skillify implementation
   - Spiral thread "SECURITY" only appears in 4 chapters, target was 6

   ## Ideas for Enrichment
   - Add a chapter on "AI for Non-Engineers" (bridges Ch 26 and Ch 62)
   - Ch 48 (Security) could use the McKinsey hack as a full case study
   - Cross-cutting: all TS chapters should use the same error handling pattern

   ## Suggested New Spiral Threads
   - TESTING: could thread from Ch 19 → Ch 21 → Ch 51 → Ch 56
   ```

4. **Get user approval** — the user decides which gaps to fill and which ideas to pursue. Don't enrich everything — focus on what matters most.

### Phase 5: Diverge Round 2 — Enrich

Dispatch targeted enrichment agents for the approved items from Phase 4:

- **Gap-fill agents** — expand thin chapters with real code and deeper explanations
- **New content agents** — write new sections or subsections for approved ideas
- **Consistency agents** — ensure cross-cutting patterns (error handling, imports, testing) are uniform
- **Spiral-strengthening agents** — add references and content to underserved spiral threads

Each agent gets the EXISTING chapter content plus specific enrichment instructions. They edit files in place — they don't rewrite from scratch.

### Phase 6: Converge Round 2 — Index & Cohesion

The final pass that turns a collection of chapters into a cohesive guide:

1. **Update all cross-references** — any new content from Phase 5 needs to be reflected in Related Chapters sections
2. **Rebuild the spiral map** in README.md — verify every thread is accurate given enrichments
3. **Update chapter tables** in README.md — titles, descriptions, difficulty levels
4. **Update Part READMEs** — reflect any new sections or restructured content
5. **Cohesion read** — skim key transition points:
   - First chapter of each Part (does it bridge from the previous Part?)
   - Last chapter of Phase 1 → first chapter of Phase 2 (does the phase transition work?)
   - Capstone chapter (does it tie everything together?)
6. **Verify code consistency** — same import patterns, same SDK versions, same error handling
7. **Final commit and present results** — summary of total chapters, lines, spiral threads, and any remaining items for future work

## Chapter Format Specification

Every chapter in the guide follows this format:

```markdown
<!--
  CHAPTER: N
  TITLE: Chapter Title
  PART: X — Part Name
  PHASE: N — Phase Name
  PREREQS: Chapter N-1 or None
  KEY_TOPICS: topic1, topic2, topic3
  DIFFICULTY: Beginner | Intermediate | Advanced | Beg→Inter | Inter→Adv
  LANGUAGE: TypeScript | Python | Mixed
  UPDATED: YYYY-MM-DD
-->

# Chapter N: Title

> **Part X — Part Name** | Phase N | Prerequisites: ... | Difficulty: ... | Language: ...

[2-3 engaging intro paragraphs, conversational and opinionated]

### In This Chapter
- Section list with brief descriptions

### Related Chapters
- [Ch N: Title](../path/file.md) — how this chapter SPIRALS to/from that one

---

## 1. MAJOR SECTION
### 1.1 Subsection

[Content with:]
- **What it is:** clear definition
- **When to use:** practical guidance
- **Trade-offs:** honest assessment
- **Real-world example:** concrete scenario
- **Code:** working examples in the chapter's language
```

## Writing Style Guidelines

- **Opinionated and conversational** — like talking to a smart colleague, not reading a textbook
- **Real code that works** — not pseudocode, not "implement this yourself"
- **Target ~1000-1500 lines per chapter** — focused, not encyclopedic
- **Code examples in the chapter's specified language** — TypeScript or Python, not both (unless the chapter is "Mixed")
- **Include package install commands** where relevant
- **Cross-reference using spiral connections** — every chapter should reference what it builds on and what builds on it

## Agent Assignment Strategy

For a 60+ chapter guide, assign agents by Part grouping:

| Agent | Assignment | Rationale |
|-------|-----------|-----------|
| 1 | Part 0 + Part 1 | Foundational chapters are tightly coupled |
| 2 | Part 2 | Core technical Part, needs focused attention |
| 3 | Part 3 + Part 4 | Knowledge + Quality are complementary |
| 4 | Part 5 | Tooling Part, distinct from code-heavy Parts |
| 5 | Part 6 | Dense architectural content |
| 6 | Part 7 | ML fundamentals, Python-specific |
| 7 | Part 8 + Part 9 | Open source + fine-tuning are a natural pair |
| 8 | Part 10 | Production concerns |
| 9 | Part 11 | Infrastructure/deployment |
| 10 | Part 12 + Appendices | Capstone + reference material |

Adjust based on Part sizes. The goal: ~4-8 chapters per agent, no agent has more than ~10.

## Spiral Curriculum Design

The fractal spiral works at three levels:

1. **Within each Part** — each chapter deepens the previous chapter in that Part
2. **Between Parts in a Phase** — later Parts spiral back to earlier Parts
3. **Between Phases** — Phase 2 revisits everything from Phase 1 at expert depth

When designing the curriculum, create a **spiral map** showing how each core concept travels through the guide:

```
CONCEPT:  Ch N (introduce) → Ch M (use) → Ch P (eval)
          → Ch Q (understand internals) → Ch R (advanced application)
```

Every core concept should appear at least 3 times across the guide at increasing depth.
