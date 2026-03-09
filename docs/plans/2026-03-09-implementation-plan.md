# Tech Skill Builder — Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Build a Claude Code plugin with two skills (course-creator, tech-tutor) and two agents (course-researcher, module-writer) for creating and delivering hands-on technical courses.

**Architecture:** Plugin with `.claude-plugin/plugin.json` manifest, two skills with reference docs following progressive disclosure, two agents with focused system prompts, and two slash commands as entry points.

**Tech Stack:** Claude Code plugin system (Markdown + YAML frontmatter), no external dependencies.

---

## Task 1: Plugin Manifest & Directory Scaffold

**Files:**
- Create: `tech-skill-builder/.claude-plugin/plugin.json`
- Create: All empty directories for the plugin structure

**Step 1: Create directory structure**

```bash
cd /Users/cemergin/tech-skill-builder
mkdir -p .claude-plugin
mkdir -p skills/course-creator/references
mkdir -p skills/tech-tutor/references
mkdir -p agents
mkdir -p commands/create-course
mkdir -p commands/learn
```

**Step 2: Write plugin.json**

```json
{
  "name": "tech-skill-builder",
  "description": "Create and deliver hands-on technical courses using proven pedagogical frameworks (4C/ID, Bloom's Taxonomy, Spiral Curriculum). Two skills: course-creator for authoring courses, tech-tutor for interactive learning.",
  "author": {
    "name": "cemergin",
    "email": ""
  }
}
```

**Step 3: Commit**

```bash
git add .claude-plugin/ skills/ agents/ commands/
git commit -m "scaffold: plugin directory structure and manifest"
```

---

## Task 2: Shared Reference — Voice & Tone

**Files:**
- Create: `skills/course-creator/references/voice-and-tone.md`
- Create: `skills/tech-tutor/references/voice-and-tone.md` (same content)

**Step 1: Write voice-and-tone.md**

The "Nerdy Friend" persona spec from the design document. Includes:
- Persona description
- Do / Don't lists
- Example tones for Foundation, Practice, and Mastery tiers
- Callout format examples (Pro tips, Bigger Picture, Ecosystem Context)

**Step 2: Commit**

```bash
git add skills/*/references/voice-and-tone.md
git commit -m "feat: add voice and tone reference (The Nerdy Friend)"
```

---

## Task 3: Shared Reference — Learning Philosophy

**Files:**
- Create: `skills/course-creator/references/learning-philosophy.md`
- Create: `skills/tech-tutor/references/learning-philosophy.md` (same content)

**Step 1: Write learning-philosophy.md**

Core principles:
- No grades, no gatekeeping, no certification theater
- Practical skill + ecosystem awareness + trade-off intuition
- Hints are generous and layered
- Tips appear proactively
- "The bigger picture" zoom-out moments

**Step 2: Commit**

```bash
git add skills/*/references/learning-philosophy.md
git commit -m "feat: add learning philosophy reference"
```

---

## Task 4: Course Creator Reference — Pedagogical Framework

**Files:**
- Create: `skills/course-creator/references/pedagogical-framework.md`

**Step 1: Write pedagogical-framework.md**

Synthesize the research into actionable guidance for the skill:
- 4C/ID: learning tasks, supportive info, JIT info, part-task practice
- Bloom's Revised Taxonomy: 6 cognitive levels mapped to Foundation/Practice/Mastery
- Spiral Curriculum: loops of increasing complexity
- Vygotsky/Scaffolding: heavy guidance early, fading over loops
- Dreyfus: novice → expert calibration
- Kolb: do → reflect → conceptualize → experiment cycle
- How these frameworks combine in practice

**Step 2: Commit**

```bash
git add skills/course-creator/references/pedagogical-framework.md
git commit -m "feat: add pedagogical framework reference"
```

---

## Task 5: Course Creator Reference — Output Format

**Files:**
- Create: `skills/course-creator/references/output-format.md`

**Step 1: Write output-format.md**

Exact specification of generated course structure:
- Directory layout (course.yaml, numbered modules, capstone)
- course.yaml schema (all fields, types, examples)
- episode.md template
- lesson.md structure (concept + examples + ecosystem context)
- exercise.md structure (steps + pro tips + verification)
- challenge.md structure (problem + layered hints)
- starter/ and solution/ conventions
- Naming conventions for modules and files

**Step 2: Commit**

```bash
git add skills/course-creator/references/output-format.md
git commit -m "feat: add course output format reference"
```

---

## Task 6: Course Creator Reference — Spiral Progression

**Files:**
- Create: `skills/course-creator/references/spiral-progression.md`

