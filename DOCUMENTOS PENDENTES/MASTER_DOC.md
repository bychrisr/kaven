> **Versão:** 1.3.0  
> **Data:** 2025-12-01  
> **Autor:** Chris  
> **Status:** Production Ready (Antigravity Edition)  
> **Propósito:** Documento mestre centralizador — especificação completa do Kaven

---

## 📋 Controle de Versão

| Versão | Data | Mudanças | Autor |
|--------|------|----------|-------|
| 1.0.0 | 2025-10-29 | Documento inicial — estrutura completa | Chris + Claude |
| 1.1.0 | 2025-10-29 | Integração `.shared` + 12 agentes | Chris + Claude |
| 1.2.0 | 2025-12-01 | Stack estabilizada + validação incremental | Chris + Claude |
| 1.3.0 | 2025-12-01 | **🚀 MIGRAÇÃO PARA ANTIGRAVITY:**<br>✅ Workflows substituem .cursorrules<br>✅ 1M tokens de contexto<br>✅ Artifacts nativos<br>✅ Browser testing automático<br>✅ Custo zero (incluído no Google One AI Premium) | Chris + Claude |

---

## 🎯 TL;DR (Scan Rápido — 2 minutos)

**O que é o Kaven:**
- Sistema que transforma **ideias caóticas** em **produtos SaaS funcionais**
- Projetado para **neurodivergência** (TDAH/TEA/AH) — externaliza memória e disciplina
- **3 fases:** Pré-Produção (PDR) → Produção (Antigravity) → Go-to-Market (condicional)

**Por que existe:**
- Projetos complexos geram **refatorações infinitas** sem estrutura prévia
- **IDEs tradicionais** não mantêm coerência arquitetural entre sessões
- Cérebro neurodivergente vê sistemas completos mas trava na execução linear

**Como funciona:**
1. **Fase 1:** Ideia → PDR ultra-detalhado (decisões arquiteturais fixas)
2. **Fase 2:** PDR → cursor_tasks.json → Antigravity gera código sequencialmente
3. **Fase 3:** Produto validado → ativa GTM (marketing, pricing, legal)

**Validação:** Projeto trivial (TaskFlow - 3 features) para testar sistema antes de projetos complexos

**Novidade v1.3:** **Migração completa para Google Antigravity** + workflows automáticos + artifacts nativos + 1M tokens

---

# PARTE I: FUNDAMENTOS

## 1. Visão & Propósito

### 1.1 Manifesto

> "O futuro não será construído por corporações, mas por indivíduos com poder de criar seus próprios sistemas. Kaven existe para libertar o criador. De uma faísca de ideia até um negócio completo — tudo se conecta, tudo se estrutura, tudo flui."

**Kaven não é:**
- ❌ Acelerador de MVPs simples (use Bolt.new/v0.dev para isso)
- ❌ Framework de low-code/no-code (não elimina código)
- ❌ Substituto para desenvolvedores (você ainda programa)

**Kaven é:**
- ✅ **Compilador de intenção** (ideia caótica → especificação formal → código)
- ✅ **Exoesqueleto cognitivo** (sistema externo que mantém coerência quando seu cérebro não consegue)
- ✅ **Flight computer** (permite pilotar "caça supersônico" sem explodir)

### 1.2 Problema Central

**Para cérebros neurodivergentes (especialmente TDAH + TEA + AH):**

1. **Paralisia de Decisão**
   - "Qual stack usar?" → 5 horas pesquisando, 0 linhas escritas
   - "Auth com OAuth ou magic link?" → paralisia por opções
   - Kaven **decide por você** (stack opinativa, sem escolhas desnecessárias)

2. **Perda de Contexto**
   - Começa 5 projetos, abandona 4 pela metade
   - Retoma projeto após 3 meses → "o que eu estava fazendo?"
   - Kaven **salva estado** (você pode pausar/retomar sem arqueologia)

3. **Refatorações Infinitas**
   - Começa com schema simples → descobre que precisa de tenancy → refatora tudo
   - "Na verdade, preciso de observabilidade" → refatora de novo
   - Kaven **decide arquitetura upfront** (zero refatorações estruturais)

