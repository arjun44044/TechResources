# Claude Introduction

A categorized reference of Claude's features — from the chat window, to the tools it can reach for, to the agentic apps built around it.

> Note: this is a snapshot, not a permanent spec — Claude's feature set changes over time, and availability varies by plan and region. For anything you need pinned down exactly, check [docs.claude.com](https://docs.claude.com) or [support.claude.com](https://support.claude.com).

### 1. Models

Every feature below runs on top of one of these underlying models. They trade off speed, cost, and depth of reasoning.

| Model                        | Tier     | What it's for                                                                                                            |
| ---------------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------ |
| **Haiku 4.5**          | Fastest  | Near-frontier intelligence at high speed — quick answers, simple edits, high-volume tasks                               |
| **Sonnet 5**           | Balanced | The default all-rounder — coding, agents, writing, analysis, everyday enterprise work                                   |
| **Opus 4.8**           | Deepest  | Most capable model for complex, long-horizon agentic coding and enterprise workloads                                     |
| **Mythos 5 / Fable 5** | Top tier | Sit above Opus in Anthropic's "Mythos" tier; Fable 5 carries extra safety layers for biology, cyber, and LLM R&D domains |

**Model picker** — Switch models mid-conversation depending on whether you need speed or depth.
*e.g. Draft with Haiku, then switch to Opus for the trickiest part of the task.*

**Extended & adaptive thinking** — Claude can reason step-by-step in a visible "thinking" pass before answering, with adjustable effort for harder problems, and can think between tool calls in agentic workflows.
*e.g. A gnarly proof or debugging task gets more visible reasoning than a quick factual question.*

**Long context windows** — Holds large amounts of text (long documents, big codebases, whole conversations) in view at once, with compaction and context-editing tools for very long sessions.
*e.g. Paste in an entire technical spec and ask for a section-by-section review.*

**Multilingual & vision** — Reads and writes across many languages and takes images as input — charts, screenshots, handwriting, photos.
*e.g. Upload a photo of a whiteboard sketch and ask Claude to turn it into a diagram.*

### 2. Conversation

The baseline experience — the parts of Claude you touch in nearly every chat.

- **Natural conversation** — Multi-turn chat that tracks what's been said and adapts tone to the topic.
  *e.g. "Actually, make that shorter" edits the last answer, not a new one.*
- **File & image uploads** — Drop in PDFs, Word docs, spreadsheets, code files, or images for summaries, edits, or Q&A.
  *e.g. Upload a 40-page contract and ask "what are the termination clauses?"*
- **Styles** — Saved preferences for tone, formatting, and verbosity, instead of repeating instructions every chat.
  *e.g. A "concise" style that keeps answers to a few sentences unless asked to expand.*
- **Custom user preferences** — A settings-level profile of how you like to work, applied quietly across chats.
  *e.g. Always show code with comments; skip the pleasantries.*

### 3. Creating Things

Claude doesn't just answer in text — it can produce documents, code, visuals, and slides you can open, download, and keep.

- **Artifacts** — Standalone, editable content (code, documents, React components, diagrams) that renders in a side panel and can be iterated on directly.
  *e.g. "Build me a habit tracker" produces a working interactive app you can tweak.*
- **Code execution** — Claude writes and actually runs code in a sandbox (Python, bash, etc.) to test logic, process data, or generate a file.
  *e.g. "Clean up this CSV and chart revenue by region" runs real pandas code.*
- **Document generation** — Real Word, PowerPoint, Excel, and PDF files — formatted and downloadable.
  *e.g. "Turn this outline into a slide deck" returns an actual .pptx.*
- **Inline diagrams & visuals** — Flowcharts, architecture diagrams, charts, and interactive explainers rendered directly in chat.
  *e.g. "Show me how OAuth login works" produces a step-by-step flow diagram.*
- **Structured outputs** (API) — Responses constrained to a schema — clean JSON your code can parse directly.
  *e.g. Extract invoice fields into a fixed JSON structure every time.*
- **Recipe & interactive widgets** — Purpose-built interactive views, like an adjustable-serving recipe card.
  *e.g. Ask for a pasta recipe and drag servings from 2 to 6; quantities scale live.*

### 4. Finding Information

