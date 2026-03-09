# Course Output Format Specification

## Directory Structure

```
courses/<course-name>/
  course.yaml                         # course metadata and progression map
  01-<module-name>/
    episode.md                        # "In This Episode" summary
    lesson.md                         # Foundation: concept explanation
    exercise.md                       # Practice: guided code-along
    challenge.md                      # Mastery: open-ended problem
    starter/                          # starter code for exercise + challenge
    solution/                         # reference solutions
  02-<module-name>/
    ...
  capstone/                           # optional: integration project
    brief.md
    starter/
    solution/
```

### Naming Conventions
- Course directory: kebab-case (e.g., `terraform-101`, `sql-for-analysts`)
- Module directories: zero-padded number + kebab-case name (e.g., `01-what-is-iac`, `02-first-resource`)
- All markdown files: lowercase, hyphenated
- Starter/solution directories contain actual runnable files matching the technology being taught

## course.yaml Schema

```yaml
name: string                          # Human-readable course name
description: string                   # 1-2 sentence course description
author: string                        # Course author name or team
created: date                         # Creation date (YYYY-MM-DD)
target_audience: string               # Who this course is for
prerequisites:                        # What learners should know
  - string
learning_objectives:                  # What learners will be able to do
  - string
difficulty_progression:               # Spiral loop definitions
  - loop: number                      # Loop number (1, 2, 3...)
    theme: string                     # Theme of this loop
    modules: [number]                 # Module numbers in this loop
sources:                              # Reference materials used to create the course
  - url: string                       # URL for web pages or GitHub repos
    type: webpage | github_repo       # Source type
  - path: string                      # Local filesystem path
    type: local_repo                  # Source type
tutoring_defaults:                    # Default tutoring modes per tier
  foundation: adaptive | socratic | strict  # Mode for lesson.md
  practice: adaptive | socratic | strict    # Mode for exercise.md
  mastery: adaptive | socratic | strict     # Mode for challenge.md
```

### Example course.yaml

```yaml
name: "Terraform for Platform Engineers"
description: "From first resource to multi-environment IaC"
author: "Platform Team"
created: 2026-03-09
target_audience: "Platform engineers new to Infrastructure as Code"
prerequisites:
  - "Basic CLI comfort (cd, ls, cat, environment variables)"
  - "AWS console familiarity (know what EC2 and S3 are)"
learning_objectives:
  - "Deploy and manage infrastructure using Terraform"
  - "Manage state across environments safely"
  - "Write reusable Terraform modules"
  - "Integrate Terraform into CI/CD pipelines"
difficulty_progression:
  - loop: 1
    theme: "Single resource basics"
    modules: [1, 2, 3]
  - loop: 2
    theme: "Multi-resource with dependencies"
    modules: [4, 5, 6]
  - loop: 3
    theme: "Production patterns"
    modules: [7, 8, 9]
sources:
  - url: "https://developer.hashicorp.com/terraform/docs"
    type: webpage
  - path: "./team-infra-repo"
    type: local_repo
  - url: "https://github.com/hashicorp/learn-terraform-provision-ec2"
    type: github_repo
tutoring_defaults:
  foundation: adaptive
  practice: adaptive
  mastery: socratic
```

## Module Files

### episode.md — "In This Episode"

Purpose: Enable skip-ahead decisions. A learner reads this to know if they need this module.

```markdown
# Episode N: <Module Title>

## In This Episode
[2-3 sentence summary of what this module covers and why it matters]

## Key Concepts
- [Concept 1]
- [Concept 2]
- [Concept 3]

## Prerequisites
- [What you should know, with self-assessment prompt]
- If you can answer "[specific question]", you're ready

## Builds On
- Episode N-1: [what was learned]
- Episode N-2: [if applicable]

## What's Next
- Episode N+1: [brief preview of what comes next and how it connects]
```

### lesson.md — Foundation

Purpose: Introduce concepts with explanations, analogies, and examples. Targets Bloom's Remember + Understand.

Structure:
```markdown
# <Concept Title>

[Opening hook — a question, scenario, or analogy that motivates the concept]

## What Is <Concept>?

[Clear explanation with everyday analogies]
[Technical definition after the intuition is built]

## Why Does This Matter?

[Real-world consequences, practical relevance]
[Connect to what the learner will actually do at work]

## How It Works

[Technical explanation with diagrams/examples]
[Code examples where relevant]

## The Bigger Picture

[Ecosystem context — what else exists, when to use what]
[Trade-offs with alternative approaches]

## Key Takeaways

- [Takeaway 1]
- [Takeaway 2]
- [Takeaway 3]
```

### exercise.md — Practice

Purpose: Guided, step-by-step code-along. Targets Bloom's Apply + Analyze.

Structure:
```markdown
# Exercise: <What You'll Build/Do>

## What We're Doing
[1-2 sentences on the goal]

## Before You Start
[Prerequisites: files to have open, tools to have installed, etc.]

## Steps

### Step 1: <Action>
[Instruction]
[Code block if needed]

> **Pro tip:** [Useful shortcut or insight]

[Prediction prompt: "Before running this, what do you think will happen?"]

### Step 2: <Action>
[Instruction]
[Code block]

### Step 3: <Action>
...

## What Just Happened?
[Reflection section — explain what the steps accomplished and why]
[Connect back to concepts from lesson.md]

## Try This
[Optional mini-experiments: "Change X to Y and see what happens"]
```

### challenge.md — Mastery

Purpose: Open-ended problem for independent solving. Targets Bloom's Evaluate + Create.

Structure:
```markdown
# Challenge: <Problem Title>

## The Scenario
[Real-world scenario that frames the problem]
[What's wrong or what needs to be built]

## Your Task
[Clear description of what to accomplish]
[Constraints or requirements]

## Success Criteria
- [ ] [Criterion 1]
- [ ] [Criterion 2]
- [ ] [Criterion 3]

## Hints

<details>
<summary>Hint 1: Direction</summary>
[Gentle nudge toward the right approach]
</details>

<details>
<summary>Hint 2: Approach</summary>
[More specific guidance on the method]
</details>

<details>
<summary>Hint 3: Almost There</summary>
[Nearly the answer — specific steps]
</details>

## Solution

<details>
<summary>View Solution</summary>

[Complete solution with explanation]
[Why this approach was chosen]
[Trade-offs with alternative approaches]

</details>
```

### starter/ Directory
Contains the initial code files learners work with. Must be runnable/valid as-is (even if incomplete). Files should have TODO comments or clear placeholders where the learner needs to make changes.

### solution/ Directory
Contains the completed reference solution. Must be runnable/valid. Should include comments explaining key decisions.

## Scaffolding by Loop Position

| Loop | lesson.md | exercise.md | challenge.md |
|------|-----------|-------------|-------------|
| 1 | Detailed analogies, step-by-step concept building | Very granular steps, many pro tips, prediction prompts at each step | 3 hints visible, solution included |
| 2 | Builds on prior concepts, less hand-holding | Moderate step granularity, fewer tips | 3 hints behind expandable sections, solution included |
| 3 | References prior modules, focuses on new aspects | High-level steps ("configure X, then Y") | 1-2 hints, solution in solution/ only |
| Capstone | Brief with requirements | N/A | Requirements + success criteria only |