4. **Perfeccionismo sem Término**
   - "Só mais um ajuste..." → nunca deploya
   - Kaven **força checkpoints** (deploy staging obrigatório em X dias)

---

## 2. Ferramenta: Google Antigravity (v1.3.0)

### 2.1 Por Que Antigravity?

**Antigravity foi escolhido vs. Cursor pelas seguintes razões técnicas:**

| **Aspecto** | **Cursor (v1.2)** | **Antigravity (v1.3)** | **Vantagem** |
|-------------|-------------------|------------------------|--------------|
| **Custo** | $40/mês ($480/ano) | Grátis (Google One AI Premium) | ✅ $480/ano economizados |
| **Contexto** | 200K tokens | **1M tokens** (5x maior) | ✅ PDR inteiro no contexto |
| **Artifacts** | Apenas diffs | **Task lists, plans, videos, screenshots** | ✅ Accountability nativa |
| **Browser Testing** | Manual | **Automático** (subagente dedicado) | ✅ Testa UI sozinho |
| **Workflows** | .cursorrules estáticas | **Workflows dinâmicos** (triggers) | ✅ Pipeline automático |
| **Learning** | Sem memória | **Knowledge base** (aprende entre projetos) | ✅ Evolução contínua |
| **Code Execution** | Você testa | **Agent testa** automaticamente | ✅ Ciclo completo |

**Decisão:** Antigravity oferece **funcionalidades superiores para Kaven** com **custo zero marginal**.

---

### 2.2 Conceitos Chave do Antigravity

#### **A) Agent Manager**
```
Dashboard para orquestrar agents.

Modes:
- Plan: Agent gera plano antes de executar (ideal para tarefas complexas)
- Fast: Agent executa imediatamente (ideal para fixes rápidos)

Kaven usa: Plan mode (sempre)
```

#### **B) Workflows**
```
Prompts salvos e reutilizáveis.
Acionados com /comando

Equivalente a: .cursorrules (Cursor) ou .mdc files (Kaven v1.2)

Kaven tem 5 workflows principais:
/kickoff    → Fase 1.1 (ideia → kickoff.json)
/pdr        → Fase 1.2 (kickoff.json → PDR.md)
/backend    → Fase 1.3 (PDR → schema.prisma)
/contracts  → Fase 1.4 (PDR → api_contracts.ts)
/tasks      → Fase 1.6 (PDR → cursor_tasks.json)
```

#### **C) Rules**
```
Instruções persistentes para o Agent.
Aplicadas automaticamente em todas as interações.

Kaven tem 2 rules principais:
1. Kaven Core Principles (fase structure, validação, stack)
2. Code Style (TypeScript, naming, error handling)
```

#### **D) Artifacts**
```
Documentos gerados automaticamente pelo Agent:

Tipos:
- Task List: O que vai fazer
- Implementation Plan: Como vai fazer
- Walkthrough: O que fez (com screenshots/videos)
- Code Changes: Diffs visuais

Benefício para TDAH:
✅ Transparência total das decisões do Agent
✅ Pode comentar direto no Artifact
✅ Histórico permanente de raciocínio
```

#### **E) Browser Integration**
```
Subagente dedicado controla Chrome:
- Navega em páginas
- Clica, digita, scrolla
- Tira screenshots
- Grava vídeos de testes

Kaven usa para:
✅ Testar UI automaticamente (TaskFlow, etc.)
✅ Validar fluxos end-to-end
✅ Gerar videos de acceptance criteria
```

---

### 2.3 Setup Antigravity

**Guia completo:** Ver `ANTIGRAVITY_SETUP_GUIDE.md`

**Resumo:**
```bash
# 1. Instalar
brew install --cask google-antigravity

# 2. Configurar
- Login com Google (Google One AI Premium)
- Model: Claude Sonnet 4.5
- Criar Rules (Kaven Core + Code Style)
- Criar Workflows (/kickoff, /pdr, /backend, /contracts, /tasks)

# 3. Validar
- Testar /kickoff com ideia simples
- Testar /pdr gerando PDR
- Verificar Artifacts gerados

# Tempo total: 2-3 horas
```

---

## 3. Stack Opinativa (v1.2.0 — MANTIDA)

### 3.1 Stack Fixa por Modo

