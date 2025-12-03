# Kickoff to JSON

> **Versão:** 2.0.0  
> **Data:** 2024-12-03  
> **Autor:** Chris + Claude Sonnet 4.5  
> **Status:** Production Ready  
> **Propósito:** Workflow Antigravity para transformar ideia caótica em PDR.seed.json estruturado (fluxo IDEA → PERSONAL/BUSINESS)

---

## 📋 Metadata

```yaml
workflow_id: kickoff
phase: pre-production
step: 1.1
input: Ideia caótica (conversação)
output: PDR.seed.json
estimated_time: 10-15 minutos
prerequisites: Nenhum
```

---

## 🎯 Objetivo

Transformar uma ideia caótica do usuário em um arquivo `PDR.seed.json` estruturado que servirá como input para o workflow `/pdr`.

---

## 📝 Steps

**IMPORTANT: Collect ALL inputs before generating PDR.seed.json. Do NOT attempt to generate partial outputs.**

### 1. Descrever a Ideia

Ask the user to describe their idea in 2-3 sentences (can be chaotic).

**Exemplo de pergunta:**
> "Descreva sua ideia em 2-3 frases. Pode ser bem informal, vou estruturar depois!"

---

### 2. Pain Point Específico

Ask about the specific pain point (not generic like "improve productivity").

**If user gives generic pain (e.g., "slow apps", "inefficient", "complex"):**

Follow up: 
> "Can you quantify? For example:
> - How much time is wasted? (e.g., '2 hours/week')
> - What specific action is slow? (e.g., 'loading takes 10 seconds')
> - What happens because of this pain? (e.g., 'users abandon the tool')"

**Only proceed when pain is specific and measurable.**

---

### 3. Target User (Persona Específica)

Ask about the target user (specific persona).

**If user gives generic answer (e.g., "personal use", "developers", "students"):**

Follow up:
> "Describe a specific person who would use this:
> - What is their role/profession?
> - What are their current frustrations?
> - What is their tech comfort level? (Low/Medium/High)"

**Only proceed when persona is detailed (name optional but encouraged).**

---

### 4. Classify Complexity (1-10)

Classify complexity based on:
- **Base:** 1 (simple CRUD)
- **External integrations:** +2 each
- **Real-time features (WebSocket):** +2
- **AI/ML:** +3
- **Multi-tenancy:** +2
- **Blockchain/Web3:** +3

**Validation:**
- If no integrations/features above: complexity = "low" (1-3)
- Explain your scoring to the user before finalizing

**Exemplo:**
- CRUD básico = 1
- CRUD + Stripe integration = 3
- CRUD + Stripe + Real-time notifications = 5

---

### 5. Infer Mode

- **personal:** solo use or <10 known users, no billing
- **business:** monetization intended, external paying users

---

### 6. Extract 2-3 Core v1 Features

Extract 2-3 core v1 features (MUST be end-to-end):
- Each feature should include: frontend + backend + DB + integrations
- **NOT** "Timer" (too vague)
- **YES** "Focus Timer (40min) + BlocksLog (Markdown + SQLite)" (specific)

**Exemplo:**
```
✅ GOOD: "Task CRUD with priority levels (High/Medium/Low) + deadline tracking"
❌ BAD: "Task management"
```

---

### 7. Calculate Estimated Weeks

Based on complexity:
- **low (1-3):** 2-4 weeks
- **medium (4-6):** 4-8 weeks
- **high (7-10):** 8-16 weeks

---

### 8. Calculate Deadline

`deadline = today + estimated_weeks`

---

### 9. Infer Data Sensitivity

- **low:** public data, no PII
- **medium:** basic PII (name, email)
- **high:** health, financial, sensitive personal data

---

### 10. Generate PDR.seed.json

Generate with this EXACT schema:

```json
{
  "project_id": "kebab-case-name",
  "timestamp": "ISO 8601 datetime",
  "origin": "idea",
  "objective": "personal|business",
  "idea": "2-3 sentences concise",
  "pain": "specific pain (not generic)",
  "target_user": "specific persona",
  "complexity": "low|medium|high",
  "complexity_score": 1-10,
  "core_v1": [
    "feature 1 (end-to-end, specific)",
    "feature 2 (end-to-end, specific)",
    "feature 3 (optional, end-to-end, specific)"
  ],
  "deadline": "YYYY-MM-DD",
  "estimated_weeks": number,
  "critical_integrations": ["string"],
  "data_sensitivity": "low|medium|high",
  "status": "kickoff_approved"
}
```

---

## ✅ Validation Rules

- `core_v1` MUST have 2-3 items (not 1, not 4+)
- `pain` MUST be specific (reject "improve productivity")
- `complexity` vs `estimated_weeks` MUST match the table above
- If user input conflicts (e.g., high complexity + 2 weeks deadline), FORCE realistic adjustment

---

## 💾 Output

1. Generate the `PDR.seed.json` and display it to the user in a formatted code block

2. **Before saving, ask explicitly:**

> "Does this PDR.seed.json accurately represent your project? 
> - Pain is specific enough?
> - Target user is clear?
> - Core v1 features are end-to-end?
> 
> Type 'yes' to save, or tell me what to adjust."

3. Only save to `pre-production/pdr/PDR.seed.json` after user confirms

---

## 📂 File Location

```
Save to: pre-production/pdr/PDR.seed.json
```

---

## 🔄 Next Workflow

After `PDR.seed.json` is approved:

```bash
/pdr  # Expande seed em PDR.md completo (15 seções)
```

---

## 📊 Success Metrics

- **Time:** < 15 minutos
- **Iterations:** < 3 (usuário aprova em no máximo 3 tentativas)
- **Completeness:** Todos os campos obrigatórios preenchidos
- **Specificity:** Pain, target_user e core_v1 são específicos (não genéricos)

---

## 🚨 Common Mistakes to Avoid

1. ❌ Aceitar pain genérico ("melhorar produtividade")
2. ❌ Aceitar target_user vago ("desenvolvedores")
3. ❌ Aceitar core_v1 vago ("gerenciar tarefas")
4. ❌ Não validar consistency entre complexity e estimated_weeks

---

## Changelog

### v2.0.0 (2024-12-03) - Versionamento e Estruturação
- Adicionado cabeçalho de versionamento
- Renomeado output: `kickoff.json` → `PDR.seed.json`
- Adicionado campo `origin: "idea"` e `objective: "personal|business"`
- Estruturado em seções com emojis
- Adicionadas métricas de sucesso
- Documentado caminho de salvamento (`pre-production/pdr/`)
- Adicionado next workflow (`/pdr`)

### v1.0.0 (2024-12-02) - Versão Original
- Workflow funcional validado com TaskFlow
- Score: 9.5/10