**Step 1: Write spiral-progression.md**

How to structure progressive difficulty:
- Loop definition: group of modules forming Foundation → Practice → Mastery
- Each loop is a complete whole-task at a specific complexity level
- Scaffolding fading: early loops have more hints, later loops remove guardrails
- Cross-loop building: how concepts compound
- Module independence: each module is self-contained, skip-ahead enabled via episode.md
- Guidelines for how many loops based on topic complexity
- Example progression for a sample course

**Step 2: Commit**

```bash
git add skills/course-creator/references/spiral-progression.md
git commit -m "feat: add spiral progression reference"
```

---

## Task 7: Course Creator Reference — Interview Questions

**Files:**
- Create: `skills/course-creator/references/interview-questions.md`

**Step 1: Write interview-questions.md**

Question bank for the interview phase:
- Core questions (topic, audience, objectives, depth, sources)
- Follow-up questions per category
- How to infer defaults from answers
- How to generate course-spec.yaml from interview answers
- Example interview flow
- course-spec.yaml template

**Step 2: Commit**

```bash
git add skills/course-creator/references/interview-questions.md
git commit -m "feat: add interview questions reference"
```

---

## Task 8: Course Creator SKILL.md

**Files:**
- Create: `skills/course-creator/SKILL.md`

**Step 1: Write SKILL.md**

Frontmatter:
```yaml
---
name: course-creator
description: This skill should be used when the user asks to "create a course", "build a course", "generate a course", "design a curriculum", "create training material", "build a tutorial", "make a learning path", or wants to author hands-on technical courses with progressive exercises.
version: 0.1.0
---
```

Body (~1,500-2,000 words):
- Overview: what the skill does
- The 5-phase workflow: Interview → Outline → Research → Write → Review
- How to use the course-researcher agent
- How to use the module-writer agent
- Quick reference for output format
- Pointers to all reference files

**Step 2: Commit**

```bash
git add skills/course-creator/SKILL.md
git commit -m "feat: add course-creator skill"
```

---

## Task 9: Tech Tutor Reference — Tutoring Modes

**Files:**
- Create: `skills/tech-tutor/references/tutoring-modes.md`

**Step 1: Write tutoring-modes.md**

Detailed specification of each tutoring mode:
- Adaptive: when and how to use, example dialogue patterns
- Socratic: question-asking patterns, hint layering, when to back off
- Strict/Direct: when learner says "just tell me", respect it
- Supportive: detecting struggle, backing up without shame
- Mode switching rules (table from design doc, expanded)
- The "Just Tell Me" escape hatch protocol
- Hint layering format (3 levels: direction → approach → almost-answer)

**Step 2: Commit**

```bash
git add skills/tech-tutor/references/tutoring-modes.md
git commit -m "feat: add tutoring modes reference"
```

---

## Task 10: Tech Tutor Reference — Assessment Rubric

**Files:**
- Create: `skills/tech-tutor/references/assessment-rubric.md`

**Step 1: Write assessment-rubric.md**

How to gauge learner level:
- Quick assessment questions at session start
- How to interpret episode.md prerequisites as self-assessment
- Signals of understanding vs confusion during a session
- When to suggest skipping ahead
- When to suggest going back
- Dreyfus-level indicators per topic area

**Step 2: Commit**

```bash
git add skills/tech-tutor/references/assessment-rubric.md
git commit -m "feat: add assessment rubric reference"
```

---

## Task 11: Tech Tutor SKILL.md

**Files:**
- Create: `skills/tech-tutor/SKILL.md`

**Step 1: Write SKILL.md**

Frontmatter:
```yaml
---
name: tech-tutor
description: This skill should be used when the user asks to "learn", "study", "go through a course", "start a tutorial", "practice", "do exercises", "learn from a course", or wants interactive tutoring through hands-on technical course content.
version: 0.1.0
---
```

Body (~1,500-2,000 words):
- Overview: what the skill does
- Session flow: Load → Assess → Entry Point → Module Session → Wrap Up
- Mode-switching behavior (summary, details in references)
- The voice and tone (summary, details in references)
- Hint and tip handling
- "The Bigger Picture" moments
- Pointers to all reference files

**Step 2: Commit**

```bash
git add skills/tech-tutor/SKILL.md
git commit -m "feat: add tech-tutor skill"
```

---

## Task 12: Agent — Course Researcher

**Files:**
- Create: `agents/course-researcher.md`

**Step 1: Write agent file**

