# Kaven Boilerplate - Technical Specification

> **Versão:** 1.0.0  
> **Data:** 2024-12-03  
> **Autor:** Chris + Claude Sonnet 4.5  
> **Status:** Draft  
> **Propósito:** Especificação técnica completa do repositório template Kaven Boilerplate (multi-tenant SaaS base)

---

## 🎯 Objetivo

Criar um repositório template clonável que serve como base para qualquer projeto SaaS (PERSONAL ou BUSINESS), contendo:
- Estrutura de pastas organizada
- Multi-tenancy (PostgreSQL + RLS)
- Painel administrativo completo
- Observabilidade (Grafana + Prometheus)
- Segurança avançada (JWT + 2FA + Encryption)
- Docker Compose + CI/CD
- Documentação embutida

---

## 📦 Repositório

```
Nome: kaven-boilerplate
URL: https://github.com/bychrisr/kaven-boilerplate
Tipo: Template Repository (clonable)
Licença: MIT (ou proprietária, a definir)
```

---

## 🏗️ Estrutura de Pastas

```
kaven-boilerplate/
├── .agent/
│   └── workflows/              # Workflows Antigravity (symlink de ~/projects/kaven/.agent/workflows/)
├── pre-production/
│   ├── prompts/               # Prompts por tipo (IDEA personal/business)
│   ├── pdr/                   # PDR.seed.json + PDR.md gerados
│   ├── analysis/              # backend_analysis.md, etc
│   └── schema/                # schema.prisma + migrations (antes de mover para production/)
├── production/
│   ├── backend/
│   │   ├── prisma/
│   │   │   ├── schema.prisma  # Multi-tenant base
│   │   │   ├── seed.ts
│   │   │   └── migrations/
│   │   ├── src/
│   │   │   ├── server.ts
│   │   │   ├── modules/
│   │   │   │   ├── auth/
│   │   │   │   │   ├── jwt.service.ts
│   │   │   │   │   ├── refresh-token.service.ts
│   │   │   │   │   ├── 2fa.service.ts
│   │   │   │   │   └── encryption.service.ts
│   │   │   │   ├── tenant/
│   │   │   │   │   ├── tenant.service.ts
│   │   │   │   │   ├── tenant.router.ts (tRPC)
│   │   │   │   │   └── tenant-context.middleware.ts
│   │   │   │   └── admin/
│   │   │   │       ├── user-management/
│   │   │   │       ├── tenant-management/
│   │   │   │       ├── audit-logs/
│   │   │   │       ├── system-config/
│   │   │   │       └── reporting/
│   │   │   ├── observability/
│   │   │   │   ├── metrics/
│   │   │   │   │   ├── tenant-metrics.ts
│   │   │   │   │   └── system-metrics.ts
│   │   │   │   ├── dashboards/
│   │   │   │   │   ├── grafana-dashboards.json
│   │   │   │   │   └── prometheus-alerts.yml
│   │   │   │   └── health-checks/
│   │   │   │       └── health.controller.ts
│   │   │   ├── middleware/
│   │   │   │   ├── auth.middleware.ts
│   │   │   │   ├── tenant-context.middleware.ts
│   │   │   │   ├── rate-limit.middleware.ts
│   │   │   │   └── logging.middleware.ts
│   │   │   └── utils/
│   │   │       ├── logger.ts (Winston)
│   │   │       └── error-handler.ts
│   │   ├── tests/
│   │   │   ├── unit/
│   │   │   ├── integration/
│   │   │   └── e2e/
│   │   ├── package.json
│   │   ├── tsconfig.json
│   │   └── .env.example
│   ├── frontend/
│   │   ├── src/
│   │   │   ├── app/              # Next.js App Router
│   │   │   │   ├── (auth)/
│   │   │   │   │   ├── login/
│   │   │   │   │   ├── register/
│   │   │   │   │   └── forgot-password/
│   │   │   │   ├── (dashboard)/
│   │   │   │   │   ├── layout.tsx
│   │   │   │   │   ├── page.tsx
│   │   │   │   │   └── settings/
│   │   │   │   └── admin/         # Painel admin multi-tenant
│   │   │   │       ├── tenants/
│   │   │   │       ├── users/
│   │   │   │       ├── audit-logs/
│   │   │   │       ├── system-config/
│   │   │   │       └── analytics/
│   │   │   ├── components/
│   │   │   │   ├── ui/           # shadcn/ui components
│   │   │   │   ├── auth/
│   │   │   │   ├── dashboard/
│   │   │   │   └── admin/
│   │   │   ├── lib/
│   │   │   │   ├── trpc.ts       # tRPC client
│   │   │   │   └── utils.ts
│   │   │   └── styles/
│   │   │       └── globals.css
│   │   ├── public/
│   │   ├── package.json
│   │   ├── tsconfig.json
│   │   ├── tailwind.config.ts
│   │   └── next.config.js
│   └── shared/
│       ├── types/                # TypeScript types compartilhados
│       ├── schemas/              # Zod schemas compartilhados
│       └── constants/
├── infra/
│   ├── docker/
│   │   ├── Dockerfile.backend
│   │   ├── Dockerfile.frontend
│   │   └── .dockerignore
│   ├── docker-compose.dev.yml
│   ├── docker-compose.staging.yml
│   ├── docker-compose.prod.yml
│   ├── docker-compose.observability.yml
│   ├── grafana/
│   │   ├── dashboards/
│   │   │   ├── business-overview.json
│   │   │   └── tenant-specific.json
│   │   └── provisioning/
│   ├── prometheus/
│   │   ├── prometheus.yml
│   │   └── alerts.yml
│   └── nginx/
│       └── nginx.conf
├── .github/
│   └── workflows/
│       ├── ci.yml               # CI básico (lint + test)
│       ├── deploy-staging.yml
│       └── deploy-prod.yml
├── scripts/
│   ├── setup.sh                 # Script de setup inicial
│   ├── seed-database.sh
│   └── backup.sh
├── docs/
│   ├── ARCHITECTURE.md          # Arquitetura do sistema
│   ├── DEPLOYMENT.md            # Guia de deploy
│   ├── DEVELOPMENT.md           # Setup local
│   └── API.md                   # Documentação da API
├── .gitignore
├── .env.example
├── README.md
└── LICENSE
```

