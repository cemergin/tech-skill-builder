# Spiral Progression Reference

## What Is Spiral Progression?

Based on Bruner's Spiral Curriculum (1960), spiral progression means revisiting the same core topics at increasing levels of complexity. Each "loop" through the material is a complete learning cycle (Foundation → Practice → Mastery) that builds on what came before.

This is NOT repetition. Each loop tackles a genuinely more complex version of the whole task.

## Structure

```
Loop 1: Foundation → Practice → Mastery
  └─ Simplest complete version of the real task
  └─ Heavy scaffolding, detailed guidance
  └─ Learner achieves: "I can do the basic version"

Loop 2: Foundation → Practice → Mastery
  └─ Adds real-world complexity (dependencies, configuration, edge cases)
  └─ Medium scaffolding, references prior modules
  └─ Learner achieves: "I can handle realistic scenarios"

Loop 3: Foundation → Practice → Mastery
  └─ Production-grade scenarios (scale, security, automation, team workflows)
  └─ Light scaffolding, learner-driven
  └─ Learner achieves: "I can do this professionally"

Capstone (optional):
  └─ Integration project combining concepts from all loops
  └─ Minimal guidance, open-ended
  └─ Learner achieves: "I can design and build this end-to-end"
```

## How Loops Build on Each Other

### Concept Compounding
Each loop introduces new concepts AND deepens understanding of previous ones.

Example (Terraform):
- **Loop 1 introduces:** resources, providers, plan/apply cycle
- **Loop 2 deepens:** resources (now with dependencies), providers (now with multiple), plan/apply (now with remote state)
- **Loop 3 deepens:** resources (now in modules), providers (now multi-region), plan/apply (now in CI/CD)

### Scaffolding Fading
Guidance reduces across loops:

| Aspect | Loop 1 | Loop 2 | Loop 3 |
|--------|--------|--------|--------|
| Lesson detail | Full explanation with analogies | Builds on prior, explains new aspects | Brief, focuses on what's new |
| Exercise steps | Very granular (10-15 steps) | Moderate (6-10 steps) | High-level (3-5 steps) |
| Hints in challenges | 3 hints, visible | 3 hints, expandable | 1-2 hints, expandable |
| Pro tips | Frequent (every 2-3 steps) | Moderate (every 4-5 steps) | Sparse (only for non-obvious things) |
| "Bigger Picture" | Every module | Every other module | Only where new ecosystem context exists |
| Solutions | Full solution with explanation | Solution with brief explanation | Solution in solution/ directory only |

### Cross-Loop References
Later modules should reference earlier ones naturally:
- "Remember when we used `terraform plan` in Module 2? Same idea, but now..."
- "In Module 3, we stored state locally. That worked fine for one person, but..."
- "You've already seen how variables work. Now let's use them across environments."

## Module Independence

**Critical design principle:** Each module must be self-contained enough that a learner can start there if they have the prerequisite knowledge.

### How to Achieve Independence
1. **episode.md** lists exact prerequisites with self-assessment questions
2. **lesson.md** briefly re-introduces necessary concepts (1-2 sentences, not full re-explanation)
3. **starter/** contains everything needed to begin (no reliance on output from prior exercises)
4. Cross-references use "Episode N covered X" rather than "As we did in the last module"

### When to Break Independence
Capstones may require completing all prior modules in a loop. This should be explicit in the episode.md:
> **Note:** This capstone builds directly on your work from Episodes 7-9. Complete those first.

## Determining Number of Loops

| Course Scope | Recommended Loops | Example |
|-------------|-------------------|---------|
| Narrow tool/concept | 2 loops | "Git Basics for Non-Engineers" |
| Standard tool/framework | 3 loops | "Terraform for Platform Engineers" |
| Broad domain | 3-4 loops | "Full-Stack Observability" |
| Deep specialization | 4+ loops | "Kubernetes Operator Development" |

### Signals That You Need More Loops
- The jump between two adjacent modules feels too steep
- A loop has more than 4 modules (consider splitting into two loops)
- The capstone requires concepts that feel disconnected from earlier modules

### Signals That You Need Fewer Loops
- Later loops feel like padding rather than genuine complexity increases
- The topic doesn't have enough depth for another full cycle
- Modules in later loops aren't whole-tasks (they're just advanced trivia)

## Example: Terraform Course Progression

### Loop 1: "Your First Infrastructure" (Modules 1-3)
| Module | Foundation | Practice | Mastery |
|--------|-----------|---------|---------|
| 01-what-is-iac | What IaC is, why it matters | Install Terraform, init a project | Convert a manual AWS setup to Terraform |
| 02-first-resource | Resources and providers | Deploy an EC2 instance | Deploy a different resource type |
| 03-plan-apply-destroy | The Terraform workflow | Full lifecycle of a resource | Debug a failed plan |

### Loop 2: "Real-World Terraform" (Modules 4-6)
| Module | Foundation | Practice | Mastery |
|--------|-----------|---------|---------|
| 04-variables-outputs | Parameterization, DRY | Add variables to existing config | Refactor hardcoded values |
| 05-state-management | State concepts, remote backends | Set up S3 backend | Migrate from local to remote state |
| 06-dependencies | Resource dependencies, data sources | Build a VPC + EC2 + SG | Design a dependency graph for a given architecture |

### Loop 3: "Production Terraform" (Modules 7-9)
| Module | Foundation | Practice | Mastery |
|--------|-----------|---------|---------|
| 07-modules | Reusable modules, registry | Create and use a module | Refactor an existing config into modules |
| 08-environments | Workspaces, directory patterns | Multi-env with shared modules | Design an env strategy with trade-offs |
| 09-cicd | Automation, plan in CI | Set up GitHub Actions pipeline | Design a full GitOps workflow |
