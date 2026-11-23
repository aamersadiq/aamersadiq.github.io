---
layout: post
title: "AI Powered Developer Journey"
subtitle: "From joining a new project to delivering the first feature"
date: 2026-08-23
categories: [ai, developer-tools]
tags: [agent-skills, claude-code, onboarding, spec-driven-development, consulting]
description: "With the right agent skills, a developer joining a new project can have their first feature scoped, built, reviewed and merged by the end of week two. Which skills help at each stage, and how much time they save."
excerpt: "Most of a new developer's first weeks go on setting up a machine, waiting for access, reading old documents and finding their way around unfamiliar code. That work repeats on every project, and repeatable work is what agent skills are for."
---

This is the journey a developer goes through when joining a new project. Every engagement follows the same steps, and most of us know them well.

You start on the project on a Monday. The first day or two go into setting up your machine. Docker, the right Node version, a JDK nobody wrote down, a VPN client, an IDE licence. You raise access requests for AWS, the database, Confluence and the private package registry, and then you wait. You read whatever documentation exists, which is usually a Confluence space with forty pages, and about eight of them are still correct. You clone the repository and slowly work out where the business logic actually lives. Around the end of week two you pick up your first user story, scope it, build it, get it reviewed, raise a pull request, fix the comments, and merge.

**The goal of this post is simple. With the right skills, the first feature should be ready by the end of week two.** Not the first commit. The feature: scoped, built, reviewed, merged.

Most of those weeks are not spent typing. They go on setting up a machine, waiting for access, reading old documents, and working out where things live in a codebase you have never seen. That work repeats on every project, and repeatable work is what **agent skills** are for.