---

## 🔧 Stack Tecnológica

### Backend

```yaml
runtime: Node.js 20+
framework: Fastify
orm: Prisma
database: PostgreSQL 15+
queue: BullMQ (Redis)
cache: Redis
auth: JWT + Refresh Tokens
encryption: AES-256-CBC
observability:
  metrics: Prometheus
  visualization: Grafana
  errors: Sentry
  logs: Winston
security:
  rate_limiting: fastify-rate-limit
  csrf: @fastify/csrf-protection
  helmet: @fastify/helmet
```

### Frontend

```yaml
framework: Next.js 14+ (App Router)
library: React 18
styling: Tailwind CSS + shadcn/ui
api: tRPC + Zod
state: Zustand (se necessário)
forms: React Hook Form + Zod
```

### DevOps

```yaml
containerization: Docker + Docker Compose
ci_cd: GitHub Actions
monitoring: Grafana + Prometheus
reverse_proxy: Nginx (opcional)
```

---

## 🗄️ Database Schema Base (Multi-Tenant)

```prisma
// prisma/schema.prisma

generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

// ============================================
// MULTI-TENANCY
// ============================================

model Tenant {
  id          String   @id @default(uuid())
  name        String
  slug        String   @unique
  status      String   @default("active") // active, suspended, deleted
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt
  
  // Relations
  users       User[]
  auditLogs   AuditLog[]
  
  @@index([slug])
  @@index([status])
}

// ============================================
// AUTHENTICATION & AUTHORIZATION
// ============================================

model User {
  id                String   @id @default(uuid())
  tenantId          String
  email             String
  passwordHash      String
  name              String?
  role              String   @default("user") // admin, user
  status            String   @default("active") // active, inactive, suspended
  emailVerified     Boolean  @default(false)
  twoFactorEnabled  Boolean  @default(false)
  twoFactorSecret   String?  // Encrypted
  createdAt         DateTime @default(now())
  updatedAt         DateTime @updatedAt
  
  // Relations
  tenant            Tenant   @relation(fields: [tenantId], references: [id], onDelete: Cascade)
  refreshTokens     RefreshToken[]
  auditLogs         AuditLog[]
  
  @@unique([tenantId, email])
  @@index([tenantId])
  @@index([email])
  @@index([role])
}

model RefreshToken {
  id          String   @id @default(uuid())
  userId      String
  token       String   @unique
  expiresAt   DateTime
  createdAt   DateTime @default(now())
  revokedAt   DateTime?
  
  // Relations
  user        User     @relation(fields: [userId], references: [id], onDelete: Cascade)
  
  @@index([userId])
  @@index([token])
  @@index([expiresAt])
}

// ============================================
// AUDIT & LOGGING
// ============================================

model AuditLog {
  id          String   @id @default(uuid())
  tenantId    String
  userId      String?
  action      String   // CREATE, UPDATE, DELETE, LOGIN, LOGOUT
  entity      String   // User, Task, etc
  entityId    String?
  changes     Json?    // Before/After snapshot
  ip          String?
  userAgent   String?
  createdAt   DateTime @default(now())
  
  // Relations
  tenant      Tenant   @relation(fields: [tenantId], references: [id], onDelete: Cascade)
  user        User?    @relation(fields: [userId], references: [id], onDelete: SetNull)
  
  @@index([tenantId])
  @@index([userId])
  @@index([action])
  @@index([createdAt])
}

// ============================================
// SYSTEM CONFIGURATION
// ============================================

model SystemConfig {
  id          String   @id @default(uuid())
  key         String   @unique
  value       Json
  description String?
  updatedAt   DateTime @updatedAt
  
  @@index([key])
}
```

