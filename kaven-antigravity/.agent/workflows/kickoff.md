---
description: "Kaven Phase 1.1 - Transform chaotic idea into kickoff.json"
---

# Kickoff to JSON

You are transforming a chaotic product idea into a structured kickoff.json file.

## Steps:

**IMPORTANT: Collect ALL inputs before generating kickoff.json. Do NOT attempt to generate partial outputs.**

1. Ask the user to describe their idea in 2-3 sentences (can be chaotic).

2. Ask about the specific pain point (not generic like "improve productivity").
   
   **If user gives generic pain (e.g., "slow apps", "inefficient", "complex"):**
   - Follow up: "Can you quantify? For example:
     - How much time is wasted? (e.g., '2 hours/week')
     - What specific action is slow? (e.g., 'loading takes 10 seconds')
     - What happens because of this pain? (e.g., 'users abandon the tool')"
   
   **Only proceed when pain is specific and measurable.**

3. Ask about the target user (specific persona).
   
   **If user gives generic answer (e.g., "personal use", "developers", "students"):**
   - Follow up: "Describe a specific person who would use this:
     - What is their role/profession?
     - What are their current frustrations?
     - What is their tech comfort level? (Low/Medium/High)"
   
   **Only proceed when persona is detailed (name optional but encouraged).**

4. Classify complexity (1-10) based on:
   - External integrations: +2 each
   - Real-time features (WebSocket): +2
   - AI/ML: +3
   - Multi-tenancy: +2
   - Blockchain/Web3: +3
   
   **Validation:**
   - Base complexity = 1 (simple CRUD)
   - If no integrations/features above: complexity = "low" (1-3)
   - Explain your scoring to the user before finalizing

5. Infer mode:
   - personal: solo use or <10 known users, no billing
   - business: monetization intended, external paying users

6. Extract 2-3 core v1 features (MUST be end-to-end):
   - Each feature should include: frontend + backend + DB + integrations
   - NOT "Timer" (too vague)
   - YES "Focus Timer (40min) + BlocksLog (Markdown + SQLite)" (specific)

7. Calculate estimated weeks based on complexity:
   - low (1-3): 2-4 weeks
   - medium (4-6): 4-8 weeks
   - high (7-10): 8-16 weeks

8. Calculate deadline: today + estimated_weeks

9. Infer data_sensitivity:
   - low: public data, no PII
   - medium: basic PII (name, email)
   - high: health, financial, sensitive personal data

10. Generate kickoff.json with this EXACT schema:

```json
{
  "project_id": "kebab-case-name",
  "timestamp": "ISO 8601 datetime",
  "idea": "2-3 sentences concise",
  "pain": "specific pain (not generic)",
  "target_user": "specific persona",
  "complexity": "low|medium|high",
  "complexity_score": 1-10,
  "mode": "personal|business",
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

## Validation Rules:

- core_v1 MUST have 2-3 items (not 1, not 4+)
- pain MUST be specific (reject "improve productivity")
- complexity vs estimated_weeks MUST match the table above
- If user input conflicts (e.g., high complexity + 2 weeks deadline), FORCE realistic adjustment

## Output:

Generate the kickoff.json and display it to the user in a formatted code block.

**Before saving, ask explicitly:**
"Does this kickoff.json accurately represent your project? 
 - Pain is specific enough?
 - Target user is clear?
 - Core v1 features are end-to-end?
 
Type 'yes' to save, or tell me what to adjust."

Only save to `kickoff.json` after user confirms.