**PERSONAL (uso próprio):**
```yaml
backend:
  runtime: "Node.js"
  framework: "Fastify"
  orm: "Prisma"
  database: "SQLite"
  queue: "BullMQ (opcional)"
  cache: "Redis (opcional)"
  auth: "JWT + Refresh Tokens"

frontend:
  framework: "Next.js 14+ (App Router)"
  library: "React 18"
  styling: "Tailwind CSS + shadcn/ui"
  api: "tRPC + Zod"

deploy:
  dev: "Docker Compose"
  prod: "Docker Compose (self-hosted)"
```

**BUSINESS (comercial):**
```yaml
backend:
  runtime: "Node.js"
  framework: "Fastify"
  orm: "Prisma"
  database: "PostgreSQL"
  queue: "BullMQ (obrigatório)"
  cache: "Redis (obrigatório)"
  auth: "JWT + Refresh Tokens"

frontend:
  framework: "Next.js 14+ (App Router)"
  library: "React 18"
  styling: "Tailwind CSS + shadcn/ui"
  api: "tRPC + Zod"

deploy:
  dev: "Docker Compose"
  staging: "Docker Compose (self-hosted)"
  prod: "Docker Compose ou cloud (Railway/Render)"
```

---

## 4. Pipeline Kaven (v1.3.0 — Antigravity)

### 4.1 Visão Geral

```
┌──────────────────────────────────────────────────────────┐
│ ENTRADA: Ideia Caótica                                   │
│ "Quero um task manager minimalista"                     │
└────────────┬─────────────────────────────────────────────┘
             │
             ▼
┌──────────────────────────────────────────────────────────┐
│ FASE 1: PRÉ-PRODUÇÃO (1-2 semanas)                      │
│                                                          │
│ 1.1 /kickoff → kickoff.json                            │
│ 1.2 /pdr → PDR.md (15 seções)                          │
│ 1.3 /backend → schema.prisma + backend_analysis.md     │
│ 1.4 /contracts → api_contracts.ts                      │
│ 1.5 Scaffold → monorepo Next.js (manual/agent)         │
│ 1.6 /tasks → cursor_tasks.json                         │
│                                                          │
│ Artifacts gerados:                                       │
│ ✅ Implementation Plans (1 por workflow)                │
│ ✅ Walkthroughs (documentando decisões)                 │
│                                                          │
│ Gate G1: PDR + artefatos validados                     │
└────────────┬─────────────────────────────────────────────┘
             │
             ▼
┌──────────────────────────────────────────────────────────┐
│ FASE 2: PRODUÇÃO (4-6 semanas)                          │
│                                                          │
│ 2.1 Sprint 0 → setup (.env, migrations)                │
│ 2.2 Execução via Agent:                                 │
│     - Agent lê task-001                                  │
│     - Agent escreve código                               │
│     - Agent TESTA automaticamente (terminal + browser)   │
│     - Agent gera video de validação                      │
│     - Agent commita e avança para task-002              │
│                                                          │
│ 2.3 Checkpoints (a cada 5 dias):                        │
│     - Agent gera relatório de progresso                  │
│     - Artifacts mostram tasks completas                  │
│                                                          │
│ 2.4 Deploy Staging → MVP funcional                     │
│                                                          │
│ Gate G2: MVP funcional + staging estável                │
└────────────┬─────────────────────────────────────────────┘
             │
             ▼
┌──────────────────────────────────────────────────────────┐
│ DECISÃO: Validar ou Arquivar                            │
│                                                          │
│ ✅ Funciona + você usa por 30 dias?                     │
│    → Ativa FASE 3 (GTM)                                 │
│                                                          │
│ ❌ Não funciona ou não usa?                             │
│    → Arquiva + post-mortem + próximo projeto           │
└──────────────────────────────────────────────────────────┘
```

---

### 4.2 Workflows Detalhados

#### **Workflow 1: /kickoff (Fase 1.1)**

**Input:** Descrição caótica da ideia (2-3 frases)

**Processo:**
```
1. Agent pergunta: "Descreva sua ideia"
2. Agent pergunta: "Qual a dor específica?"
3. Agent pergunta: "Quem é o usuário?"
4. Agent classifica complexidade (1-10)
5. Agent infere mode (personal vs. business)
6. Agent extrai 2-3 core features
7. Agent calcula estimated_weeks
8. Agent gera kickoff.json

Artifact gerado:
✅ Implementation Plan (mostra raciocínio de classificação)
```