Claude's training data has a cutoff — these tools let it reach past that, and past what's in the chat, to ground answers in something current or specific.

- **Web search** — Live searches with cited sources for anything time-sensitive.
  *e.g. "Who's the current CEO of X?" triggers a search instead of a guess.*
- **Web fetch** — Opens a specific URL and reads the full page content.
  *e.g. Paste a job posting link and ask "does my resume match this?"*
- **Image search** — Finds relevant photos to illustrate an answer when seeing something helps more than reading about it.
  *e.g. "Things to do in Kyoto" comes with photos of each spot mentioned.*
- **Live sports & weather** — Pulls current scores, standings, box scores, or local forecasts.
  *e.g. "Did the Lakers win last night?" returns the actual final score.*
- **Deep research** — For broad, multi-part questions, Claude scales up how many searches it runs, comparing and citing sources as it goes.
  *e.g. "Compare how three outlets covered the same policy decision."*

### 5. Memory & Personalization

Ways Claude can carry context across a single long conversation, or across separate ones — all opt-in and visible in settings.

- **Memory across chats** — Recalls useful details from earlier conversations (your role, ongoing projects, preferences) without you re-explaining.
  *e.g. Mention your stack once; Claude remembers it for future coding questions.*
- **Search past chats** — Look back through prior conversations to find something discussed before.
  *e.g. "What did we decide about the pricing tiers last week?"*
- **Projects** — A dedicated space with its own files, instructions, and chat history for work spanning many sessions.
  *e.g. A "Q3 Marketing" project holding brand guidelines and past drafts.*
- **Custom instructions** — Standing guidance (for a project or the whole account) that Claude applies automatically.
  *e.g. "Always answer in British English and cite sources."*

### 6. Connectors & Integrations

Claude can reach into apps you already use, through the open MCP (Model Context Protocol) standard.

- **MCP connectors** — Lets Claude talk to external tools and data sources (internal docs, project trackers, calendars) through a consistent interface.
  *e.g. Connect Google Drive so Claude can pull up "our Q3 sales deck" directly.*
- **MCP Apps** — Consumer connectors (music, food delivery, rideshare, restaurant booking) offered as choices, not picked automatically.
  *e.g. "Book me a table tonight" surfaces reservation options to choose from.*
- **Google Workspace & more** — Works with Drive, Slack, and similar tools directly when a request is clearly about your own data.
  *e.g. "Summarize the thread where we discussed the launch date" pulls it from Slack.*
- **Maps & places** — Looks up real venues, shows them on an interactive map, and lays out multi-stop itineraries.
  *e.g. "Plan a walking day trip in Lisbon" returns a mapped, ordered itinerary.*

### 7. Agentic Surfaces

Beyond the chat window, Claude powers standalone tools that act more independently.

- **Claude Code** — Agentic coding tool for developers; delegate tasks from the command line, desktop app, or mobile.
  *e.g. "Fix the failing tests in this repo and open a PR."*
- **Claude Cowork** — Agentic desktop app for non-developers, handling heavier multi-step knowledge work (research, analysis, long documents).
  *e.g. "Pull together a competitor analysis from these ten reports."*
- **Claude in Chrome** — A browsing agent that navigates sites, clicks, and fills in forms on your behalf.
  *e.g. "Find and fill out the return form for this order."*
- **Claude in Excel** — A spreadsheet agent that builds formulas and cleans data directly inside your workbook.
  *e.g. "Build a rolling 12-month forecast from these actuals."*
- **Claude in PowerPoint** — A slides agent that builds and edits presentations directly.
  *e.g. "Turn this memo into a 10-slide investor deck."*

### 8. Platforms & Access

The same Claude models, available through different front doors depending on how you work.

- **claude.ai (web, desktop, mobile)** — The consumer chat interface, syncing conversations across devices.
  *e.g. Start a chat on your laptop, continue it later on your phone.*
- **Claude API & Platform** — Lets developers build Claude into their own products, with access to every model, tool, and feature.
  *e.g. An internal support bot built on the Messages API.*
- **Cloud marketplaces** — Also available via Amazon Bedrock, Google Cloud, and Microsoft Foundry.
  *e.g. Run Claude via Bedrock inside an existing AWS security boundary.*
