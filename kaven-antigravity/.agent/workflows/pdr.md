# PDR Generator (Seed to Expanded)

> **Versão:** 2.0.0  
> **Data:** 2024-12-03  
> **Autor:** Chris + Claude Sonnet 4.5  
> **Status:** Production Ready  
> **Propósito:** Workflow Antigravity para gerar PDR.md completo (15 seções) a partir de PDR.seed.json

---

## 📋 Metadata

```yaml
workflow_id: pdr
phase: pre-production
step: 1.2
input: PDR.seed.json
output: PDR.md (15 seções, 3000-5000 palavras)
estimated_time: 30-60 minutos
prerequisites: PDR.seed.json aprovado
```

---

## 🎯 Objetivo

Gerar um Product Definition Report (PDR) completo com EXATAMENTE 15 seções obrigatórias, expandindo o PDR.seed.json em especificação técnica detalhada.

---

## ✅ Prerequisites

1. Verify that `PDR.seed.json` exists in `pre-production/pdr/`
2. Read the entire `PDR.seed.json` file
3. Verify `origin` field (must be "idea" - SOLUTIONS flow not implemented yet)

---

## 📝 Steps

**IMPORTANT:** Maintain consistency across all sections. If Section 1 defines a metric, Section 12 must use the SAME metric. If Section 6 mentions a field, Section 10's stack must support it.

### Generate PDR.md with EXACTLY 15 Sections:

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

---

## 📖 Section Guidelines

### Section 1: Executive Summary (300 words max)

- **What:** Product in 1 sentence
- **Why:** Pain being solved
- **Who:** Target user
- **How:** Core mechanism (from core_v1)
- **Success:** Metric-norte (1 primary metric)

---

### Section 2: Problem & Pain

- Expand the "pain" from PDR.seed.json
- Quantify if possible (e.g., "wastes 2h/day")
- Explain consequences (what happens if unsolved?)

---

### Section 3: Target User & Personas

- Expand "target_user" from PDR.seed.json
- Create 1-2 personas with:
  - Name, role, context
  - Frustrations
  - Goals
  - Tech comfort level

---

### Section 4: Core Features (v1)

- Copy "core_v1" from PDR.seed.json
- For EACH feature, detail:
  - User story (As a... I want... So that...)
  - Frontend requirements
  - Backend requirements
  - Data model implications

---

### Section 5: User Stories

- Generate 5-10 user stories covering core_v1
- Format: "As a [user], I want [action], so that [benefit]"
- Priority: P0 (must-have), P1 (should-have)

---

### Section 6: Information Architecture

- List main entities (e.g., User, Task, Project, **Tenant** se business)
- Define relationships (1:1, 1:N, N:M)
- Sketch basic schema (textual, not code yet)
- **For each entity field, specify:**
  - Required vs Optional
  - Max length (for strings)
  - Default value (if any)
  - Validation rules (e.g., "email format", "positive number")

**IMPORTANT:** If `objective: "business"`, ALWAYS include multi-tenant architecture:
- Add `Tenant` entity
- All main entities MUST have `tenant_id` foreign key
- Document RLS (Row Level Security) requirements

---

### Section 7: UI/UX Principles

- Design system: shadcn/ui + Tailwind CSS
- Color palette (suggest 3-5 colors based on project mood)
- Typography (suggest font pairings)
- Layout patterns (dashboard, forms, lists)

---

### Section 8: Data & Integrations

- Data sources (from "critical_integrations" in PDR.seed.json)
- APIs needed (if any)
- LGPD considerations (based on "data_sensitivity")

---

### Section 9: Non-Functional Requirements

- **Performance:** target load time, max users
- **Security:** auth method (JWT + Refresh Tokens), encryption
- **Observability:** Grafana + Prometheus (metrics), Sentry (errors), Winston (logs)
- **Reliability:** uptime target (e.g., 99%)

**If `objective: "business"`:**
- Add: Multi-tenant isolation requirements
- Add: Tenant-specific metrics collection
- Add: Per-tenant rate limiting

---

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
    database: "PostgreSQL (with RLS for business, without for personal)"
    queue: "BullMQ (optional for personal, required for business)"
    cache: "Redis (optional for personal, required for business)"
    auth: "JWT + Refresh Tokens"
    observability: "Grafana + Prometheus + Sentry"
  
  deploy:
    dev: "Docker Compose"
    staging: "Docker Compose"
    prod: "Docker Compose (self-hosted)"
