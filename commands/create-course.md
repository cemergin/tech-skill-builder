---
description: Generate a hands-on technical course with progressive exercises
argument-hint: Topic, or path to existing course-spec.yaml
---

# Create Course

Generate a structured, hands-on technical course using proven pedagogical frameworks.

Initial request: $ARGUMENTS

## Workflow

### Phase 1: Requirements Gathering

If a `course-spec.yaml` path was provided:
1. Read and validate the spec file
2. Present summary for confirmation
3. Proceed to Phase 2

If no spec file:
1. Conduct the interactive interview (see `skills/course-creator/references/interview-questions.md`)
2. Ask one question at a time — topic, audience, objectives, depth, reference materials
3. Generate `course-spec.yaml` from answers
4. Present for approval

### Phase 2: Outline Generation

1. Using the course spec, generate the full course outline
2. Define spiral loops (Foundation → Practice → Mastery cycles)
3. Name and sequence all modules
4. Write the `course.yaml` file
5. Present outline for author review and approval

### Phase 3: Research

1. Dispatch the `course-researcher` agent with all source materials from the spec
2. The agent fetches URLs, reads repositories, explores git branches, and searches the web
3. It produces a structured knowledge base organized by spiral loop
4. Review the knowledge base for completeness

### Phase 4: Content Generation

For each module in the outline:
1. Dispatch the `module-writer` agent with:
   - The full `course.yaml`
   - Episode summaries of all modules (for cross-referencing)
   - The relevant knowledge base chunk
   - The module's position in the spiral (loop number, tier)
2. The agent writes: `episode.md`, `lesson.md`, `exercise.md`, `challenge.md`, `starter/`, `solution/`
3. Review generated content for quality and consistency

### Phase 5: Review & Refine

1. Present the complete course structure to the author
2. Highlight any modules that may need revision
3. Accept feedback and regenerate specific modules if needed

## Key Principles

- Follow the voice and tone spec — "The Nerdy Friend" (see `skills/course-creator/references/voice-and-tone.md`)
- Follow the learning philosophy — no gatekeeping, hints are generous (see `skills/course-creator/references/learning-philosophy.md`)
- Follow the pedagogical framework (see `skills/course-creator/references/pedagogical-framework.md`)
- Follow the output format exactly (see `skills/course-creator/references/output-format.md`)
- Follow the spiral progression guidelines (see `skills/course-creator/references/spiral-progression.md`)
- Each module must be self-contained (skip-ahead enabled)
- Scaffolding fades across loops (heavy → medium → light)