- **Team & Enterprise plans** — Shared workspaces with admin controls, usage management, and higher limits.
  *e.g. Central billing and access controls for a whole department.*

### 9. Trust & Safety

Guardrails that shape every feature above, rather than sitting off to the side.

- **Built-in safety training** — Trained to decline harmful requests (weapons, malware, exploitative content) regardless of framing.
  *e.g. Declines to write malware even when framed as "for education."*
- **Even-handedness** — On contested political or moral questions, presents the strongest case for different sides rather than pushing its own opinion.
  *e.g. Asked to argue a policy position, it presents opposing views too.*
- **Wellbeing-aware responses** — Watches for signs of distress or crisis and responds with care and real resources.
  *e.g. Offers crisis resources directly, without waiting to be asked.*
- **Copyright discipline** — Paraphrases rather than reproduces text from the web; never reproduces song lyrics or poems in full.
  *e.g. Summarizes a news article in its own words instead of quoting paragraphs.*

---

# ----Claude Features

These are essentially the features that make Claude more than just a chatbot—they turn it into a complete AI workspace. Some are already available, while others are newer or available only on certain plans or in beta.

I'll explain each one in detail, what it does, why it exists, and how **you (as a MERN developer and future software business owner)** could use it.

### 1. Memory Import

**What is it?**

Memory Import lets Claude import your memories or preferences from another AI service (such as ChatGPT, if supported) instead of making you teach it everything again.

Think of it as migrating your AI profile.

Instead of saying:

> I'm Arun...
>
> I use MERN...
>
> I run CreoGrid...
>
> I'm learning Prisma...

every time...

Claude can import these.

**Example**

Instead of teaching Claude:

```
I use React.

I like TypeScript.

I build SaaS.

I prefer Tailwind.
```

It already knows.

**Why useful?**

The AI becomes useful immediately.

Less repetition.

Better personalization.

**For you**

Imagine moving from ChatGPT to Claude.

Instead of rebuilding months of context...

Claude already knows

* your projects
* coding style
* goals
* preferences

### 2. Gmail Connectors

Claude connects directly to Gmail.

It can search your emails.

Example

You ask

> Show me interview emails from July.

Claude searches Gmail.

Or

> Summarize today's emails.

It reads them.

Or

> Draft replies to all recruiter emails.

Claude prepares responses.

Or

> Find invoices from Amazon.

Claude searches Gmail.

This is much faster than manually searching.

**Your use**

Imagine

```
300 recruiter emails
```

You ask

> Which companies rejected me?

Claude answers instantly.

### 3. Calendar Connector

Connects Claude to your calendar.

Example

You ask

> What's tomorrow?

Claude replies

```
10 AM

Interview

2 PM

Client Meeting

6 PM

Gym
```

It can also

* schedule meetings
* find free slots
* detect conflicts
* suggest times

**Your use**

```
Schedule client demo next week.

Avoid interview timings.

Reserve evenings for coding.
```

### 4. Slack Connector

Reads Slack channels.

Can answer

> What happened in engineering today?

Instead of reading 500 messages...

Claude summarizes.

Can also

* search conversations
* find decisions
* answer questions

Example

```
When did we decide to migrate to Prisma?
```

Claude finds the discussion.

### 5. Notion Connector

Reads Notion workspace.

Example

```
Summarize my Product Requirements.
```

Claude reads all pages.

Or

```
Create roadmap from my notes.
```

For you

Imagine

```
Business ideas

Marketing

Research

Todos

Client Notes
```

Claude connects everything.

### 6. Google Drive Connector

Claude can access

* Docs
* Sheets
* PDFs
* Slides

Example

```
Compare Resume V1 and Resume V4.
```

Or

```
Find all invoices.
```

Or

```
Summarize 40 PDFs.
```

### 7. GitHub Connector

Probably the most useful for developers.

Claude can

* read repositories
* explain code
* review pull requests
* inspect issues
* understand branches

Example

```
Find every place JWT is used.
```

Claude searches.

Or

```
Review PR #42.
```

Claude reviews.

Or

```
Explain project architecture.
```

Claude studies the repository.

### 8. Artifacts

One of Claude's biggest features.

Artifacts are interactive documents Claude creates beside the chat.

Instead of dumping code into chat...

Claude creates

* React apps
* HTML pages
* diagrams
* reports
* markdown
* spreadsheets
* presentations
* dashboards

