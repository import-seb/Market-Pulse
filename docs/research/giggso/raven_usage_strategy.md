# Raven Usage Strategy for Market Pulse

## Purpose

Raven should **support our learning, not replace it**.

Our capstone is valuable because we are researching unfamiliar market-prediction methods, deciding what is appropriate, implementing those methods ourselves, and learning from the mistakes we make along the way.

If we ask Raven broad questions too early, it can shortcut that process by suggesting methods, architectures, or implementation ideas before we have had the chance to discover them ourselves.

Our goal is therefore to use Raven for the **last 10%**:

> We do the research, synthesis, design, and implementation. Raven helps us find holes, challenge our reasoning, catch mistakes, and protect the repository.

---

## The Core Workflow

```text
Human research
    ↓
Human synthesis
    ↓
Human chooses candidate methodology
    ↓
Raven pre-implementation critique
    ↓
Human verifies Raven's criticisms
    ↓
Human implementation
    ↓
Raven review / guards
    ↓
Human fixes and learns
```

Raven should act primarily as:

- a **research critic**
- a **pre-implementation reviewer**
- an **implementation critic**
- a **code reviewer**
- a **security guard**

Raven should **not** act primarily as:

- our researcher
- our methodology chooser
- our code generator
- a replacement for understanding what we are implementing

---

# 1. Pre-Implementation Review — Our Main Use Case

This is where Raven may provide the most value to us.

Before implementing a methodology, we should first complete enough research to have:

- a clearly defined prediction problem
- candidate data sources
- a proposed target
- a proposed feature set or signal families
- known timing/release/revision constraints
- candidate methodology
- validation strategy
- baseline
- assumptions
- known limitations

Only after we have formed our own position should we ask Raven to critique it.

## What We Want From Raven

We want Raven to look for things such as:

- unsupported assumptions
- contradictions between our findings
- temporal leakage
- revision leakage
- mismatches between the method and available data
- incorrect validation logic
- missing baselines
- unrealistic assumptions about data availability
- implementation traps
- important relationships between findings that we almost noticed but did not fully connect
- a high-value idea that is directly adjacent to our existing research

The goal is not:

> "Tell us how to solve this problem."

The goal is:

> "Here is how we think we should solve it. Attack our reasoning."

## Recommended Prompt

```text
We have completed our own methodology research for this part of Market Pulse.

Do not replace our research process and do not give us a broad survey of alternative
methods.

Review the research, assumptions, and candidate methodology that we provide.

Your job is to find:

1. unsupported assumptions
2. contradictions
3. temporal or data leakage risks
4. places where our proposed method does not match the available data
5. weaknesses in our validation plan
6. implementation risks
7. important connections between our findings that we may have missed
8. unusually high-value ideas that are directly adjacent to what we already discovered

Do not implement anything.

Do not select a completely different methodology unless you identify a serious flaw
that makes our current approach inappropriate.

Explain why each criticism matters so that we can investigate and verify it ourselves.

We want the final 10% of insight, not the first 90%.
```

---

# 2. Human Verification Comes After Raven

Raven's response is **not automatically correct**.

If Raven says:

> "This may create revision leakage."

we should not immediately change the pipeline.

We should ask:

- Why?
- Which variable creates the problem?
- Which date matters?
- Does the source documentation confirm this?
- Would this information actually have been available on the prediction date?

Then **we verify the criticism ourselves** using the papers, documentation, and data.

This keeps Raven in the role of critic rather than authority.

---

# 3. During Implementation

Once we begin implementing a researched methodology, Raven can review whether our code actually matches what we intended to build.

For example:

```text
We chose walk-forward validation based on our research.

Review our current implementation only.

Do not recommend a different modeling methodology and do not modify the files.

Check whether our code correctly implements walk-forward validation and specifically
look for:

- incorrect temporal ordering
- train/test contamination
- feature calculations that use future information
- release-date or revision-date leakage
- mistakes in window construction

Explain any problems you find. We will make the changes ourselves.
```

This allows Raven to test our understanding without doing the intellectual work for us.

---

# 4. Code Review

After we implement a feature, pipeline component, model, or data-processing step, Raven can act as a reviewer.

Useful review targets include:

- correctness
- leakage
- reproducibility
- error handling
- unnecessary complexity
- security
- dependency problems
- architectural inconsistencies
- testing gaps

The preferred pattern is:

```text
We wrote this implementation ourselves.

Review it without modifying the files.

Identify bugs, methodological mistakes, security problems, and maintainability issues.

Explain why each issue is a problem.

Do not rewrite the implementation for us unless we explicitly ask.
```