**Output:** `kickoff.json` validado

**Tempo:** 10-15 minutos

---

#### **Workflow 2: /pdr (Fase 1.2)**

**Input:** `kickoff.json`

**Processo:**
```
1. Agent lê kickoff.json
2. Agent gera Implementation Plan:
   - 15 seções listadas
   - Decisões-chave por seção
   - Estimativa de tempo
3. Agent gera PDR.md seção por seção:
   - Section 1: Executive Summary
   - Section 2: Problem & Pain
   - ...
   - Section 15: Appendices
4. Agent valida estrutura (exatamente 15 seções)
5. Agent gera Walkthrough Artifact

Artifacts gerados:
✅ Implementation Plan (15 seções planejadas)
✅ Walkthrough (decisões documentadas)
```

**Output:** `PDR.md` com 15 seções

**Tempo:** 30-60 minutos

---

#### **Workflow 3: /backend (Fase 1.3)**

**Input:** `PDR.md`

**Processo:**
```
1. Agent lê Section 6: Information Architecture
2. Agent identifica entidades e relacionamentos
3. Agent gera schema.prisma
4. Agent valida: npx prisma validate
5. Se válido: gera backend_analysis.md
6. Se inválido: corrige e repete

Artifacts gerados:
✅ Implementation Plan (entidades + relacionamentos)
✅ Walkthrough (decisões de schema)
✅ Entity diagram (visual)
```

**Output:** 
- `schema.prisma` validado
- `backend_analysis.md`

**Tempo:** 15-30 minutos

---

#### **Workflow 4: /contracts (Fase 1.4)**

**Input:** `PDR.md` + `schema.prisma`

**Processo:**
```
1. Agent lê PDR Section 5: User Stories
2. Agent lê schema.prisma (models)
3. Agent gera api_contracts.ts:
   - CREATE mutation por entidade
   - READ queries (getAll, getById)
   - UPDATE mutation
   - DELETE mutation
4. Agent valida: npx tsc --noEmit
5. Se válido: finaliza
6. Se inválido: corrige tipos

Artifacts gerados:
✅ Implementation Plan (endpoints planejados)
✅ Walkthrough (decisões de API design)
```

**Output:** `api_contracts.ts` compilável

**Tempo:** 20-40 minutos

---

#### **Workflow 5: /tasks (Fase 1.6)**

**Input:** `PDR.md` + `schema.prisma` + `api_contracts.ts`

**Processo:**
```
1. Agent lê PDR Section 13: Incremental Roadmap
2. Agent quebra roadmap em tasks atômicas
3. Agent gera cursor_tasks.json:
   - 1 task = 1 dia (4-8 horas)
   - Dependencies explícitas
   - Acceptance criteria binários
4. Agent gera dependency graph
5. Agent valida: sem ciclos, deps satisfeitas

Artifacts gerados:
✅ Implementation Plan (tasks listadas)
✅ Task dependency graph (visual)
✅ Timeline visualization
✅ Walkthrough (decisões de quebra)
```

**Output:** `cursor_tasks.json` validado

**Tempo:** 30-60 minutos

---

### 4.3 Fase 2: Implementação via Agent

**Diferença vs. Cursor (v1.2):**

```diff
Cursor (v1.2):
- Você: "Implement task-001"
- Cursor: gera código
- Você: npm run test
- Você: reporta bugs
- Cursor: corrige
- Você: commit

Antigravity (v1.3):
- Você: "Implement task-001"
+ Agent: gera Implementation Plan
+ Agent: escreve código
+ Agent: roda npm run test automaticamente
+ Agent: abre browser e testa UI
+ Agent: grava video de validação
+ Agent: se falhar, itera sozinho (até 3x)
+ Agent: commita automaticamente
+ Agent: gera Walkthrough Artifact
- Você: revisa Artifact e aprova
```

**Vantagem:** Ciclo completo automático (gerar → testar → validar → commit)

---

## 5. Estratégia de Validação (v1.2.0 — MANTIDA)

### 5.1 Sequência Incremental

