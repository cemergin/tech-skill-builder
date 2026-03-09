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

### Prerequisites

- [Claude Code CLI](https://docs.anthropic.com/en/docs/claude-code) installed and authenticated

### How Claude Code plugins work

Claude Code loads plugins from a local cache at `~/.claude/plugins/cache/`. Each plugin provides **skills** (reusable knowledge Claude can invoke), **commands** (slash commands like `/create-course`), and **agents** (specialized sub-agents for parallel work). Plugins are tracked in `~/.claude/plugins/installed_plugins.json`.

There are two ways to get a plugin into that cache:

- **Marketplace install** — Claude Code clones the repo for you, caches it, and registers it automatically. Updates are a single command. This is the standard path for using a plugin.
- **Local install** — You manually copy files into the cache and register the plugin yourself. This is for plugin developers who want to iterate on files without going through git each time.

---

### Option 1: Marketplace install (recommended)

This registers the GitHub repo as a custom marketplace source, then installs the plugin from it. Claude Code handles cloning, caching, and registration.

```bash
# 1. Add this repo as a marketplace source
#    This tells Claude Code "look here for plugins" — similar to adding an apt repo or npm registry.
#    It clones the repo into ~/.claude/plugins/cache/ and indexes its plugin.json manifest.
claude plugin marketplace add https://github.com/cemergin/tech-skill-builder

# 2. Install the plugin from the marketplace you just added
#    This reads the manifest, copies skills/agents/commands into the cache,
#    and adds an entry to installed_plugins.json so Claude Code loads it on startup.
claude plugin install tech-skill-builder

# 3. Restart Claude Code to pick up the new plugin
#    Plugins are loaded at session start — a restart is required after any install or update.
```

**Updating:**

```bash
# Pull the latest from the repo and refresh the cached plugin files
claude plugin marketplace update tech-skill-builder
claude plugin update tech-skill-builder
# Then restart Claude Code
```

---

### Option 2: Local install (for development)

Use this if you're contributing to the plugin or want to test changes before committing. You clone the repo yourself and copy files directly into Claude Code's local plugin cache.

```bash
# 1. Clone the repo
git clone https://github.com/cemergin/tech-skill-builder.git
cd tech-skill-builder

# 2. Create the local cache directory
#    ~/.claude/plugins/cache/local/ is where manually-installed plugins live.
#    Each plugin gets its own subdirectory named after the plugin.
mkdir -p ~/.claude/plugins/cache/local/tech-skill-builder

# 3. Copy plugin files into the cache
#    The key directories:
#      .claude-plugin/  — contains plugin.json (the manifest that tells Claude Code
#                         the plugin name, version, and description)
#      skills/          — SKILL.md files + reference docs that Claude loads as knowledge
#      agents/          — .md files defining specialized sub-agents
#      commands/        — COMMAND.md files that register slash commands (/create-course, /learn)
cp -R .claude-plugin skills agents commands README.md \
      ~/.claude/plugins/cache/local/tech-skill-builder/

# 4. Register the plugin
#    Open ~/.claude/plugins/installed_plugins.json and add this entry inside the "plugins" object.
#    This is the registry file Claude Code reads at startup to know which plugins to load.
#
#    "tech-skill-builder@local": [{
#      "scope": "user",
#      "installPath": "~/.claude/plugins/cache/local/tech-skill-builder",
#      "version": "0.1.0",
#      "installedAt": "<current ISO timestamp>",
#      "lastUpdated": "<current ISO timestamp>",
#      "gitCommitSha": ""
#    }]

# 5. Restart Claude Code to load the new plugin
```

**Updating during development:**

After making changes to your local clone, re-copy the changed files:

```bash
# From the repo root — re-copy whichever directories you changed
cp -R skills/ ~/.claude/plugins/cache/local/tech-skill-builder/skills/
# Then restart Claude Code
```

---

### Verifying the installation

After restarting Claude Code, confirm the plugin is loaded:

```bash
claude plugin list
```

You should see `tech-skill-builder` in the output. The `/create-course` and `/learn` commands should now be available.

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