---

# 5. Guards — Always On

Raven's guards are separate from asking Raven questions.

These are useful because they can catch repository problems while we work.

According to Raven's current documentation, its local guards can check for things such as:

- committed API keys or secrets
- vulnerable dependencies / CVEs
- unapproved dependencies
- style problems
- architecture inconsistencies
- some database-related problems

Some checks run during edits and others run through Git's **pre-commit hook**.

That means a normal workflow can look like:

```bash
git add .
git commit -m "Add retail claims ingestion"
```

Raven's pre-commit checks run before Git accepts the commit.

If something serious is detected, the commit can be blocked.

This is useful even if we never ask Raven to write code.

---

# 6. Main Pitfall: Raven Can Steal the Research

Raven's default guided/education behavior does **not** completely solve our learning concern.

Why?

Because read-only research is allowed.

Even if Raven is prevented from editing code, we could still ask:

> "What is the best methodology for forecasting this?"

and receive the exact concepts that we were supposed to discover through our own research.

Therefore:

## Avoid broad methodology prompts before research

Bad:

```text
How should we predict retail demand?
```

Bad:

```text
What features should we use?
```

Bad:

```text
What machine-learning models should we try?
```

Bad:

```text
Design our validation strategy.
```

These questions outsource the most educational part of the project.

Better:

```text
Here is the validation strategy we selected and why.

Try to break it.
```

---

# 7. Other Pitfalls

## Blindly Accepting Raven's Advice

Raven is another model-assisted system. It can still be wrong.

Treat findings as hypotheses to investigate.

---

## Asking for Fixes Too Quickly

Bad workflow:

```text
Raven finds bug
→ Raven fixes bug
→ we move on
```

Preferred workflow:

```text
Raven finds bug
→ Raven explains bug
→ we understand bug
→ we fix bug
→ Raven verifies
```

---

## Letting Raven Expand the Scope

We may ask about one narrow methodological issue and receive ideas that expand the project.

Keep the review tied to:

- our sprint goal
- our chosen market
- the question currently being investigated

---

## Using Raven Because It Exists

Raven itself says that its additional process can be unnecessary for routine feature work and quick lookups.

We should use it when it adds value, not force every task through a complicated AI workflow.

---

# Technical Setup

## Important Mental Model

Raven is **not a website that we connect to GitHub once**.

For the open-source version, much of Raven runs locally on the developer's machine and inside the project.

There are two different layers to think about:

### Machine-level installation

Each developer installs Raven's tooling on their own computer.

### Project-level configuration

The Market Pulse repository contains Raven configuration describing the project.

---

# 8. What Each Teammate Needs

Each teammate who wants Raven's local guards and development integration should install Raven locally.

One teammate installing Raven does **not** automatically install the Git pre-commit hooks on everyone else's clone.

This matters because Git hooks such as:

```text
.git/hooks/pre-commit
```

live inside each developer's local Git clone and normally are not shared through GitHub.

Therefore, for a three-person team:

```text
Sebastian's computer → Raven installed + project setup
Maria's computer     → Raven installed + project setup
Maksim's computer    → Raven installed + project setup
```

All three work against the same GitHub repository.

---

# 9. Installation

Raven's current README lists Claude Code as its primary plugin path.

The documented plugin installation is:

```bash
git clone https://github.com/giggsoinc/raven.git
claude plugin install ./raven/plugin
```

After installing, restart the Claude Code session.

Raven also includes support files for other hosts, but we should start with the officially documented path Ravi gave us unless he tells us he expects a different host.

---

# 10. Machine Setup

Raven's current README then calls for a one-time machine installation:

```bash
bash <(curl -fsSL https://raw.githubusercontent.com/giggsoinc/raven/main/install.sh)
```

This is documented as **once per machine**.

Because our team may be using Windows, the Raven repository also contains PowerShell setup files such as:

```text
raven-setup.ps1
```

We should follow the repository's current Windows instructions rather than blindly running Unix commands in Windows Command Prompt.

Git Bash or WSL may also be appropriate for the Bash installation path.

---

# 11. Project Setup

Inside the Market Pulse repository, Raven documents:

```bash
cd market-pulse
raven-setup
```

This is **once per project clone/environment** to configure the project and local hooks.

Raven generates a project manifest:

```text
.raven/manifest.json
```

The manifest tells Raven things such as:

- whether this is solo/team/enterprise
- language / stack information
- project type
- guard configuration

For us, this should be configured in **team mode**.

---

# 12. What Should Be Shared in GitHub

The normal idea should be:

```text
.raven/manifest.json
```

Shared with the repository so the team uses the same project configuration.

Potential notification credentials go in:

```text
.raven/manifest.secrets.json
```

Raven documents that this secrets file is gitignored.

**Never commit that file or any credentials.**

---

# 13. Local Git Hooks

After project setup, Raven can install local Git hooks.

The important one is the pre-commit gate.

Conceptually:

```text
Developer writes code
       ↓
git add
       ↓
git commit
       ↓
Raven pre-commit checks
       ↓
PASS ─────────→ commit created

BLOCK ────────→ developer fixes issue
```

These checks are valuable even if we do zero AI-assisted coding.

---

# 14. Team Workflow

A practical team workflow could be:

## Step A — Research ticket

Each teammate performs assigned research normally.

Findings go into our repository research documents.

No Raven methodology suggestions yet.

---

## Step B — Research synthesis

The team compares findings and develops its own candidate approach.

Example:

```text
Research findings
+ data availability
+ publication timing
+ leakage constraints
+ baseline
= proposed methodology
```

---

## Step C — Create a pre-implementation review document

Before coding a major methodology, create something like:

```text
research/reviews/retail_nowcast_preimplementation.md
```

It can contain:

```markdown
## Problem

## What we found

## Proposed methodology

## Data available

## Prediction date

## Feature timing assumptions

## Validation strategy

## Baseline

## Known risks

## Questions we are unsure about
```

This gives Raven a bounded packet of **our work** to critique.

---

## Step D — Raven critique

Give Raven the document and use the restricted critic prompt from this file.

The response should identify holes, not restart the research.

---

## Step E — Human verification

Convert Raven's important criticisms into research questions.

Example:

```text
Raven claim:
BLS revision timing may leak information.

Human follow-up:
Read BLS methodology documentation and determine whether our downloaded historical
values represent initial releases or later revisions.
```

Only verified findings should change our methodology.

---

## Step F — Implementation

We write the implementation ourselves.

---

## Step G — Raven implementation review

Ask Raven to inspect the implementation against the methodology we already chose.

No automatic rewriting.

---

## Step H — Commit

```bash
git add .
git commit -m "Implement walk-forward validation"
```

Raven guards run.

Fix any legitimate security/dependency/quality problems.

Then push and open the PR normally.

---

# 15. Suggested Repository Structure

We do not need to restructure the whole project around Raven.

A lightweight structure could be:

```text
market-pulse/
│
├── .raven/
│   └── manifest.json
│
├── research/
│   ├── findings.md
│   ├── data_leakage.md
│   └── reviews/
│       └── retail_preimplementation.md
│
├── src/
│
├── tests/
│
└── README.md
```

Raven should sit **around** our existing workflow rather than become the workflow itself.

---

# 16. Recommended Team Rule

We should adopt one simple rule:

> **Raven does not get first contact with an unsolved methodology question.**

Humans get first contact.

Once we have researched the problem and formed a position, Raven gets to attack it.

That preserves the educational value of the capstone while still letting us use Raven for what it is especially good at: discipline, criticism, review, and protection against avoidable mistakes.

---

# 17. Our Raven Roles

| Role | Use Raven? | How |
|---|---|---|
| Initial methodology research | No | Team researches first |
| Choosing methods from scratch | No | Team makes first decision |
| Research red-team | Yes | Find holes after synthesis |
| Pre-implementation review | Yes | Attack our proposed design |
| Coding methodology | No by default | We implement it |
| Implementation critique | Yes | Check whether code matches method |
| Code review | Yes | Find bugs and quality issues |
| Secret scanning | Yes | Always on |
| CVE/dependency checks | Yes | Always on |
| Debugging | Sometimes | After we have tried to understand it |
| Automatic fixes | Avoid by default | We fix issues ourselves |

---

# Bottom Line

For Market Pulse, Raven should not be the teammate who knows the answer first.

It should be the teammate who walks in after we think we know the answer and says:

> "Okay. Now let me try to break it."

That gives us Raven's additional insight without sacrificing the research, implementation experience, and understanding that we want to carry into future data-science projects.

---

## Technical Reference

Raven repository:

https://github.com/giggsoinc/raven

The setup and behavior described above are based on the repository README available on September 8, 2026. Raven is actively developed, so installation commands and features should be checked against the current README when the team actually installs it.