A skill is a folder with a `SKILL.md` file in it. Instructions, and sometimes scripts and reference files, that an agent picks up when your request matches. Anthropic wrote the [Agent Skills spec](https://github.com/anthropics/skills) and most tools now follow it, so Claude Code, Cursor, Codex, Copilot CLI and Gemini CLI all read the same format.

The question is which skills help at which stage, and how much of those weeks we can get back.

| Stage | Today | With skills | Skills that help |
|---|---|---|---|
| Environment & access | 1-3 days | Day 1 | `devcontainer-setup`, Claude Code Dev Container Feature, `wizard`, your own onboarding skill |
| Knowledge curation | 2-5 days, usually skipped | Days 2-3 | DiUS [`knowledge-curation`](https://github.com/DiUS/agent-toolkit/tree/main/skills/knowledge-curation) |
| Exploring the code | 3-5 days | Days 3-5 | DiUS [`codebase-discovery`](https://github.com/DiUS/agent-toolkit/tree/main/skills/codebase-discovery), [Understand-Anything](https://github.com/Egonex-AI/Understand-Anything), [Graphify](https://github.com/Graphify-Labs/graphify) |
| First feature | 3-10 days | Week 2 | [Spec Kit](https://github.com/github/spec-kit), [eng-flow](https://github.com/m3dcodie/eng-flow), [superpowers](https://github.com/obra/superpowers), [agent-skills](https://github.com/addyosmani/agent-skills) |

The rest of this post walks through each stage.

---

## Stage 1: onboarding, the machine and the access

There is no single skill that handles onboarding at a new project. What exists is a set of parts you can put together yourself, and the option of writing the missing skill.

### Getting the machine to build

The first job is turning a repository into a working development environment.

**Use a dev container.** Installing tools onto your own machine is slow, hard to repeat and hard to undo. A dev container describes the environment once, and every developer runs the same copy of it. Trail of Bits publish a [`devcontainer-setup`](https://github.com/trailofbits/skills) skill that builds ready-made dev containers with the right language tooling and Claude Code already installed. Anthropic ship a [Dev Container Feature](https://github.com/anthropics/devcontainer-features/tree/main/src/claude-code) you can drop into any `devcontainer.json`:

```json
{
  "image": "mcr.microsoft.com/devcontainers/base:ubuntu",
  "features": {
    "ghcr.io/anthropics/devcontainer-features/claude-code:1.0": {}
  }
}
```

The [reference container](https://github.com/anthropics/claude-code/tree/main/.devcontainer) in the Claude Code repo goes further. It keeps your login between rebuilds, and it ships an `init-firewall.sh` script that limits which sites the container can reach. The firewall limits what a session can reach on the network, and the container limits where file changes land, so an agent running with fewer permission prompts stays inside both.

**Let the agent read the repo and work out the tools.** This part does not need a special skill. Point an agent at the repository and ask it to work out the setup from what is already committed: `package.json` engines, `.nvmrc`, `.tool-versions`, `pyproject.toml`, `docker-compose.yml`, Terraform versions, and the CI workflow files. The CI config runs on every build, so it stays accurate about what the project needs. A README can go out of date without anyone noticing. Then ask the agent for the gap between what CI expects and what your machine has.

**Turn the answer into a script, not a chat log.** Matt Pocock's [`wizard`](https://github.com/mattpocock/skills) skill writes interactive bash setup scripts. The agent reads the repo, writes `scripts/setup-dev.sh`, and you commit it. The next developer runs one command, and the setup work sits in the repository instead of in a chat log.

**Give the agent knowledge of the project's stack.** Microsoft's [skills repository](https://github.com/microsoft/skills) has 175 SDK-specific skills across Python, .NET, TypeScript, Java and Rust. Azure Cosmos DB patterns, FastAPI routers, Pydantic models, that kind of thing, tested against more than a thousand scenarios. If the project is on a stack the model does not know well, install these before you write any code.

### Getting access sorted

Access requests go through the organisation's own process and depend on other people approving them. Two things help.

**Connect the systems instead of describing them.** MCP servers for GitHub, Atlassian/Confluence and your cloud provider let the agent read the real onboarding pages, the real ticket templates and the real repository permissions, so you do not have to copy them out by hand. If the current process is spread across eleven Confluence pages, an agent that can search the space directly will find the right one.

**Write the onboarding skill the project does not have.** Most projects have an onboarding runbook. It is usually a bit out of date, partly stored in people's heads, and not in a form an agent can use. Take it, add the tickets you had to raise and the things nobody told you, and save it as a `SKILL.md` in the project repository:

```markdown
---
name: onboard-new-engineer
description: Set up a development environment and request access for a new
  engineer joining this project. Use when someone new joins the team.
---

# Onboarding

## Environment
1. ... (real commands, not prose)

## Access requests
| System | Request via | Approver | Typical SLA |
|---|---|---|---|

## Verify
Run these and check each one passes before calling it done.
```

Anthropic's `skill-creator` skill will help you get the structure right. The second person to join then starts from your setup, and it stays in the repository after you leave.

---

## Stage 2: knowledge curation, capturing the *why*

This stage is usually skipped, and there is less tooling for it than the others.

Almost every skill out there looks at code. Code tells you *what* the system does. It will not tell you why the discount cap is 30%, which regulator asked for the audit trail, or which of two conflicting rules in the spec is the one that shipped. That knowledge sits in functional specs, ADRs, Confluence pages, and in people's heads.

DiUS's [`knowledge-curation`](https://github.com/DiUS/agent-toolkit/tree/main/skills/knowledge-curation) skill works the other way round. It takes the business and technical documents and builds a knowledge base an agent can use while planning and building. It breaks documents down instead of shortening them, and it asks instead of guessing.

Point it at a document and it splits the content into business rules, workflows, journeys, glossary terms and constraints, filed across three levels: platform, domain and feature. Platform holds the things that cut across everything. Domain holds a business capability. Feature holds one capability inside a domain. Folders like `rules/`, `workflows/`, `questions/`, `constraints/` and `tech/` sit at the level they belong to.

The skill applies a few rules to everything it extracts.

**It does not guess.** Every fact has a `basis` field with one of four values: `documented`, `stated`, `inferred`, `assumed`, plus a link to the source. You can always answer "where did that come from?"

**It asks instead of filling in.** When it finds a gap it asks you in chat rather than writing something that sounds right. The gap stays visible instead of being filled with generated text.

**It records contradictions instead of solving them.** When two parts of a document disagree it keeps both and raises an open question. It does not choose between them.

**The agent never marks anything as verified.** Every fact starts as `status: draft`. Only a human who knows the domain can confirm it. Example content is tagged `example: true` and cannot be used as a fact.

**A coverage register tracks what has been covered.** A domain marked as unknown appears in the register. A domain that was never covered at all does not.

Four read-only scripts check the rules: the frontmatter schema, unique IDs in the right place, no example content leaking in, and the correct folder level.

The output is built to be fed back to an agent cheaply. Every domain and feature has an `index.md` that is a pointer rather than a summary. It lists what is there and what it depends on, so an agent working on one feature loads that feature's rules and nothing else. You never have to push a 90 page spec into context to answer one question. Facts link to each other by ID rather than by file path, so you can move folders around without breaking links, and each fact lives in exactly one file, so there are no copies going out of sync.

You can build this during the same week you would spend reading Confluence.

---

## Stage 3: exploring the code, capturing the *how*

Stage 2 covered the why. This stage covers the how: the architecture, the patterns, and the integrations. The output is a map of the codebase that you and an agent can both work from.

Three tools cover this stage, and they take different approaches.

### codebase-discovery: onboarding docs backed by evidence

DiUS's [`codebase-discovery`](https://github.com/DiUS/agent-toolkit/tree/main/skills/codebase-discovery) follows one rule. The code tells you what the system does. Only people know why. So it reads the code first and forms ideas backed by evidence, then checks those ideas with people through short interviews.

It runs in six phases:

1. **Pre-check.** Find the docs that already exist (README, `CLAUDE.md`, `AGENTS.md`) and note where they disagree with the code.
2. **Deep recon.** Look at structure, data models, contracts, and the places where the business logic is dense.
3. **Interview.** One question at a time, each one based on something it found in the code.
4. **Synthesis.** Write short onboarding documents, dated, with a note on where each claim came from.
5. **Verify.** Go back over its own claims and check each one traces to code or to a named person.
6. **Finish.** Report where the old docs had drifted, and optionally update `CLAUDE.md` and `AGENTS.md`.

It has a code-only mode for when nobody is available to talk to yet, which covers your first week, and in that mode it states the limitation once per document rather than on every claim. It also only marks the exceptions, `[unchecked]`, `[assumption]`, `[outdated]`, instead of labelling everything. It works with any agent and needs no plugins or MCP servers, only plain files, which matters if you are restricted in what you can install.

Because the interview questions come out of real code, the first conversation with the tech lead starts from something specific: "I can see the pricing service skips the rules engine for enterprise accounts, was that on purpose?" rather than "so, tell me about the architecture."

### Understand-Anything: the interactive map

[Understand-Anything](https://github.com/Egonex-AI/Understand-Anything) turns a codebase into a knowledge graph you can click through. It mixes two approaches. Tree-sitter reads the structure, so imports, definitions and calls come out the same every time. LLM agents then add the meaning: summaries, intent, business domains. Seven agents run the file analysis in parallel.

The commands line up with the journey:

- `/understand` builds the graph
- `/understand-dashboard` lets you explore it visually, coloured by layer (API, Service, Data, UI, Utility), with search and auto-generated guided tours
- `/understand-onboard` writes an onboarding guide for new team members
- `/understand-domain` pulls out business domains, flows and process steps
- `/understand-explain` goes deep on one file or function
- `/understand-diff` shows what your current changes affect
- `/understand-chat` answers questions in plain language

The graph is saved to `.ua/knowledge-graph.json`. You build it once and commit it, and the team reuses it without running the pipeline again. Opening the dashboard needs only Node.js 18+, no API key and no LLM calls, so the next person to join can open it without running anything.

`/understand-diff` applies to the first feature. It shows what your change touches before you raise the PR.

### Graphify: everything, not just code

[Graphify](https://github.com/Graphify-Labs/graphify) takes the widest range of input. Code in about 40 languages, plus Markdown, PDFs, SQL schemas, Terraform, Office documents, images and video with transcription. It builds a graph rather than a vector index, so every connection can be followed and explained.

```bash
uv tool install graphifyy   # double-y on PyPI, but the command is `graphify`
graphify install
```

Then `/graphify .` in your IDE produces three things: an interactive `graph.html`, a `GRAPH_REPORT.md` with key concepts and unexpected connections, and `graph.json` for querying offline. After that:

```bash
graphify query "what connects auth to the database?"
graphify path AuthService OrderRepository
graphify explain "PricingEngine"
```

Reading source code happens entirely on your machine and gives the same result every time. No API calls, no model involved. This applies when there are rules about where data can go. Only documents, PDFs and media need an LLM.

Two details worth knowing. Every connection is tagged either `EXTRACTED`, meaning it was written explicitly in the source, or `INFERRED`, meaning it was worked out by following references, so you can tell what was found from what was derived. That is the same approach to tracking sources that `knowledge-curation` uses. And comments like `# NOTE:` and `# WHY:` become nodes in the graph, linked to the code they explain. The graph also groups itself into subsystems using the Leiden algorithm, with no LLM needed.

### How to use all three

They answer different questions, so they suit different points in the week.

Graphify handles the first wide sweep, because it can read the Confluence PDF exports and the SQL schema next to the code. Understand-Anything produces the dashboard you commit, which the next person to join can open directly. And codebase-discovery produces the document you read yourself, plus the interview questions for the tech lead.

---

## Stage 4: the first feature, scope, build, review, merge

You have an environment, a knowledge base and a map. Now you need to deliver something. There are more tools at this stage than at any other, and they differ mainly in how much process they impose.

### Spec Kit: the spec drives the build

In GitHub's [Spec Kit](https://github.com/github/spec-kit) the spec is kept and drives the implementation, rather than being written once and set aside.

```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git@vX.Y.Z
specify init my-project --integration <agent-name>
```

The work runs through six phases, each with its own command:

1. `/speckit.constitution` sets project principles, quality standards, and the things you will not compromise on
2. `/speckit.specify` covers what to build and why
3. `/speckit.clarify` fills in the parts that are not clear yet
4. `/speckit.plan` handles architecture and technology choices
5. `/speckit.tasks` breaks the plan into tasks you can build
6. `/speckit.implement` builds them, then `/speckit.converge` compares the codebase against the spec and shows what is left

Steps 5 and 6 repeat until the two match. `/speckit.analyze` checks that the spec, the plan and the tasks agree with each other. It works with 30+ agents and supports extensions, presets and bundles, so a team can package its own way of working once and reuse it on every project.

The `/speckit.constitution` step records the project's quality standards, which are often not written down anywhere. The agent then follows those rather than generic defaults.

### eng-flow: the whole delivery process, with gates

[eng-flow](https://github.com/m3dcodie/eng-flow/blob/main/PROCESS.md) covers the delivery process end to end. Its own framing: "MVP mode and production mode are not a maturity ranking, they're a match to context."

It starts at Phase 0, routing, with one question. Has someone with authority already approved building this? If no, it runs idea validation first, using four questions about the user, the current situation, the timing, and the smallest proof you need. If yes, you go straight into the production track.

The MVP track is deliberately light. A one page spec, a flat checklist, a rough time and token estimate before you branch, then one continuous build loop with automatic commits, stopping only for breaks or for requirements nobody has decided yet.

The production track has ten stages. Spec/BRD, then domain model, then architecture with tech stack, ADRs and a security policy. Then an engineering review gate covering coupling, scaling, security and failure cases, written to `eng-review.md`. Then a UI/UX design gate if the spec has UI journeys. Then the work gets broken into epics, stories and tasks. Then you build with TDD, one task at a time. Then code review across five areas (correctness, readability, architecture, security, performance) where Critical and Required findings block the merge. Then QA against the running app. Then ship: gate check, merge, version bump, PR, and a GO/NO-GO call with a rollback plan. Then a blameless retro.

Three things run across all the stages.

A decision ledger (`decisions.jsonl`) records every judgement call: the reason, how it was made, and who owned it, whether that is AI-decided, user-confirmed, user-revised or user-delegated. A `--guide` flag shows them as they happen. This leaves a record of what the AI decided and what a human decided.

A learning loop. The retro adds durable learnings, with a confidence score, to a project level `learnings.md`. Stage 5 reads that file *before* starting the next task, so later tasks pick up what earlier ones found.

Analytics (`analytics.jsonl`) covering cycle time, both elapsed and active, plus rework cycles and bug rate. These give you figures to report back.

There is also a hard stop before risky or hard to undo work: auth, migrations, payments, deploys, secrets. All of it needs explicit sign-off.

### superpowers: TDD and subagents

[superpowers](https://github.com/obra/superpowers) is a set of skills built on four ideas. Test-driven development, a repeatable process instead of improvising, keeping things simple, and checking rather than assuming.

The workflow starts with brainstorming, where the agent asks questions and shows design options in small pieces for you to approve. Then git setup, a clean branch with a passing test baseline. Then planning, broken into tasks of 2 to 5 minutes each, with exact file paths and full code details. Then execution, where subagents take one task each and get reviewed twice, first against the spec and then for code quality. Then TDD proper: red, green, refactor, with the failing test written first. Then code review, then finish, where all tests pass and you choose merge or PR.

The skill library also covers debugging with root cause analysis, checking your work before calling it done, git worktrees for parallel branches, and writing new skills.

The 2 to 5 minute task size keeps each subagent's scope small, so a task that goes wrong is cheap to discard and rerun.

### addyosmani/agent-skills: 25 skills across the lifecycle

Addy Osmani's [agent-skills](https://github.com/addyosmani/agent-skills) groups its 25 skills by delivery stage, holding "the workflows, quality gates, and best practices that senior engineers use when building software."

```bash
npx skills add addyosmani/agent-skills --list   # look first
npx skills add addyosmani/agent-skills          # all 25
```

Grouped by phase:

- **Define:** `interview-me` (one question at a time), `idea-refine`, `spec-driven-development`, `constraint-driven-development`
- **Plan:** `planning-and-task-breakdown`
- **Build:** `incremental-implementation` (thin slices behind feature flags), `test-driven-development`, `context-engineering`, `source-driven-development` (base decisions on official docs), `doubt-driven-development` (challenge decisions while you build), `frontend-ui-engineering`, `api-and-interface-design`
- **Verify:** `browser-testing-with-devtools`, `debugging-and-error-recovery`
- **Review:** `code-review-and-quality`, `code-simplification`, `security-and-hardening`, `performance-optimization`
- **Ship:** `git-workflow-and-versioning`, `ci-cd-and-automation`, `deprecation-and-migration`, `documentation-and-adrs`, `observability-and-instrumentation`, `shipping-and-launch`

There is also a meta-skill, `using-agent-skills`, which routes new work to the right one. It helps once the library is large enough that you cannot remember everything in it.

Three of these apply directly to a first feature. `incremental-implementation` keeps changes to thin slices, which limits how far a misread of the architecture spreads. `source-driven-development` works from official docs, which stops the agent inventing functions that do not exist. And `doubt-driven-development` challenges decisions while you build, so a wrong assumption surfaces before the work is finished.

### mattpocock/skills: working well with agents

Matt Pocock's [skills](https://github.com/mattpocock/skills) collection focuses on how you work with an agent rather than how fast it goes.

```bash
claude plugins install mattpocock-skills
# or, if you want to edit them yourself:
npx skills@latest add mattpocock/skills
```

The engineering skills include `grill-with-docs`, a long planning session that builds up the project's terms and context, plus `to-spec`, `to-tickets`, `implement` (build from a spec with TDD and review), `wayfinder` (plan long multi-session work as decision tickets), `codebase-design`, `domain-modeling`, `code-review` (checked against both standards and the spec), `resolving-merge-conflicts` and `diagnosing-bugs`.

The productivity skills cover the communication side. `grill-me` questions your plan. `handoff` writes the notes one agent needs to pass work to another. `wait-what` clears up unclear messages. And `writing-for-agents` sets rules for documentation an agent can read.

`handoff` and `writing-for-agents` matter when work changes hands, since every engagement ends and someone else picks the work up.

### Before you install any of them

A skill is a set of instructions your agent will follow inside the project's codebase, with real credentials nearby. Read the `SKILL.md` before you install it. Pin the version. Check skill installs with the team the same way you would check any other dependency.

---

## The two-week plan

Not all of the skills above. This is the short list, and the order to run it in.

**Day 1, environment.**
A dev container with the Claude Code feature and the firewall script. `devcontainer-setup` from Trail of Bits. Ask the agent to read the CI config and write a setup script, then commit it. Raise every access request the same day, since those depend on other people.

**Days 2-3, knowledge.**
`knowledge-curation` pointed at the functional specs and the ADRs. Connect Confluence through MCP so it reads the real pages. The coverage register will look mostly empty at first, and the empty entries show which areas need a conversation.

**Days 3-5, the map.**
`codebase-discovery` in code-only mode first, then take its questions into your first real conversation with the tech lead. Understand-Anything for the dashboard you commit and leave behind. Graphify if there is a lot of material that is not code.

**End of week one, pick up the story.**
You now have a working environment, a knowledge base with the business rules in it, and a map of the code. Without this, that point usually arrives in week three.

**Week 2, build, review, merge.**
Spec Kit or eng-flow, depending on how much process the work needs. `incremental-implementation`, `source-driven-development` and `doubt-driven-development` from Addy Osmani's set. superpowers if the work breaks down well into small tasks. Run `/understand-diff` before you raise the PR so you know what your change touches. Then `handoff` at the end.

**Ongoing.**
Write the onboarding skill the project did not have. Commit the knowledge base and the graph. Keep `learnings.md` up to date.

The saving comes from week one, not from writing code faster.

---

## What this does not fix

A few limits worth stating.

**Access requests still wait on people.** An agent can draft every ticket, find the right approver and chase the queue, but it cannot make a security team approve your AWS role on a Tuesday. Start those in your first hour anyway.

**Two weeks is a target, not a guarantee.** A large legacy system, a slow access process, or a first story that turns out much bigger than it looked will all push it out. Treat the plan as a default to aim at rather than a commitment to make in week one.

**Tracking sources only helps if you read them.** `knowledge-curation`, `codebase-discovery` and Graphify all mark what was found and what was derived. That only works if you check the marks. An `assumed` fact promoted to `verified` without checking carries the same weight as a confirmed one, and the rest of the team will treat it that way.

**Stage 1 has the least tooling.** Stages 2, 3 and 4 have working skills you can install today. Stage 1 is still assembled from parts, even though it is the step that repeats most often on every project.

**Most of the value is in what you commit.** You can reach the first merge at the end of week two instead of week four. But if the setup script, the knowledge base, the graph and the onboarding skill stay on your laptop, the saving applies to you and not to the team.

---

### Skills and tools referenced

| Tool | Link |
|---|---|
| Agent Skills spec & examples | https://github.com/anthropics/skills |
| DiUS agent-toolkit | https://github.com/DiUS/agent-toolkit |
| DiUS knowledge-curation | https://github.com/DiUS/agent-toolkit/tree/main/skills/knowledge-curation |
| DiUS codebase-discovery | https://github.com/DiUS/agent-toolkit/tree/main/skills/codebase-discovery |
| Understand-Anything | https://github.com/Egonex-AI/Understand-Anything |
| Graphify | https://github.com/Graphify-Labs/graphify |
| GitHub Spec Kit | https://github.com/github/spec-kit |
| eng-flow | https://github.com/m3dcodie/eng-flow/blob/main/PROCESS.md |
| superpowers | https://github.com/obra/superpowers |
| addyosmani/agent-skills | https://github.com/addyosmani/agent-skills |
| mattpocock/skills | https://github.com/mattpocock/skills |
| Trail of Bits skills | https://github.com/trailofbits/skills |
| Microsoft skills | https://github.com/microsoft/skills |
| Claude Code Dev Container Feature | https://github.com/anthropics/devcontainer-features/tree/main/src/claude-code |
| Claude Code reference devcontainer | https://github.com/anthropics/claude-code/tree/main/.devcontainer |
