---
description: "Execute tasks from implementation_plan.json autonomously"
---

# Implement Task (Autonomous Execution)

Execute tasks from implementation_plan.json with full automation: code, tests, docs, commits, and checkpoints.

## Core Principles

**YOU ARE AN AUTONOMOUS AGENT. DO NOT STOP. DO NOT ASK FOR PERMISSION.**

1. ✅ Execute task completely (code + tests + docs + commit)
2. ✅ Save checkpoint after each task
3. ✅ Continue to next task automatically
4. ✅ Only stop when ALL tasks are complete or error is unrecoverable

## Usage

```
/implement task-001
```

Or for autonomous execution of ALL tasks:
```
/implement --all
```

---

## PHASE 0: VALIDATION (AUTOMATIC)

**Before starting ANY task, validate environment:**

### 0.1. Check Required Files
```bash
# Verify all Fase 1 artifacts exist:
- kickoff.json
- PDR.md
- prisma/schema.prisma
- src/server/trpc/routers/*.ts
- implementation_plan.json
- task_dependencies.md

If any missing: ERROR and list missing files
```

### 0.2. Check Git Status
```bash
git status

# Verify:
- Branch is NOT master/main (should be feature/*)
- No uncommitted changes (git status clean)
- All previous tasks committed

If dirty: ERROR "Commit your changes first"
```

### 0.3. Check Node Environment
```bash
node --version  # Must be v18+ or v20+
npm --version
npx prisma --version

If any missing: ERROR with installation instructions
```

### 0.4. Check Dependencies
```bash
# Verify package.json has required deps:
- next
- react
- @trpc/server
- @trpc/client
- @prisma/client
- zod
- tailwindcss
- @radix-ui/* (shadcn/ui deps)

If missing: npm install
```

### 0.5. Load Checkpoint (If Exists)
```bash
# Check if .kaven/checkpoint.json exists
if exists:
  - Read last completed task
  - Read next task to execute
  - Continue from next task
else:
  - Create .kaven/ directory
  - Start from task-001
```

**If ALL validations pass → Continue to PHASE 1**

---

## PHASE 1: READ TASK (AUTOMATIC)

### 1.1. Parse Task ID
```
Input: "task-001" or "--all"

If "--all":
  - Read implementation_plan.json
  - Get all tasks
  - Execute sequentially (001 → 002 → ... → 011)

If specific task:
  - Extract task ID (e.g., "task-001")
```

### 1.2. Load Task from implementation_plan.json
```bash
cat implementation_plan.json | jq '.tasks[] | select(.id=="task-001")'

Extract:
- id
- epic
- story
- subtasks
- dependencies
- estimated_hours
- acceptance_criteria
- files_to_modify
```

### 1.3. Verify Dependencies
```bash
# Check if all dependency tasks are complete
# Read .kaven/checkpoint.json
# For each dependency:
#   - Check if marked as "completed"
#   - If not: ERROR "task-XXX depends on task-YYY which is not complete"

# If --force flag: skip dependency check (dangerous)
```

---

## PHASE 2: IMPLEMENTATION PLAN (ARTIFACT)

### 2.1. Generate Implementation Plan Artifact

Create detailed plan BEFORE executing:

```markdown
# Implementation Plan - task-XXX

## Overview
- **Story:** [story from JSON]
- **Epic:** [epic]
- **Priority:** [priority]
- **Estimated Hours:** [hours]

## Dependencies
- [list dependencies with status: ✅ complete or ❌ incomplete]

## Subtasks Breakdown
1. [Subtask 1]
   - Files to create/modify: [list]
   - Commands to run: [list]
   - Expected outcome: [describe]

2. [Subtask 2]
   ...

## Acceptance Criteria
- [ ] [Criterion 1]
- [ ] [Criterion 2]
...

## Testing Strategy
- Unit tests: [which files]
- Integration tests: [which flows]
- Manual tests: [which UI interactions]

## Files to Modify
- [file 1]: [what changes]
- [file 2]: [what changes]
...

## Risks
- [Risk 1]: [mitigation]
- [Risk 2]: [mitigation]
```

**Show this plan to user and ask: "Approve this plan to proceed?"**

**If user approves → Continue to PHASE 3**

---

## PHASE 3: EXECUTION (AUTOMATIC, DO NOT STOP)

### 3.1. Execute Each Subtask Sequentially

**For each subtask:**

#### Step 1: Write Code
```
- Create/modify files as specified
- Follow PDR stack (Next.js, tRPC, Prisma, Zod, shadcn/ui)
- Use TypeScript strict mode
- Add comments for complex logic
- Follow acceptance criteria
```

