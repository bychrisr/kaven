# PDR to Tasks (Implementation Plan Generator)

> **Versão:** 2.0.0  
> **Data:** 2024-12-03  
> **Autor:** Chris + Claude Sonnet 4.5  
> **Status:** Production Ready  
> **Propósito:** Workflow Antigravity para gerar implementation_plan.json com tasks atômicas

---

## 📋 Metadata

```yaml
workflow_id: tasks
phase: pre-production
step: 1.5
input: PDR.md (Section 13)
output: implementation_plan.json + task_dependencies.md
estimated_time: 30-60 minutos
prerequisites: PDR + schema + contracts validados
```

---

# PDR to Tasks

You are generating implementation_plan.json with sequential implementation tasks.

## Prerequisites:

1. Verify that `PDR.md` exists.
2. Verify that `schema.prisma` exists.
3. Verify that `api_contracts.ts` (or router files) exist.
4. Read PDR Section 13: Incremental Roadmap.

## Steps:

**IMPORTANT:** Every feature in kickoff.json's core_v1 MUST have at least one task. Do not skip features.

1. Read the week-by-week roadmap from PDR Section 13.

2. Break each week into atomic tasks where:
   - 1 task = 1 day of work (4-8 hours)
   - Each task has clear inputs and outputs
   - Each task is testable (has acceptance criteria)

3. Generate `implementation_plan.json` with this structure:

```json
{
  "project": "project-name",
  "total_tasks": number,
  "estimated_hours": number,
  "tasks": [
    {
      "id": "task-001",
      "epic": "Setup|Backend|Frontend|Features|Deploy",
      "story": "One-line user story",
      "priority": "P0|P1|P2",
      "subtasks": [
        "Subtask 1 (specific action)",
        "Subtask 2 (specific action)"
      ],
      "dependencies": ["task-000"],
      "estimated_hours": 4-8,
      "acceptance_criteria": [
        "Criterion 1 (testable, binary yes/no)",
        "Criterion 2 (testable, binary yes/no)"
      ],
      "files_to_modify": [
        "path/to/file1.ts",
        "path/to/file2.tsx"
      ]
    }
  ]
}
```

## Task Sequence (Standard):

1. **task-001: Setup Environment**
   - Epic: Setup
   - Install dependencies (package.json)
   - Configure Prisma
   - Setup .env
   - Estimated: 4 hours

2. **task-002: Database Setup**
   - Epic: Backend
   - Run migrations (npx prisma migrate dev)
   - Generate Prisma client
   - Create seed script
   - Estimated: 3 hours

3. **task-003: tRPC Router Validation**
   - Epic: Backend
   - **Note:** Router was already generated in /contracts workflow
   - This task is for VALIDATION, not creation
   - Test CRUD operations manually (curl/Postman)
   - Verify Zod validation works
   - Estimated: 2-4 hours

4. **task-004 to task-00X: UI Components**
   - Epic: Frontend
   - 1 task per major component (List, Form, Item)
   - Use shadcn/ui
   - Estimated: 5-8 hours each

5. **task-00X+1: Integration**
   - Epic: Frontend
   - Connect frontend to tRPC
   - Add error handling
   - Add loading states
   - Estimated: 6 hours

6. **task-00X+2 to task-00X+N: Features**
   - Epic: Features
   - 1 task per core_v1 feature
   - Based on PDR Section 4
   - Estimated: varies

7. **task-00X+N+1: Deploy**
   - Epic: Deploy
   - Docker Compose setup
   - Environment variables
   - Staging deploy
   - Estimated: 4 hours

## Task Rules:

- Dependencies MUST be satisfied (no circular deps)
- Acceptance criteria MUST be binary (pass/fail, not subjective)
- estimated_hours sum MUST match PDR roadmap total (within 10%)
- P0 = must-have (core_v1), P1 = should-have, P2 = nice-to-have
- files_to_modify helps Agent know where to work
- **For delete operations:** Include "confirmation dialog" in acceptance criteria
- **For empty states:** Include "empty state shows message" in acceptance criteria
- **For loading states:** Include "loading skeleton appears" in acceptance criteria

## Example Task (TaskFlow):

```json
{
  "id": "task-003",
  "epic": "Backend",
  "story": "Implement tRPC router with full CRUD for tasks",
  "priority": "P0",
  "subtasks": [
    "Create task.ts router in src/server/trpc/routers/",
    "Implement create mutation with CreateTaskSchema",
    "Implement getAll query with filters (status, search)",
    "Implement getById query",
    "Implement update mutation with UpdateTaskSchema",
    "Implement delete mutation",
    "Test all endpoints with curl"
  ],
  "dependencies": ["task-002"],
  "estimated_hours": 6,
  "acceptance_criteria": [
    "All CRUD operations work via tRPC",
    "Zod validation prevents invalid inputs",
    "Filters (status, search) return correct results",
    "TypeScript compiles with zero errors"
  ],
  "files_to_modify": [
    "src/server/trpc/routers/task.ts",
    "src/server/trpc/router.ts"
  ]
}
```

**Note:** This JSON serves as a guide for implementation. Each task can be executed using:
- Antigravity Agent Manager: "Implement task-XXX from implementation_plan.json"
- Cursor Composer: Import JSON and use Composer agents
- Manual: Read task and implement yourself

## Validation:

After generating:

1. **Check JSON is valid** (parse test)
2. **Verify no circular dependencies:**
   - Create dependency graph
   - Detect cycles
   - If cycle found: ERROR and explain which tasks form the cycle
3. **Verify estimated_hours sum matches PDR roadmap** (within 10% tolerance)
4. **Verify all P0 tasks cover core_v1 features:**
   - Read kickoff.json's core_v1 array
   - For each feature, find at least one P0 task that implements it
   - If missing: add task for that feature
5. **Verify critical path is realistic:**
   - Identify longest dependency chain (Setup → ... → Deploy)
   - Ensure critical path ≤ 80% of total estimated_hours (allows parallelization)

## Output:

Save to `implementation_plan.json` in project root.

Also generate a visual dependency graph (Mermaid format) and save to `task_dependencies.md`.

Show me the task list (id + story) and ask for approval before generating full file.
