# Interview Questions Reference

## Overview

When no `course-spec.yaml` exists, the course-creator skill conducts an interactive interview to gather requirements. The interview produces a `course-spec.yaml` as output — both paths converge on the same artifact.

## Core Questions

Ask these in order. Each question builds context for the next.

### Q1: Topic & Scope
**Ask:** "What topic or skill should this course teach?"

**Follow-ups based on answer:**
- If too broad (e.g., "Kubernetes"): "That's a big space. Should we focus on a specific aspect — like deploying apps, managing clusters, writing operators, or something else?"
- If too narrow (e.g., "kubectl get pods"): "That's quite specific. Should we broaden to cover Kubernetes CLI basics, or is this genuinely a focused tutorial on pod inspection?"
- If clear: proceed

**Output:** `name` and `description` fields in course.yaml

### Q2: Target Audience
**Ask:** "Who will be taking this course? What's their role and current skill level?"

**Follow-ups:**
- If vague (e.g., "engineers"): "What kind of engineers — frontend, backend, platform, data? And are they brand new to [topic] or do they have some exposure?"
- If mixed audience: "Should we target the lowest common denominator, or create separate tracks?"

**Output:** `target_audience` and `prerequisites` fields

### Q3: Learning Objectives
**Ask:** "After completing this course, what should learners be able to do? Think concrete, practical outcomes."

**Follow-ups:**
- If too abstract (e.g., "understand Terraform"): "Can you give me a specific task? Like 'deploy a web app to AWS using Terraform' or 'write reusable Terraform modules'?"
- If too many: "Those are great goals. Let's prioritize — which 3-5 are most important for v1?"

**Reframe as action verbs:** "Deploy X", "Configure Y", "Debug Z", "Design W"

**Output:** `learning_objectives` field

### Q4: Depth & Duration
**Ask:** "How deep should this go? Options:
- **Quick intro** (2 loops, ~6 modules) — get started and handle basic scenarios
- **Solid foundation** (3 loops, ~9 modules) — basics through production patterns
- **Deep dive** (4+ loops, ~12+ modules) — comprehensive, including advanced topics"

**Follow-ups:**
- "Any time constraints? Is this for a 1-day workshop, a week-long onboarding, or self-paced?"
- "Are there specific advanced topics that must be covered?"

**Output:** `difficulty_progression` structure (number of loops)

### Q5: Reference Materials
**Ask:** "Do you have any reference materials I should use? This dramatically improves course quality. Things like:
- Documentation URLs (official docs, tutorials, blog posts)
- GitHub repositories (your team's repos or open source examples)
- Local codebases (paths to repos on disk)
- Internal docs or wikis
- Anything else the course should draw from"

**Follow-ups:**
- "Any specific git branches I should explore in those repos?"
- "Are there internal conventions or patterns the course should teach?"
- "Any resources that are particularly good examples of how things should be done?"

**Output:** `sources` field

## Optional Follow-up Questions

Ask only if relevant based on core answers:

### Technology Setup
"What tools/versions should learners have installed? Any specific environment setup?"

### Team Conventions
"Are there team-specific patterns, naming conventions, or architectural decisions the course should reflect?"

### Anti-patterns
"Are there common mistakes or anti-patterns you've seen that the course should specifically address?"

### Tutoring Preferences
"For the interactive tutor, any preferences on style?
- **Adaptive** (default) — explains clearly, adjusts to learner
- **Socratic** — asks questions, gives hints rather than answers
- **A mix** — adaptive for concepts, Socratic for challenges"

**Output:** `tutoring_defaults` field

## Generating course-spec.yaml

After the interview, generate the spec file and present for approval:

```yaml
# Generated from interview on [date]
name: "[from Q1]"
description: "[from Q1]"
author: "[ask or infer]"
created: [today's date]
target_audience: "[from Q2]"
prerequisites:
  - "[from Q2]"
learning_objectives:
  - "[from Q3]"
difficulty_progression:
  - loop: 1
    theme: "[inferred from scope]"
    modules: [1, 2, 3]
  - loop: 2
    theme: "[inferred]"
    modules: [4, 5, 6]
sources:
  - url: "[from Q5]"
    type: webpage
tutoring_defaults:
  foundation: adaptive
  practice: adaptive
  mastery: socratic
```

**Present to author:** "Here's the course spec based on our conversation. Take a look — anything to adjust before I start generating content?"

## When course-spec.yaml Already Exists

If the file exists at the course path:
1. Read and parse it
2. Present a summary: "I found an existing course spec. Here's what it says: [summary]. Shall I proceed with this, or would you like to modify anything?"
3. Validate all required fields are present
4. Proceed to outline generation

## Interview Best Practices

- Ask one question at a time — don't overwhelm
- Offer concrete examples with each question
- Use multiple choice where possible
- Suggest reasonable defaults ("Most teams go with 3 loops — does that work for you?")
- Capture the author's language — if they say "devs" not "engineers," use that in the course
- Be ready to explain any term the author doesn't recognize
- The interview should feel like a conversation, not a form
