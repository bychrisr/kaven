# KAVEN PROJECT - Contributing Guide

> **Versão:** 1.0.0  
> **Data:** 2024-12-03  
> **Autor:** Chris + Claude Sonnet 4.5  
> **Status:** Production Ready  
> **Propósito:** Guia de contribuição e regras de versionamento para o projeto Kaven

---

## 🎯 Contexto do Projeto

Você está trabalhando no **Kaven v2.0.0**, um sistema que transforma ideias caóticas em SaaS funcionais através de 3 fases determinísticas:
- **Fase 1:** Pré-Produção (PDR como fonte de verdade)
- **Fase 2:** Produção (código via Antigravity)
- **Fase 3:** Go-to-Market (comercialização, se business)

**Ferramenta oficial:** Google Antigravity + Gemini 3 Pro  
**Stack Backend:** Node.js + Fastify + Prisma + PostgreSQL  
**Stack Frontend:** Next.js + shadcn/ui (SSR + API routes)

---

## 📂 Arquivos do Projeto

Localização: `/mnt/project/`

**Artefatos principais:**
- `BRAINSTORM.md` - Fluxo centralizado 3 fases (4 fluxos kickoff)
- `TASKFLOW_SPEC.md` - Projeto de validação v1.4.0
- `kickoff.md` - Template PDR Boilerplate
- `pdr.md` - Template PDR expandido
- `CHANGELOG.md` - Histórico de versões
- `INDEX.md` - Índice de documentação
- `README.md` - Overview central
- `SUMMARY.md` - Sumário de entrega
- `VALIDATION_REPORT.md` - Resultados TaskFlow
- `CONTRIBUTING.md` - Este documento

**Workflows Antigravity:** (`.agent/workflows/`)
- `kickoff.md` - Ideia → PDR.seed.json
- `pdr.md` - seed → PDR.md expandido
- `backend.md` - PDR → schema.prisma
- `contracts.md` - schema → tRPC + Zod
- `tasks.md` - PDR → implementation_plan.json
- `implement.md` - tasks → código funcional
- `observability.md` - Ativar/configurar observabilidade

---

## ✏️ REGRAS DE EDIÇÃO E VERSIONAMENTO

### Ao EDITAR arquivo existente:

#### 1. LER O ARQUIVO COMPLETO PRIMEIRO
```
⚠️ NUNCA edite sem ler o conteúdo atual
```

#### 2. VERIFICAR CABEÇALHO DE VERSÃO
```markdown
> **Versão:** X.Y.Z  
> **Data:** YYYY-MM-DD  
> **Autor:** Chris / Claude  
> **Status:** Draft | Review | Production Ready  
> **Propósito:** [descrição sucinta]
```

#### 3. INCREMENTAR VERSÃO (Semantic Versioning)

**MAJOR (X.0.0) - Breaking Changes:**
- Quebra compatibilidade com versão anterior
- Reestruturação completa do arquivo
- Mudança de formato (JSON → YAML, Markdown → JSON)
- Remoção de seções obrigatórias
- Exemplo: v1.4.0 → v2.0.0

**MINOR (0.Y.0) - Backwards Compatible:**
- Adiciona novas seções/features
- Expande funcionalidades existentes
- Adiciona novos workflows
- Exemplo: v1.4.0 → v1.5.0

**PATCH (0.0.Z) - Bug Fixes:**
- Correções textuais
- Melhorias de exemplos
- Clarificações
- Typos
- Exemplo: v1.4.0 → v1.4.1

#### 4. DOCUMENTAR MUDANÇAS

Adicionar seção `## Changelog` no final do arquivo:

```markdown
## Changelog

### v1.5.0 (2024-12-03) - Nova seção de observabilidade
- Adicionada Section 16: Observability (métricas multi-tenant)
- Atualizada Section 10: Stack (incluído Grafana + Prometheus)
- Exemplos de dashboards adicionados

### v1.4.1 (2024-12-02) - Correções
- Corrigido typo em Section 6
- Melhorado exemplo de schema.prisma
```

#### 5. ATUALIZAR CABEÇALHO
```markdown
> **Versão:** 1.5.0  
> **Data:** 2024-12-03  
> **Autor:** Chris + Claude Sonnet 4.5  
> **Status:** Production Ready  
> **Propósito:** [atualizado se mudou]
```

---

### Ao CRIAR novo arquivo:

#### 1. SEMPRE INCLUIR CABEÇALHO COMPLETO

```markdown
# TÍTULO DO ARQUIVO

> **Versão:** 0.1.0  
> **Data:** 2024-12-03  
> **Autor:** Chris + Claude Sonnet 4.5  
> **Status:** Draft  
> **Propósito:** [objetivo claro do arquivo]

---

[conteúdo]

---

## Changelog

### v0.1.0 (2024-12-03)
- Criação inicial do documento
- [listar seções/features principais]
```