```
TaskFlow (trivial)
  ├─ 3 features core
  ├─ 7 tasks
  ├─ 3 semanas
  └─ Valida: prompts funcionam?

     ↓ (se sucesso)

Expense Tracker (médio)
  ├─ 5 features
  ├─ 12 tasks
  ├─ 5 semanas
  └─ Valida: escala para projetos médios?

     ↓ (se sucesso)

BrainOS (complexo)
  ├─ 10+ features
  ├─ 30+ tasks
  ├─ 12 semanas
  └─ Valida: funciona em projetos reais?
```

---

## 6. Roadmap Executável (Antigravity)

### 6.1 Dias 1-2: Setup Antigravity

**Objetivo:** Ferramenta instalada e workflows configurados

**Tasks:**
1. [ ] Instalar Antigravity (via brew ou .exe)
2. [ ] Login com Google (Google One AI Premium)
3. [ ] Configurar Model: Claude Sonnet 4.5
4. [ ] Instalar Browser Extension
5. [ ] Criar 2 Rules:
   - Kaven Core Principles
   - Code Style
6. [ ] Criar 5 Workflows:
   - /kickoff
   - /pdr
   - /backend
   - /contracts
   - /tasks
7. [ ] Testar /kickoff com ideia simples
8. [ ] Validar que Artifacts são gerados

**Entrega:** Antigravity funcional com workflows Kaven

**Tempo:** 2-3 horas

**Ver:** `ANTIGRAVITY_SETUP_GUIDE.md` para detalhes

---

### 6.2 Dias 3-5: Validar Workflows (Fase 1)

**Dia 3: Kickoff + PDR**
```
1. /kickoff com TaskFlow:
   - "Sistema de gestão de tarefas minimalista"
   - 3 features: CRUD, filtros, export JSON
   
2. /pdr
   - Gerar PDR completo (15 seções)
   - Revisar Artifacts
   - Validar estrutura

Critério de sucesso:
✅ kickoff.json válido
✅ PDR.md com 15 seções
✅ Artifacts documentam decisões
```

**Dia 4: Backend + Contracts**
```
1. /backend
   - Gerar schema.prisma
   - Validar: npx prisma validate
   
2. /contracts
   - Gerar api_contracts.ts
   - Validar: npx tsc --noEmit

Critério de sucesso:
✅ Schema válido
✅ Contracts compilam
✅ Entity diagram gerado (Artifact)
```

**Dia 5: Tasks**
```
1. /tasks
   - Gerar cursor_tasks.json
   - Revisar dependency graph (Artifact)
   - Validar: sem ciclos, deps OK

Critério de sucesso:
✅ cursor_tasks.json com 7 tasks
✅ Dependency graph visual
✅ Timeline realista (≤ 21 dias)
```

---

### 6.3 Dias 6-21: Implementar TaskFlow (Fase 2)

**Processo por task:**
```
Para cada task (001 até 007):

1. Agent Manager → New Task
2. Prompt: "Implement task-XXX from cursor_tasks.json"
3. Agent:
   - Gera Implementation Plan
   - Escreve código
   - Roda testes (terminal)
   - Testa UI (browser, se aplicável)
   - Grava video de validação
   - Commita
4. Você:
   - Revisa Walkthrough Artifact
   - Aprova ou pede ajustes
5. Próxima task

Tempo por task: 4-8 horas (1 dia útil)
Total: 7 dias úteis (pode fazer 2-3 por dia se focar)
```

**Checkpoints:**
```
Dia 10 (após task-003):
- Revisar progresso
- Artifacts mostram decisões?
- Código funciona?

Dia 15 (após task-005):
- Revisar progresso
- UI funciona?
- Testes passam?

Dia 21 (após task-007):
- TaskFlow completo
- Deploy staging
- Usar por 7 dias
```

---

### 6.4 Dia 22-30: Validação + Retrospectiva

**Dia 22-28: Uso Diário**
```
Usar TaskFlow como task manager real:
- Criar tasks
- Filtrar
- Exportar JSON
- Anotar bugs/melhorias
```

