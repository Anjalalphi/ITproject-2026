# TODO Application V1 - Implementation Plan

This document outlines the step-by-step implementation procedure for the V1 TODO application, starting from database setup to deployment and verification across local, preview, and production environments.

---

## Step 1: Database Setup (SQL)
- **Target File:** `sql/todo/020_create_todo_tasks.sql`
- **Task:** Write and save the SQL script to create the `todo_tasks` table. Run it via the Neon SQL Editor.
- **Verification:** Check Neon Console to ensure the `todo_tasks` table exists with columns: `id`, `title`, `completed`, `created_at`.
- **Troubleshooting:** If table creation fails, verify your database connection string in `.env`.

---

## Step 2: Model Implementation (`todoModel.ts`)
- **Target File:** `src/apps/todo/todoModel.ts`
- **Task:** Implement database query functions using `pool` from `db.ts`:
  - `getAllTodos()`: `SELECT * FROM todo_tasks ORDER BY id DESC;`
  - `createTodo(title)`: `INSERT INTO todo_tasks (title) VALUES ($1) RETURNING *;`
  - `updateTodoStatus(id, completed)`: `UPDATE todo_tasks SET completed = $2 WHERE id = $1 RETURNING *;`
  - `deleteTodo(id)`: `DELETE FROM todo_tasks WHERE id = $1 RETURNING *;`
- **Expected Result:** Database queries successfully execute and return typed results.

---

## Step 3: Controller Implementation (`todoController.ts`)
- **Target File:** `src/apps/todo/todoController.ts`
- **Task:** Implement HTTP request handlers with validation (e.g., checking `title` length 1–100 chars, handling 400, 422, 404, 500 errors).
- **Expected Result:** Proper JSON responses and status codes are returned for success and failure cases.

---

## Step 4: Router Implementation (`todoRouter.ts`)
- **Target File:** `src/apps/todo/todoRouter.ts`
- **Task:** Define Express Router mapping REST endpoints (`GET`, `POST`, `PATCH`, `DELETE`) to controller methods under `/api/v1/todos`.
- **Expected Result:** Endpoints correctly route traffic to the controller functions.

---

## Step 5: Main App Integration (`index.ts`)
- **Target File:** `index.ts`
- **Task:** Import `todoRouter`, mount it to `/api/v1/todos`, and add a route for `/todo` to render `views/todo/index.ejs`.
- **Expected Result:** Existing routes (like `/` and `/api/db-check`) remain intact while new todo routes become active.

---

## Step 6: Frontend API Client (`todo.js`)
- **Target File:** `public/todo/todo.js`
- **Task:** Implement the `TodoApi` class using `fetch` to interact with `/api/v1/todos`.
- **Expected Result:** Clean asynchronous methods (`getAll`, `create`, `update`, `delete`) communicating with the backend.

---

## Step 7: Frontend View (`index.ejs`)
- **Target File:** `views/todo/index.ejs`
- **Task:** Create the EJS template featuring a task input form, a task list, and interactive toggles/delete buttons linked to `TodoApi`.
- **Expected Result:** Users can view, add, complete, and delete tasks dynamically without full page reloads.

---

## Step 8: Local Testing & Verification
- **Commands:** 
  - `npm run dev`
- **Verification URL:** `http://localhost:3000/todo` and `http://localhost:3000/api/db-check`
- **Expected Result:** Local server runs without errors, CRUD operations work smoothly against the Neon DB.

---

## Step 9: Git Commit, Pull Request & Deployment
- **Commands:**
  - `git checkout -b feature/todo-app`
  - `git add .`
  - `git commit -m "feat: implement V1 todo application"`
  - `git push origin feature/todo-app`
- **Task:** Create a Pull Request on GitHub and merge to main. Verify Vercel preview and production deployments.
- **Expected Result:** Production URL correctly loads the TODO app connected to Neon DB.