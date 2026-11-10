# TODO Application V1 - Design Document

## 1. File Structure & Responsibilities
The TODO feature follows a modular architecture within the existing Express + TypeScript project:

```text
├── src/apps/todo/
│   ├── todoRouter.ts        # Defines API routes (/api/v1/todos) and maps them to controller methods
│   ├── todoController.ts    # Handles HTTP requests/responses, input validation, and error handling
│   └── todoModel.ts         # Encapsulates SQL queries interacting with the Neon PostgreSQL database
├── views/todo/
│   └── index.ejs            # Frontend HTML view rendered at /todo
├── public/todo/
│   └── todo.js              # Client-side JavaScript containing the TodoApi class for Fetch requests
└── sql/
    └── todo/
        └── 020_create_todo_tasks.sql # SQL script to create the tasks table