**Dia 29-30: Decisão**
```
Retrospectiva:
✅ Kaven funcionou?
✅ Antigravity foi superior vs. manual?
✅ Artifacts ajudaram?
✅ 1M tokens fez diferença?
✅ Browser testing funcionou?

Se SIM (≥4/5):
→ Kaven v1.3 VALIDADO
→ Seguir para Expense Tracker (projeto médio)
→ Antigravity é ferramenta oficial

Se NÃO:
→ Identificar gargalos
→ Ajustar workflows
→ Testar novamente ou voltar para Cursor
```

---

## 7. Métricas de Sucesso

### 7.1 Kaven (Sistema)

**Eficiência:**
- Tempo Fase 1: < 2 semanas (vs. manual = 1-2 meses)
- Tempo por workflow: < 1 hora cada
- Taxa de aprovação G1: > 90%

**Qualidade:**
- Taxa de refatorações estruturais: < 5%
- Artifacts gerados: 100% (todos os workflows)
- Walkthroughs legíveis: > 90%

**Antigravity (específico):**
- Browser tests automáticos: ≥ 80% das UI tasks
- 1M tokens usado: ≥ 50% do contexto disponível
- Learning entre projetos: observável após 2+ projetos

---

### 7.2 TaskFlow (Primeiro Teste)

**Fase 1:**
- PDR gerado: ≤ 4 horas ✅
- Schema válido: sim/não
- Contracts compilam: sim/não
- Artifacts completos: 5/5 workflows

**Fase 2:**
- Tasks implementadas: 7/7 (100%)
- Tempo total: ≤ 21 dias
- Refatorações estruturais: 0
- Browser tests automáticos: ≥ 5/7 tasks

**Validação:**
- Uso diário: ≥ 5/7 dias
- Bugs críticos: 0
- NPS (auto-avaliação): ≥ 7/10
- Artifacts úteis: ≥ 8/10

---

## 8. Troubleshooting Antigravity

### P: Workflow não executou
**R:** 
- Verificar formato YAML correto (---  name: ... ---)
- Verificar trigger definido (/kickoff)
- Verificar Rules ativas (Settings → Rules)

### P: Agent não entendeu instruções
**R:**
- Simplificar prompt do workflow
- Adicionar mais exemplos
- Usar Fast mode em vez de Plan (mais direto)

### P: Artifacts muito verbosos
**R:**
- Normal (Antigravity gera muita documentação)
- Focar em Implementation Plan e Walkthrough
- Task List é secundário

### P: Browser testing falhou
**R:**
- Verificar Browser Extension ativa
- Verificar permissões do Chrome
- Agent pode precisar de retry manual

### P: Agent iterou 3x e ainda falhou
**R:**
- Revisar acceptance criteria (ambíguo?)
- Implementar manualmente essa task
- Reportar bug ao Agent (comentário no Artifact)

---

## 9. Comparação: Cursor vs. Antigravity

### 9.1 Quando Usar Cada Um

**Use Antigravity (Kaven padrão):**
- ✅ Projetos Kaven (Fase 1 + 2)
- ✅ Tarefas com múltiplas etapas (pipeline)
- ✅ Precisa testar UI automaticamente
- ✅ Quer documentação automática (Artifacts)
- ✅ Contexto grande (> 200K tokens)

**Use Cursor (fallback/alternativa):**
- ✅ Debugging granular (precisa de controle linha por linha)
- ✅ Antigravity está indisponível (downtime)
- ✅ Task específica que Antigravity falhou 3x
- ✅ Preferência pessoal (você gosta mais do Cursor)

**Use Ambos (híbrido):**
- Antigravity: Fase 1 (gerar PDR, schema, contracts, tasks)
- Cursor: Fase 2 (implementação manual quando necessário)
- Decisão: caso a caso

---

## 10. Changelog Detalhado

### v1.3.0 (2025-12-01) — MIGRAÇÃO ANTIGRAVITY

**🚀 Mudanças Fundamentais:**

1. **Ferramenta Oficial: Google Antigravity**
   - ✅ Custo zero (incluído no Google One AI Premium)
   - ✅ 1M tokens de contexto (5x maior que Cursor)
   - ✅ Artifacts nativos (transparência total)
   - ✅ Browser testing automático
   - ✅ Learning entre projetos

