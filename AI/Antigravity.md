# ----Introduction & Major parts of Antigravity

**Google Antigravity IDE** , which is Google's AI-first IDE built on VS Code. It has many familiar VS Code parts, plus several AI-specific panels and workflows. 

Here's a complete breakdown of its major parts.

### 1. Activity Bar (Left-most Vertical Bar)

This is similar to VS Code.

Contains icons for:

* Explorer
* Search
* Source Control (Git)
* Run & Debug
* Extensions
* Google Extensions
* Customizations
* Agent-related views

Purpose:

* Navigate the entire IDE.

### 2. Explorer

Shows

* folders
* files
* repositories
* workspace

Functions

* create files
* rename
* delete
* drag
* open multiple folders

Exactly like VS Code.

### 3. Editor

The largest middle area.

Where you

* write code
* edit code
* compare files
* see markdown
* review generated code

Supports

* multiple tabs
* split editors
* syntax highlighting
* AI inline editing

### 4. Tab AI

Google calls this  **Tab** .

Think of it as smarter autocomplete.

Instead of only predicting

```ts
const x =
```

it predicts

* whole lines
* entire functions
* multiple blocks
* project-aware completions

It learns from your project context.

### 5. Command (Natural Language)

Instead of writing code manually you can write

> Create an Express middleware for JWT authentication.

or

> Convert this JavaScript file into TypeScript.

The AI edits your files.

### 6. Agent

This is the biggest feature.

Unlike autocomplete,

Agent can

* inspect project
* read files
* edit files
* create files
* execute terminal commands
* search project
* run builds
* fix errors
* test code

