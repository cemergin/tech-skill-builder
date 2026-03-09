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

### Course Creator
Generate structured, hands-on technical courses from a topic + reference materials.
- Interactive interview or spec file to define scope, audience, and depth
- Ingests URLs, local repos, GitHub repos, and documentation as source material
- Produces a full course: concept lessons, code-along exercises, and progressive challenges
- Spiral progression: Foundation → Practice → Mastery loops of increasing complexity

### Tech Tutor
Interactive tutoring that brings course content to life.
- Mode-switching: adaptive (concepts), Socratic (challenges), direct (on request), supportive (when struggling)
- Skip-ahead support: each module has an "In This Episode" summary for self-assessment
- Conversational enrichment of authored course material

## Installation

### Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed and authenticated (v1.0.33+)

### Install from marketplace (recommended)

Add this repo as a marketplace source, then install the plugin:

```bash
# 1. Add the marketplace
/plugin marketplace add cemergin/tech-skill-builder

# 2. Install the plugin
/plugin install tech-skill-builder@tech-skill-builder
```

Or from the CLI:

```bash
claude plugin marketplace add cemergin/tech-skill-builder
claude plugin install tech-skill-builder@tech-skill-builder
```

#### Installation scopes

By default, the plugin installs to **user** scope (available across all projects). You can choose a different scope:

- **User** (default): available everywhere, stored in `~/.claude/settings.json`
- **Project**: shared with teammates via `.claude/settings.json` in the repo
- **Local**: just for you in this repo, gitignored

```bash
# Install for your whole team on a specific project
claude plugin install tech-skill-builder@tech-skill-builder --scope project
```

#### Updating

```bash
/plugin marketplace update tech-skill-builder
/plugin install tech-skill-builder@tech-skill-builder
```

### Install for local development

Clone the repo and load it directly with `--plugin-dir`:

```bash
git clone https://github.com/cemergin/tech-skill-builder.git
claude --plugin-dir ./tech-skill-builder
```

This is useful for testing changes before committing. Restart Claude Code to pick up edits.

### Team configuration

Add the marketplace to your project's `.claude/settings.json` so teammates are prompted to install it automatically:

```json
{
  "extraKnownMarketplaces": {
    "tech-skill-builder": {
      "source": {
        "source": "github",
        "repo": "cemergin/tech-skill-builder"
      }
    }
  },
  "enabledPlugins": {
    "tech-skill-builder@tech-skill-builder": true
  }
}
```

### Verify installation

After installing, confirm the plugin loaded:

```bash
/plugin
```

Go to the **Installed** tab — you should see `tech-skill-builder`. The skills and commands should now be available.

## Usage

### Creating a Course

```bash
# Start the interactive course creator (command)
/tech-skill-builder:create-course Terraform for our platform team

# Or provide a spec file
/tech-skill-builder:create-course ./courses/terraform-101/course-spec.yaml
```

The skill will:
1. Interview you (or read your spec) to understand scope, audience, and depth
2. Generate a course outline with spiral progression
3. Research your provided URLs, repos, and docs
4. Write each module with lessons, exercises, and challenges
5. Present for your review

### Learning from a Course

```bash
# Start an interactive tutoring session (command)
/tech-skill-builder:learn ./courses/terraform-101
```

The tutor will:
1. Assess your current level
2. Suggest an entry point (or let you choose)
3. Walk you through lessons, exercises, and challenges
4. Adapt its teaching style to your needs

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

## Plugin Structure

```
tech-skill-builder/
  .claude-plugin/
    plugin.json                         # plugin manifest
    marketplace.json                    # marketplace catalog
  skills/
    course-creator/                     # course authoring skill (model-invoked)
      SKILL.md
      references/                       # pedagogical framework, output format, etc.
    tech-tutor/                         # interactive tutoring skill (model-invoked)
      SKILL.md
      references/                       # tutoring modes, assessment rubric, etc.
  agents/
    course-researcher.md                # fetches URLs, repos, web search
    module-writer.md                    # writes individual course modules
  commands/
    create-course.md                    # /create-course user-invocable command
    learn.md                            # /learn user-invocable command
```

## Target Audience

Built for diverse technical teams:
- Engineers (onboarding to new tools, frameworks, or codebases)
- Data scientists (learning infrastructure, SQL, new libraries)
- Designers (learning APIs, design systems tooling)
- PMs & analysts (learning SQL, CLI tools, data pipelines)
- Anyone building technical skills through hands-on practice

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

Test your changes locally with:

```bash
claude --plugin-dir ./tech-skill-builder
```

## License

MIT