Frontmatter:
```yaml
---
name: course-researcher
description: Use this agent when creating a course and needing to gather knowledge from reference materials. Fetches URLs, reads repositories, explores git branches, and performs web searches to build a structured knowledge base for course content generation.
model: inherit
color: cyan
tools: ["Read", "Glob", "Grep", "Bash", "WebFetch", "WebSearch"]
---
```

System prompt:
- Role: expert researcher specializing in technical documentation synthesis
- Process: receive sources → fetch/read each → synthesize into structured notes per spiral loop
- Output format: knowledge base document with sections per source
- Quality: cite sources, note gaps, flag conflicting information
- Git exploration: how to explore branches, read key files, understand repo structure

**Step 2: Commit**

```bash
git add agents/course-researcher.md
git commit -m "feat: add course-researcher agent"
```

---

## Task 13: Agent — Module Writer

**Files:**
- Create: `agents/module-writer.md`

**Step 1: Write agent file**

Frontmatter:
```yaml
---
name: module-writer
description: Use this agent when generating individual course modules. Takes an outline, research notes, and pedagogical context to write one complete module (episode.md, lesson.md, exercise.md, challenge.md, starter/, solution/).
model: inherit
color: magenta
tools: ["Read", "Write", "Glob", "Grep", "Bash"]
---
```

System prompt:
- Role: expert technical course writer with the Nerdy Friend voice
- Process: receive module spec → write episode.md → lesson.md → exercise.md → challenge.md → starter/ → solution/
- Voice: reference the voice-and-tone spec (warm, funny, self-aware, supportive)
- Pedagogy: Foundation/Practice/Mastery within module, scaffolding level based on loop position
- Quality: practical examples, ecosystem context, layered hints, pro tips
- Must read voice-and-tone.md and learning-philosophy.md before writing

**Step 2: Commit**

```bash
git add agents/module-writer.md
git commit -m "feat: add module-writer agent"
```

---

## Task 14: Command — /create-course

**Files:**
- Create: `commands/create-course/COMMAND.md`

**Step 1: Write command file**

```yaml
---
description: Generate a hands-on technical course with progressive exercises
argument-hint: Optional topic or path to course-spec.yaml
---
```

Body: Invoke the course-creator skill with $ARGUMENTS. Brief instructions on the workflow.

**Step 2: Commit**

```bash
git add commands/create-course/COMMAND.md
git commit -m "feat: add /create-course command"
```

---

## Task 15: Command — /learn

**Files:**
- Create: `commands/learn/COMMAND.md`

**Step 1: Write command file**

```yaml
---
description: Start an interactive tutoring session with a technical course
argument-hint: Path to course directory or course name
---
```

Body: Invoke the tech-tutor skill with $ARGUMENTS. Brief instructions on session start.

**Step 2: Commit**

```bash
git add commands/learn/COMMAND.md
git commit -m "feat: add /learn command"
```

---

## Task 16: Update README

**Files:**
- Modify: `README.md`

**Step 1: Update README with final structure**

Add installation instructions, usage examples, and link to design document.

**Step 2: Commit**

```bash
git add README.md
git commit -m "docs: update README with usage examples and installation"
```

---

## Task 17: Push & PR

**Step 1: Push branch**

```bash
git push -u origin feature/plugin-scaffold
```

**Step 2: Create PR**

```bash
gh pr create --title "feat: tech-skill-builder plugin scaffold" \
  --body "## Summary
- Plugin manifest and directory structure
- course-creator skill with 6 reference docs
- tech-tutor skill with 4 reference docs
- course-researcher agent (parallel research)
- module-writer agent (content generation)
- /create-course and /learn commands
- Pedagogical framework based on 4C/ID, Bloom's, Spiral Curriculum, Vygotsky, Dreyfus, Kolb

## Design Doc
See docs/plans/2026-03-09-tech-skill-builder-design.md"
```

---

## Execution Order

Tasks 1-7 can be partially parallelized:
- **Task 1** (scaffold) must go first
- **Tasks 2-7** (reference docs) are independent of each other — can be parallelized
- **Task 8** (course-creator SKILL.md) depends on Tasks 2-7
- **Tasks 9-10** (tutor references) are independent of 2-7
- **Task 11** (tech-tutor SKILL.md) depends on Tasks 9-10
- **Tasks 12-13** (agents) depend on Task 1 only
- **Tasks 14-15** (commands) depend on Task 1 only
- **Task 16** (README) depends on all prior tasks
- **Task 17** (push & PR) depends on all prior tasks

**Recommended parallel batches:**
1. Task 1
2. Tasks 2, 3, 4, 5, 6, 7, 9, 10, 12, 13, 14, 15 (all independent)
3. Tasks 8, 11 (depend on references)
4. Task 16
5. Task 17
