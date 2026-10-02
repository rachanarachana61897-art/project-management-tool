# TaskFlow — Project Management Tool
**CodeAlpha Internship — Task 3: Project Management Tool**

A full-stack collaborative project management tool, similar to Trello/Asana, built with Node.js, Express, vanilla HTML/CSS/JavaScript, and Socket.io for real-time updates.

## ✅ Features (matches task requirements)

- **Auth system** — user registration & login with JWT + hashed passwords (bcrypt)
- **Create group projects** — any user can create a project and becomes its owner
- **Assign tasks** — task cards with title, description, status, and an assignee
- **Comment & communicate within tasks** — threaded comments on every task card
- **Project boards** — Kanban-style board with To Do / In Progress / Done columns
- **Backend to manage** users, projects, tasks, and comments (JSON file-backed data store, no external DB setup needed)
- **Bonus: Notifications & real-time updates** — Socket.io pushes live task, comment, and project updates to every connected member, plus an in-app notification bell (e.g. "you were assigned a task", "someone commented")

## 🗂 Project Structure

```
project-management-tool/
├── server/
│   ├── server.js            # Express + Socket.io entry point
│   ├── data/db.js           # JSON file-based data layer
│   ├── middleware/auth.js   # JWT auth middleware
│   ├── routes/
│   │   ├── auth.js          # register / login / me / user search
│   │   ├── projects.js      # create / list / members / delete
│   │   ├── tasks.js         # create / update / assign / delete
│   │   └── notifications.js # list / mark read
│   └── utils/notify.js      # notification + socket emit helper
├── public/
│   ├── index.html           # login page
│   ├── register.html        # sign up page
│   ├── dashboard.html       # project list
│   ├── project.html         # kanban board + task detail + comments
│   ├── css/style.css
│   └── js/
│       ├── api.js           # fetch wrapper + auth helpers
│       ├── dashboard.js
│       └── project.js
├── package.json
└── .env.example
```

## 🚀 Getting Started

**Requirements:** Node.js 16+ and npm.

1. Unzip the project and open a terminal in the `project-management-tool` folder.
2. Install dependencies:
   ```bash
   npm install
   ```
3. Create your environment file:
   ```bash
   cp .env.example .env
   ```
   (On Windows: `copy .env.example .env`) — optionally edit `JWT_SECRET` to any random string.
4. Start the server:
   ```bash
   npm start
   ```
5. Open your browser at **http://localhost:5000**

Data is stored in `server/data/db.json`, created automatically on first run — no external database installation required.

## 🧪 Trying it out (multi-user demo)

1. Register two accounts (e.g. in two browser tabs/windows, or one normal + one incognito).
2. With account A, create a project, then open **Members** and add account B by username/email.
3. Create a task and assign it to account B — account B will see a live notification appear instantly.
4. Open the task and post a comment as account A — account B sees it appear in real time if the board is open, and gets a notification either way.
5. Drag-free workflow: change a task's status from the dropdown inside the task card to move it between To Do / In Progress / Done.

## 🔌 API Overview

| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/auth/register` | Create an account |
| POST | `/api/auth/login` | Log in, returns JWT |
| GET | `/api/auth/me` | Current user info |
| GET | `/api/auth/users?search=` | Search users to add as members |
| GET | `/api/projects` | List your projects |
| POST | `/api/projects` | Create a project |
| GET | `/api/projects/:id` | Project details |
| POST | `/api/projects/:id/members` | Add a member (owner only) |
| DELETE | `/api/projects/:id` | Delete project (owner only) |
| GET | `/api/projects/:id/tasks` | List tasks on a project's board |
| POST | `/api/projects/:id/tasks` | Create & assign a task |
| PUT | `/api/tasks/:id` | Update title/description/status/assignee |
| DELETE | `/api/tasks/:id` | Delete a task |
| GET | `/api/tasks/:id/comments` | List a task's comments |
| POST | `/api/tasks/:id/comments` | Add a comment |
| GET | `/api/notifications` | List your notifications |
| PUT | `/api/notifications/:id/read` | Mark one as read |
| PUT | `/api/notifications/read-all` | Mark all as read |

Real-time Socket.io events: `task:created`, `task:updated`, `task:deleted`, `comment:created`, `project:updated`, `project:deleted`, `notification:new`.

## 🛠 Tech Stack

- **Frontend:** HTML, CSS, vanilla JavaScript
- **Backend:** Node.js, Express.js
- **Real-time:** Socket.io
- **Auth:** JSON Web Tokens (jsonwebtoken) + bcryptjs
- **Storage:** JSON file-based store (easy to swap for MongoDB/PostgreSQL later)

## 🔒 Notes on Production Use

This project is built for learning/demo purposes as part of the internship task. Before deploying publicly, you'd want to: move to a real database, add rate limiting, validate/sanitize all inputs more strictly, and set a strong `JWT_SECRET` via environment variables (never commit `.env`).
