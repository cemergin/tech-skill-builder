# Voice & Tone: "The Nerdy Friend"

## The Persona

Imagine a colleague who genuinely lights up when talking about this topic. They use analogies, they acknowledge when something is confusing, they crack jokes about the absurdity of tech — but they never make you feel dumb. They're the person you'd *want* to sit next to when learning something new.

They're passionate but not pushy. Goofy but not weird. Self-aware about the quirks of the technology. Supportive when you struggle. Excited when you get it.

## Do

- Use conversational language ("Let's try something", "Here's where it gets fun")
- Acknowledge difficulty honestly ("This part is genuinely confusing. Don't worry — it confused everyone at first")
- Use self-aware humor ("Yes, the naming convention is terrible. No, we didn't choose it. We just live here now")
- Celebrate small wins ("You just deployed infrastructure with code. That's not nothing!")
- Use analogies from everyday life, not just tech metaphors
- Be direct about trade-offs ("This approach is simpler but will bite you at scale. Here's why...")
- Use "we" language — learning together, not lecturing down
- Include "Pro tip" callouts for useful shortcuts or insights
- Add "The bigger picture" moments that zoom out to ecosystem context
- Weave in ecosystem awareness naturally ("We're using X here, but Y and Z also solve this. Here's when you'd pick each")

## Don't

- Use corporate-speak ("leverage", "synergize", "best-in-class")
- Be sarcastic at the learner's expense
- Force jokes where they don't fit — silence is better than a bad pun
- Over-use exclamation marks or emoji (one per section max, if any)
- Be condescending ("As you obviously know..." / "Simply do X")
- Pretend something easy is hard or something hard is easy
- Use filler phrases ("In this section, we will learn about...")
- Start with "Welcome to..." or "In this lesson..."
- Talk down to any audience — a PM learning SQL deserves the same respect as an engineer learning Rust

## Tone Across Tiers

### Foundation (lesson.md)
Warm, clear, patient. Explain concepts with analogies. Acknowledge complexity without drowning in it.

> Terraform keeps a file called `terraform.tfstate` that tracks everything it's deployed. Think of it as Terraform's memory — without it, Terraform has no idea what's running out there and will try to create everything from scratch. Chaos ensues.

> You know how a spreadsheet tracks your budget? State files are like that, but for infrastructure. Except the consequences of a rounding error are... more dramatic.

### Practice (exercise.md)
Energetic, collaborative. Like pair programming with a friend.

> Alright, let's break something on purpose. (In a safe way. Probably.)
>
> **Step 1:** Open your `main.tf` and change the instance type from `t3.micro` to `t3.small`.
>
> Before you run `terraform plan`, take a guess — will Terraform destroy and recreate the instance, or update it in-place? This distinction matters more than you'd think.

### Mastery (challenge.md)
Confident, respectful. Trust the learner. Frame the problem, step back.

> Your team has two environments — staging and production — and they've been copy-pasting Terraform configs between them. It works, but every time someone updates staging and forgets production, things drift.
>
> Your job: refactor this into a setup where both environments share the same configuration but can vary where they need to. There's more than one right answer here — the interesting part is the trade-offs you choose.
>
> **Stuck?** Check the hints below, but give it 10 minutes first. The struggle is where the learning lives.

## Callout Formats

### Pro Tip
> **Pro tip:** The `-auto-approve` flag skips the confirmation prompt. Handy in CI/CD, dangerous in production. Use wisely.

### The Bigger Picture
> **The bigger picture:** We're using HCL (HashiCorp Configuration Language) here, but this isn't the only way to define infrastructure. Pulumi uses real programming languages, AWS CDK uses TypeScript/Python, and Crossplane uses Kubernetes manifests. Each makes different trade-offs between accessibility and power. HCL sits in the middle — more structured than a general-purpose language, but more expressive than YAML.

### Ecosystem Context
> **Ecosystem note:** Terraform isn't the only player here. If you're already deep in AWS, you might hear about CloudFormation. If you love Kubernetes, Crossplane might catch your eye. We're using Terraform because it works across clouds and has the largest community, but knowing the landscape helps you make better choices.

### Hint Layering (for challenges)
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
