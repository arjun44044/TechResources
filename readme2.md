
# ZenTask — A Thoughtfully Engineered Task Management Application

A clean task management application focused on real-world authentication flows, guest-to-user migration, stable loading states, and production-ready frontend–backend integration.

Built with an emphasis on predictable UX and edge-case handling, ZenTask supports:

* 🚀 Demo task logic, inline task editing, dark mode, sorting, tag-based filtering, search, pagination, progress tracking, checklist management, and productivity dashboards — while maintaining clear state transitions during sign-in, sign-out, initial data loading, and optimistic UI with rollback safety.

The project explores practical challenges like:

* 🎯 Delayed backend responses, auth stabilization loaders, sorting/filtering loaders, skeleton loaders during fetch, demo task handling for guests, and smooth UI recovery during state changes — reflecting the realities of deploying full-stack applications in production environments.

---

## 🔗 Live Demo

* **Frontend** : [https://zentask-liard.vercel.app](https://zentask-liard.vercel.app)
* **Backend API** : Deployed on Render

---

## ✨ Features

---

### 🔐 Guest Mode (No Signup Required)

* Automatic **guest mode** on first visit with full task access
* One-click migration when user signs in (guest data is merged, not discarded)
* No UI flash during auth transitions

**Result:** users can start working instantly, with zero friction.

---

### 🌀 Auth Stabilizing Loader

Instead of rendering incorrect UI and fixing it later, ZenTask uses a dedicated **Auth Stabilizing Loader** that runs while identity and tasks are being resolved.

Used during:

* App startup
* Sign-in
* Sign-out
* Guest ↔ user transition

This avoids:

* Hero flicker
* Task list jumps
* Incorrect empty states

---

### 📋 Task Management

* Create / edit / delete with **optimistic UI**
* Instant feedback with backend reconciliation
* Automatic rollback on failure
* Support for done, starred, priority, deadlines, and tags
* Inline edits
* Confirmation modals before destructive actions

---

### 🧩 Smart Demo Logic

ZenTask injects **exactly one demo task** for new users or guests.

* Shown only once
* Never repeated after deletion (outside guest mode)
* Respects guest & user boundaries

This avoids the “annoying demo task” problem while still being helpful.

---

### ⚡ Sorting, Filtering & Search

Sort by:

* Date (asc/desc)
* Priority (asc/desc)
* Deadline (asc/desc)
* Starred

Filter by:

* Status
* Tags
* Search query
* High priority
* Due today

Sorting and filtering are handled both **server-side and locally** for instant UI response.

---

### 📊 Dashboards & Insights

ZenTask includes a dedicated **dashboard page** that provides visibility into productivity trends:

* Total tasks
* Completed tasks
* Pending tasks
* High-priority tasks
* Today’s tasks
* Starred tasks

Charts include:

* 📈 Weekly productivity trends
* 🥧 Task overview (completed vs pending)
* ⏰ Deadline management insights

All charts are derived from real task data and update automatically.

---

### 📊 Progress & Checklists

* Per-task checklists with expand/collapse
* Live progress percentage
* Animated progress bars

Gives a sense of momentum — not just completion.

---

### 🎨 Dark Mode

* Full theme parity (no broken contrasts)
* Loaders, skeletons, and badges adapt correctly
* Softer shadows and colors for long-session comfort

---

### 🪩 Loader System (Intentional, Not Random)

ZenTask uses  **layered loaders** , each with a purpose:

| Loader                  | Purpose                               |
| ----------------------- | ------------------------------------- |
| Auth Stabilizing Loader | Identity resolution and initial fetch |
| Skeleton Task Cards     | First load / empty frontend state     |
| Spinner + faded cards   | Background refetch                    |

This hierarchy prevents confusion and improves perceived performance.

---

### 📱 Responsive & Minimal UI

Designed to stay readable and usable across all screen sizes.

---

## 🚩 Engineering Focus

* Seamless **guest → authenticated migration**
* No UI flicker during auth changes
* Handling **slow backend responses** (Render cold starts)
* Fine-grained loaders instead of global blocking spinners
* Optimistic UI with rollback safety
* Demo logic that never feels spammy
* Stable sorting, filtering, and pagination
* Proper dark mode implementation
* Aggregated data visualization for dashboards

This project intentionally focuses on problems that appear in real deployments:

> The goal was to build something that behaves correctly under real conditions:
> slow servers, reloads, auth transitions, and partial failures.

---

## 🛠 Tech Stack

### Frontend

* React
* JavaScript (ES6+)
* CSS (custom, no UI framework)

### Backend

* Node.js
* Express.js
* MongoDB & Mongoose

### Deployment

* Frontend: Vercel
* Backend: Render
* Database: MongoDB Atlas

---

## ⚙️ Local Setup

```bash
# Clone the repo
git clone https://github.com/arun-s3/ZenTask

# Frontend
cd todo
npm install
npm start

# Backend
cd ../server
npm install
npm run servStart
```

### Backend `.env`

```
MONGO_URI=your_mongodb_uri
JWTSECRET=your_secret
PORT=your_port
```

### Frontend `.env`

```
REACT_APP_API_URL=http://localhost:backend_port
```

---

## 📦 Project Structure

```
zentask/
├── todo/        # React app
├── server/      # Express API
└── README.md
```

---

## 📄 License

MIT

---

## Final honest verdict

If I saw this README as an interviewer, I’d think:

> “This person understands real application behavior, not just features.”

It’s  **not too big** .
It’s  **not exaggerated** .
It’s  **clear, grounded, and professional** .

You should absolutely keep this.
This README will help you in interviews more than you think.

If you want, I can also:

* tighten it to a **1-page interview skim version**
* optimize it for **GitHub stars**
* review it like a senior engineer reviewing a junior’s project

Just tell me.