2. **Workflows Substituem .cursorrules**
   - ❌ REMOVIDO: .cursor/agents/*.mdc
   - ❌ REMOVIDO: .cursor/rules/*.mdc
   - ✅ NOVO: Antigravity Workflows (/kickoff, /pdr, /backend, /contracts, /tasks)
   - ✅ NOVO: Antigravity Rules (Global + Project)

3. **Pipeline Automático**
   - ✅ Agent testa código automaticamente (terminal + browser)
   - ✅ Agent gera videos de validação
   - ✅ Agent itera sozinho (até 3x)
   - ✅ Artifacts documentam cada decisão

4. **Setup Guide Dedicado**
   - ✅ ANTIGRAVITY_SETUP_GUIDE.md (passo a passo completo)
   - ✅ 2-3 horas de setup (incluindo workflows)

**Compatibilidade:**
- Stack: MANTIDA (v1.2.0)
- Validação: MANTIDA (TaskFlow → Expense Tracker → BrainOS)
- Roadmap: ATUALIZADO (Antigravity-specific)

**Breaking Changes:**
- Workflows não são compatíveis com .cursorrules (formato diferente)
- Precisa reconfigurar Rules no Antigravity (não importa de Cursor)
- Artifacts são novos (não existiam no Cursor)

---

### v1.2.0 (2025-12-01)
- Stack estabilizada
- Validação incremental (TaskFlow primeiro)
- Paradoxo auto-construção removido

### v1.1.0 (2025-10-29)
- Integração .shared + 12 agentes

### v1.0.0 (2025-10-29)
- Documento inicial

---

## 11. Próximos Passos Imediatos

### Hoje (2025-12-01):
1. ✅ Ler `ANTIGRAVITY_SETUP_GUIDE.md`
2. ✅ Instalar Antigravity
3. ✅ Configurar Model + Rules + Workflows
4. ✅ Testar /kickoff com ideia simples

### Amanhã (2025-12-02):
1. Gerar kickoff.json do TaskFlow (/kickoff)
2. Gerar PDR.md do TaskFlow (/pdr)
3. Revisar Artifacts
4. Validar estrutura

### Próxima Semana (Dia 3-7):
1. /backend → schema.prisma
2. /contracts → api_contracts.ts
3. /tasks → cursor_tasks.json
4. Checkpoint: Fase 1 completa?

### Semanas 2-3 (Dia 8-21):
1. Implementar TaskFlow (7 tasks)
2. Checkpoints a cada 5 dias
3. Deploy staging
4. Uso por 7 dias

### Semana 4 (Dia 22-30):
1. Retrospectiva
2. Decisão: Kaven validado?
3. Se sim: Expense Tracker
4. Se não: iterar ou abortar

---

## 12. Recursos Adicionais

### Documentação Oficial:
- **Antigravity:** https://antigravity.google/docs/home
- **Codelabs:** https://codelabs.developers.google.com/getting-started-google-antigravity
- **Blog Google:** https://developers.googleblog.com/en/build-with-google-antigravity

### Community:
- Medium: "Tutorial: Getting Started with Google Antigravity"
- GitHub: Procurar por "antigravity workflows" ou "antigravity rules"

### Kaven Docs:
- `ANTIGRAVITY_SETUP_GUIDE.md` (este repositório)
- `KAVEN_ROADMAP_30_DIAS.md` (roadmap original, atualizar para Antigravity)
- `TASKFLOW_SPEC.md` (especificação do projeto de validação)

---

## ✅ CHECKLIST FINAL

Antes de começar o TaskFlow:

- [ ] Antigravity instalado e funcionando
- [ ] Browser extension ativa
- [ ] Model = Claude Sonnet 4.5
- [ ] 2 Rules configuradas (Kaven Core + Code Style)
- [ ] 5 Workflows criados e testados
- [ ] Agent Manager configurado (Kaven Pipeline Agent)
- [ ] Validação com kickoff.json passou
- [ ] PDR.md de teste gerado (15 seções)
- [ ] Artifacts foram revisados
- [ ] Repositório kaven-antigravity/ criado

**Se todos = ✅ → Você está pronto para começar!**

---

**FIM DO DOCUMENTO — KAVEN v1.3.0 (Antigravity Edition)**

**Próximo passo:** Execute `ANTIGRAVITY_SETUP_GUIDE.md`
