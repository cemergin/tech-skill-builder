# tech-skill-builder

A Claude Code plugin for creating and delivering hands-on technical courses.

Built on proven pedagogical frameworks:
- **4C/ID Model** — whole-task learning with progressive complexity
- **Bloom's Revised Taxonomy** — cognitive level progression (Remember → Create)
- **Spiral Curriculum** — revisit concepts at increasing depth
- **Vygotsky's ZPD + Scaffolding** — guided support that fades as skills grow
- **Kolb's Experiential Learning** — learn by doing, reflect, conceptualize, experiment
- **Dreyfus Model** — calibrate guidance from novice to expert

## What It Does

Two skills that work together:

### `/create-course` — Course Creator
Generate structured, hands-on technical courses from a topic + reference materials.
- Interactive interview or spec file to define scope, audience, and depth
- Ingests URLs, local repos, GitHub repos, and documentation as source material
- Produces a full course: concept lessons, code-along exercises, and progressive challenges
- Spiral progression: Foundation → Practice → Mastery loops of increasing complexity

### `/learn` — Tech Tutor
Interactive tutoring that brings course content to life.
- Mode-switching: adaptive (concepts), Socratic (challenges), strict (guided exercises)
- Skip-ahead support: each module has an "In This Episode" summary for self-assessment
- Conversational enrichment of authored course material

## Course Output Structure

```
courses/<course-name>/
  course.yaml                  # metadata, prerequisites, spiral progression map
  01-<module-name>/
    episode.md                 # "In This Episode" — summary, key concepts, prerequisites
    lesson.md                  # Foundation: concept explanation with examples
    exercise.md                # Practice: step-by-step code-along
    challenge.md               # Mastery: open-ended problem with hints
    starter/                   # starter code files
    solution/                  # reference solutions
  02-<module-name>/
    ...
  capstone/                    # optional: ties all loops together
    brief.md
    starter/
    solution/
```

## Target Audience

Built for diverse technical teams:
- Engineers (onboarding to new tools, frameworks, or codebases)
- Data scientists (learning infrastructure, SQL, new libraries)
- Designers (learning APIs, design systems tooling)
- PMs & analysts (learning SQL, CLI tools, data pipelines)
- Anyone building technical skills through hands-on practice

## Installation

```bash
# Add to your Claude Code plugins
claude plugin add cemergin/tech-skill-builder
```

## Pedagogical Approach

Each module follows a **micro-progression**:
1. **Foundation** (Remember/Understand) — read the concept, see examples
2. **Practice** (Apply/Analyze) — code-along with guided steps
3. **Mastery** (Evaluate/Create) — solve a challenge independently

Modules are grouped into **spiral loops** — each loop revisits the domain at a higher complexity level. Early modules have more scaffolding; later modules remove guardrails.

## License

MIT