**RLS (Row Level Security):**
- Configurado no Prisma Middleware
- Todas as queries filtradas automaticamente por `tenantId`
- Exceto: Admin global (super admin)

---

## 🔐 Segurança

### Autenticação

```typescript
// JWT Access Token: 2 horas
// Refresh Token: 7 dias
// 2FA: TOTP (Google Authenticator)
// Encryption: AES-256-CBC para dados sensíveis
```

### Rate Limiting

```typescript
// Por IP: 100 req/min
// Por Tenant: 1000 req/min
// Login attempts: 5/15min
```

### CSRF Protection

```typescript
// Tokens CSRF em todos os forms
// SameSite cookies
```

### XSS Protection

```typescript
// Content Security Policy (CSP)
// Input sanitization (Zod + DOMPurify)
```

---

## 📊 Observabilidade

### Métricas Padrão

**Sistema (PERSONAL):**
- CPU, Memory, Disk usage
- HTTP request duration
- Database query duration
- Error rate

**Multi-Tenant (BUSINESS):**
- Todas as métricas acima POR TENANT
- Active users per tenant
- API calls per tenant
- Storage usage per tenant

### Dashboards Grafana

1. **System Overview** (sempre)
2. **Tenant Overview** (se business)
3. **Performance** (sempre)
4. **Errors & Alerts** (sempre)

---

## 🚀 Setup Inicial

### 1. Clonar Boilerplate

```bash
git clone https://github.com/bychrisr/kaven-boilerplate.git meu-projeto
cd meu-projeto
rm -rf .git
git init
```

### 2. Configurar Ambiente

```bash
cp .env.example .env
# Editar .env com suas credenciais

# Instalar dependências
cd production/backend && npm install
cd ../frontend && npm install
```

### 3. Subir Infraestrutura

```bash
cd infra/
docker compose -f docker-compose.dev.yml up -d
```

### 4. Migrar Database

```bash
cd production/backend
npx prisma migrate dev
npx prisma db seed
```

### 5. Iniciar Desenvolvimento