```

**Adjust based on `objective` from PDR.seed.json:**
- If `personal`: PostgreSQL without RLS, BullMQ optional, Redis optional
- If `business`: PostgreSQL with RLS (multi-tenant), BullMQ required, Redis required

---

### Section 11: Top-3 Risks

Identify 3 technical or product risks:

For each risk:
- Description
- Probability (Low/Medium/High)
- Impact (Low/Medium/High)
- Mitigation strategy

---

### Section 12: Success Metrics

- **Metric-norte** (1 primary metric, e.g., "tasks completed/week")
- **Activation metric** (e.g., "user creates 5 tasks")
- **Retention metric** (e.g., "user returns 3/7 days")
- How to measure each metric

**MUST match Section 1 metric.**

---

### Section 13: Incremental Roadmap

Based on "estimated_weeks" from PDR.seed.json, break into week-by-week:

**Example for 3 weeks:**
- Week 1: Setup + Auth + Core entity CRUD
- Week 2: Feature 1 implementation
- Week 3: Feature 2 + Deploy staging

**MUST be incremental** (each week builds on previous).

---

### Section 14: Gate G1 Criteria

Checklist for Phase 1 completion:

- [ ] PDR has 15 sections
- [ ] Stack is defined in Section 10 (YAML completo)
- [ ] Roadmap is week-by-week in Section 13
- [ ] Multi-tenancy documented (if business)
- [ ] Observability strategy defined
- [ ] schema.prisma validates (checked in /backend workflow)
- [ ] API contracts compile (checked in /contracts workflow)
- [ ] implementation_plan.json has no circular dependencies (checked in /tasks workflow)

---

### Section 15: Appendices

- **Glossary** (if domain-specific terms)
- **References** (links to similar products, articles)
- **Open questions** (TBD items)

---

## ✅ Validation

After generating, verify:

1. **Exactly 15 sections** (count headers with `grep "^## " PDR.md`)
2. **Section 10 has stack YAML**
3. **Section 13 has week-by-week roadmap**
4. **Total length: 3000-5000 words** (substantial but not bloated)
5. **Consistency check:**
   - Section 1 metric matches Section 12 metric
   - Section 6 entities match Section 10 database choice
   - Section 4 features are covered in Section 13 roadmap
   - If business: multi-tenancy in Section 6, 9, 10

---

## 💾 Output

1. Save to `pre-production/pdr/PDR.md`

2. **Before saving, show me:**
   - All 15 section headers (as a checklist)
   - A brief summary of key decisions:
     - Stack choice (personal vs business)
     - Multi-tenancy (yes/no)
     - Observability (enabled by default)
     - Core v1 features prioritization
     - Any assumptions made
     - Open questions or risks identified

3. This transparency helps validate the PDR before committing

---

## 📂 File Locations

```
Input:  pre-production/pdr/PDR.seed.json
Output: pre-production/pdr/PDR.md
```

---

## 🔄 Next Workflow

After `PDR.md` is approved:

```bash
/backend  # Gera schema.prisma a partir de Section 6
```

---

## 📊 Success Metrics

- **Time:** 30-60 minutos
- **Completeness:** 15/15 seções presentes
- **Word count:** 3000-5000 palavras
- **Consistency:** 0 conflitos entre seções
- **Score target:** > 9.0/10 (validação manual)

---

## 🚨 Common Mistakes to Avoid

1. ❌ Inconsistência entre Section 1 e Section 12 (métricas diferentes)
2. ❌ Section 6 menciona campos que Section 10 não suporta
3. ❌ Roadmap (Section 13) não cobre todos os core_v1 features
4. ❌ Esquecer multi-tenancy se `objective: "business"`
5. ❌ Stack YAML incompleto ou fora do padrão

---

## Changelog

### v2.0.0 (2024-12-03) - Versionamento e Multi-Tenancy
- Adicionado cabeçalho de versionamento
- Input renomeado: `kickoff.json` → `PDR.seed.json`
- Adicionada lógica de multi-tenancy (Section 6, 9, 10)
- Observabilidade (Grafana + Prometheus) obrigatória
- Database sempre PostgreSQL (com ou sem RLS)
- Adicionadas métricas de sucesso
- Documentados caminhos de salvamento
- Adicionado next workflow (`/backend`)
- Gate G1 expandido (incluindo multi-tenancy check)

### v1.0.0 (2024-12-02) - Versão Original
- Workflow funcional validado com TaskFlow
- Score: 9.6/10
- 15 seções obrigatórias
