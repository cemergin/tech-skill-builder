# Pedagogical Framework Reference

## Overview

This reference synthesizes six evidence-based pedagogical frameworks into a unified approach for technical course creation. Each framework contributes a specific lens for designing effective learning experiences.

## Framework 1: 4C/ID Model (van Merriënboer)

The Four-Component Instructional Design model is the primary framework for structuring course content.

### The Four Components

**1. Learning Tasks**
Whole, authentic tasks of increasing complexity. Every module presents a complete, meaningful task — not isolated sub-skills. Module 1 might be "deploy a single EC2 instance"; Module 7 might be "deploy a multi-environment setup with state management."

Guidelines:
- Tasks should reflect real work the learner will do
- Start with the simplest version of the whole task
- Each subsequent task adds complexity while maintaining completeness
- Group tasks into "task classes" (spiral loops) of equivalent difficulty

**2. Supportive Information**
Mental models, strategies, and domain knowledge that help learners approach tasks. This is the "why" and "how to think about it" content.

Guidelines:
- Present before or during the task, not after
- Explain the mental model, not just the procedure
- Include trade-offs, alternatives, and ecosystem context
- This maps to lesson.md content

**3. Just-in-Time (JIT) Information**
Procedural steps and facts needed exactly when performing a task. The "do this now" instructions.

Guidelines:
- Present at the exact moment of need
- Keep concise and actionable
- Fade over loops (provide in Loop 1, reduce in Loop 2, omit in Loop 3)
- This maps to exercise.md step-by-step instructions

**4. Part-Task Practice**
Drill on specific sub-skills that need to become automatic. Only for aspects requiring fluency.

Guidelines:
- Use sparingly — only when a sub-skill is genuinely recurrent
- Example: CLI commands, keyboard shortcuts, syntax patterns
- Embed within exercises rather than separate drill sections
- Not every module needs part-task practice

## Framework 2: Bloom's Revised Taxonomy

Maps cognitive complexity to the three tiers within each module.

### Cognitive Levels → Course Tiers

| Bloom's Level | Course Tier | What the Learner Does |
|---------------|-------------|----------------------|
| Remember | Foundation (lesson.md) | Recognize and recall concepts, terminology |
| Understand | Foundation (lesson.md) | Explain concepts, interpret examples |
| Apply | Practice (exercise.md) | Use knowledge in guided, structured tasks |
| Analyze | Practice (exercise.md) | Break down problems, identify patterns |
| Evaluate | Mastery (challenge.md) | Judge approaches, assess trade-offs |
| Create | Mastery (challenge.md) | Design solutions, build something new |

### Application Guidelines
- Foundation content targets Remember + Understand
- Exercise content targets Apply + Analyze
- Challenge content targets Evaluate + Create
- Within each tier, start at the lower level and build up
- Each spiral loop targets the same Bloom's levels but with more complex subject matter

## Framework 3: Spiral Curriculum (Bruner)

Governs how topics are revisited at increasing depth across the course.

### Structure
```
Loop 1: Foundation → Practice → Mastery  (simplified whole-task)
Loop 2: Foundation → Practice → Mastery  (adds real-world complexity)
Loop 3: Foundation → Practice → Mastery  (production-grade scenarios)
Capstone: Integration project (optional)
```

### Guidelines
- Each loop is self-contained — a learner completing Loop 1 has usable skills
- Topics from Loop 1 reappear in Loops 2-3 at deeper levels
- New concepts may be introduced in later loops
- The number of loops depends on topic complexity (2-4 is typical)
- Each loop should feel like a natural progression, not repetition

### Example Progression (Terraform)
- **Loop 1:** Single resource, local state, basic plan/apply
- **Loop 2:** Multiple resources with dependencies, remote state, variables/outputs
- **Loop 3:** Modules, multi-environment, CI/CD integration, state management patterns

## Framework 4: Vygotsky's ZPD + Scaffolding

Governs how much guidance to provide and when to remove it.

### Zone of Proximal Development
The sweet spot between "can do alone" and "can't do even with help." Course content should target this zone — challenging enough to stretch, supported enough to succeed.

### Scaffolding Strategy by Loop
| Loop | Scaffolding Level | Guidance |
|------|------------------|----------|
| Loop 1 | Heavy | Detailed steps, many hints, explicit explanations |
| Loop 2 | Medium | Fewer steps, hints available but not prominent, concepts referenced not re-explained |
| Loop 3 | Light | Problem statement + constraints, minimal hints, learner drives approach |
| Capstone | Minimal | Brief with requirements only, learner designs and implements |

### Fading Techniques
- **Reduce step granularity:** Loop 1 "Step 1, Step 2, Step 3" → Loop 3 "Do X (refer to Module 2 if needed)"
- **Hide hints deeper:** Loop 1 shows Hint 1 by default → Loop 3 all hints behind expandable sections
- **Remove worked examples:** Loop 1 shows complete examples → Loop 3 shows only problem statements
- **Shift from "how" to "what":** Loop 1 explains the procedure → Loop 3 states the goal

## Framework 5: Dreyfus Model of Skill Acquisition

Calibrates the type of guidance to the learner's stage.

### Stages and Guidance

| Stage | Characteristics | Course Guidance |
|-------|----------------|-----------------|
| Novice | Follows rules, no context | Explicit rules, step-by-step, "always do X" |
| Advanced Beginner | Recognizes patterns from experience | Situational guidelines, "in this case, do X" |
| Competent | Plans deliberately, sets priorities | Multiple approaches, trade-off analysis |
| Proficient | Intuitive grasp, sees the big picture | Advanced patterns, edge cases, optimization |
| Expert | Intuitive, fluid, no longer needs rules | Reference material only, challenge problems |

### Application
- Early modules (Loop 1) target Novice → Advanced Beginner
- Middle modules (Loop 2) target Advanced Beginner → Competent
- Later modules (Loop 3) target Competent → Proficient
- Capstone targets Proficient (Expert is beyond course scope)

## Framework 6: Kolb's Experiential Learning Cycle

Structures the learning experience within each module.

### The Cycle
1. **Concrete Experience** → Do something hands-on (exercise.md)
2. **Reflective Observation** → Think about what happened ("What did you notice?")
3. **Abstract Conceptualization** → Learn the underlying concept (lesson.md)
4. **Active Experimentation** → Try variations, explore (challenge.md)

### Application in Modules
While modules are structured as lesson → exercise → challenge, the Kolb cycle suggests:
- Lessons should start with a motivating example or question (not pure theory)
- Exercises should include reflection prompts ("Before running this, predict what will happen")
- Challenges should encourage experimentation ("Try a different approach and compare")

## Combining the Frameworks

When creating a course, apply frameworks in this order:

1. **Spiral Curriculum:** Define the loops and their themes
2. **4C/ID:** Design whole-tasks for each module within loops
3. **Bloom's:** Ensure each module covers the right cognitive levels
4. **Vygotsky:** Set scaffolding levels per loop (heavy → light)
5. **Dreyfus:** Calibrate guidance style to target learner stage
6. **Kolb:** Structure the learning flow within each module
