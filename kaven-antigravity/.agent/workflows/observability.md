# Observability (Configuração Grafana + Prometheus)

> **Versão:** 1.0.0  
> **Data:** 2024-12-03  
> **Autor:** Chris + Claude Sonnet 4.5  
> **Status:** Production Ready  
> **Propósito:** Workflow Antigravity para ativar/configurar observabilidade (Grafana + Prometheus + métricas multi-tenant)

---

## 📋 Metadata

```yaml
workflow_id: observability
phase: production
step: 2.5 (opcional durante desenvolvimento)
input: PDR.md + Boilerplate existente
output: Dashboards Grafana configurados + Métricas ativas
estimated_time: 15-30 minutos
prerequisites: Backend rodando (npm run dev)
```

---

## 🎯 Objetivo

Ativar (se PERSONAL) ou configurar (se BUSINESS) o sistema de observabilidade já presente no Kaven Boilerplate.

**Observabilidade inclui:**
- **Grafana:** Dashboards visuais
- **Prometheus:** Coleta de métricas
- **Métricas multi-tenant:** Se business (por tenant)
- **Métricas padrão:** Se personal (sistema todo)

---

## 📝 Steps

### 1. Verificar Mode (PERSONAL vs BUSINESS)

Ler `PDR.seed.json` ou `PDR.md` para identificar `objective`:

```json
{
  "objective": "personal" | "business"
}
```

---

### 2. Ativar Observabilidade (se PERSONAL)

**Por que ativar?**
- No Kaven Boilerplate, observabilidade existe mas está **desativada** por padrão para PERSONAL (reduzir overhead)

**Passos:**

1. Editar `production/backend/.env`:
   ```bash
   OBSERVABILITY_ENABLED=true
   ```

2. Subir containers:
   ```bash
   cd infra/
   docker compose -f docker-compose.observability.yml up -d
   ```

3. Verificar Grafana acessível:
   ```
   http://localhost:3001
   Login: admin / admin
   ```

---

### 3. Configurar Métricas (se BUSINESS)

**Por que configurar?**
- BUSINESS = multi-tenant = métricas por tenant
- Precisamos configurar dashboards e alertas

**Passos:**

1. Verificar que observabilidade já está ativa (BUSINESS = sempre ativo)

2. Configurar métricas multi-tenant:
   ```typescript
   // production/backend/src/observability/metrics/tenant-metrics.ts
   
   export const tenantMetrics = {
     // Já existente no Boilerplate, apenas configurar:
     time_response_per_tenant: histogram({
       name: 'http_request_duration_ms_per_tenant',
       help: 'HTTP request duration by tenant',
       labelNames: ['tenant_id', 'method', 'route', 'status'],
     }),
     
     errors_per_tenant: counter({
       name: 'errors_total_per_tenant',
       help: 'Total errors by tenant',
       labelNames: ['tenant_id', 'error_type'],
     }),
     
     queue_length_per_tenant: gauge({
       name: 'queue_length_per_tenant',
       help: 'BullMQ queue length by tenant',
       labelNames: ['tenant_id', 'queue_name'],
     }),
     
     cpu_usage_per_tenant: gauge({
       name: 'cpu_usage_per_tenant',
       help: 'CPU usage by tenant',
       labelNames: ['tenant_id'],
     }),
   };
   ```

3. Importar dashboards Grafana:
   ```bash
   # Dashboards estão em infra/grafana/dashboards/
   # - business-overview.json (visão geral de todos os tenants)
   # - tenant-specific.json (drill-down por tenant)
   ```

4. Configurar alertas (Alertmanager):
   ```yaml
   # infra/prometheus/alerts.yml
   
   groups:
     - name: tenant_alerts
       rules:
         - alert: HighErrorRateTenant
           expr: rate(errors_total_per_tenant[5m]) > 0.05
           for: 5m
           labels:
             severity: warning
           annotations:
             summary: "High error rate for tenant {{ $labels.tenant_id }}"
   ```

---

### 4. Validar Métricas Funcionando

**Checklist:**

1. **Prometheus coletando:**
   ```
   http://localhost:9090/targets
   ```
   Status: UP para todos os targets

2. **Grafana dashboards:**
   ```
   http://localhost:3001/dashboards
   ```
   Ver dados aparecendo nos gráficos

3. **Métricas custom funcionando:**
   ```bash
   # Fazer request ao backend:
   curl http://localhost:3000/api/trpc/task.getAll
   
   # Verificar métrica incrementada:
   curl http://localhost:9090/api/v1/query?query=http_request_duration_ms_per_tenant
   ```

---

## 📊 Métricas Padrão Disponíveis

### Se PERSONAL:

```yaml
system_metrics:
  - cpu_usage_percent
  - memory_usage_mb
  - disk_usage_percent
  - http_request_duration_ms
  - http_requests_total
  - errors_total
  - database_query_duration_ms
  - database_connections_active
```

### Se BUSINESS (adiciona):

```yaml
tenant_metrics:
  - time_response_per_tenant
  - errors_per_tenant
  - queue_length_per_tenant
  - cpu_usage_per_tenant
  - active_users_per_tenant
  - api_calls_per_tenant
  - storage_usage_per_tenant
```

---

## 🚨 Troubleshooting

### Problema: Grafana não carrega dashboards

**Solução:**
```bash
docker compose -f docker-compose.observability.yml restart grafana
```

### Problema: Prometheus não coleta métricas

**Verificar:**
1. Backend expondo `/metrics` endpoint?
   ```bash
   curl http://localhost:3000/metrics
   ```
2. Prometheus config correto?
   ```bash
   cat infra/prometheus/prometheus.yml
   ```

### Problema: Métricas por tenant não aparecem

**Verificar:**
1. Middleware tenant-aware ativo?
   ```typescript
   // production/backend/src/middleware/tenant-context.ts
   export const tenantContextMiddleware = ...
   ```
2. Todas as requests têm `tenant_id` no contexto?

---

## 💾 Output

**Files Modified:**
- `production/backend/.env` (se PERSONAL, ativar)
- `infra/prometheus/alerts.yml` (se BUSINESS, configurar alertas)

**Containers Running:**
- Grafana (porta 3001)
- Prometheus (porta 9090)
- Alertmanager (porta 9093) - se BUSINESS

---

## 📂 File Locations

```
Input:  PDR.seed.json (verificar objective)
Modify: production/backend/.env
Modify: infra/prometheus/alerts.yml (se business)
Access: http://localhost:3001 (Grafana)
```

---

## 🔄 Next Steps

Após observabilidade ativa:

1. **Durante desenvolvimento:**
   - Monitorar dashboards enquanto codifica
   - Validar que métricas estão corretas

2. **Antes de deploy:**
   - Configurar alertas para produção
   - Documentar runbooks (o que fazer se alerta disparar)

---

## 📊 Success Metrics

- **Time:** < 30 minutos
- **Grafana acessível:** http://localhost:3001
- **Prometheus coletando:** Targets UP
- **Dashboards funcionando:** Dados aparecendo
- **Alertas configurados:** Se business

---

## Changelog

### v1.0.0 (2024-12-03) - Criação Inicial
- Workflow para ativar/configurar observabilidade
- Suporte PERSONAL (ativar) e BUSINESS (configurar)
- Métricas multi-tenant documentadas
- Troubleshooting incluído
