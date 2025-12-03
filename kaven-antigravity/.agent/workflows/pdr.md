---
description: "Kaven Phase 1.2 - Generate complete PDR from kickoff.json"
---

# JSON to PDR

You are generating a complete Product Definition Report (PDR) with EXACTLY 15 sections.

## Prerequisites:

1. Verify that `kickoff.json` exists in the project root.
2. Read the entire `kickoff.json` file.

## Steps:

**IMPORTANT:** Maintain consistency across all sections. If Section 1 defines a metric, Section 12 must use the SAME metric. If Section 6 mentions a field, Section 10's stack must support it.

1. Generate a PDR.md file with EXACTLY these 15 sections (no more, no less):

   1. Executive Summary
   2. Problem & Pain
   3. Target User & Personas
   4. Core Features (v1)
   5. User Stories
   6. Information Architecture
   7. UI/UX Principles
   8. Data & Integrations
   9. Non-Functional Requirements
   10. Architectural Decisions
   11. Top-3 Risks
   12. Success Metrics
   13. Incremental Roadmap
   14. Gate G1 Criteria
   15. Appendices

## Section Guidelines:

### Section 1: Executive Summary (300 words max)
- What: Product in 1 sentence
- Why: Pain being solved
- Who: Target user
- How: Core mechanism (from core_v1)
- Success: Metric-norte (1 primary metric)

### Section 2: Problem & Pain
- Expand the "pain" from kickoff.json
- Quantify if possible (e.g., "wastes 2h/day")
- Explain consequences (what happens if unsolved?)

### Section 3: Target User & Personas
- Expand "target_user" from kickoff.json
- Create 1-2 personas with:
  - Name, role, context
  - Frustrations
  - Goals
  - Tech comfort level

### Section 4: Core Features (v1)
- Copy "core_v1" from kickoff.json
- For EACH feature, detail:
  - User story (As a... I want... So that...)
  - Frontend requirements
  - Backend requirements
  - Data model implications

### Section 5: User Stories
- Generate 5-10 user stories covering core_v1
- Format: "As a [user], I want [action], so that [benefit]"
- Priority: P0 (must-have), P1 (should-have)

### Section 6: Information Architecture
- List main entities (e.g., User, Task, Project)
- Define relationships (1:1, 1:N, N:M)
- Sketch basic schema (textual, not code yet)
- **For each entity field, specify:**
  - Required vs Optional
  - Max length (for strings)
  - Default value (if any)
  - Validation rules (e.g., "email format", "positive number")

### Section 7: UI/UX Principles
- Design system: shadcn/ui + Tailwind CSS
- Color palette (suggest 3-5 colors based on project mood)
- Typography (suggest font pairings)
- Layout patterns (dashboard, forms, lists)

### Section 8: Data & Integrations
- Data sources (from "critical_integrations" in kickoff.json)
- APIs needed (if any)
- LGPD considerations (based on "data_sensitivity")

### Section 9: Non-Functional Requirements
- Performance: target load time, max users
- Security: auth method (JWT + Refresh Tokens), encryption
- Observability: Sentry (errors), Winston (logs)
- Reliability: uptime target (e.g., 99%)

### Section 10: Architectural Decisions
MUST include this EXACT stack definition:

```yaml
stack:
  frontend:
    framework: "Next.js 14+ (App Router)"
    library: "React 18"
    styling: "Tailwind CSS + shadcn/ui"
    api: "tRPC + Zod"
  
  backend:
    runtime: "Node.js"
    framework: "Fastify"
    orm: "Prisma"
    database: "SQLite (personal) or PostgreSQL (business)"
    queue: "BullMQ (optional for personal, required for business)"
    cache: "Redis (optional for personal, required for business)"
    auth: "JWT + Refresh Tokens"
  
  deploy:
    dev: "Docker Compose"
    prod: "Docker Compose (self-hosted)"
```

Adjust database based on "mode" from kickoff.json.

### Section 11: Top-3 Risks
- Identify 3 technical or product risks
- For each risk:
  - Description
  - Probability (Low/Medium/High)
  - Impact (Low/Medium/High)
  - Mitigation strategy

### Section 12: Success Metrics
- Metric-norte (1 primary metric, e.g., "tasks completed/week")
- Activation metric (e.g., "user creates 5 tasks")
- Retention metric (e.g., "user returns 3/7 days")
- How to measure each metric

### Section 13: Incremental Roadmap
Based on "estimated_weeks" from kickoff.json, break into week-by-week:

Example for 3 weeks:
- Week 1: Setup + Auth + Core entity CRUD
- Week 2: Feature 1 implementation
- Week 3: Feature 2 + Deploy staging

MUST be incremental (each week builds on previous).

### Section 14: Gate G1 Criteria
Checklist for Phase 1 completion:
- [ ] PDR has 15 sections
- [ ] Stack is defined in Section 10
- [ ] Roadmap is week-by-week in Section 13
- [ ] schema.prisma validates (npx prisma validate)
- [ ] api_contracts.ts compiles (npx tsc --noEmit)
- [ ] implementation_plan.json has no circular dependencies

### Section 15: Appendices
- Glossary (if domain-specific terms)
- References (links to similar products, articles)
- Open questions (TBD items)

## Validation:

After generating, verify:
- Exactly 15 sections (count headers with `grep "^## " PDR.md`)
- Section 10 has stack YAML
- Section 13 has week-by-week roadmap
- Total length: 3000-5000 words (substantial but not bloated)
- **Consistency check:**
  - Section 1 metric matches Section 12 metric
  - Section 6 entities match Section 10 database choice
  - Section 4 features are covered in Section 13 roadmap

## Output:

Save to `PDR.md` in project root.

**Before saving, show me:**
1. All 15 section headers (as a checklist)
2. A brief summary of key decisions:
   - Stack choice (SQLite vs PostgreSQL)
   - Core v1 features prioritization
   - Any assumptions made
   - Open questions or risks identified

This transparency helps validate the PDR before committing.