```bash
# Terminal 1 (Backend)
cd production/backend
npm run dev

# Terminal 2 (Frontend)
cd production/frontend
npm run dev
```

---

## 📋 Checklist de Implementação (Sprint 1-2)

### Sprint 1: Core Infrastructure (Semana 1)

- [ ] **Task B1:** Criar repositório GitHub
- [ ] **Task B2:** Estrutura de pastas completa
- [ ] **Task B3:** PostgreSQL + Prisma multi-tenant
  - [ ] Schema base (Tenant, User, RefreshToken, AuditLog)
  - [ ] Migrations
  - [ ] Seed script (1 admin, 2 tenants de teste)
- [ ] **Task B4 (Parte 1):** Backend auth (JWT + Refresh)
  - [ ] Login/Logout
  - [ ] JWT generation/validation
  - [ ] Refresh token rotation

### Sprint 2: Admin + Observability (Semana 2)

- [ ] **Task B4 (Parte 2):** Backend auth (2FA + Encryption)
  - [ ] 2FA TOTP
  - [ ] Encryption service (AES-256-CBC)
- [ ] **Task B4 (Parte 3):** Painel admin backend
  - [ ] User management (CRUD)
  - [ ] Tenant management (CRUD)
  - [ ] Audit logs (read-only)
- [ ] **Task B5:** Frontend admin UI
  - [ ] Login/Register pages
  - [ ] Dashboard layout (shadcn/ui)
  - [ ] Admin pages (users, tenants, logs)
- [ ] **Task B6:** Observabilidade
  - [ ] Prometheus metrics
  - [ ] Grafana dashboards
  - [ ] Health checks
- [ ] **Task B7:** Docker + CI
  - [ ] Dockerfiles
  - [ ] docker-compose (dev, staging, prod)
  - [ ] GitHub Actions (CI básico)

---

## 🎯 Acceptance Criteria

### Boilerplate está completo quando:

1. ✅ Repositório clonable (GitHub template)
2. ✅ `npm install` funciona (backend + frontend)
3. ✅ `docker compose up` funciona (dev environment)
4. ✅ Login/Logout funciona
5. ✅ Multi-tenancy funciona (RLS validado)
6. ✅ Painel admin funciona (CRUD users + tenants)
7. ✅ Observabilidade funciona (Grafana + Prometheus)
8. ✅ Testes passam (unit + integration básicos)
9. ✅ CI passa (lint + test + build)
10. ✅ Documentação completa (README + ARCHITECTURE + DEVELOPMENT)

---

## 📚 Documentação Obrigatória

Criar em `docs/`:

1. **README.md:** Overview + Quick Start
2. **ARCHITECTURE.md:** Diagrama de arquitetura + decisões técnicas
3. **DEVELOPMENT.md:** Setup local + workflows de desenvolvimento
4. **DEPLOYMENT.md:** Deploy staging + prod
5. **API.md:** Documentação da API (tRPC routers)

---

## 🚨 Riscos Identificados

| Risco | Probabilidade | Impacto | Mitigação |
|-------|---------------|---------|-----------|
| Complexidade alta do boilerplate | Alta | Alto | Começar versão mínima (auth + 1 tenant) |
| RLS difícil de configurar | Média | Alto | Usar Prisma middleware (mais simples que RLS nativo) |
| Observabilidade complexa | Média | Médio | Usar configs prontas (Grafana templates) |
| Tempo de implementação | Alta | Alto | Dividir em sprints menores (1-2 semanas cada) |

---

## 🔄 Próximos Passos

Após Boilerplate pronto:

1. Validar com projeto real (Todoist-like)
2. Iterar baseado em feedback
3. Publicar como template público (se open-source)
4. Documentar em kaven/README.md

---

## Changelog

### v1.0.0 (2024-12-03) - Especificação Inicial
- Estrutura de pastas completa
- Stack tecnológica definida
- Schema Prisma multi-tenant base
- Checklist de implementação (Sprint 1-2)
- Riscos identificados
- Acceptance criteria definidos