#### 2. CONVENÇÃO DE NOMES

**Workflows:** `nome_workflow.md` (minúsculo, underscore)
- Exemplo: `kickoff.md`, `observability.md`

**Documentação:** `NOME_DOC.md` (MAIÚSCULO, underscore)
- Exemplo: `README.md`, `CHANGELOG.md`, `CONTRIBUTING.md`

**Especificações:** `Nome_Projeto_SPEC.md` (CamelCase + sufixo)
- Exemplo: `TaskFlow_SPEC.md`, `Boilerplate_SPEC.md`

**Reports:** `Nome_REPORT.md` (CamelCase + sufixo)
- Exemplo: `VALIDATION_REPORT.md`, `RESEARCH_REPORT.md`

#### 3. ADICIONAR AO INDEX.md

Sempre que criar novo arquivo:
1. Atualizar `INDEX.md` com referência ao novo arquivo
2. Incluir em jornada de leitura apropriada
3. Incrementar versão do INDEX.md (PATCH mínimo)

---

### Ao ATUALIZAR CHANGELOG.md do projeto:

#### 1. ADICIONAR ENTRADA NA VERSÃO CORRETA

```markdown
## vX.Y.Z (YYYY-MM-DD) - NOME_RELEASE

### 🚀 Major Changes
- [mudança significativa que quebra compatibilidade]

### ✅ Features
- [nova feature backwards compatible]

### 📝 Documentation
- [documentação atualizada/criada]

### 🐛 Fixes
- [bug corrigido]

### 🔧 Technical
- [mudança técnica interna]
```

#### 2. ATUALIZAR BREAKING CHANGES (se aplicável)

```markdown
## Breaking Changes Summary

### vX.Y.Z → vX.Y-1.Z
- ✅ [o que quebrou]
- ⚠️ [como migrar]
- 📦 [artefatos afetados]
```

---

## 🔄 WORKFLOW DE TRABALHO

### 1. Início da Sessão
```
1. Ler /mnt/project/README.md
2. Ler /mnt/project/CHANGELOG.md
3. Verificar última versão de cada artefato
4. Confirmar contexto com Chris
```

### 2. Durante Edições
```
1. view /mnt/project/[arquivo] (ler completo)
2. Propor mudanças ao Chris
3. Confirmar aprovação
4. str_replace para editar (ou create_file se novo)
5. Atualizar changelog interno do arquivo
6. Atualizar CHANGELOG.md do projeto (se relevante)
7. Atualizar INDEX.md (se novo arquivo)
```

### 3. Fim da Sessão
```
1. Listar arquivos modificados
2. Resumir versões incrementadas
3. Confirmar consistência entre artefatos
4. Sugerir próximos passos
```

---

## 🚫 PROIBIÇÕES ABSOLUTAS

❌ **NUNCA** edite arquivo sem ler conteúdo atual  
❌ **NUNCA** crie arquivo sem cabeçalho versionado  
❌ **NUNCA** incremente versão sem documentar changelog  
❌ **NUNCA** use informação desatualizada (sempre ler arquivos)  
❌ **NUNCA** assuma que memória está atualizada (arquivos são fonte de verdade)  
❌ **NUNCA** faça mudanças sem aprovação de Chris

---

## ✅ VALIDAÇÕES OBRIGATÓRIAS

Antes de finalizar qualquer edição:

### 1. Consistência de Versão
- [ ] Cabeçalho atualizado?
- [ ] Changelog interno preenchido?
- [ ] Data correta (YYYY-MM-DD)?
- [ ] Status apropriado (Draft/Review/Production Ready)?

### 2. Consistência de Referências
- [ ] Se arquivo A menciona arquivo B, B existe?
- [ ] Versões referenciadas estão corretas?
- [ ] Links internos funcionam?

### 3. Completude
- [ ] Todos os TODOs resolvidos ou documentados?
- [ ] Exemplos funcionam?
- [ ] Código compila (se aplicável)?

### 4. Integração
- [ ] CHANGELOG.md atualizado (se relevante)?
- [ ] INDEX.md atualizado (se novo arquivo)?
- [ ] README.md menciona mudança (se significativa)?

---

## 📋 TEMPLATE DE RESPOSTA PADRÃO

Ao editar arquivos, use este formato:

```markdown
# 📝 Editando: [NOME_ARQUIVO.md]

## Mudanças Propostas

**Versão:** X.Y.Z → X.Y+1.Z

**Tipo:** MINOR (nova seção) | PATCH (correção) | MAJOR (reestruturação)

**Changelog:**
- [Mudança 1]
- [Mudança 2]
- [Mudança 3]

## Arquivos Afetados

1. [NOME_ARQUIVO.md]
   - Seção X: [mudança específica]
   - Versão: X.Y.Z → X.Y+1.Z

2. CHANGELOG.md (se aplicável)
   - Adicionar entrada vX.Y+1.Z

3. INDEX.md (se novo arquivo)
   - Adicionar referência na seção apropriada

## Aprovação

Posso prosseguir com estas mudanças? ✅
```

---

## 🎯 PRINCÍPIOS FUNDAMENTAIS

1. **Arquivos são a fonte de verdade** (não memória)
2. **Versionar SEMPRE** (rastreabilidade total)
3. **Documentar TUDO** (changelog interno + externo)
4. **Pedir aprovação** antes de mudanças significativas
5. **Manter consistência** entre todos os artefatos
6. **Atomic commits** (1 mudança lógica = 1 commit)
7. **Test before commit** (validar mudanças)

---

## 🔍 CHECKLIST PRÉ-COMMIT

Antes de considerar trabalho completo:

- [ ] Todos os arquivos editados têm versão incrementada
- [ ] Todos os changelogs internos atualizados
- [ ] CHANGELOG.md do projeto atualizado (se relevante)
- [ ] INDEX.md atualizado (se novos arquivos)
- [ ] README.md menciona mudança (se breaking/major)
- [ ] Nenhum TODO deixado sem documentar
- [ ] Exemplos testados (mentalmente ou via código)
- [ ] Referências cruzadas validadas
- [ ] Aprovação de Chris confirmada

---

## 📦 ESTRUTURA DO KAVEN BOILERPLATE

```
kaven-boilerplate/
├─ .agent/
│  └─ workflows/              # Workflows Antigravity
├─ pre-production/
│  ├─ prompts/               # Prompts por tipo (IDEA/SOLUTIONS)
│  ├─ pdr/                   # PDR.seed.json + PDR.md
│  ├─ analysis/              # Análises técnicas
│  └─ schema/                # schema.prisma + RLS
├─ production/
│  ├─ backend/
│  │  ├─ prisma/
│  │  │  ├─ schema.prisma   # Multi-tenant base
│  │  │  └─ seed.ts
│  │  ├─ src/
│  │  │  ├─ modules/
│  │  │  │  ├─ auth/
│  │  │  │  ├─ tenant/
│  │  │  │  └─ admin/       # Painel admin
│  │  │  └─ observability/
│  │  │     ├─ metrics/     # Prometheus
│  │  │     └─ dashboards/  # Grafana
│  ├─ frontend/
│  │  └─ src/
│  │     ├─ app/            # Next.js App Router
│  │     │  └─ admin/       # UI admin multi-tenant
│  │     └─ components/
│  └─ shared/
├─ infra/
│  ├─ docker-compose.yml    # PostgreSQL + Grafana + Prometheus
│  └─ .github/workflows/
│     └─ ci.yml
└─ README.md
```

---

## 🚀 ROADMAP v2.0.0

### Sprint 0: Fundação (Semana 1)
- [x] A1: Criar CONTRIBUTING.md
- [ ] A2: Versionar artefatos existentes
- [ ] A3: Criar BOILERPLATE_SPEC.md
- [ ] A4: Criar ROADMAP_v2.0.0.md

### Sprint 1-2: Kaven Boilerplate (Semanas 2-3)
- [ ] B0: Research observabilidade
- [ ] B1: Criar repositório
- [ ] B2: Estrutura de pastas
- [ ] B3: PostgreSQL + Prisma
- [ ] B4: Painel admin
- [ ] B5: Observabilidade
- [ ] B6: Docker + CI

### Sprint 3-4: Workflows v2.0.0 (Semanas 4-5)
- [ ] C1: Refazer /kickoff
- [ ] C2: Refazer /pdr
- [ ] C3: Refazer /backend
- [ ] C4: Refazer /contracts
- [ ] C5: Refazer /tasks
- [ ] C6: Refazer /implement
- [ ] C7: Criar /observability

### Sprint 5: Validação (Semana 6)
- [ ] D1-D5: Validar com Todoist-like

### Sprint 6: Documentação (Semana 7)
- [ ] E1-E4: Atualizar docs + release

---

## 📞 Contato

**Autor:** Chris  
**Colaborador:** Claude Sonnet 4.5  
**Versão Kaven:** v2.0.0 (em desenvolvimento)  
**Data Release Target:** Q1 2025

---

## Changelog

### v1.0.0 (2024-12-03)
- Criação inicial do CONTRIBUTING.md
- Definição de regras de versionamento (Semantic Versioning)
- Template de resposta padrão
- Checklist pré-commit
- Estrutura Kaven Boilerplate documentada
- Roadmap v2.0.0 incluído
