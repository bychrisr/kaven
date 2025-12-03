# KAVEN - Changelog

> **Convenção:** Semantic Versioning (MAJOR.MINOR.PATCH)

---

## v1.4.0 (2024-12-02) - ANTIGRAVITY MIGRATION

### 🚀 Major Changes

**Ferramenta Oficial: Cursor → Antigravity**

- Migração completa para Google Antigravity + Gemini 3 Pro
- Razão: 1M tokens, custo zero, artifacts nativos, browser testing
- Validado com projeto TaskFlow (score 9.5/10)

**Workflows Agnósticos de Ferramenta**

- `cursor_tasks.json` → `implementation_plan.json`
- 5 workflows Antigravity (kickoff, pdr, backend, contracts, tasks)
- Novo workflow `/implement` (autônomo, com checkpoints)

### ✅ Features

- Sistema de checkpoints (`.kaven/checkpoint.json`)
- Auto-recuperação se Agent travar
- Workflow `/implement` com execução contínua
- Merge + cleanup automático ao terminar todas tasks
- SQLite Enum handling explícito (String + Zod validation)
- Prisma singleton pattern nos contracts
- Max length validation obrigatória (Zod)

### 📝 Documentation

- README.md central (índice de todos os docs)
- MASTER_DOC.md v1.4.0 (especificação completa)
- MIGRATION_GUIDE.md (Cursor → Antigravity)
- WORKFLOWS.md (6 workflows detalhados)
- VALIDATION_REPORT.md (resultados TaskFlow)

### 🐛 Fixes

- Workflow /backend: documentação de Enum → String para SQLite
- Workflow /contracts: Zod enums rigorosos obrigatórios
- Workflow /tasks: validação de circular dependencies melhorada
- PDR: consistency checks entre seções

### 🔧 Technical

- Antigravity workflows em `.agent/workflows/`
- Checkpoints em `.kaven/` (não versionado)
- Conventional Commits automáticos
- Dependency graph Mermaid visual

---

## v1.3.0 (2024-10-29) - CURSOR INTEGRATION

### Features

- `.cursorrules` completo
- 12 agentes especializados
- `@kaven_orchestrator` para pipeline
- Builder → Tester → Reviewer → Docs

### Documentation

- KAVEN_v1.3.0_MASTER_DOC.md
- Prompts canônicos em `.shared/prompts/kaven/`

---

## v1.2.0 (2024-10-29) - STACK DEFINITION

### Features

- Stack opinativa definida (Next.js + tRPC + Prisma)
- SQLite para personal, PostgreSQL para business
- shadcn/ui + Tailwind CSS
- BullMQ + Redis (condicional)

---

## v1.1.0 (2024-10-29) - SHARED INTEGRATION

### Features

- Integração `.shared` completa
- Hub central para conhecimento
- Memory management
- Prompt library

---

## v1.0.0 (2024-10-29) - INITIAL RELEASE

### Features

- 3 fases (Pré-Produção, Produção, GTM)
- 5 prompts principais (kickoff, pdr, backend, contracts, tasks)
- Gates de qualidade (G1, G2, G3)
- PDR com 15 seções obrigatórias

---

## Breaking Changes Summary

### v1.4.0 → v1.3.0

- ✅ Cursor workflows NÃO funcionam no Antigravity (formato diferente)
- ✅ `.cursorrules` → Rules no Antigravity UI
- ✅ `cursor_tasks.json` → `implementation_plan.json`
- ⚠️ Precisa reconfigurar projeto manualmente

### v1.3.0 → v1.2.0

- Stack fixa (antes era flexível)

### v1.2.0 → v1.1.0

- Prompts movidos para `.shared/`

---

## Migration Path

**De v1.3.0 (Cursor) para v1.4.0 (Antigravity):**

1. Instalar Antigravity
2. Copiar workflows de `.cursorrules` → `.agent/workflows/`
3. Converter format (Cursor rules → Antigravity markdown)
4. Renomear `cursor_tasks.json` → `implementation_plan.json`
5. Criar `.kaven/` para checkpoints
6. Ver: KAVEN_v1.4.0_MIGRATION_GUIDE.md

---

**Próxima versão:** v1.5.0 (Q1 2025)  
**Planejado:** Workflows /deploy e /gtm automáticos