---
name: course-creator
description: This skill should be used when the user asks to "create a course", "build a course", "generate a course", "design a curriculum", "create training material", "build a tutorial", "make a learning path", "create onboarding material", "build exercises", or wants to author hands-on technical courses with progressive exercises and spiral progression.
---

## Overview

The course-creator skill produces hands-on technical courses with progressive exercises, spiral progression, and a warm, expert voice. It orchestrates the full workflow from requirements gathering through content generation — collecting the author's goals, building a structured outline with spiral loops, researching reference materials, and dispatching agents to write individual modules. The output is a complete course directory with lesson content, guided exercises, open-ended challenges, and runnable starter/solution code.

## Course Creation Workflow

Follow these five phases in order. Each phase produces artifacts consumed by subsequent phases.

### Phase 1: Requirements Gathering

Determine the course specification through one of two paths:

- **Interactive interview:** If no `course-spec.yaml` exists, conduct an interactive interview using the question bank in `references/interview-questions.md`. Ask one question at a time, offer concrete examples, and suggest reasonable defaults. Cover topic/scope, target audience, learning objectives, depth/duration, and reference materials.
- **Existing spec file:** If a `course-spec.yaml` already exists at the course path, read and validate it. Present a summary to the author and confirm before proceeding.

Both paths converge on a validated `course-spec.yaml` containing the course name, description, audience, prerequisites, learning objectives, spiral loop structure, source references, and tutoring defaults.

### Phase 2: Outline Generation

Generate a `course.yaml` file that defines the full course structure:

1. Map learning objectives to spiral loops (Foundation, Practice, Mastery per loop).
2. Determine the number of loops based on topic breadth and depth — typically 2-4 loops.
3. Define module titles and assign them to loops, ensuring each loop represents a complete skill level (basic, realistic, production-grade).
4. Verify the outline follows spiral progression principles: each loop revisits core concepts at increasing complexity, scaffolding fades across loops, and every module is a whole-task.
5. Present the outline to the author for review and approval before proceeding.

Follow the `course.yaml` schema defined in `references/output-format.md`.

### Phase 3: Research

Dispatch the `course-researcher` agent to gather and synthesize knowledge from all sources listed in the course spec. See "Using the Agents" below for dispatch details.

Wait for the researcher to return a structured knowledge base organized by spiral loop before proceeding to content generation. Review the knowledge base for completeness — if significant gaps exist for any module, dispatch additional targeted research.

### Phase 4: Content Generation

Dispatch the `module-writer` agent once per module, sequentially or in small batches. Provide each instance with the context it needs (see "Using the Agents" below).

Write modules in order — earlier modules produce episode summaries that later modules reference for cross-linking and continuity. After each module is written, verify it follows the output format spec and voice guidelines.

### Phase 5: Review & Refine

Present the completed course to the author for review:

- Summarize the full course structure (loops, modules, progression).
- Highlight any modules where research was thin or content quality is uncertain.
- Offer to regenerate specific modules with adjusted guidance.
- Accept feedback and re-dispatch the module-writer for any modules that need revision.

## Pedagogical Approach

Apply spiral progression: organize the course into loops where each loop cycles through Foundation, Practice, and Mastery tiers at increasing complexity. Scaffolding fades across loops — Loop 1 provides heavy guidance with detailed steps and visible hints, while Loop 3 provides minimal scaffolding with high-level instructions and learner-driven problem solving. Each module is self-contained with an `episode.md` summary enabling skip-ahead decisions. Refer to `references/pedagogical-framework.md` for the full framework (4C/ID, Bloom's, Spiral Curriculum, Vygotsky's ZPD, Dreyfus, Kolb) and `references/spiral-progression.md` for detailed scaffolding fading rules.

## Using the Agents

### course-researcher

**When to dispatch:** After the outline is approved (Phase 3), before content generation begins.

**What context to pass:**
- The complete `course.yaml` outline (loops, modules, themes)
- The full list of sources from the course spec (URLs, repo paths, types)
- Any specific branches or directories the author highlighted
- Knowledge gaps or specific questions to investigate

**Expected output:** A structured knowledge base document organized by spiral loop, containing key concepts, code examples, best practices, common pitfalls, and ecosystem context per source. The module-writer consumes this directly.

### module-writer

**When to dispatch:** After research is complete (Phase 4), once per module.

**What context to pass:**
- The full `course.yaml` outline (so the agent understands this module's position)
- The relevant chunk of the research knowledge base for this module's topics
- Episode summaries from all previously written modules (for cross-references)
- The module's spiral loop number (determines scaffolding level)
- The course directory path for file creation

**Expected output:** A complete module directory containing `episode.md`, `lesson.md`, `exercise.md`, `challenge.md`, `starter/`, and `solution/`. The agent reads the voice, philosophy, format, and progression references on its own before writing.

## Voice & Tone

All generated course content must follow "The Nerdy Friend" persona — warm, funny, self-aware, passionate about the subject, and genuinely supportive. Use conversational language, acknowledge difficulty honestly, celebrate small wins, and never condescend. The tone adjusts across tiers: patient and clear in lessons, energetic and collaborative in exercises, confident and respectful in challenges. Refer to `references/voice-and-tone.md` for the complete voice spec, including callout formats (Pro Tip, The Bigger Picture, Ecosystem Note, Hint Layering).

## Output Format

Each course produces a directory under `courses/<course-name>/` containing:

- `course.yaml` — Course metadata, learning objectives, spiral loop definitions, source references, and tutoring defaults.
- Numbered module directories (`01-<name>/`, `02-<name>/`, etc.) each containing: `episode.md` (skip-ahead summary), `lesson.md` (Foundation), `exercise.md` (Practice), `challenge.md` (Mastery), `starter/` (runnable starting code), and `solution/` (complete reference solution).
- Optional `capstone/` directory with `brief.md`, `starter/`, and `solution/`.

Follow the naming conventions and file schemas defined in `references/output-format.md`.

## Additional Resources

| Reference File | Description |
|---|---|
| `references/voice-and-tone.md` | The Nerdy Friend persona — complete voice spec with do/don't rules, tone-per-tier examples, and callout formats |
| `references/learning-philosophy.md` | Core learning philosophy — no gatekeeping, practical skills focus, hints-not-suffering approach |
| `references/pedagogical-framework.md` | Six integrated frameworks: 4C/ID, Bloom's Taxonomy, Spiral Curriculum, Vygotsky's ZPD, Dreyfus Model, Kolb's Experiential Learning |
| `references/output-format.md` | Exact file structure spec — course.yaml schema, module file templates, scaffolding-by-loop table |
| `references/spiral-progression.md` | How spiral loops build on each other — concept compounding, scaffolding fading rules, cross-loop references, loop count guidance |
| `references/interview-questions.md` | Question bank for interactive requirements gathering — core questions, follow-ups, and spec generation |
| `agents/course-researcher.md` | Agent spec for research — fetches URLs, explores repos, performs web searches, produces structured knowledge base |
| `agents/module-writer.md` | Agent spec for writing — creates one complete module with all files following voice, format, and progression references |
