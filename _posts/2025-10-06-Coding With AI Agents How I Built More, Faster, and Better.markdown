---
title: "Coding With AI Agents: How I Built More, Faster, and Better"
date: 2025-10-06 10:32:45
categories: [ai]
tags: [ai]
---

I have been using AI agents <strong>Cline/Roo</strong>, <strong>Claude Code</strong>, and <strong>GitHub Copilot</strong>, and want to share what that experience has been like for me.
With their help, I’ve been able to migrate a service domain data access layer from <strong>Entity Framework</strong> to <strong>Dapper</strong>, build an operational tool in <strong>Next.js</strong> that queries multiple databases and provides consolidated dashboards with search and drill down functionality, and experiment with a personal project using <strong>Python</strong>, <strong>SageMaker</strong>, and <strong>Postgres</strong> to process and transform large datasets.
Each project had its own challenges, but what tied them together was how AI agents fundamentally changed the way I approached development. They didn’t just save me time, they also made me more ambitious, more experimental, and more confident in tackling things I might have avoided in the past.

<h4>How I Used the Tools</h4>
<h5>GitHub Copilot: My Inline Coding Partner</h5>
<ul>
    <li><strong>Refactoring code:</strong> Making it more concise and elegant.</li>
    <li><strong>Adding comments:</strong> Extracting common logic into reusable methods.</li>
    <li><strong>Unit testing:</strong> Writing tests and fixing compilation errors.</li>
    <li><strong>Explaining code:</strong> Providing method-level explanations with runnable examples that deepened my understanding.</li>
</ul>
What surprised me most was how much Copilot improved my flow. Instead of breaking concentration to look up syntax or boilerplate, I could stay in the zone and let Copilot handle the repetitive parts.
<h5>Claude Code: My Terminal Companion</h5>
<ul>
    <li><strong>Wrote scripts:</strong> Quickly and cleanly.</li>
    <li><strong>Executed commands:</strong> Ran them directly in the terminal.</li>
    <li><strong>Error handling:</strong> Managed common errors gracefully, often suggesting fixes before I even thought of them.</li>
</ul>
This was a game changer for me. Instead of wrestling with obscure error messages, I had an AI that not only spotted the issue but also explained it in plain language and offered a solution.
<h5>Cline & Roo: My Architects</h5>
<ul>
    <li><strong>Cline:</strong> Felt more informal — I could throw requirements at it, and it would just start working.</li>
    <li><strong>Roo:</strong> More structured. Its “plan mode” (or architecture mode) generated detailed documents: implementation overviews, checklists, and summaries. Sometimes this felt like overkill for small tasks, but for larger ones, it was invaluable.</li>
</ul>
I especially liked using Roo for requirement briefings and brainstorming. It forced me to think through architecture and design decisions before diving into code. Cline was better when I just wanted to get moving quickly.

<h4>My Coding Workflow with AI</h4>
<ol>
  <li>
    <h5>Provide context up front</h5>
    I now begin every project with a system prompt that sets the stage. It explains what the project is, the best practices I want followed, the languages and frameworks I’m using, and any constraints or non‑negotiables. Being explicit here prevents the AI from filling in gaps with its own assumptions.
  </li>
  <li>
    <h5>Decompose tasks</h5>
    Instead of asking for everything at once, I break the work into smaller, targeted tasks. I use planning or architecture modes to shape the structure before any code is written. Narrowing the scope keeps the AI focused and makes the results more reliable.
  </li>
  <li>
    <h5>Start with the skeleton</h5>
    The first step is always to create the basic application structure: frontend pages, backend APIs, and a simple data flow with static or mock data. This gives me an end‑to‑end path early, so I can see the shape of the project and catch missing pieces quickly.
  </li>
  <li>
    <h5>Build module by module</h5>
    When I move into functional areas, I provide requirements for both backend and frontend together. For example, in a dashboard module I’ll specify the database schema, the queries to aggregate results and calculate metrics, the paging requirements for APIs, and the exact frontend layout I want — including panels, grids, buttons, and even subtle UI hints like a “go to top” button. With this level of detail, I’ve been able to create surprisingly complex UIs, such as grids with color‑changing cells and collapsible rows.
  </li>
  <li>
    <h5>Keep it modular</h5>
    I mirror folder structures between frontend and backend. If I have a dashboard module, there’s a <code>/dashboard</code> folder in both places. This makes it easier for the AI to pick up context when I ask for changes and keeps boundaries clear.
  </li>
  <li>
    <h5>Define relationships explicitly</h5>
    If a dashboard button should link to a product list page, I spell that out as a cross‑module requirement. Without this, the AI tends to invent its own connections, which can cause confusion later.
  </li>
  <li>
    <h5>Keep context small</h5>
    I target specific areas of the application rather than asking for sweeping changes across the whole codebase. It’s faster, more accurate, and easier to refine. Once something works in one place, I can ask the AI to replicate the pattern elsewhere.
  </li>
  <li>
    <h5>Review carefully</h5>
    AI sometimes takes shortcuts, like writing multiple queries instead of a single join. When that happens, I correct one instance and then ask it to apply the fix everywhere. Careful review is non‑negotiable if I want quality results.
  </li>
  <li>
    <h5>Test and verify</h5>
    I write unit tests, spot‑check code, and make sure best practices are followed. Testing isn’t just about quality — it also helps me understand what the AI produced and why.
  </li>
  <li>
    <h5>Refine and iterate</h5>
    Over time, I’ve learned how to get the best out of each agent. I keep notes on what works, what doesn’t, and the prompts that get the best results. Each project teaches me something new. What started as frustration has turned into a workflow that feels like a partnership. The agents don’t just save me time — they push me to think more clearly, design more carefully, and build with more confidence.
  </li>
</ol>

<h4>The Future of AI in Software Development</h4>
AI agents are reshaping how we build software, and I see several key shifts ahead:
<ul>
    <li><strong>Smaller teams: </strong>With AI amplifying individual productivity, small teams can achieve what once required large departments.</li>
    <li><strong>From agents to fleets: </strong>The evolution will be from single AI assistants to coordinated groups of agents working together.</li>
    <li><strong>Shift from coding to orchestration: </strong>Developers will spend less time writing every line of code and more time guiding, reviewing, and orchestrating AI agents.</li>
    <li><strong>Rise of prompt driven architecture: </strong>Requirements, prompts, and context management will become first class citizens, much like APIs and design patterns are today.</li>
    <li><strong>Continuous learning systems: </strong>AI agents will learn from the codebase itself, spotting patterns, suggesting refactors, and predicting bugs before they happen.</li>
    <li><strong>Blurring of roles: </strong>Boundaries between frontend, backend, DevOps, and data engineering will soften as AI enables individuals to span multiple domains.</li>
    <li><strong>Faster prototyping, faster failure: </strong>AI can spin up prototypes in hours, encouraging more experimentation and accelerating innovation cycles.</li>
    <li><strong>Ethics and governance: </strong>Trust, transparency, and governance will be critical to ensure AI generated code is secure, maintainable, and aligned with business goals.</li>
</ul>

With AI agents, I can now build complete applications in days that would have taken months in the traditional workflow.
I could never, ever go back to the old way of developing software.
