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

## Plugin Structure

```
tech-skill-builder/
  .claude-plugin/plugin.json          # plugin manifest
  skills/
    course-creator/                   # course authoring skill
      SKILL.md
      references/                     # pedagogical framework, output format, etc.
    tech-tutor/                       # interactive tutoring skill
      SKILL.md
      references/                     # tutoring modes, assessment rubric, etc.
  agents/
    course-researcher.md              # fetches URLs, repos, web search
    module-writer.md                  # writes individual course modules
  commands/
    create-course/COMMAND.md          # /create-course entry point
    learn/COMMAND.md                  # /learn entry point
```

## Installation

```bash
# Add to your Claude Code plugins
claude plugin add cemergin/tech-skill-builder
```

## Usage

### Creating a Course

```bash
# Start the interactive course creator
/create-course Terraform for our platform team

# Or provide a spec file
/create-course ./courses/terraform-101/course-spec.yaml
```

The skill will:
1. Interview you (or read your spec) to understand scope, audience, and depth
2. Generate a course outline with spiral progression
3. Research your provided URLs, repos, and docs
4. Write each module with lessons, exercises, and challenges
5. Present for your review

### Learning from a Course

```bash
# Start an interactive tutoring session
/learn ./courses/terraform-101
```

The tutor will:
1. Assess your current level
2. Suggest an entry point (or let you choose)
3. Walk you through lessons, exercises, and challenges
4. Adapt its teaching style to your needs

## Pedagogical Approach

Each module follows a **micro-progression**:
1. **Foundation** (Remember/Understand) — read the concept, see examples
2. **Practice** (Apply/Analyze) — code-along with guided steps
3. **Mastery** (Evaluate/Create) — solve a challenge independently

Modules are grouped into **spiral loops** — each loop revisits the domain at a higher complexity level. Early modules have more scaffolding; later modules remove guardrails.

## Voice & Tone

All generated content uses **"The Nerdy Friend"** voice — imagine a colleague who genuinely lights up when talking about this topic. They use analogies, acknowledge when something is confusing, crack jokes about the absurdity of tech, but never make you feel dumb.

## Design Document

See [docs/plans/2026-03-09-tech-skill-builder-design.md](docs/plans/2026-03-09-tech-skill-builder-design.md) for the full design with pedagogical research, architecture decisions, and voice specification.

## Contributing

PRs welcome! If you'd like to improve the pedagogical framework, add new tutoring modes, or enhance the course output format, check the reference docs in `skills/*/references/` for context.

## License

MIT
