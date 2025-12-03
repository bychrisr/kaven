# PDR to Backend (schema.prisma Generator)

> **Versão:** 2.0.0  
> **Data:** 2024-12-03  
> **Autor:** Chris + Claude Sonnet 4.5  
> **Status:** Production Ready  
> **Propósito:** Workflow Antigravity para gerar schema.prisma multi-tenant a partir de PDR Section 6

---

## 📋 Metadata

```yaml
workflow_id: backend
phase: pre-production
step: 1.3
input: PDR.md (Section 6)
output: schema.prisma + backend_analysis.md
estimated_time: 15-30 minutos
prerequisites: PDR.md aprovado
```

---

# PDR to Backend

You are generating a Prisma schema and backend analysis from the PDR.

## Prerequisites:

1. Verify that `PDR.md` exists.
2. Read Section 6: Information Architecture.
3. Read Section 10: Architectural Decisions (to know if SQLite or PostgreSQL).

## Steps:

1. Extract entities from Section 6 of PDR.md.

2. For each entity, identify:
   - Fields (name, type)
   - Relationships (@relation)
   - Indexes (on filterable fields)
   - Constraints (unique, optional)

3. Generate `schema.prisma` with this structure:

```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "sqlite"  // or "postgresql" based on PDR Section 10
  url      = env("DATABASE_URL")
}

// Models here
model Entity {
  id          String   @id @default(uuid())
  field1      String
  field2      Int?
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt

  @@index([field1])
}

// Enums here (if needed)
enum Status {
  VALUE1
  VALUE2
}
```

4. Generate `backend_analysis.md` with:
   - Entity-Relationship diagram (textual)
   - RLS policies (if PostgreSQL + multi-tenancy)
   - Key architectural decisions
   - Security considerations

## Schema Requirements:

- Use `uuid()` for IDs (not auto-increment)
- Add `createdAt` and `updatedAt` to all models (DateTime)
- Add `@@index` on fields that will be filtered (status, dates, foreign keys)
- Use `String?` for optional fields (question mark)
- **CRITICAL - SQLite Enum Handling:**
  - If provider = "sqlite": Use `String` type for enum fields (SQLite has no native Enum)
  - Add comment documenting valid values: `status String // Enum: TODO, IN_PROGRESS, DONE`
  - Add `@default()` with string value: `@default("TODO")`
  - Document that Zod validation will enforce enum constraints
  - If provider = "postgresql": Use native Prisma enums
- If mode=personal: `provider = "sqlite"`
- If mode=business: `provider = "postgresql"`

## Example Schema (TaskFlow):

```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "sqlite"
  url      = env("DATABASE_URL")
}

model Task {
  id          String     @id @default(uuid())
  title       String     @db.VarChar(200)  // Max 200 chars
  description String?
  status      String     @default("TODO")  // Enum: TODO, IN_PROGRESS, DONE
  priority    String     @default("MEDIUM") // Enum: LOW, MEDIUM, HIGH
  deadline    DateTime?
  createdAt   DateTime   @default(now())
  updatedAt   DateTime   @updatedAt

  @@index([status])
  @@index([createdAt])
  @@index([priority])
}
```

**Note:** For SQLite, enums are stored as Strings with application-level validation (Zod).

## Validation:

After generating `schema.prisma`, validate it:

```bash
npx prisma validate
```

If validation fails with "Enum not supported" error:
- You're using SQLite with Prisma 6.x (has bugs)
- Downgrade to Prisma 5.10.0: `npm install @prisma/client@5.10.0 prisma@5.10.0`
- Convert enums to String fields as documented above
- Re-run validation

If validation fails with other errors:
- Fix syntax errors
- Check relation fields (@relation)
- Ensure all models have `@@index` where needed

Only proceed when validation passes successfully.

## Output:

1. Save `schema.prisma` to `prisma/schema.prisma`
2. Save `backend_analysis.md` to project root

Show me the generated schema and ask for approval before saving.
