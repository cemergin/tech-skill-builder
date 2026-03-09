# Tech Skill Builder — Design Document

**Date:** 2026-03-09
**Status:** Approved
**Author:** cemergin + Claude

## Overview

A Claude Code plugin with two complementary skills for creating and delivering hands-on technical courses. Course authors use `/create-course` to generate structured curriculum from topics and reference materials. Learners use `/learn` to go through the content interactively with adaptive tutoring.

Built for diverse technical teams: engineers, data scientists, designers, PMs, analysts — anyone building practical technical skills.

## Pedagogical Foundations

### Core Frameworks

| Framework | How We Use It |
|-----------|---------------|
| **4C/ID Model** (van Merriënboer) | Whole-task learning from day one. Learning tasks + supportive info + JIT info + part-task practice |
| **Bloom's Revised Taxonomy** | Cognitive progression: Remember → Understand → Apply → Analyze → Evaluate → Create |
| **Spiral Curriculum** (Bruner) | Revisit topics at increasing depth. Each loop is a complete Foundation → Practice → Mastery cycle |
| **Vygotsky's ZPD + Scaffolding** | Heavy guidance early, gradually removed. Claude as "more knowledgeable other" |
| **Dreyfus Model** | Calibrate guidance depth: novices get rules, experts get space |
| **Kolb's Experiential Learning** | Do → Reflect → Conceptualize → Experiment cycle in every module |
| **Keller's ARCS** | Attention, Relevance, Confidence, Satisfaction drive engagement |

### Learning Philosophy

This is not school. There are no grades, no pass/fail, no certification theater. The only metric that matters is: *can you do the thing and understand why you're doing it that way?*

Courses exist to build:
1. **Practical skill** — you can actually use this tool/concept in real work
2. **Ecosystem awareness** — you know what else exists, when to use what, and where to look when you're stuck
3. **Trade-off intuition** — you understand *why* things are done a certain way, not just *how*

Hints, tips, and "here's how I think about it" moments are not cheating — they're the whole point.

### Difficulty Progression: Spiral Loops

Each course consists of multiple spiral loops. Each loop contains modules that follow:

```
Loop 1: Foundation → Practice → Mastery  (simple concept)
           ↓ builds on
Loop 2: Foundation → Practice → Mastery  (adds complexity)
           ↓ builds on
Loop 3: Foundation → Practice → Mastery  (real-world scenario)
           ↓ builds on
Capstone: Tie it all together (optional)
```

**Micro-level** (within each module): lesson.md → exercise.md → challenge.md
**Macro-level** (across modules): modules progress from simple whole-tasks to complex ones

Each module is self-contained. Learners can skip ahead by reading the `episode.md` summary.

## Voice & Tone: "The Nerdy Friend"

**The persona:** A colleague who genuinely lights up when talking about this topic. They use analogies, acknowledge when something is confusing, crack jokes about the absurdity of tech — but never make you feel dumb.

**Do:**
- Use conversational language ("Let's try something", "Here's where it gets fun")
- Acknowledge difficulty honestly ("This part is genuinely confusing. Don't worry — it confused everyone at first")
- Use self-aware humor ("Yes, the naming convention is terrible. No, we didn't choose it. We just live here now")
- Celebrate small wins ("You just deployed infrastructure with code. That's not nothing!")
- Use analogies from everyday life
- Be direct about trade-offs ("This approach is simpler but will bite you at scale")
- Use "we" language — learning together, not lecturing down

**Don't:**
- Use corporate-speak ("leverage", "synergize", "best-in-class")
- Be sarcastic at the learner's expense
- Force jokes where they don't fit
- Over-use exclamation marks or emoji
- Be condescending ("As you obviously know..." / "Simply do X")
- Pretend something easy is hard or something hard is easy

## Plugin Architecture

### File Structure

```
tech-skill-builder/
  README.md
  plugin.json
  skills/
    course-creator/
      SKILL.md
      references/
        pedagogical-framework.md
        output-format.md
        spiral-progression.md
        interview-questions.md
        voice-and-tone.md
        learning-philosophy.md
    tech-tutor/
      SKILL.md
      references/
        tutoring-modes.md
        assessment-rubric.md
        voice-and-tone.md
        learning-philosophy.md
  agents/
    course-researcher/
      AGENT.md
    module-writer/
      AGENT.md
  commands/
    create-course/
      COMMAND.md
    learn/
      COMMAND.md
```

### Components

#### Skill: `course-creator`

Orchestrates course generation through five phases:

1. **Interview** — Ask ~5 questions: topic, audience, learning objectives, depth, reference materials. If a `course-spec.yaml` exists, confirm and skip.
2. **Outline** — Generate `course.yaml` with spiral progression map. Author reviews and approves.
3. **Research** — Dispatch `course-researcher` agent to fetch/parse all source materials. Produces knowledge base documents per spiral loop.
4. **Write** — Dispatch `module-writer` agent for each module. Agent receives outline, episode summaries, knowledge base, voice spec.
5. **Review** — Author reviews generated content. Can regenerate or manually edit any module.