Example

You ask

```
Build calculator.
```

Instead of

```
Code...
```

Claude creates

```
Interactive Calculator

You can click buttons.

Edit code.

Preview instantly.
```

**For you**

Build

* Landing pages
* Admin dashboards
* React components
* Business proposals

without leaving Claude.

### 9. Visualize

Claude can automatically generate visual representations of ideas.

Examples

* flowcharts
* timelines
* architecture diagrams
* organization charts
* process diagrams

Example

```
Visualize OAuth Login.
```

Claude creates

```
User

↓

Google

↓

Callback

↓

Backend

↓

JWT

↓

Dashboard
```

Much easier to understand than paragraphs.

### 10. Projects

Projects are like folders for chats.

Instead of

```
One huge chat.
```

You create

```
ResearchHub

Gym Ecommerce

CreoGrid

Learning React

Job Search
```

Each Project has

* chats
* files
* instructions
* knowledge
* context

Claude remembers only that project's context.

**Very useful**

No context pollution.

### 11. Chrome Extension

Lets Claude interact with your browser.

Examples

Summarize

* webpage
* PDF
* YouTube transcript

Explain

```
Current tab
```

Or

```
Summarize this documentation.
```

without copy-paste.

### 12. Cowork

Cowork is a collaborative mode where Claude works alongside you rather than simply answering isolated prompts.

Think

```
You

↓

Claude

↓

Continue

↓

Review

↓

Improve

↓

Repeat
```

Instead of

Question

↓

Answer

↓

End

Claude stays engaged as an active collaborator throughout a longer task.

### 13. Scheduled Tasks

Claude performs actions in the future.

Examples

```
Tomorrow

Remind me

Update README.
```

Or

```
Every Monday

Summarize GitHub Issues.
```

Or

```
Every evening

Create study plan.
```

Think of it as AI-powered reminders and recurring automations.

### 14. Dispatch

Dispatch is about delegating work.

Instead of

```
One huge prompt
```

Claude breaks it into multiple parallel jobs.

Example

```
Analyze frontend.

Analyze backend.

Review security.

Generate tests.

Update README.
```

Each runs independently.

Similar in spirit to multi-agent systems.

### 15. Claude Code

Probably Claude's biggest feature for developers.

Claude Code is a coding agent that runs in your terminal.

It can

* edit files
* search project
* run tests
* execute commands
* commit code
* understand architecture
* refactor projects

Instead of

```
Paste code.
```

Claude directly edits your project.

Very similar in concept to Google Antigravity's Agent or coding agents in modern AI IDEs.

### 16. Claude Channels

Channels are dedicated workspaces focused on a specific topic or team.

Example

```
Marketing

Sales

Engineering

Design
```

Each channel has

* conversations
* files
* shared context
* collaborators

Think of them as persistent discussion spaces rather than isolated chats.

### 17. Claude Skills

Skills are reusable capabilities or instruction bundles that teach Claude how to perform recurring tasks consistently.

Examples

```
React Expert

Node Backend Reviewer

Legal Contract Checker

Research Summarizer

SEO Writer
```

Instead of rewriting the same prompt every time, you activate the appropriate skill.

For your business, you might create skills like:

* "Generate React components using TypeScript and Tailwind"
* "Review Express APIs for security issues"
* "Write professional proposals for local business clients"

### 18. Claude Design

Claude Design focuses on design-oriented work.

It can help with

* UI layouts
* wireframes
* component ideas
* design systems
* typography
* spacing
* color palettes
* user flows

Rather than only generating code, it helps plan and refine the user experience before implementation.

### How these features fit together

You can think of Claude as having four major layers:

* **Knowledge & Context:** Memory Import, Projects, Connectors (Gmail, Calendar, Slack, Notion, Google Drive, GitHub)
* **Creation:** Artifacts, Visualize, Claude Design
* **Automation & Collaboration:** Cowork, Scheduled Tasks, Dispatch, Channels
* **Development:** Claude Code and Skills

For someone building software and a business like  **CreoGrid** , this means Claude can become more than a coding assistant. It can act as a research assistant, project manager, document writer, UI designer, code reviewer, automation agent, and collaborative workspace—all connected to the tools you already use.

---
