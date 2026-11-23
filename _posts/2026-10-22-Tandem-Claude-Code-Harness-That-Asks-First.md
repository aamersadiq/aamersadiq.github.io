---
layout: post
title: "Tandem: A Claude Code Harness"
date: 2026-10-23
last_modified_at: 2026-10-23
categories: [ai, developer-tools]
tags: [claude-code, ai-agents, human-in-the-loop, multi-repo]
description: "A lightweight Claude Code harness for the three things a new engineer does on a team: deliver a Jira ticket, review a teammate's pull request, and debug an issue. Built on the ideas of Spec Kit and superpowers, it works across several repos, asks one question at a time, records every decision, and stops at gates only a human can pass."
excerpt: "The previous post looked at which agent skills help a developer joining a new project. Now we build a harness ourselves: lighter than the full-fledged ones, and shaped around how a new developer is actually introduced to a team."
---

On a tandem bike, two people ride. Both pedal, but only one steers.

That is the idea behind **Tandem**, a harness I built for [Claude Code](https://www.anthropic.com/claude-code). A harness here means a set of skills, rules and hooks that shape how the agent works. Claude does the reading, writing and checking. The developer steers. Nothing that changes what gets built is decided without them, and no stage moves on until they say so.

**The harness is on GitHub: [github.com/aamersadiq/tandem](https://github.com/aamersadiq/tandem).**

## From the last post to a harness of our own

In the [previous post](https://aamersadiq.github.io/2026/Ai-Powered-Developer-Journey/) we followed a developer from their first day on a new project to their first merged feature, and looked at which agent skills help at each stage: setting up the machine, capturing the business knowledge, mapping the code, and delivering the first feature. For that last stage we pointed at full harnesses: GitHub's [Spec Kit](https://github.com/github/spec-kit), [eng-flow](https://github.com/m3dcodie/eng-flow), Jesse Vincent's [superpowers](https://github.com/obra/superpowers) and Addy Osmani's [agent-skills](https://github.com/addyosmani/agent-skills).

Now it's time to build a harness ourselves.

Look at what a new engineer actually does once they're set up on a team. It comes down to three jobs:

1. **Pick up a Jira ticket and deliver it:** understand it, ask questions, plan it, build it, get it reviewed, raise the PR.
2. **Review other people's code:** read a teammate's pull request and give useful, fair feedback.
3. **Debug an issue:** something is broken, and they need to find out why before they fix it.

Tandem covers exactly those three, plus the setup that comes before them:

| The job | Tandem |
|---------|--------|
| Get to know the team's repos | `/setup`: explores every repo, confirms what it found with you, writes the rules |
| Deliver a Jira ticket | `/spec` → `/plan` → `/build` → `/review` → `/ship` |
| Review a teammate's PR | `/code-review` (writes the review, never posts it) |
| Debug an issue | `/debug` (root cause first, then the fix) |

Spec Kit, superpowers and eng-flow are full-fledged harnesses, and they're excellent. Tandem is built on their ideas (the details are further down), but it's deliberately **lighter**. It's also shaped around **how a new developer is introduced to a team**:

- They don't know the codebase yet, so they read first and **ask before assuming**.
- They work across **the team's several repos**, not one.
- A senior signs off their work at each step, so **every stage has a gate that only a human can pass**.
- Their decisions are **written down**, so the next person can see why.

The rest of this post covers how it works, what it borrows, and what it leaves out.

## The problem

We had four problems with AI coding agents:

1. **They fill gaps with guesses.** A short ticket becomes a full feature. The choices that matter (error behaviour, permissions, what's out of scope) get made quietly, and you find them in the diff.
2. **They say "done" too early.** "Tests should pass now" is not the same as tests passing.
3. **They approve their own work.** Even when a process says "ask the human", the agent is the one writing "approved" into the file.
4. **Real work spans more than one repo.** A front-end and an API, each with its own stack, commands and habits. Most setups assume one repo.

We also wanted the agent's writing to be plain. Short sentences, facts, and file references, without words like "robust" or "seamless".

## The shape of Tandem

Tandem lives in a folder that holds several repos side by side:

```
workspace/
├── CLAUDE.md, README.md, .mcp.json, .claude/, work/   ← Tandem (only here)
├── web-app/                                            ← your repos, no Claude files inside
└── api/
```

All the Claude files sit at the top. The repos stay clean, and a hook blocks anyone (including Claude) from adding Claude files inside them.

A feature moves through five skills. Each one is run by hand and ends at a **gate**. The next stage won't start until you pass that gate:

```
/spec ─[G1]─► /plan ─[G2]─► /build ─[G3]─► /review ─[G4]─► /ship ─[G5]─► done
```

Everything for a feature goes in one folder, `work/features/<feature>/`. Three files run through the whole flow:

- **`State.md`**: where the feature is now, including the status of each gate. Every skill reads it first and updates it last, so you can stop at any point and pick up in a new session.
- **`Decisions.md`**: every decision, with the question, the options, who decided and why. Append-only.
- **`Dashboard.html`**: a page built from those files that shows where everything stands.

## The five steps

### /spec: look first, then ask one question at a time

`/spec` starts by reading the ticket (from Jira, through an MCP connection) or your own text.

A ticket rarely holds the whole picture. The business rules usually live in Confluence pages, design docs and ADRs. So `/spec` **asks you for them**: "Are there other documents I should read for this?" It reads each link you give it: Confluence through the same MCP connection, plus files and web pages. Every source is listed in the spec with what it contributed. If it can't open one, it says so and asks you to paste the relevant part, rather than guessing what the document says. And when two sources disagree, it doesn't pick a winner. The conflict becomes a question for you.

Then, before asking anything else, it **reads the code** in each repo that might be affected. Its first message plays back what it understood, with what it found in the documents and the code that bears on the request.

Then it asks you how big this is: a small change or a project. A small change gets a short spec. A project gets the full treatment.

Then comes the interview, **one question at a time**:

- The first question is open: *what problem does this solve, and for whom?* No options are attached, because options steer people.
- After that it works through a fixed list of areas: scope, users, data rules, error and empty states, security, other systems, edge cases, and vague words like "fast" with no number. It asks about the gaps with the biggest effect first.
- Each question is a full sentence followed by one line on *why it matters*. When it's a choice, Claude gives its recommendation and the reason.
- After each answer, the spec is updated straight away and any text the answer contradicts is replaced.

The spec ends up with user stories ordered by priority, where the top story must be useful on its own. It also gets Given/When/Then scenarios, numbered requirements (`FR-001`) and measurable success criteria (`SC-001`). Before hand-off it runs its own quality check. For a project, a **fresh agent reads the spec as the person who has to build it** and lists what's still unclear. Only the questions that are really yours come back to you.

**Exit gate G1, Spec approved:** repos confirmed, every source you gave read (or listed as unreadable, with the reason), every requirement testable, no open question blocking the plan, and your approval.

### /plan: approaches first, then tasks

`/plan` surveys the code, finds a working example of the same pattern in each repo, and then **lays out two or three ways to build the feature**, with what each costs. You pick one.

It checks the plan against a short list of **team principles**. These are checkable rules like "every endpoint checks who is calling" and "no new dependency without approval". Breaking one needs a reason from you, on record.

Then it writes numbered tasks (`TL-0001`, ...). Each task belongs to **exactly one repo**, which keeps cross-repo work honest. Each task also says:

- which requirements it satisfies, and why it's needed
- its **interfaces**: the exact names and shapes it provides to other tasks or the other repo
- the test to write first
- a "done when" check: a command plus the expected result

The plan then checks itself. Does every requirement have a task, and does every task map back to one? Is there any "TBD" or "handle errors" left with no detail? Do names match between the two repos? If you want pauses inside the build, for example before a database migration, they go in as checkpoint tasks.

**Exit gate G2, Plan approved:** you chose the approach, principles are checked, coverage is complete, and nothing is written until you pass it.

### /build: test first, then show the evidence

For each task, `/build` writes the test, **runs it, and watches it fail**. A test that passes straight away is testing the wrong thing. Then it writes the smallest code that makes the test pass, runs lint, and checks the task line by line against the plan. The evidence (the failure message, the pass count) goes on the task board.

Test-first is the default, not a religion. You can opt out for a task, such as config or a throwaway spike, and that opt-out is recorded.

`/build` stops when:

- it reaches a checkpoint you asked for in the plan
- the plan doesn't fit the code
- three fix attempts in a row have failed. At that point the problem is probably the design, not one more line.

**Exit gate G3, Build complete:** every task done with evidence, every checkpoint signed off, and fresh checks passing in every repo.

### /review: a second opinion that hasn't read the builder's notes

`/review` hands the change to a separate reviewer agent. It runs on a **different model** from the one that wrote the code, and it starts with none of the conversation that produced it. If you've switched models and the two match, `/review` tells you before it starts.

The review runs in two passes, each by a fresh agent:

1. **Blind pass.** The reviewer sees only the spec and the code. It never opens the builder's plan, task notes or evidence. That matters because a reviewer who reads the plan first tends to judge the code against the plan. If the plan misread the spec, both look "correct". The blind pass goes through the spec one requirement at a time. It asks whether the code does it, no less and no more, and whether a test shows it.
2. **Plan pass.** A second reviewer reads everything. It checks that each planned task was delivered, that interfaces match across repos, and that the "test failed first" evidence is real.

Both passes build the code, run the tests, and check quality, security and the team principles. Any serious finding, failing check or secret means "changes requested".

Both reports are saved in the review file **word for word**. The main session still checks each finding against the code, because an agent's report is a claim, not evidence. But its checks sit in a column beside the reviewer's words and never replace them. The session that wrote the code doesn't get to filter what you see.

You decide, finding by finding, whether to fix or leave it, and each call is recorded. If you choose fixes, the work goes back to `/build`, and the build gate has to be passed again.

**Exit gate G4, Review accepted:** both passes ran and their reports are saved, every serious finding has your call, no secrets, no principle broken without a reason.

It is still not a human review: both models are Claude, so they can share blind spots. That's why the final call on every finding, and on the gate, is yours.

### /ship: one PR per repo, with your yes at each step

`/ship` re-runs every check fresh, because code may have changed since review. It then asks how you want to finish: open a PR, push the branch only, or leave it.

For each repo it drafts a PR description for you to edit: a summary, how to test it, what changed, the tests, and links to the feature's PRs in the other repos. Then it creates the branch, commits only the planned files, pushes, and opens the PR. It asks before each of those steps and never merges.

It also writes three hand-over docs:

- a summary that makes sense on its own
- a release playbook (order across repos, checks, rollback)
- a map of what the change touches and how risky it is

**Exit gate G5, Shipped:** checks passed at ship time, PRs raised as you chose, docs written, and you confirm the hand-over.

## Gates: you pass them, not the agent

Every gate is a short checklist in the stage's own document. Claude ticks an item only when it can point to evidence: a file, a command result, a decision.

What matters is **who passes the gate**. Early on, the skills simply asked "Pass gate G1?" and recorded the yes. That works until the agent misreads a reply or skips a step, and then a gate says "Passed" when nobody passed it. Asking politely isn't the same as enforcing.

So passing a gate is now a command **you** run yourself:

```
! node .claude/scripts/gate.cjs pass clear-892 G1
```

In Claude Code, the `!` prefix runs a command as you, not as the agent. The script refuses to pass gates out of order. It also refuses an unmet checklist unless you give an override reason, which is then recorded with the unmet items. It writes your name and the date into the feature's files.

A hook blocks the agent from running that command, from loading the script, and from any edit that would turn a gate into "Passed". The agent can fill in the checklist and tell you it's ready. It can't approve its own work.

If you send work back, for example to fix review findings, the later gates reopen and have to be passed again, in order.

## A dashboard per feature

Markdown files are good for the agent and fine for you, but scanning six of them to see where a feature stands gets old. So every feature also gets a `Dashboard.html`. A small script builds it from the Markdown files, and a hook rebuilds it every time one of those files changes. It shows:

- a step bar with the gates between the steps
- the next command to run, and how many gates have passed
- every open question from every stage in one list, including gates waiting for your sign-off
- for each stage: its gate checklist, its decisions and its documents

A top-level page lists all features with their phase and gates passed, plus code reviews and bug investigations.

Because the page is generated, not written by the agent, it can't drift away from the files it shows.

## The other jobs: reviewing, debugging and setup

Delivering a ticket is only one of a new engineer's three jobs. These skills cover the other two, plus the setup that comes first:

- **/code-review** reviews *someone else's* pull request. It reads the PR through GitHub, gets an independent review (saved unedited), and checks the findings against the code. You keep or drop each finding. It writes a file with a draft comment for each one. **It never posts anything.** What goes on the PR is up to you.
- **/debug** handles bugs: reproduce first, trace back to the root cause, test one idea at a time, and stop to ask after three failed attempts. The fix starts with a test that fails.
- **/setup** fits Tandem to a new folder of repos. It explores each one: the stack, the commands (CI config first, since that's what really runs), the tests, the conventions, and the files that hold secrets. It goes through the team principles with you one by one. Then it asks you to confirm what it found and shows every file change before writing anything.

There is also **/deliver**, an opt-in run of all five stages with no questions, for spikes and demos. It records every guess as "needs confirmation". It can't pass gates either: all five are left waiting for you to check and pass afterwards.

## Rules suggest, hooks enforce

Rules are guidance Claude reads. Some always apply (how to ask, how to prove work, how gates work). Others load only when Claude works on matching files, such as a Next.js rule for `.tsx` files or a .NET rule for `.cs` files.

Hooks are scripts that run on every tool call, whatever Claude "thinks". Tandem has seven:

| Hook | What it does |
|------|--------------|
| guard-secrets | Blocks reading or editing `.env` files, keys and similar. `.env.example` is fine |
| guard-repo-clean | Blocks creating Claude files inside a child repo |
| guard-git | Blocks force push, `git reset --hard`, and pushes to `main`/`master` |
| guard-gates | Blocks the agent from passing a stage gate. Only the user can |
| guard-harness | Protects the hooks and settings themselves: the agent can still write changes to them, but every change is shown to the user for approval, and shell edits are blocked |
| format-after-edit | Runs the repo's formatter on each edited file and reports failures back to Claude |
| dashboard-after-edit | Rebuilds the feature's dashboard when its files change |

The hooks read their settings from one JSON file, so `/setup` can adjust them without touching code. They also come with a self-test.

## What we borrowed, and what we didn't

Tandem is built on the full harnesses from the previous post. We studied two of them closely, and both are worth reading in full.

**From spec-kit** we took:

- the fixed list of areas for finding gaps, and asking the highest-impact question first
- user stories ranked so the first one stands alone
- numbered requirements and measurable success criteria
- a quality checklist for the spec itself
- the cross-check that every requirement has a task and every task maps back
- the idea of a **constitution**: a few principles that plans are checked against

**From superpowers** we took:

- asking *why* before offering options, and playing back understanding
- sizing the work, and laying out approaches before planning
- the fresh "builder" read of the spec
- spelling out interfaces between tasks
- test-first with a test you've seen fail
- **evidence before claims**
- root cause before fix
- checking review feedback against the code instead of agreeing by reflex
- skill descriptions that say *when* to use the skill rather than summarising it

**We left out** spec-kit's CLI, extension system and branch-numbering scripts, and superpowers' rule that a skill must be invoked before every response, its visual companion, and its subagent-per-task build. They're good ideas in their own setting. Ours is narrower on purpose: the three jobs a new engineer does on a team, across that team's several repos, with skills run by name and a human as the gate between every step. That's what keeps it light.

**Our own additions** are the parts that came from working across repos with a human in charge:

- tasks that each belong to one repo, and linked PRs
- a decision log for every feature
- stage gates that only the human can pass
- a blind first review pass, with reports kept word for word
- a dashboard built from the files

## What it feels like to use

It is slower at the start and faster overall. The spec interview takes a few minutes, and you run five short gate commands per feature. In return, the plan has no surprises, the build has evidence for every task, and the review findings are real. When you come back after a week, `State.md` and the dashboard tell you exactly where you were, and `Decisions.md` tells you why.

The main change for the developer is that you stop reading diffs to find out what the agent decided. You decided it, it's written down, and every "approved" in the files was typed by you.

## Getting Tandem

Tandem is at **[github.com/aamersadiq/tandem](https://github.com/aamersadiq/tandem)**, MIT licensed. It runs on Windows, Mac and Linux. The repo also includes the full design history: every decision, the options considered, and why. To use it:

1. Copy its files into the folder that holds your repos.
2. Open Claude Code in that folder.
3. Run `/setup`. It explores your repos, agrees the team principles with you, and fits the rules and hooks to your stack before you start your first `/spec`.

The README covers each stage, what it produces, and what it hands to the next.

## Trying the idea yourself

You don't need Tandem itself to use the main ideas. In any Claude Code project you can:

1. Add a rule that says *ask one question at a time, and ask why before which*.
2. Keep a `Decisions.md` per feature and make every skill append to it.
3. Add a rule that says *no claim without a fresh command run*.
4. Use a separate reviewer agent on a different model. Give its first pass only the spec and the code, keep its report unedited, and check its findings before acting on them.
5. Put the things that must never happen (secrets, force pushes) in hooks, not rules.
6. Make approval a command the human runs, and block the agent from running it. Then "approved" means a person approved it.

Start there. The rest is detail, and it's all in [the repo](https://github.com/aamersadiq/tandem).