#### Skill: `tech-tutor`

Interactive tutoring that brings course content to life:

1. **Load** — Read `course.yaml`, find modules
2. **Assess** — Quick check: "Have you done any of this before?"
3. **Entry Point** — Show episode summaries, learner picks where to start
4. **Module Session** — Walk through lesson → exercise → challenge with mode-switching
5. **Wrap Up** — Summarize what was learned, suggest next module

#### Agent: `course-researcher`

Fetches and synthesizes knowledge from provided sources.

**Tools:** WebFetch, WebSearch, Bash (gh CLI, git), Read, Glob, Grep

**Input:** Source URLs, local repo paths, GitHub repos, course outline, subtopics
**Output:** Structured knowledge base document per spiral loop

#### Agent: `module-writer`

Writes one complete module with pedagogical structure and voice.

**Tools:** Write, Read, Bash (verify code)

**Input:** course.yaml, episode summaries, knowledge base chunk, voice spec, learning philosophy, output format spec
**Output:** Complete module directory (episode.md, lesson.md, exercise.md, challenge.md, starter/, solution/)

### Commands

| Command | Invokes | Description |
|---------|---------|-------------|
| `/create-course` | course-creator skill | Generate a hands-on technical course |
| `/learn` | tech-tutor skill | Learn interactively from a course |

## Course Output Format

### Directory Structure

```
courses/<course-name>/
  course.yaml
  01-<module-name>/
    episode.md                 # "In This Episode" — summary, key concepts, prerequisites
    lesson.md                  # Foundation: concept explanation with examples
    exercise.md                # Practice: step-by-step code-along
    challenge.md               # Mastery: open-ended problem with hints
    starter/                   # starter code files
    solution/                  # reference solutions
  02-<module-name>/
    ...
  capstone/                    # optional
    brief.md
    starter/
    solution/
```

### course.yaml Schema

```yaml
name: string
description: string
author: string
created: date
target_audience: string
prerequisites: [string]
learning_objectives: [string]
difficulty_progression:
  - loop: number
    theme: string
    modules: [number]
sources:
  - url: string          # web pages, GitHub repos
    type: webpage | github_repo | local_repo
  - path: string         # local paths
    type: local_repo
tutoring_defaults:
  foundation: adaptive | socratic | strict
  practice: adaptive | socratic | strict
  mastery: adaptive | socratic | strict
```

### episode.md Structure

Each module's episode.md contains:
- **In This Episode** — 2-3 sentence summary
- **Key Concepts** — bullet list of what you'll learn
- **Prerequisites** — what you should know (with self-assessment question)
- **Builds On** — which prior episodes this extends
- **What's Next** — where this leads

### Tutoring Modes

| Mode | When | Behavior |
|------|------|----------|
| **Adaptive** | New concepts, exercises | Explain clearly, use analogies, check understanding |
| **Socratic** | Challenges | Ask questions back, give hints not answers, celebrate attempts |
| **Strict/Direct** | Learner requests it | Give the answer, explain why — no gatekeeping |
| **Supportive** | Learner struggling | Back up, re-explain foundation, no shame |

The "Just Tell Me" escape hatch: if someone is stuck, the tutor asks "Want a hint or the full answer?" and respects the choice. Hints are layered (gentle nudge → approach suggestion → nearly the answer). Learning happens even by reading a well-explained solution.

### Hint Layering in Challenges

```markdown
<details>
<summary>Hint 1: Direction</summary>
Think about how you'd separate things that change from things that don't...
</details>

<details>
<summary>Hint 2: Approach</summary>
Terraform modules let you parameterize configurations. What if each environment
was just a different set of parameters to the same module?
</details>

<details>
<summary>Hint 3: Almost there</summary>
Create a `modules/` directory with your shared config. Then create
`environments/staging/main.tf` and `environments/prod/main.tf` that each
call the module with different variables. Check the `source` argument.
</details>
```

## References

- [4C/ID Model — van Merriënboer](https://www.4cid.org/wp-content/uploads/2021/04/vanmerrienboer-4cid-overview-of-main-design-principles-2021.pdf)
- [Bloom's Revised Taxonomy — UCF](https://fctl.ucf.edu/teaching-resources/course-design/blooms-taxonomy/)
- [Vygotsky's ZPD and Scaffolding](https://educationaltechnology.net/vygotskys-zone-of-proximal-development-and-scaffolding/)
- [Kolb's Experiential Learning Cycle](https://www.simplypsychology.org/learning-kolb.html)
- [Dreyfus Model of Skill Acquisition](https://en.wikipedia.org/wiki/Dreyfus_model_of_skill_acquisition)
- [ADDIE Instructional Design Model](https://educationaltechnology.net/the-addie-model-instructional-design/)
- [Scaffolded Learning — Vaia](https://www.vaia.com/en-us/explanations/education/designing-curricula/scaffolded-learning/)
