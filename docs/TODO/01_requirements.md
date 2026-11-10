# TODO Application V1 - Requirements Definition

## 1. Objective & Scope
- **Objective:** Build a CRUD-capable TODO application on top of an existing Express + TypeScript + EJS project connected to a Neon (PostgreSQL) database, and deploy it to Vercel.
- **Target Audience:** 4th-year students learning full-stack development, REST APIs, and deployment pipelines.
- **In-Scope (V1):**
  - Database connection using existing `DATABASE_URL`.
  - Dedicated route at `/todo` rendering `views/todo/index.ejs`.
  - REST API endpoints under `/api/v1/todos` (GET, POST, PATCH, DELETE).
  - Client-side interaction using `Fetch` via a `TodoApi` class.
  - Multi-environment verification (Local, Vercel Preview, Vercel Production).
- **Out-of-Scope (V1):**
  - User authentication / authorization.
  - Priority levels, filtering, and title updates (reserved for V2).
  - Personal identifiable information (PII) — only dummy data for educational purposes will be used.

---

## 2. Functional Requirements & Acceptance Criteria

### REQ-01: Get All TODOs (List)
- **Operation:** `GET /api/v1/todos`
- **Input:** None (Query parameters not required for V1).
- **Result:** 
  - Success (`200 OK`): Returns JSON containing an array of TODO items.
  - If 0 items exist, returns `200 OK` with `{ "data": [] }`.
- **Acceptance Criteria:** When the `/todo` page loads, it fetches all existing tasks. If the list is empty, the UI displays "No TODOs found."

### REQ-02: Create a TODO
- **Operation:** `POST /api/v1/todos`
- **Input:** JSON body `{ "title": "Task description" }`
- **Input Constraints:** `title` is required, must be trimmed of leading/trailing whitespace, and must be between 1 and 100 characters.
- **Result:**
  - Success (`201 Created`): Returns the newly created TODO object.
  - Error (`400 Bad Request` / `422 Unprocessable Entity`): If `title` is missing, empty, or exceeds 100 characters.
- **Acceptance Criteria:** Submitting the form adds the new task to the database and instantly updates the UI list without a full page reload.

### REQ-03: Update Completion Status (Partial Update)
- **Operation:** `PATCH /api/v1/todos/:id`
- **Input:** URL parameter `id` (integer >= 1), JSON body `{ "completed": true/false }`
- **Result:**
  - Success (`200 OK`): Returns the updated TODO item.
  - Error (`404 Not Found`): If the specified `id` does not exist.
  - Error (`400` / `422`): If `id` is invalid or `completed` is not a boolean.
- **Acceptance Criteria:** Toggling a checkbox on a task updates its completion status in the database and visually reflects the change.

### REQ-04: Delete a TODO
- **Operation:** `DELETE /api/v1/todos/:id`
- **Input:** URL parameter `id` (integer >= 1)
- **Result:**
  - Success (`204 No Content`): Successful deletion with no body returned (or appropriate success JSON response per common specs).
  - Error (`404 Not Found`): If the task does not exist.
- **Acceptance Criteria:** Clicking the delete button removes the task from the database and removes it from the UI list.

---

## 3. Common Data & Error Conditions
- **Database Schema:** 
  - `id` (SERIAL, Primary Key)
  - `title` (TEXT, 1–100 chars, trimmed)
  - `completed` (BOOLEAN, default `false`)
  - `created_at` (TIMESTAMPTZ, default `CURRENT_TIMESTAMP`)
- **Error HTTP Status Codes:**
  - `400 Bad Request`: Malformed JSON or invalid syntax.
  - `422 Unprocessable Entity`: Validation failure (e.g., title length out of bounds).
  - `404 Not Found`: Resource ID does not exist.
  - `500 Internal Server Error`: Unexpected server/database error.
  - `503 Service Unavailable`: Database connection failure.

---

## 4. Environment Verification Methods
- **Local:** Run `npm run dev` and verify endpoints at `http://localhost:3000/api/db-check` and `http://localhost:3000/todo`.
- **Vercel Preview / Production:** Push to GitHub, verify deployment via Vercel dashboard, and test `/api/db-check` and `/todo` against the same shared Neon PostgreSQL database instance.