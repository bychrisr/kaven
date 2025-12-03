# Schema to Contracts (tRPC + Zod Generator)

> **Versão:** 2.0.0  
> **Data:** 2024-12-03  
> **Autor:** Chris + Claude Sonnet 4.5  
> **Status:** Production Ready  
> **Propósito:** Workflow Antigravity para gerar tRPC routers + Zod schemas com multi-tenancy awareness

---

## 📋 Metadata

```yaml
workflow_id: contracts
phase: pre-production
step: 1.4
input: schema.prisma
output: tRPC routers + Zod schemas
estimated_time: 20-40 minutos
prerequisites: schema.prisma validado
```

---

# PDR to Contracts

You are generating tRPC API contracts with Zod validation.

## Prerequisites:

1. Verify that `PDR.md` exists.
2. Verify that `schema.prisma` exists.
3. Read PDR Section 5: User Stories.
4. Read `schema.prisma` to understand models.

## Steps:

**CRITICAL for SQLite:** If schema.prisma uses String fields for enums (due to SQLite limitation), you MUST create Zod enums to enforce validation at the application level. This is the ONLY way to prevent invalid values in SQLite databases.

1. For EACH model in `schema.prisma`, generate tRPC router with:
   - CREATE mutation (input: Zod schema with required fields)
   - READ query: `getAll` (with filters) + `getById`
   - UPDATE mutation (input: partial Zod schema)
   - DELETE mutation (input: id only)

2. Generate Zod schemas for validation:
   - CreateSchema: all required fields + optional fields
   - UpdateSchema: CreateSchema.partial() (all fields optional)
   - GetAllSchema: filters (status, search, dates)

3. Use Prisma types for type safety.

## Contract Structure:

**IMPORTANT - Prisma Client Singleton:**
Before creating routers, ensure tRPC context uses singleton pattern:

```typescript
// src/server/trpc/trpc.ts
import { PrismaClient } from '@prisma/client';

const globalForPrisma = global as unknown as { prisma: PrismaClient };

export const prisma = globalForPrisma.prisma || new PrismaClient();

if (process.env.NODE_ENV !== 'production') globalForPrisma.prisma = prisma;

export const createTRPCContext = async () => {
  return { prisma };
};
```

Then create routers:

```typescript
import { z } from 'zod';
import { router, publicProcedure } from '../trpc';

// Enums (if any in schema)
export const EntityStatus = z.enum(['VALUE1', 'VALUE2']);

// Schemas
export const CreateEntitySchema = z.object({
  field1: z.string().min(1).max(200),  // ALWAYS add max length
  field2: z.string().optional(),
  status: EntityStatus.default('VALUE1'),
});

export const UpdateEntitySchema = CreateEntitySchema.partial();

export const GetEntitiesSchema = z.object({
  status: EntityStatus.optional(),
  search: z.string().optional(),
});

// Router
export const entityRouter = router({
  // CREATE
  create: publicProcedure
    .input(CreateEntitySchema)
    .mutation(async ({ ctx, input }) => {
      return ctx.prisma.entity.create({ data: input });
    }),

  // READ (with filters)
  getAll: publicProcedure
    .input(GetEntitiesSchema)
    .query(async ({ ctx, input }) => {
      return ctx.prisma.entity.findMany({
        where: {
          ...(input.status && { status: input.status }),
          ...(input.search && {
            OR: [
              { field1: { contains: input.search } },
              { field2: { contains: input.search } },
            ],
          }),
        },
        orderBy: { createdAt: 'desc' },
      });
    }),

  // READ ONE
  getById: publicProcedure
    .input(z.string().uuid())
    .query(async ({ ctx, input }) => {
      return ctx.prisma.entity.findUnique({ 
        where: { id: input } 
      });
    }),

  // UPDATE
  update: publicProcedure
    .input(z.object({
      id: z.string().uuid(),
      data: UpdateEntitySchema,
    }))
    .mutation(async ({ ctx, input }) => {
      return ctx.prisma.entity.update({
        where: { id: input.id },
        data: input.data,
      });
    }),

  // DELETE
  delete: publicProcedure
    .input(z.string().uuid())
    .mutation(async ({ ctx, input }) => {
      return ctx.prisma.entity.delete({ 
        where: { id: input } 
      });
    }),
});
```

## Validation Rules:

- **All string inputs MUST have `.min()` and `.max()`** (NEVER allow unbounded strings)
- Date inputs use `z.string().datetime()` (not `z.date()`)
- Enums MUST match Prisma schema exactly (even if stored as String in SQLite)
- **For SQLite String-based enums:** Create `z.enum(['VALUE1', 'VALUE2'])` to enforce validation
- getAll MUST support at least: status filter + text search
- Use `contains` for text search (SQLite compatible)
- **Optional:** Add `mode: 'insensitive'` for case-insensitive search (if needed)

## Validation:

After generating, validate TypeScript compilation:

```bash
npx tsc --noEmit src/server/trpc/routers/entity.ts
```

Must compile with zero errors.

## Output:

1. Create `src/server/trpc/routers/` directory if not exists
2. For each model, save `[model].ts` (e.g., `task.ts`)
3. Save main router to `src/server/trpc/router.ts` that imports all routers

Show me the generated contracts and ask for approval before saving.