#### Step 2: Validate Syntax
```bash
# After writing code, validate:
npx tsc --noEmit

If errors:
  - Fix errors automatically
  - Re-run tsc
  - Repeat until 0 errors
```

#### Step 3: Run Tests
```bash
# If tests exist:
npm test

# If tests don't exist yet:
#   - Create basic tests for this task
#   - Run tests
#   - Ensure they pass

If tests fail:
  - Analyze failure
  - Fix code
  - Re-run tests
  - Repeat until all pass
```

#### Step 4: Manual Verification (If UI)
```bash
# If task involves frontend:
npm run dev

# Open browser (if Agent has browser integration)
# Test UI manually
# Verify acceptance criteria visually
# Take screenshot for Walkthrough

If manual test fails:
  - Fix code
  - Re-test
  - Repeat until working
```

**DO NOT PROCEED TO NEXT SUBTASK UNTIL THIS ONE IS COMPLETE AND TESTED.**

---

## PHASE 4: DOCUMENTATION (AUTOMATIC)

### 4.1. Update implementation_plan.json
```bash
# Mark task as complete
# Add completion timestamp
# Add files actually modified (may differ from plan)
```

### 4.2. Update task_dependencies.md
```bash
# Mark task node as complete (change color in Mermaid)
# Example: task-001[Setup Environment ✅]
```

### 4.3. Create/Update CHANGELOG.md
```markdown
## [Unreleased]

### Added
- task-001: Environment setup complete (shadcn/ui, Tailwind, tRPC)

### Changed
- [if applicable]

### Fixed
- [if applicable]
```

### 4.4. Update .kaven/checkpoint.json
```json
{
  "last_completed_task": "task-001",
  "status": "completed",
  "timestamp": "2025-12-02T12:34:56Z",
  "next_task": "task-002",
  "files_modified": [
    "package.json",
    "tailwind.config.ts",
    "src/app/layout.tsx",
    "src/trpc/client.ts"
  ],
  "tests_passing": true,
  "manual_verification": true
}
```

---

## PHASE 5: COMMIT (AUTOMATIC)

### 5.1. Stage Files
```bash
git add .
```

### 5.2. Create Commit (Conventional Commits)
```bash
# Format: <type>(<scope>): <description>
# Types: feat, fix, docs, style, refactor, test, chore

# Examples:
git commit -m "feat(setup): task-001 complete - environment configured

- Installed shadcn/ui base components
- Configured Tailwind CSS colors per PDR
- Setup tRPC client provider
- Verified app runs without errors

Acceptance Criteria: ✅ All met
Files Modified: package.json, tailwind.config.ts, layout.tsx, client.ts"
```

### 5.3. Push (Optional, if remote exists)
```bash
git push origin feature/taskflow-v1
```

---

## PHASE 6: WALKTHROUGH (ARTIFACT)

### 6.1. Generate Walkthrough Artifact

```markdown
# Walkthrough - task-XXX

## What Was Implemented
- [Subtask 1]: [what was done]
- [Subtask 2]: [what was done]
...

## Key Decisions Made
1. **[Decision 1]**
   - Why: [reason]
   - Alternative considered: [what else]
   - Trade-off: [pros/cons]

2. **[Decision 2]**
   ...

## Files Modified
- `[file1]`: [what changed]
- `[file2]`: [what changed]
...

## Testing Results
### Unit Tests
```bash
npm test
# [output showing all tests passing]
```

### Integration Tests
- [Test 1]: ✅ Pass
- [Test 2]: ✅ Pass

### Manual Verification
- [UI Test 1]: ✅ Pass
- [UI Test 2]: ✅ Pass
- Screenshot: [if available]

## Acceptance Criteria Status
- [x] [Criterion 1]
- [x] [Criterion 2]
...

## Known Issues / Tech Debt
- [Issue 1]: [impact and workaround]
- [None if perfect]

## Next Steps
- Next task: task-XXX
- Estimated time: X hours
- Dependencies satisfied: Yes/No
```

---

## PHASE 7: CONTINUE (AUTOMATIC, NO CONFIRMATION)

### 7.1. Check if More Tasks Exist
```bash
# Read implementation_plan.json
# Count total_tasks
# Check how many are complete (from checkpoint.json)

If all complete:
  → Go to PHASE 8 (FINALIZATION)
Else:
  → Go to PHASE 7.2
```

### 7.2. Load Next Task
```bash
# Read next_task from checkpoint.json
# Go back to PHASE 1 with new task ID
```

