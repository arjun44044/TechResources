# ----Github Copilot Introduction & its Features

GitHub Copilot has evolved far beyond an autocomplete tool. Today it's an **AI software engineering platform** that assists throughout the development lifecycle—from writing code to planning, reviewing, debugging, testing, documenting, and even acting as an autonomous coding agent.

Below is a comprehensive overview of its major features.

### 1. Inline Code Completion

This is the feature Copilot became famous for.

As you type, it predicts:

* the next word
* the next line
* entire functions
* entire classes
* repetitive boilerplate

Example:

```ts
function calculateTotal(items) {
```

Copilot might complete the whole function.

It uses:

* current file
* surrounding code
* imported libraries
* project context

instead of only the current line.

### 2. Next Edit Suggestions

One of the newer features.

Instead of only predicting code where your cursor is,

Copilot predicts

> "The next place you're likely to edit."

Example

You rename

```ts
UserService
```

Copilot automatically suggests edits to

* imports
* interfaces
* tests
* usages

across the project.

This feels closer to an AI teammate than autocomplete. ([GitHub Docs](https://docs.github.com/en/copilot/get-started/features?ref=dailyneural.com&utm_source=chatgpt.com "GitHub Copilot features - GitHub Docs"))

### 3. Copilot Chat

A chat interface inside your IDE.

You can ask:

> Explain this code.

> Optimize this function.

> Convert JS to TS.

> Why is this bug happening?

It understands

* open files
* workspace
* terminal output
* Git changes

instead of requiring you to paste everything. ([GitHub Docs](https://docs.github.com/en/copilot/get-started/features?ref=dailyneural.com&utm_source=chatgpt.com "GitHub Copilot features - GitHub Docs"))

### 4. Agent Mode

One of the biggest additions.

Instead of answering one prompt,

Copilot becomes an autonomous coding agent.

It can

* inspect repositories
* edit multiple files
* run builds
* fix errors
* rerun tests
* continue until the task is complete

Example

```
Implement JWT authentication.
```

It may

* install packages
* create middleware
* update routes
* edit frontend
* add tests

without asking after every step. ([GitHub](https://github.com/features/copilot?utm_source=chatgpt.com "GitHub Copilot · Your AI pair programmer · GitHub"))

### 5. Multi-file Editing (Copilot Edits)

Instead of changing one file,

Copilot edits many files simultaneously.

Example

Rename

```
Product
```

to

```
InventoryItem
```

It updates

* interfaces
* APIs
* imports
* tests
* documentation

across the project.

### 6. Plan Mode

Before making changes,

Copilot can create a plan.

Example

```
Add OAuth Login
```

Plan

```
Step 1
Install packages

Step 2
Create auth middleware

Step 3
Create callback route

Step 4
Update frontend

Step 5
Test
```

You approve before implementation begins. ([GitHub](https://github.com/features/ai?utm_source=chatgpt.com "GitHub AI · AI built into every step of your workflow · GitHub"))

### 7. Codebase Understanding

Copilot understands your repository.

Ask

> How does authentication work?

It searches

* files
* imports
* dependencies
* architecture

instead of guessing.

### 8. Explain Code

Example

```
Explain this regex.
```

or

```
Explain this Prisma query.
```

Useful when learning unfamiliar code.

### 9. Debugging

Paste an error or let Copilot inspect your workspace.

Example

```
TypeError

Cannot read property...
```

Copilot can

* identify causes
* suggest fixes
* explain why it happened

### 10. Error Fixes

If your project fails

```
npm run build
```

Copilot can inspect

* compiler output
* stack traces
* affected files

and propose fixes.

### 11. Test Generation

Generate

* Jest
* Vitest
* Playwright
* Cypress
* unit tests
* integration tests

Example

```
Generate tests for auth middleware.
```

### 12. Documentation Generation

Generate

* README
* API docs
* JSDoc
* comments
* onboarding guides

Automatically from code.

### 13. Commit Message Generation

After Git changes

Copilot suggests

```
feat(auth):

Add Google OAuth login

- Added callback route
- Updated middleware
- Added tests
```

instead of a generic "update".

### 14. Pull Request Description Generation

Reads

* commits
* changed files
* code

Creates

* summary
* testing instructions
* review notes

for pull requests. ([GitHub Docs](https://docs.github.com/en/copilot/get-started/what-is-github-copilot?utm_source=chatgpt.com "What is GitHub Copilot? - GitHub Docs"))

### 15. Code Review Assistance

Can review

* style
* bugs
* maintainability
* security
* performance

before you submit a PR.

### 16. CLI Integration

GitHub Copilot also works in the terminal.

You can ask

```
Find large files

Explain this command

Generate Docker command

Why did npm fail?
```

It can also perform agentic tasks directly from the CLI. ([GitHub](https://github.com/features/copilot/whats-new?utm_source=chatgpt.com "See what’s new with GitHub Copilot · GitHub"))

### 17. Multiple AI Models

Instead of one model,

Copilot supports choosing among different AI models (availability depends on your plan and environment).

Different models can be better for

* reasoning
* coding
* speed
* cost

### 18. MCP Support

Supports

Model Context Protocol.

Meaning it can connect to

* databases
* GitHub
* APIs
* browsers
* custom tools
* enterprise systems

through MCP servers. ([GitHub](https://github.com/features/copilot?utm_source=chatgpt.com "GitHub Copilot · Your AI pair programmer · GitHub"))

### 19. Custom Agents

Organizations can build their own agents.

Example

```
Security Agent

↓

Checks vulnerabilities

↓

Suggests fixes
```

Or

```
React Agent

↓

Always use Tailwind

↓

Always use TypeScript
```

### 20. Custom Instructions

Create reusable project instructions.

Example

```
Always use

Prisma

Tailwind

Server Actions

TypeScript

Never use any
```

```
any
```

unless necessary.

Copilot follows these automatically.

### 21. Spaces

One of Copilot's knowledge management features.

Spaces organize

* documents
* repositories
* instructions
* references

into reusable knowledge contexts.

Instead of repeating

```
Here's our API docs...
```

every conversation,

the Space already contains them. ([GitHub Docs](https://docs.github.com/en/copilot/get-started/what-is-github-copilot?utm_source=chatgpt.com "What is GitHub Copilot? - GitHub Docs"))

### 22. Context Awareness

Uses

* current file
* neighboring files
* repository
* Git history
* terminal
* diagnostics

to improve suggestions.

### 23. Security Assistance

Can detect

* SQL injection
* XSS
* unsafe APIs
* credential leaks

and recommend safer alternatives.

### 24. Terminal & Build Understanding

If

```
npm install
```

fails,

Copilot understands

* logs
* compiler messages
* dependency conflicts

and helps resolve them.

### 25. IDE Integration

Works in

* VS Code
* Visual Studio
* JetBrains IDEs
* Neovim
* Xcode (selected features)
* Eclipse (selected features)

### 26. GitHub Integration

Available throughout GitHub itself.

Examples

* Issues
* Pull Requests
* Reviews
* Discussions
* Repository navigation

without leaving GitHub.

### 27. Enterprise Features

For organizations,

Copilot supports

* policy management
* custom models
* organization-wide instructions
* audit controls
* data residency
* administrator controls

### 28. Agent Customizations

Teams can define

* reusable workflows
* approval rules
* hooks
* instruction files
* agent behavior

so every developer gets consistent AI assistance.

### 29. Repository Search & Navigation

Ask

```
Where is authentication implemented?

Where are all API routes?

Find every Redis usage.
```

Copilot locates the relevant code quickly.

### 30. Natural Language Coding

Instead of writing code first,

you describe intent.

Example

```
Build a dashboard with

JWT authentication

Prisma

Role permissions

Tailwind
```

Copilot generates and iterates on the implementation.

### How it compares to other AI coding tools

* **GitHub Copilot** is strongest when you're already working inside GitHub and supported IDEs. It combines autocomplete, chat, repository awareness, agentic coding, GitHub workflow integration, and MCP support into a single platform.
* **Claude Code** is especially strong at repository-scale reasoning, long-running coding sessions, and terminal-driven development.
* **Google Antigravity** emphasizes agent workflows, browser automation, artifacts, and deep integration with Google's ecosystem.

For your workflow— **React, Next.js, TypeScript, Prisma, PostgreSQL, Node.js, AWS, and client projects through CreoGrid** —the Copilot features you'll likely use the most are  **Agent Mode, Plan Mode, Multi-file Editing, Chat, CLI integration, custom instructions, MCP support, and repository-aware code understanding** . Those features can significantly reduce repetitive work while still letting you review and control the changes.

---

# ----Shortcuts

Below are the **default GitHub Copilot shortcuts for VS Code on Windows** (which also apply to Google Antigravity since it's VS Code-based, unless you've customized keybindings).

| Feature                                               | Shortcut                                     | What it does                                                                                                                                                                                                                     |
| ----------------------------------------------------- | -------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Accept inline suggestion**                    | **Tab**                                | Accepts the current AI suggestion. ([GitHub Docs](https://docs.github.com/en/copilot/reference/keyboard-shortcuts?tool=jetbrains&utm_source=chatgpt.com "Keyboard shortcuts for GitHub Copilot in the IDE - GitHub Docs"))             |
| **Dismiss suggestion**                          | **Esc**                                | Hides the current suggestion. ([GitHub Docs](https://docs.github.com/en/copilot/reference/keyboard-shortcuts?tool=jetbrains&utm_source=chatgpt.com "Keyboard shortcuts for GitHub Copilot in the IDE - GitHub Docs"))                  |
| **Next suggestion**                             | **Alt + ]**                            | Shows the next completion. ([GitHub Docs](https://docs.github.com/en/copilot/reference/keyboard-shortcuts?tool=jetbrains&utm_source=chatgpt.com "Keyboard shortcuts for GitHub Copilot in the IDE - GitHub Docs"))                     |
| **Previous suggestion**                         | **Alt + [**                            | Shows the previous completion. ([GitHub Docs](https://docs.github.com/en/copilot/reference/keyboard-shortcuts?tool=jetbrains&utm_source=chatgpt.com "Keyboard shortcuts for GitHub Copilot in the IDE - GitHub Docs"))                 |
| **Trigger inline suggestion manually**          | **Alt + \**                                | Requests a completion immediately. ([GitHub Docs](https://docs.github.com/en/copilot/reference/keyboard-shortcuts?tool=jetbrains&utm_source=chatgpt.com "Keyboard shortcuts for GitHub Copilot in the IDE - GitHub Docs"))             |
| **Open Copilot panel (additional suggestions)** | **Ctrl + Enter**                       | Opens a panel with alternative generated code. ([GitHub Docs](https://docs.github.com/en/copilot/reference/keyboard-shortcuts?tool=jetbrains&utm_source=chatgpt.com "Keyboard shortcuts for GitHub Copilot in the IDE - GitHub Docs")) |
| **Open Chat View**                              | **Ctrl + Alt + I**                     | Opens the full Copilot Chat sidebar. ([Visual Studio Code](https://code.visualstudio.com/docs/agents/reference/ai-features-cheat-sheet?utm_source=chatgpt.com "AI features in VS Code cheat sheet"))                                   |
| **Inline Chat**                                 | **Ctrl + I**                           | Starts an inline conversation in the current editor or terminal. ([Visual Studio Code](https://code.visualstudio.com/docs/agents/reference/ai-features-cheat-sheet?utm_source=chatgpt.com "AI features in VS Code cheat sheet"))       |
| **Switch Chat to Agent Mode**                   | **Shift + Ctrl + I**                   | Switches the chat to use AI agents. ([Visual Studio Code](https://code.visualstudio.com/docs/agents/reference/ai-features-cheat-sheet?utm_source=chatgpt.com "AI features in VS Code cheat sheet"))                                    |
| **New Chat Session**                            | **Ctrl + N** *(when Chat has focus)* | Starts a fresh conversation. ([Visual Studio Code](https://code.visualstudio.com/docs/agents/reference/ai-features-cheat-sheet?utm_source=chatgpt.com "AI features in VS Code cheat sheet"))                                           |
| **Voice Chat**                                  | **Hold Ctrl + I**                      | Starts voice input for Copilot Chat (if enabled). ([Visual Studio Code](https://code.visualstudio.com/docs/agents/reference/ai-features-cheat-sheet?utm_source=chatgpt.com "AI features in VS Code cheat sheet"))                      |

### Features that don't have a default shortcut

Many Copilot features are accessed through the UI, right-click menus, or the Command Palette rather than a built-in hotkey.

| Feature                 | How to access                                                                                                                                                                                                                                                                          |
| ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Agent Mode              | Chat →**Agent**                                                                                                                                                                                                                                                                 |
| Plan Mode               | Chat →**Plan**                                                                                                                                                                                                                                                                  |
| Multi-file Edits        | Chat →**Edits**                                                                                                                                                                                                                                                                 |
| Explain Code            | Select code → Right-click →**Explain**                                                                                                                                                                                                                                         |
| Fix Code                | Select code → Right-click →**Fix**                                                                                                                                                                                                                                             |
| Generate Tests          | Select code → Right-click →**Generate Tests**                                                                                                                                                                                                                                  |
| Review Changes          | Source Control / Pull Request views                                                                                                                                                                                                                                                    |
| Generate Commit Message | Source Control panel                                                                                                                                                                                                                                                                   |
| Generate PR Description | GitHub Pull Request interface                                                                                                                                                                                                                                                          |
| Repository Search       | Ask in Chat ("Where is JWT implemented?")                                                                                                                                                                                                                                              |
| MCP Tools               | Available automatically when configured                                                                                                                                                                                                                                                |
| Custom Agents           | Select from the Chat/Agent interface                                                                                                                                                                                                                                                   |
| Skills                  | Invoke via slash commands (for example,`/skill-name`) if configured. ([Reddit](https://www.reddit.com/r/GithubCopilot/comments/1r379sg/new_vs_code_stable_with_hooks_queuing_steering/?utm_source=chatgpt.com "New VS Code stable with hooks, queuing, steering, and skills as /command")) |

### Command Palette shortcuts

These are the shortcuts you'll use frequently in addition to Copilot itself.

| Action               | Shortcut                   |
| -------------------- | -------------------------- |
| Open Command Palette | **Ctrl + Shift + P** |
| Open Extensions      | **Ctrl + Shift + X** |
| Open Explorer        | **Ctrl + Shift + E** |
| Open Search          | **Ctrl + Shift + F** |
| Open Source Control  | **Ctrl + Shift + G** |
| Open Terminal        | **Ctrl + `**         |

### Power-user shortcuts you'll use every day

If you're coding full-time with Copilot, these are the ones you'll likely use constantly:

1. **Tab** → Accept AI suggestion.
2. **Esc** → Reject suggestion.
3. **Alt + ]** → Next suggestion.
4. **Alt + [** → Previous suggestion.
5. **Alt + \** → Force a suggestion.
6. **Ctrl + I** → Inline Chat.
7. **Ctrl + Alt + I** → Open Chat.
8. **Shift + Ctrl + I** → Switch to Agent Mode.
9. **Ctrl + Enter** → Open additional suggestions.
10. **Ctrl + Shift + P** → Access every Copilot command via the Command Palette. ([GitHub Docs](https://docs.github.com/en/copilot/reference/keyboard-shortcuts?tool=jetbrains&utm_source=chatgpt.com "Keyboard shortcuts for GitHub Copilot in the IDE - GitHub Docs"))

Once you become comfortable with these, you can perform most coding, debugging, refactoring, and agent-driven workflows without leaving the keyboard.

---

# ----Copilot Modes

These are some of the most important concepts in  **GitHub Copilot (2026)** . Once you understand  **Ask** ,  **Edit** ,  **Agent** , and  **Context** , you'll know how to use Copilot efficiently instead of treating it like a normal chatbot.

**-- The Three Modes**

Think of Copilot as having three levels of capability.

```
Ask
 ↓
Edit
 ↓
Agent
```

Each level gets more powerful.

### 1. Ask Mode

Ask Mode is the safest mode.

It  **never changes your project** .

It only answers questions.

Think of it as ChatGPT that already knows your codebase.

**What can Ask do?**

It can

* Explain code
* Explain errors
* Explain architecture
* Answer framework questions
* Search your project
* Compare files
* Explain libraries
* Suggest improvements

Example

You ask

```
How does authentication work in this project?
```

Copilot

* searches files
* reads imports
* understands routes

Then explains.

It doesn't modify anything.

Another example

```
Why is this React component rerendering?
```

Copilot analyzes the component.

Returns explanation.

Nothing changes.

Think of Ask as

```
Project

↓

Read

↓

Answer
```

**-- When should you use Ask?**

Examples

```
Explain Prisma Schema

Explain JWT

Find every API route

Where is Redux configured?

Why is build failing?

How does login work?
```

No file modifications.

### 2. Edit Mode

Edit Mode can modify code.

Unlike Ask,

it writes code.

Suppose you select

```tsx
<button>Submit</button>
```

You ask

```
Make this look modern.
```

Copilot edits

```tsx
<button className="rounded-xl bg-blue-600 px-4 py-2 text-white hover:bg-blue-700">
```

Another example

You highlight

```
20 lines
```

Ask

```
Convert to TypeScript.
```

Copilot edits only the selected code.

**Edit Mode can**

✅ edit

✅ rewrite

✅ refactor

✅ optimize

✅ generate tests

✅ improve performance

Think of Edit Mode as

```
Read

↓

Modify

↓

Show Diff

↓

You Accept
```

Usually you'll see

```
Accept

Reject

Undo
```

**--When should you use Edit?**

Examples

```
Convert JS to TS

Optimize this function

Add comments

Fix ESLint

Generate Jest tests

Convert CSS to Tailwind

Use Prisma instead of SQL
```

### 3. Agent Mode

Agent Mode is completely different.

Instead of editing one file,

it completes entire tasks.

Example

You ask

```
Implement Google Authentication.
```

Agent may

```
Install packages

↓

Create middleware

↓

Update routes

↓

Update frontend

↓

Modify Prisma

↓

Run tests

↓

Fix errors

↓

Retry

↓

Complete
```

without asking after every step.

Agent can

✅ Create files

✅ Delete files

✅ Rename files

✅ Run npm

✅ Install packages

✅ Run tests

✅ Run builds

✅ Search project

✅ Fix compiler errors

✅ Continue automatically

Imagine saying

```
Build an Admin Dashboard.
```

Agent may edit

```
Dashboard.tsx

Sidebar.tsx

Navbar.tsx

Routes.ts

API

Database

Styles

Tests
```

30+ files.

This is why Agent feels like

an AI junior developer.

### Ask vs Edit vs Agent

| Feature                   | Ask | Edit      | Agent |
| ------------------------- | --- | --------- | ----- |
| Reads code                | ✅  | ✅        | ✅    |
| Explains                  | ✅  | ✅        | ✅    |
| Edits selected code       | ❌  | ✅        | ✅    |
| Creates files             | ❌  | Limited   | ✅    |
| Multi-file edits          | ❌  | Sometimes | ✅    |
| Runs terminal             | ❌  | ❌        | ✅    |
| Installs packages         | ❌  | ❌        | ✅    |
| Runs tests                | ❌  | ❌        | ✅    |
| Fixes build automatically | ❌  | ❌        | ✅    |
| Long-running tasks        | ❌  | ❌        | ✅    |

---

# ----Context

Context is probably the most important concept.

Without context,

AI guesses.

With context,

AI understands.

Imagine asking

```
Explain this.
```

Without context

```
??
```

AI doesn't know.

With context

```
Current file

Open tabs

Workspace

Selected code

Terminal

Git changes
```

Now AI understands.

Think of context as

```
Everything the AI can see.
```

### What automatically becomes Context?

Usually

* Current file
* Cursor position
* Selected code
* Open editors
* Workspace
* Git status
* Terminal output
* Compiler errors
* Build logs

Suppose

```
npm run build
```

fails.

Terminal becomes context.

You ask

```
Fix it.
```

Copilot already knows the error.

No copy-paste needed.

### Adding Context

Sometimes you want more information.

You manually add it.

Examples

```
README

Schema.prisma

.env.example

package.json

API.md

Database.sql
```

Now AI reads them too.

Suppose

```
Explain login.
```

Add

```
auth.ts

middleware.ts

schema.prisma
```

Answer becomes much better.

### Types of Context

##### 1 Current File

```
Navbar.tsx
```

Only this file.

##### 2 Selection

Highlight

```
20 lines
```

Only those lines.

##### 3 Folder

Example

```
components/
```

Entire folder.

##### 4 Workspace

Whole repository.

##### 5 Multiple Files

Example

```
schema.prisma

auth.ts

middleware.ts
```

All become context.

##### 6 Git Diff

Only changed files.

Useful

```
Review my changes.
```

##### 7 Terminal

Build output

Runtime logs

Errors

##### 8 Web

If web search is enabled,

documentation can become context.

### Adding Files as Context

Usually

```
@
```

or

```
Add Context
```

You'll see

```
@file

@folder

@terminal

@problems

@changes

@workspace
```

Example

```
@schema.prisma

Explain relationships.
```

### Adding Tools as Context

Modern Copilot can use tools.

Instead of only reading files,

it can call tools.

Examples

##### Terminal Tool

```
Run

npm test
```

Result becomes context.

##### Git Tool

Read

```
Branches

Commits

Diff

History
```

##### GitHub Tool

Read

```
Issues

PRs

Discussions

Repositories
```

##### Problems Tool

Reads

```
ESLint

TypeScript

Compiler Errors
```

Automatically.

##### Search Tool

Searches entire project.

Example

```
Find JWT usage.
```

##### File Tool

Reads

```
Any file

Create file

Rename

Delete
```

(primarily in Edit and Agent modes, subject to your approval and settings).

##### Test Tool

Runs

```
Jest

Vitest

Playwright
```

Results become context.

##### Build Tool

Runs

```
npm run build
```

Reads output.

Attempts fixes (in Agent mode).

##### Browser Tool

If configured,

Agent can

```
Open localhost

Click buttons

Take screenshots

Verify UI
```

##### MCP Tools

This is the newest concept.

Instead of only

```
Filesystem
```

Copilot can access

```
GitHub

Postgres

Slack

Jira

Notion

Stripe

Custom ERP
```

through MCP servers.

Example

```
Show all customers created today.
```

Instead of generating SQL,

Agent

↓

Postgres MCP

↓

Database

↓

Results

### Complete Flow

Imagine you ask

```
Implement Google Login.
```

Context

```
Current file

schema.prisma

package.json

Workspace

Terminal

Git

Auth folder
```

Tools

```
Filesystem

Terminal

npm

Git

Problems

Tests
```

Agent

```
Reads Context

↓

Uses Tools

↓

Edits Files

↓

Runs Build

↓

Fixes Errors

↓

Runs Tests

↓

Shows Diff

↓

Done
```

### For your workflow

Since you're building **React, Next.js, TypeScript, Prisma, PostgreSQL, and AWS** applications, a productive pattern is:

* Use **Ask** when you're learning a codebase or debugging.
* Switch to **Edit** for localized refactors, conversions (e.g., JavaScript → TypeScript), and UI improvements.
* Use **Agent** for complete features like authentication, dashboards, API integrations, or project-wide refactoring.
* Always provide the most relevant **context** (such as `schema.prisma`, `package.json`, the affected components, and terminal errors). The quality of Copilot's output is often directly proportional to the quality of the context you give it.

---

# ----Fetch Tool As Context

The **Fetch** tool is one of the context tools available to GitHub Copilot (especially in Chat and Agent mode). Its purpose is simple:

> **Fetch retrieves information from external resources and makes that information available as context for the AI.**

Instead of you copying and pasting documentation or web pages into the chat, Copilot can fetch them directly (subject to your permissions and network access).

### What is the Fetch tool?

Think of it like this:

```text
You
   │
   ▼
"Read this URL"
   │
   ▼
Fetch Tool
   │
   ▼
Downloads the content
   │
   ▼
Adds it to Copilot's context
   │
   ▼
Copilot answers using that content
```

Without Fetch:

```text
Website
   ↓
You copy everything
   ↓
Paste into chat
   ↓
AI reads it
```

With Fetch:

```text
Website
   ↓
Fetch Tool
   ↓
AI reads it directly
```

### What can Fetch retrieve?

Depending on your Copilot configuration and permissions, it can fetch things like:

* Web pages
* API documentation
* Markdown documentation
* Technical guides
* Public GitHub files
* Documentation sites
* Internal documentation (if your organization allows it)
* Raw text files

### Example 1 – Reading documentation

You ask:

```text
Read https://react.dev/reference/react/useEffect and explain it.
```

Instead of asking you to paste the page:

```
Fetch
    ↓
Downloads page
    ↓
Provides it as context
    ↓
Copilot explains it
```

### Example 2 – API documentation

Suppose you're using Stripe.

You ask:

```text
Read the latest Stripe Checkout documentation.
Generate a Next.js example.
```

The flow becomes:

```
Fetch
↓
Stripe Docs
↓
Context
↓
Copilot generates code
```

### Example 3 – GitHub README

Suppose a library has a README.

You ask:

```text
Read this GitHub README.

Explain installation.
```

Fetch reads:

* README
* Examples
* Installation steps

Then Copilot explains them.

### Example 4 – Internal company documentation

Many companies have documentation like:

```
https://wiki.company.com/authentication
```

Instead of copying it:

```
Fetch
↓
Company Wiki
↓
Context
↓
Copilot understands company rules
```

This is especially useful in enterprise environments.

### Fetch vs Web Search

These are different.

**--- Web Search**

Purpose:

Find information.

Example:

```text
What is the latest version of Next.js?
```

AI searches the web.

**-- Fetch**

Purpose:

Read a **specific resource** you provide.

Example:

```text
Read this documentation.

Explain it.
```

| Web Search            | Fetch                            |
| --------------------- | -------------------------------- |
| Searches the internet | Reads a specific URL or resource |
| Finds information     | Retrieves specified content      |
| Good for discovery    | Good for analysis                |

### Fetch vs MCP

People often confuse these.

**-- Fetch**

Reads information.

```
Website
↓
Read
↓
Context
```

**-- MCP**

Performs actions or queries through connected tools.

```
Database
↓
Query

GitHub
↓
Create PR

Slack
↓
Send message
```

MCP is interactive.

Fetch is primarily read-only.

### Fetch vs File Context

Suppose you have:

```
README.md
```

Already in your project.

You can add it as context directly.

No Fetch needed.

Suppose you have:

```
https://react.dev
```

Not inside your project.

Then Fetch is useful.

### Fetch in Agent Mode

Suppose you ask:

```text
Upgrade this project to React 20.

Use the official migration guide.
```

Agent might:

```
Fetch React migration guide
↓
Read it
↓
Analyze project
↓
Modify files
↓
Run tests
```

### Fetch + Context

Imagine this prompt:

```text
Read:

https://tanstack.com/router

Use it to replace React Router.
```

Context becomes:

```
Workspace
+
Current project
+
Fetched documentation
```

The AI now has both your code and the relevant external documentation.

### Limitations

Fetch generally works best with:

* Public web pages
* Documentation sites
* Markdown files
* Text-based content

It may not work well with:

* Login-protected pages (unless your environment provides authenticated access)
* Highly interactive web applications
* Content rendered only after complex client-side JavaScript
* Binary files (images, videos, ZIP archives)

### When should you use Fetch?

Use Fetch when you want Copilot to work from a specific source rather than its general knowledge. Typical use cases include:

* Reading official framework documentation before generating code.
* Using a specific API reference.
* Following a migration guide.
* Summarizing a long technical article.
* Explaining a GitHub README or design document.
* Incorporating organization documentation into the conversation (where supported).

### How Fetch fits into the overall picture

```text
Your Prompt
      │
      ▼
   Add Context
      │
      ├── Current file
      ├── Workspace
      ├── Terminal output
      ├── Git changes
      └── Fetch (external documentation)
                 │
                 ▼
         Copilot understands both
         your project and the external source
                 │
                 ▼
        Ask / Edit / Agent produces the result
```

For your stack ( **React, Next.js, TypeScript, Prisma, PostgreSQL, AWS** ), Fetch is particularly valuable when you're integrating a new library or upgrading versions. Instead of relying on the model's prior knowledge, you can have Copilot read the **official documentation or migration guide** first, which often leads to more accurate code and fewer outdated patterns.

---

# ----Simple Browser and Browser as Context

### What is the Simple Browser?

VS Code has a built-in browser called  **Simple Browser** .

It lets you open websites directly inside the editor.

For example

```
localhost:3000
```

or

```
https://example.com
```

without leaving VS Code.

Instead of

```
VS Code
↓

Alt+Tab

↓

Chrome

↓

Alt+Tab

↓

VS Code
```

everything stays inside VS Code.

### Opening the Simple Browser

**-- Method 1**

Press

```
Ctrl + Shift + P
```

Type

```
Simple Browser: Show
```

Then enter

```
http://localhost:3000
```

or

```
http://localhost:5173
```

or any URL.

**-- Method 2**

If your development server is running,

click the localhost URL shown in the terminal.

VS Code often gives the option to open it in the embedded browser.

### Why use the Simple Browser?

Because Copilot can understand what you're looking at.

Instead of saying

> "The login page has a blue button."

it can actually inspect the page.

### Browser Context

This is the really interesting feature.

The browser becomes part of the AI's context.

Imagine your app is

```
Dashboard

Sidebar

Navbar

Analytics Cards

Charts

Footer
```

Normally you'd tell AI

```
The button on the second card...
```

Instead,

you click the element.

The AI now knows exactly which element you mean.

### Selecting an Element

In Agent workflows,

there's an option similar to

```
Select Element
```

or

```
Pick Element
```

When enabled,

moving your mouse over the page highlights elements.

For example

Hover over

```
Button
```

↓

Highlighted

↓

Click

↓

Element becomes context.

Instead of saying

```
The blue button under the chart...
```

Copilot receives something like

```
button.submit

class="bg-blue-500"

Located in

Dashboard.tsx
```

Now it knows exactly what you're referring to.

### What Information Gets Added?

Suppose you select

```
Login Button
```

Copilot may receive context including:

* HTML element
* CSS classes
* DOM hierarchy
* Accessibility attributes
* Computed text
* React component mapping (when available)
* Screenshot of the selected area

This is much richer than a plain screenshot.

### Example

Imagine this page

```
--------------------------

Dashboard

Welcome Arun

[Add User]

Statistics

--------------------------
```

Hover

```
Add User
```

Click

Now ask

```
Make this button green and add an icon.
```

Instead of searching the project,

Copilot already knows

* which button
* which component
* where it lives

Agent edits the correct file.

### Another Example

Select

```
Sidebar
```

Ask

```
Make this collapsible.
```

Agent now understands

* exact element
* related CSS
* component hierarchy

and edits only what's necessary.

### Inspect Mode

This works similarly to Chrome DevTools.

Hover

```
Card
```

↓

Shows outline

↓

Click

↓

Selected

↓

Added as context

You don't need to manually inspect HTML.

### Screenshot Context

Sometimes you don't even select an element.

Instead

```
Take Screenshot
```

The screenshot itself becomes context.

Then ask

```
Improve this UI.
```

Copilot analyses

* spacing
* typography
* colors
* layout
* accessibility

before suggesting edits.

### DOM Context

Instead of only pixels,

Agent can understand the DOM.

Example

```
<div>

<button>

<Card>

<nav>

<form>
```

This lets it distinguish between

```
button
```

and

```
link
```

or

```
card
```

without guessing.

### React Component Mapping

One of the nicest parts.

Suppose

```
<Button />
```

renders

```
<button>
```

Selecting the button can help Copilot trace it back to the originating React component.

Instead of editing generated HTML,

it edits

```
Button.tsx
```

or

```
Dashboard.tsx
```

where the UI is actually defined.

### Using It With Agent Mode

Imagine your page

```
Products

+ Add Product

Table
```

Select

```
Table
```

Ask

```
Replace this table with cards.
```

Agent may

```
Read selected element

↓

Locate component

↓

Update JSX

↓

Update CSS

↓

Run application

↓

Verify UI

↓

Done
```

### Browser Tools

The browser can also become an interactive tool for Agent.

For example:

```
Open localhost

↓

Click Login

↓

Type email

↓

Click Submit

↓

Wait

↓

Take screenshot

↓

Compare

↓

Fix errors
```

This is similar to browser automation tools such as Playwright, but integrated into the AI workflow.

### When should you use element selection?

It's especially useful for:

* **UI redesigns** : "Make this card look modern."
* **Styling fixes** : "Increase the spacing in this section."
* **Accessibility improvements** : "Improve the contrast and keyboard navigation for this form."
* **Responsive fixes** : "Make this navbar mobile-friendly."
* **Component refactoring** : "Turn this repeated block into a reusable React component."

### Complete workflow

```
Run application
        │
        ▼
Open Simple Browser
        │
        ▼
Hover an element
        │
        ▼
Select it
        │
        ▼
Element + DOM + page state become context
        │
        ▼
Ask Copilot:
"Make this look like a modern SaaS dashboard."
        │
        ▼
Agent locates the React component
        │
        ▼
Edits the appropriate files
        │
        ▼
Runs the app again
        │
        ▼
Shows you the updated result
```

For someone building **React/Next.js** applications like you, this workflow is much faster than saying  *"the third button below the analytics chart"* . By selecting the actual UI element, you remove ambiguity, allowing Copilot Agent to identify the corresponding component and make more accurate changes.

---

# ---Custom Copilot Instruction File

Custom Copilot Instructions are one of the most useful features in  **GitHub Copilot (2026)** . They let you define **persistent rules and preferences** that Copilot should follow every time it helps with your project.

Think of it as giving Copilot a  **team handbook** .

Instead of repeating:

> "Use TypeScript, don't use `any`, use Tailwind, use Server Actions..."

on every prompt, you write it once in an instructions file.

### What is a Custom Copilot Instructions file?

It's a text or Markdown file stored with your project that contains instructions for Copilot.

```text
Your Prompt
      │
      ▼
Copilot
      ▲
      │
Instructions File
      │
Always loaded as context
```

Whenever Copilot answers in Ask, Edit, or Agent mode, it also reads these instructions.

### Why use it?

Without instructions:

```text
Prompt:
Create a login page.
```

Copilot might choose:

* JavaScript
* CSS Modules
* Context API
* Express
* JWT

With instructions:

```text
Always use:
- TypeScript
- Tailwind CSS
- Prisma
- Next.js App Router
- Server Actions
```

The generated code follows your standards automatically.

### What can you specify?

##### 1. Tech stack

Example:

```md
Use:

- React 19
- Next.js App Router
- TypeScript
- Tailwind CSS
- Prisma
- PostgreSQL
```

##### 2. Coding style

```md
Use arrow functions.

Prefer async/await.

Avoid nested if statements.

Keep functions under 50 lines.
```

##### 3. Naming conventions

```md
Components:

PascalCase

Variables:

camelCase

Constants:

UPPER_SNAKE_CASE

Files:

kebab-case
```

##### 4. TypeScript rules

```md
Never use any.

Prefer interfaces.

Enable strict typing.

Use discriminated unions when appropriate.
```

##### 5. React rules

```md
Use functional components.

Use hooks.

Avoid class components.

Prefer custom hooks for reusable logic.
```

##### 6. Next.js rules

```md
Use App Router.

Prefer Server Components.

Use Server Actions.

Use Route Handlers.

Avoid Pages Router.
```

##### 7. Styling

```md
Use Tailwind only.

Do not use CSS Modules.

Use shadcn/ui components.

Keep spacing consistent.
```

##### 8. Database conventions

```md
Use Prisma.

Use UUIDs.

Add indexes where appropriate.

Soft delete with deletedAt.
```

##### 9. API conventions

```md
Validate input.

Return consistent JSON.

Handle errors centrally.

Never expose stack traces.
```

##### 10. Testing

```md
Generate Vitest tests.

Use React Testing Library.

Aim for meaningful coverage.
```

##### 11. Documentation

```md
Add JSDoc for exported functions.

Keep README updated for new features.
```

##### 12. Security

```md
Sanitize input.

Use parameterized queries.

Store secrets in environment variables.

Never hardcode API keys.
```

### Example instruction file

```md
# Project Instructions

This is a Next.js SaaS project.

Always use:

- TypeScript
- Tailwind CSS
- Prisma
- PostgreSQL
- Server Actions

Never use:

- any
- inline styles
- class components

Naming:

- Components: PascalCase
- Hooks: useSomething
- Files: kebab-case

Always:

- Handle loading states
- Handle errors
- Use accessible HTML
- Keep components small
```

### Where is it stored?

Depending on the project and current Copilot support, organizations commonly keep these instructions in project-level configuration files or designated Copilot instruction files that are committed to the repository. GitHub has expanded support for repository-scoped instruction files so the guidance is shared with everyone working on the project.

**Most used path--- .github / copilot-instructions.md**

### How Copilot uses it

Suppose your file says:

```md
Never use Redux.

Use Zustand.
```

You ask:

```text
Create authentication state management.
```

Copilot generates:

```text
✓ Zustand
```

instead of

```text
✗ Redux
```

without you mentioning it again.

##### Ask Mode

Instructions influence explanations.

Example:

```text
Explain authentication.
```

If your project uses Prisma and Auth.js,

Copilot explains **your** authentication architecture instead of generic examples.

##### Edit Mode

Suppose you ask:

```text
Convert this component.
```

Instructions say:

```md
Use Tailwind.
```

The edits will generally use Tailwind instead of another styling approach.

##### Agent Mode

This is where instructions become especially valuable.

Suppose you ask:

```text
Build an admin dashboard.
```

Without instructions:

```
Agent chooses architecture itself.
```

With instructions:

```
Uses Prisma

Uses Tailwind

Uses App Router

Uses shadcn/ui

Uses Server Actions

Uses your naming conventions

Uses your folder structure
```

The entire feature follows your team's standards.

### Organization instructions

Companies can also define organization-wide instructions.

For example:

```text
Company Rules

↓

Always use Azure

↓

Always use company logger

↓

Always use internal UI library

↓

Always follow security policy
```

Every repository can inherit or extend these rules, helping teams generate consistent code.

### Repository instructions vs Personal instructions

| Personal                          | Repository                          |
| --------------------------------- | ----------------------------------- |
| Applies to your own Copilot usage | Shared with everyone on the project |
| Good for personal preferences     | Good for team standards             |
| Can differ between developers     | Keeps the whole team consistent     |

### Best practices

A good instructions file should:

* Describe the project's architecture.
* Specify the preferred tech stack.
* Define naming conventions.
* State what to avoid.
* Include security and testing expectations.
* Stay concise and up to date.

Avoid trying to describe every coding rule ever invented. Focus on the conventions that make **this project** unique.

### An example for your stack

If you were creating a Copilot instructions file for your **ResearchHub** or future **CreoGrid** projects, it might include guidance like:

```md
Project Stack

- Next.js App Router
- React
- TypeScript
- Tailwind CSS
- Prisma
- PostgreSQL
- Auth.js

Coding Standards

- Never use any.
- Prefer Server Components.
- Use Server Actions for mutations.
- Keep components under 200 lines.
- Extract reusable logic into custom hooks.

UI

- Use shadcn/ui.
- Mobile-first responsive design.
- Accessible HTML and ARIA where needed.

Backend

- Validate inputs with Zod.
- Use Prisma transactions when updating related records.
- Return typed responses.
- Handle errors consistently.

Testing

- Write Vitest unit tests for business logic.
- Keep critical user flows testable.
```

With a file like this, Copilot's Ask, Edit, and Agent modes become much more consistent because they start every task with the same understanding of how you want the project built.

---

# ----MCP & MCP Servers

MCP (Model Context Protocol) is one of the biggest changes in AI development. If APIs were the standard way for  **applications to talk to applications** , MCP is becoming the standard way for  **AI models to talk to tools and data sources** . It was introduced by Anthropic in late 2024 and is now supported by GitHub Copilot, Claude, VS Code, JetBrains, and many other AI development tools. ([GitHub Docs](https://docs.github.com/en/copilot/how-tos/provide-context/use-mcp-in-your-ide/extend-copilot-chat-with-mcp?tool=visualstudio&utm_source=chatgpt.com "Extending GitHub Copilot Chat with Model Context Protocol (MCP) servers - GitHub Docs"))

### What problem does MCP solve?

Before MCP, every AI tool needed a custom integration.

For example:

```text
GitHub Copilot  → GitHub API
Claude          → GitHub API
Cursor          → GitHub API
OpenAI          → GitHub API
```

Each AI had to build and maintain its own integration.

With MCP:

```text
GitHub
    │
GitHub MCP Server
    │
──────────────────────────────
Claude
Copilot
VS Code
JetBrains
OpenAI-compatible clients
```

The AI only needs to understand the  **MCP protocol** , not every service individually. ([GitHub Docs](https://docs.github.com/en/copilot/how-tos/provide-context/use-mcp-in-your-ide/extend-copilot-chat-with-mcp?tool=visualstudio&utm_source=chatgpt.com "Extending GitHub Copilot Chat with Model Context Protocol (MCP) servers - GitHub Docs"))

### What is an MCP Server?

An MCP Server is simply a program that exposes tools, resources, and prompts to an AI.

Think of it as a translator.

Instead of saying:

> "Call GitHub REST API."

the AI says:

> "Ask the GitHub MCP Server to create a pull request."

The server converts that into the appropriate API calls.

### What can an MCP Server expose?

An MCP server can expose three main things.

##### 1. Tools

These perform actions.

Examples:

* Create GitHub Issue
* Read PostgreSQL data
* Send Slack message
* Create Jira ticket
* Execute SQL
* Run Docker
* Search Notion

Example:

```text
AI

↓

create_issue()

↓

GitHub MCP

↓

GitHub API

↓

Issue Created
```

##### 2. Resources

Resources are read-only context.

Examples:

* README
* Wiki pages
* Database schema
* Documentation
* Repository files

Copilot can add these as chat context through  **Add Context → MCP Resources** . ([GitHub Docs](https://docs.github.com/en/copilot/how-tos/provide-context/use-mcp-in-your-ide/extend-copilot-chat-with-mcp?tool=visualstudio&utm_source=chatgpt.com "Extending GitHub Copilot Chat with Model Context Protocol (MCP) servers - GitHub Docs"))

##### 3. Prompts

Servers can even provide predefined prompts.

Example:

```text
/ github.review-pr
```

or

```text
/ postgres.optimize-query
```

These are reusable workflows supplied by the MCP server itself. ([GitHub Docs](https://docs.github.com/en/copilot/how-tos/provide-context/use-mcp-in-your-ide/extend-copilot-chat-with-mcp?tool=visualstudio&utm_source=chatgpt.com "Extending GitHub Copilot Chat with Model Context Protocol (MCP) servers - GitHub Docs"))

### How GitHub Copilot uses MCP

Imagine you're in Agent Mode.

You ask:

> "Review my open pull requests and summarize the failures."

Without MCP:

Copilot cannot access GitHub directly.

With the GitHub MCP Server:

```text
You

↓

Copilot Agent

↓

GitHub MCP Server

↓

Repositories

↓

Pull Requests

↓

Actions

↓

Summary
```

### Local vs Remote MCP Servers

There are two kinds.

**-- Local MCP Server**

Runs on your computer.

Example:

```text
VS Code

↓

Memory MCP

↓

Local Database
```

Advantages:

* Fast
* Private
* Can access local files
* Good for development

**-- Remote MCP Server**

Runs on another machine.

Example:

```text
VS Code

↓

GitHub MCP

↓

https://api.githubcopilot.com/mcp/
```

Advantages:

* No installation
* Easy setup
* Always updated

GitHub recommends the hosted GitHub MCP server for most users. ([GitHub Docs](https://docs.github.com/en/copilot/how-tos/provide-context/use-mcp-in-your-ide/set-up-the-github-mcp-server?utm_source=chatgpt.com "Setting up the GitHub MCP Server - GitHub Docs"))

### Popular MCP Servers

Some commonly used servers include:

* GitHub
* Fetch
* Filesystem
* Memory
* PostgreSQL
* SQLite
* Git
* Slack
* Notion
* Google Drive
* Brave Search
* Context7 (current documentation)
* Jira
* Linear
* Docker

There are also hundreds of community-built servers. ([arXiv](https://arxiv.org/abs/2606.30317?utm_source=chatgpt.com "MCP Server Architecture Patterns for LLM-Integrated Applications"))

##### Example: PostgreSQL MCP

Instead of writing SQL manually:

You ask:

> Show today's new users.

Flow:

```text
AI

↓

Postgres MCP

↓

SELECT ...

↓

Database

↓

Results

↓

AI explains
```

##### Example: GitHub MCP

You ask:

> Create a bug report.

Flow:

```text
Agent

↓

GitHub MCP

↓

GitHub API

↓

Issue Created
```

No REST API code is needed.

##### Example: Slack MCP

You ask:

> Tell the frontend team deployment finished.

Flow:

```text
AI

↓

Slack MCP

↓

Slack API

↓

Message Sent
```

##### Example: Fetch MCP

You ask:

> Read the latest React documentation.

Flow:

```text
AI

↓

Fetch MCP

↓

Website

↓

Documentation

↓

Context

↓

Answer
```

### How to add MCP Servers in GitHub Copilot

There are  **two methods** .

##### Method 1 (Recommended)

Use the built-in MCP Registry.

Open the Extensions view, search with:

```text
@mcp
```

or use the MCP Registry UI, install a server, trust it, and start it. GitHub now provides a registry-backed experience for supported servers. ([GitHub Docs](https://docs.github.com/en/copilot/how-tos/provide-context/use-mcp-in-your-ide/set-up-the-github-mcp-server?utm_source=chatgpt.com "Setting up the GitHub MCP Server - GitHub Docs"))

##### Method 2

Manually configure `mcp.json`.

This gives you complete control.

### What is mcp.json?

`mcp.json` is the configuration file that tells Copilot:

* Which MCP servers exist
* How to start them
* Authentication
* Environment variables
* Startup commands
* Arguments

Think of it like:

```text
package.json

↓

for npm

mcp.json

↓

for AI tools
```

**-- Basic structure**

```json
{
  "servers": {

  }
}
```

Everything goes inside `"servers"`.

##### Local server example

```json
{
  "servers": {
    "memory": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-memory"
      ]
    }
  }
}
```

This tells Copilot:

* start with `npx`
* install if needed
* launch the Memory MCP server. ([GitHub Docs](https://docs.github.com/en/copilot/how-tos/provide-context/use-mcp-in-your-ide/extend-copilot-chat-with-mcp?tool=visualstudio&utm_source=chatgpt.com "Extending GitHub Copilot Chat with Model Context Protocol (MCP) servers - GitHub Docs"))

##### GitHub server example

```json
{
  "servers": {
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp/"
    }
  }
}
```

After authentication, Copilot can use GitHub tools. ([GitHub Docs](https://docs.github.com/en/copilot/how-tos/provide-context/use-mcp-in-your-ide/set-up-the-github-mcp-server?ref=blog.jillshaheen.com&utm_source=chatgpt.com "Setting up the GitHub MCP Server - GitHub Docs"))

##### Fetch server example

```json
{
  "servers": {
    "fetch": {
      "command": "uvx",
      "args": [
        "mcp-server-fetch"
      ]
    }
  }
}
```

This provides web-fetching capabilities. ([GitHub Docs](https://docs.github.com/en/enterprise-cloud%40latest/copilot/how-tos/provide-context/use-mcp-in-your-ide/extend-copilot-chat-with-mcp?tool=jetbrains&utm_source=chatgpt.com "Extending GitHub Copilot Chat with Model Context Protocol (MCP) servers - GitHub Enterprise Cloud Docs"))

##### What do the fields mean?

**-- command**

Program used to start the server.

Example:

```text
npx
```

or

```text
uvx
```

or

```text
python
```

**-- args**

Arguments passed to the command.

Example:

```text
[
 "-y",
 "@modelcontextprotocol/server-memory"
]
```

Equivalent to:

```bash
npx -y @modelcontextprotocol/server-memory
```

**-- url**

Used for remote servers.

Example:

```text
https://api.githubcopilot.com/mcp/
```

**-- type**

Defines how to connect.

Examples:

```text
http
```

or

```text
stdio
```

**-- env**

Environment variables.

Example:

```json
{
  "env": {
    "OPENAI_API_KEY": "...",
    "DATABASE_URL": "..."
  }
}
```

Useful for secrets.

**-- inputs**

Some servers ask the user for values when they start.

For example:

```json
"inputs": [
  {
    "type": "promptString"
  }
]
```

VS Code can prompt you and then supply the value to the server. ([GitHub Docs](https://docs.github.com/en/enterprise-cloud%40latest/copilot/how-tos/provide-context/use-mcp-in-your-ide/extend-copilot-chat-with-mcp?tool=jetbrains&utm_source=chatgpt.com "Extending GitHub Copilot Chat with Model Context Protocol (MCP) servers - GitHub Enterprise Cloud Docs"))

##### Where is mcp.json?

Typically there are two scopes:

**User scope**

Used by all projects.

**Workspace scope**

Usually:

```text
.vscode/mcp.json
```

Only that project uses those servers. GitHub also supports discovery of existing MCP configurations in some environments. ([GitHub Docs](https://docs.github.com/en/copilot/how-tos/provide-context/use-mcp-in-your-ide/extend-copilot-chat-with-mcp?tool=visualstudio&utm_source=chatgpt.com "Extending GitHub Copilot Chat with Model Context Protocol (MCP) servers - GitHub Docs"))

### How Copilot discovers tools

After the server starts:

```text
mcp.json

↓

Server Starts

↓

Server advertises tools

↓

Copilot discovers them

↓

Tools appear in Agent Mode
```

When you open Agent mode and click the **Tools** icon, you'll see the available MCP tools. ([GitHub Docs](https://docs.github.com/en/copilot/how-tos/provide-context/use-mcp-in-your-ide/extend-copilot-chat-with-mcp?tool=visualstudio&utm_source=chatgpt.com "Extending GitHub Copilot Chat with Model Context Protocol (MCP) servers - GitHub Docs"))

### Complete workflow

Imagine you install:

* GitHub MCP
* PostgreSQL MCP
* Fetch MCP

Now you ask:

> "Read today's GitHub issues, check the users database for affected customers, read the latest documentation, then create a fix plan."

Internally:

```text
Agent

↓

GitHub MCP
↓
Issues

↓

Postgres MCP
↓
Customers

↓

Fetch MCP
↓
Latest Docs

↓

Reasoning

↓

Plan
```

This is what makes modern AI agents feel much more capable than a traditional chatbot.

### MCP vs APIs

| Traditional API                         | MCP                                |
| --------------------------------------- | ---------------------------------- |
| Your code calls the API                 | AI calls an MCP tool               |
| You write authentication logic          | MCP server manages authentication  |
| You write request/response handling     | AI uses standardized tool calls    |
| Different integration for every service | Same protocol across many services |
| Built for software                      | Built for AI agents                |

For someone working with  **React, Next.js, Prisma, PostgreSQL, AWS, and GitHub** , the most valuable MCP servers are likely to be:

1. **GitHub MCP** – repositories, issues, pull requests, Actions.
2. **PostgreSQL MCP** – query and inspect databases from Agent mode.
3. **Fetch MCP** – retrieve the latest framework documentation.
4. **Filesystem MCP** – work with local project files.
5. **Context7 MCP** – provide up-to-date library documentation instead of relying only on the model's training data. Community feedback consistently highlights it as a useful addition for modern frameworks. ([reddit.com](https://www.reddit.com/r/GithubCopilot/comments/1nzkgws?utm_source=chatgpt.com "How to make Copilot fetch current docs instead of using old training data"))

---