You give goals instead of individual instructions. ([Google Antigravity](https://www.antigravity.google/docs/ide-overview?utm_source=chatgpt.com "Google Antigravity Documentation"))

Example

```
Implement JWT Authentication.
```

The Agent may

* create middleware
* update routes
* install packages
* modify README
* fix imports

all automatically.

### 7. Agent Manager

Manages AI agents.

You can have

* multiple agents
* different tasks
* different workspaces

Example

Agent 1

```
Build Login Page
```

Agent 2

```
Fix Backend Bugs
```

Agent 3

```
Generate Documentation
```

They work independently. ([Google Antigravity](https://www.antigravity.google/docs/ide-overview?utm_source=chatgpt.com "Google Antigravity Documentation"))

### 8. Artifacts

One of Antigravity's unique concepts.

Artifacts are outputs created by the agent, such as:

* architecture diagrams
* implementation plans
* markdown reports
* code diffs
* screenshots
* browser recordings
* verification reports

Instead of just saying "Done," the agent provides evidence of its work. ([Google Antigravity](https://www.antigravity.google/docs/ide-overview?utm_source=chatgpt.com "Google Antigravity Documentation"))

### 9. Browser Agent

Very powerful.

The AI can control a browser.

Example tasks

* Login
* Click buttons
* Fill forms
* Read dashboards
* Test UI
* Capture screenshots
* Record user flows

Useful for

* UI testing
* end-to-end testing
* debugging web apps

([Google Antigravity](https://www.antigravity.google/docs/ide-overview?utm_source=chatgpt.com "Google Antigravity Documentation"))

### 10. Terminal

Built-in terminal.

Supports

* npm
* pnpm
* yarn
* git
* docker
* python
* bash
* powershell

The AI can execute terminal commands (subject to permissions/settings). ([Google Antigravity](https://www.antigravity.google/docs/ide-overview?utm_source=chatgpt.com "Google Antigravity Documentation"))

### 11. Problems Panel

Shows

* TypeScript errors
* ESLint errors
* build failures
* compiler errors

Same as VS Code.

### 12. Output Panel

Shows

* build logs
* extension logs
* agent logs
* language server output

### 13. Debug Panel

For debugging applications.

Supports

* breakpoints
* watch variables
* call stack
* console

### 14. Source Control

Git integration.

Supports

* commits
* branches
* merges
* pull requests
* GitHub integration

### 15. Search

Project-wide search.

Supports

* regex
* replace
* file filtering

### 16. Extensions

Install extensions just like VS Code.

Examples

* ESLint
* Prettier
* Docker
* GitLens
* Python
* Java
* C#
* Markdown tools

### 17. Google Extensions

This is the screen you mentioned earlier.

Google provides curated extensions/plugins such as

* Android
* Modern Web Guidance
* Google Data Cloud
* Google AI integrations
* Firebase tools
* Google Cloud integrations

These add specialized capabilities to the IDE. ([Google Antigravity](https://www.antigravity.google/docs/plugins?utm_source=chatgpt.com "Google Antigravity Documentation"))

### 18. Build with Google

This is also what you saw.

It is Google's collection of official plugins.

Examples include

* Android
* Modern Web Guidance
* Google Antigravity
* Firebase
* Cloud-related plugins

They install extra rules, skills, hooks, or integrations for the agent. ([Google Antigravity](https://www.antigravity.google/docs/plugins?utm_source=chatgpt.com "Google Antigravity Documentation"))

### 19. Google Data Cloud Extension

You asked about this previously.

It helps developers connect to Google Cloud data services from within the IDE.

Useful if you're working with

* BigQuery
* Cloud SQL
* Spanner
* Vertex AI
* Google Cloud projects

If you're currently focused on MERN, Next.js, React, or Node.js, you don't need it right away—you can install it later if you start using Google Cloud services.

### 20. Status Bar

Bottom of IDE.

Shows

* Git branch
* TypeScript version
* Node version
* errors
* encoding
* line endings
* notifications

### 21. AI Chat

Conversation window with the Agent.

Examples

```
Explain this code.

Optimize performance.

Convert to Prisma.

Write tests.

Refactor authentication.
```

The Agent has project context, making it more capable than a generic chat.

### 22. Feedback System

A distinctive feature of Antigravity.

You can leave comments on an Artifact like:

> Change this architecture.

> Improve naming.

> Don't use Redux.

The Agent incorporates this feedback into subsequent work. ([Google Antigravity](https://www.antigravity.google/product/antigravity-ide?utm_source=chatgpt.com "Google Antigravity - Antigravity IDE"))

### How these parts work together

```
Activity Bar
     │
     ├── Explorer
     ├── Search
     ├── Git
     ├── Extensions
     └── Google Extensions

Editor
     │
     ├── Tab AI
     ├── Command
     ├── Agent
     ├── Artifacts
     └── AI Chat

Bottom
     ├── Terminal
     ├── Problems
     ├── Output
     └── Debug

Right
     ├── Agent
     ├── Browser Agent
     └── Artifact Review
```

### For your workflow as a MERN/Next.js developer

Based on your current goals (React, TypeScript, Next.js, Prisma, PostgreSQL, AWS, and building products for clients), the parts you'll use most are:

* **Explorer** – manage your project files.
* **Editor** – write and review code.
* **Tab AI** – speed up coding with context-aware completions.
* **Agent** – implement features, refactor code, and fix bugs.
* **AI Chat** – ask project-specific questions.
* **Terminal** – run npm, Prisma, Git, and deployment commands.
* **Source Control** – manage Git workflows.
* **Browser Agent** – test your web applications automatically.
* **Artifacts** – review implementation plans and generated documentation before accepting changes.

Once you're comfortable with those, features like asynchronous agents, plugins, and Google Cloud integrations become valuable for larger or more automated projects.

---

# ----Settings

The Antigravity-specific settings are different from the standard VS Code settings. They mainly control  **AI behavior, agent permissions, security, models, browser automation, and project isolation** . Antigravity 2.0 organizes them into **Global Settings** and  **Project Settings** , so you can have different rules for different projects. ([Google Antigravity](https://antigravity.google/docs/settings?1=1&utm_source=chatgpt.com "Google Antigravity Documentation"))

Here's a complete breakdown.

### 1. Account Settings

These control your Antigravity account.

**- Sign In**

Choose which Google account you're using.

**- Telemetry**

Allows Google to collect interaction data.

Options

* On
* Off

When ON

* helps improve models
* shares interaction logs

When OFF

* your interactions aren't used for model improvement

Good choice if

* company policy requires privacy
* you're working on confidential projects

([Google Antigravity](https://antigravity.google/docs/settings?1=1&utm_source=chatgpt.com "Google Antigravity Documentation"))

### 2. Appearance

Controls the IDE appearance.

Includes

* Theme
* Font
* Panel Layout
* Editor Size
* Window Layout

Mostly similar to VS Code but integrated into Antigravity's settings. ([Google Antigravity](https://antigravity.google/docs/settings?1=1&utm_source=chatgpt.com "Google Antigravity Documentation"))

### 3. Model Usage

One of Antigravity's biggest differences.

You choose which reasoning model powers the Agent.

Examples include supported Gemini models and, where available, other compatible models. Different models may offer different balances of speed, cost, and reasoning ability. ([Google Antigravity](https://antigravity.google/docs/settings?1=1&utm_source=chatgpt.com "Google Antigravity Documentation"))

Use cases

Fast model

* autocomplete
* simple coding

Powerful model

* architecture
* debugging
* large refactors

### 4. Browser Integration

Controls Browser Agent.

Browser Agent can

* open Chrome
* click buttons
* login
* inspect pages
* take screenshots
* test applications

Settings include

* Enable Browser Agent
* Browser permissions
* Debug connection

Useful for frontend testing.

([Google Antigravity](https://www.antigravity.google/docs/ide-overview?utm_source=chatgpt.com "Google Antigravity Documentation"))

### 5. Global Permissions

Default permissions for every project.

Examples

Can agent

* execute terminal?
* edit files?
* delete files?
* create folders?
* use browser?
* access internet?

These become defaults for new projects.

([Google Antigravity](https://antigravity.google/docs/settings?1=1&utm_source=chatgpt.com "Google Antigravity Documentation"))

### 6. Customizations

Very powerful.

Contains

* MCP Servers
* Custom Skills
* Build with Google plugins

Think of it as extending your AI.

For example

```
My Company Skill

Always use our ESLint config.

Always use Tailwind.

Never use Bootstrap.

Always generate Jest tests.
```

Now every agent follows those instructions.

([Google Antigravity](https://antigravity.google/docs/settings?1=1&utm_source=chatgpt.com "Google Antigravity Documentation"))

### 7. Keyboard Shortcuts

Same as VS Code.

Customize

* Agent shortcuts
* Chat
* Terminal
* Navigation

([Google Antigravity](https://antigravity.google/docs/settings?1=1&utm_source=chatgpt.com "Google Antigravity Documentation"))

### 8. Project Folders

Project-specific.

Choose

* one folder
* multiple folders
* multiple repositories

Agent understands all simultaneously.

Example

```
Frontend
Backend
Shared Package
Design System
```

One Agent sees all four.

### 9. Local vs Worktree Mode

Unique to Antigravity.

**- Local**

Edits your actual project.

**- Worktree**

Creates isolated Git worktree.

Benefits

* safer
* experiment freely
* no damage to main branch

Excellent for AI-generated changes.

### 10. Terminal Execution Policy

One of the most important settings.

Options

**- Request Review**

Every command needs approval.

Example

```
npm install

git reset

rm

docker compose up
```

You'll be asked first.

**- Always Proceed**

Agent executes automatically.

Faster.

Riskier.

Recommended only for trusted projects.

([Google Antigravity](https://antigravity.google/docs/agent-settings?utm_source=chatgpt.com "Google Antigravity Documentation"))

### 11. Outside Workspace File Access

Controls whether the agent can access files outside your project.

Options

* Always Allow
* Always Ask
* Always Deny

Example

Project

```
ResearchHub
```

Agent tries

```
C:\Users\Arun\Documents
```

If set to

Always Ask

You'll receive a prompt.

Very important for privacy.

([Google Antigravity](https://antigravity.google/docs/agent-settings?utm_source=chatgpt.com "Google Antigravity Documentation"))

### 12. Sandbox Mode

Runs terminal commands inside an isolated environment.

Advantages

* safer
* protects your computer
* limits accidental damage

Recommended when experimenting with unfamiliar code.

([Google Antigravity](https://antigravity.google/docs/settings?1=1&utm_source=chatgpt.com "Google Antigravity Documentation"))

### 13. Project Permissions

Permissions remembered per project.

Examples

Allow

* npm install
* pnpm
* git status

But deny

* rm -rf
* modifying system folders

The Agent learns trusted actions.

([Google Antigravity](https://antigravity.google/docs/features?utm_source=chatgpt.com "Google Antigravity Documentation"))

### 14. Project Customizations

Different AI instructions for different projects.

Example

ResearchHub

```
Always use Prisma.

Always use PostgreSQL.

Always use Server Actions.
```

Gym Ecommerce

```
Always use MongoDB.

Always use Express.

Always use Mongoose.
```

Very useful.

([Google Antigravity](https://antigravity.google/docs/settings?1=1&utm_source=chatgpt.com "Google Antigravity Documentation"))

### 15. Tab Settings

These affect Antigravity's intelligent autocomplete.

**- Autocomplete**

Enable or disable AI completion.

**- Supercomplete**

Can rewrite multiple places in a file, not just complete the current line.

**- Tab-to-Jump**

Suggests the next logical place to edit.

**- Tab-to-Import**

Automatically inserts missing imports.

**- Tab Speed**

Options

* Slow
* Default
* Fast

Fast

* more suggestions

Slow

* waits for more context

**- Highlight Inserted Text**

Highlights AI-inserted code.

Useful for reviewing generated changes.

**- Clipboard Context**

Lets Antigravity use your clipboard to improve suggestions.

### Allow Gitignored Files

Allows AI assistance inside files excluded by `.gitignore`.

([Google Antigravity](https://www.antigravity.google/docs/tab?utm_source=chatgpt.com "Google Antigravity Documentation"))

### 16. Conversation Settings

Standalone conversations can have their own settings even when they're not attached to a project.

These include

* terminal policy
* file access
* permissions

Useful for quick experiments.

([Google Antigravity](https://antigravity.google/docs/settings?1=1&utm_source=chatgpt.com "Google Antigravity Documentation"))

### 17. Security Presets

Instead of changing dozens of permissions manually, Antigravity provides presets.

Common presets include:

* **Default** – balanced security and convenience.
* **Full Machine** – broader access to your machine.
* **Unrestricted** – minimal restrictions; use only in highly trusted environments.

You can also customize permissions beyond these presets.

### Settings I recommend for your workflow

Since you're developing  **React, Next.js, TypeScript, Prisma, PostgreSQL, Node.js, AWS, and client projects** , I'd recommend:

| Setting                  | Recommendation                 | Why                                                   |
| ------------------------ | ------------------------------ | ----------------------------------------------------- |
| Model                    | Best reasoning model available | Better architecture and debugging                     |
| Terminal Execution       | Request Review                 | Prevents accidental commands                          |
| Outside Workspace Access | Always Ask                     | Protects personal files                               |
| Sandbox Mode             | On                             | Safer experimentation                                 |
| Supercomplete            | On                             | Speeds up coding                                      |
| Tab-to-Jump              | On                             | Faster navigation                                     |
| Tab-to-Import            | On                             | Eliminates import boilerplate                         |
| Clipboard Context        | On                             | Improves context-aware suggestions                    |
| Highlight Inserted Text  | On                             | Makes AI changes easy to review                       |
| Browser Agent            | On                             | Great for testing React/Next.js apps                  |
| Worktree Mode            | On for large features          | Keeps AI changes isolated until you're ready to merge |

This setup provides a good balance between productivity and safety while giving you the full benefit of Antigravity's agentic features.

---

# ----FLOW Framework

- **FRAME** - Prompts like- Iam gonna give you an overview of the app I wanna build.I want you to challenge my thinking and help me refine the problem........(now write what you want to make). Use Claude for this better. Answer all the questions Claude is asking.

  - Then finally write- I would like you to design for me an SOP, a maximum of 500 characters which I could then give to an AI to develop a one-page app.

    [SOP- Standard Of Procedure]
- **LAYOUT**- Feed the SOP to Antigravity, with inspiration images of UI from sites like Dribble. Also can add "brand_guidelines.md" (for typography, etc) and "gemini.md" (to tell how to implement, instructions on how to design, etc), can put brand logo. Just add these files in the left files toolbar of Antigravity
- **ORCHESTRATION**- Can annotate or comment on any of the lines or images(UI snapshots' any small elements too) in the implementation plan provided by Antigravity. Also check the generated "Task list" which also can be commented
  -You can spawn multiple agents for the same workplace in Antigravity(not the IDE but one with white icon) that does different tasks which for example lets say to researh

Can also tell it open in localhost