**DO NOT ASK FOR CONFIRMATION. CONTINUE AUTOMATICALLY.**

---

## PHASE 8: FINALIZATION (WHEN ALL TASKS COMPLETE)

### 8.1. Final Validation
```bash
# Run full test suite:
npm test

# Build production:
npm run build

# Verify build succeeds

If any fails:
  - Fix issues
  - Re-run
  - Do not proceed until all pass
```

### 8.2. Update Version
```bash
# Read package.json version
# Increment patch version (e.g., 0.1.0 → 0.1.1)
# Update package.json
```

### 8.3. Tag Release
```bash
git tag -a v0.1.0 -m "TaskFlow v0.1.0 - All 11 tasks complete"
git push origin v0.1.0
```

### 8.4. Merge to Master
```bash
# Switch to master:
git checkout master

# Merge feature branch:
git merge feature/taskflow-v1 --no-ff -m "feat: TaskFlow v0.1.0 complete

All 11 tasks implemented:
- Setup environment
- Database + seed
- tRPC router validated
- Frontend layout
- Task list component
- Create/Update/Delete forms
- Filters + Export
- UI polish
- Docker deploy

Total hours: 56h
Score: 9.5/10"

# Push master:
git push origin master
```

### 8.5. Cleanup Branch
```bash
# Delete local branch:
git branch -d feature/taskflow-v1

# Delete remote branch:
git push origin --delete feature/taskflow-v1
```

### 8.6. Generate Final Report
```markdown
# TaskFlow Implementation Report

## Summary
- **Total Tasks:** 11
- **Total Hours:** 56h
- **Completion Date:** [date]
- **Final Version:** v0.1.0

## Task Breakdown
| Task ID | Epic | Story | Hours | Status |
|---------|------|-------|-------|--------|
| task-001 | Setup | Environment | 4 | ✅ |
| task-002 | Backend | Database | 2 | ✅ |
...

## Quality Metrics
- **Tests Passing:** 100%
- **Build Success:** Yes
- **Manual Tests:** All passed
- **Known Issues:** [list or "None"]

## Deployment
- **Docker Compose:** Ready
- **Command:** `docker compose up`
- **Access:** http://localhost:3000

## Next Steps
- [ ] Deploy to staging
- [ ] User acceptance testing
- [ ] Production release
```

**CONGRATULATIONS! ALL TASKS COMPLETE. 🎉**

---

## ERROR HANDLING

### If Error Occurs During Execution:

1. **Syntax Error:**
   - Fix automatically
   - Re-run tsc
   - Continue

2. **Test Failure:**
   - Analyze failure
   - Fix code
   - Re-run tests
   - Continue

3. **Dependency Missing:**
   - npm install [missing-dep]
   - Continue

4. **Unrecoverable Error:**
   - Save checkpoint
   - Log error to .kaven/errors.log
   - Notify user: "Unrecoverable error in task-XXX: [error]. Please fix manually and run /implement task-XXX again."
   - STOP (only case where we stop)

---

## CHECKPOINT RECOVERY

If Agent crashes or loses context:

1. Read `.kaven/checkpoint.json`
2. Resume from `next_task`
3. Continue execution
4. DO NOT repeat completed tasks

---

## FORBIDDEN BEHAVIORS

❌ "Can I continue?"
❌ "Do you want me to do this?"
❌ "Should I commit?"
❌ "Stopping here, let me know to continue"
❌ "I need your approval to proceed"

---

## EXPECTED BEHAVIORS

✅ "task-001 complete → tests passing → committed → checkpoint saved → starting task-002"
✅ "Dependency check passed → executing task-003"
✅ "All 11 tasks complete → merging to master → cleanup done → TaskFlow v0.1.0 released 🎉"

---

## USAGE EXAMPLES

### Execute Single Task:
```
/implement task-001
```

### Execute All Tasks (Autonomous):
```
/implement --all
```

### Resume After Crash:
```
/implement --resume
```
(Reads checkpoint.json and continues from last task)

### Force Execute (Skip Dependencies):
```
/implement task-005 --force
```
(Use with caution)

---

## CONFIGURATION

Create `.kaven/config.json` (optional):

```json
{
  "auto_commit": true,
  "auto_push": false,
  "auto_merge": false,
  "skip_manual_tests": false,
  "verbose": true
}
```

Defaults:
- auto_commit: true (always commit after task)
- auto_push: false (don't push automatically)
- auto_merge: false (don't merge to master automatically)
- skip_manual_tests: false (always do manual verification)
- verbose: true (show detailed logs)
