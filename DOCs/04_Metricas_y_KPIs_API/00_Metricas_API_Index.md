# Métricas y KPIs de API Testing

> **Rol de este archivo:** Índice del directorio de métricas y KPIs para API Testing.
> **Cuándo leer este archivo:** Cuando necesitas definir métricas, KPIs o criterios de aceptación para pruebas de API.

---

## Contenido del Directorio

| Archivo | Descripción |
|---------|-------------|
| [01_Metricas_API_Exhaustivas.md](01_Metricas_API_Exhaustivas.md) | Catálogo completo de métricas de API testing: cobertura, calidad, performance, contract, security |

---

## Resumen de Métricas Principales

### Métricas de Cobertura
- Endpoint Coverage: >= 80%
- Scenario Coverage: >= 90%
- Line Coverage: >= 70%

### Métricas de Calidad
- Pass Rate: >= 95%
- Defect Density: <= 5 per kLOC
- Escape Rate: <= 10%

### Métricas de Performance
- Response Time P95: <= 200ms
- Throughput: >= 100 req/s
- Error Rate: <= 1%

### Métricas de Contract
- Contract Compliance: 100%
- Breaking Change Detection: 100%
- Schema Validation: >= 99%

---

## Referencias Rápidas

### Para definir NFRs
Usar las métricas de `01_Metricas_API_Exhaustivas.md` como base para definir NFRs numéricos.

### Para análisis de resultados
Consultar `DOCs/09_Analisis_y_Bottlenecks_API/` para técnicas de RCA.

### Para dashboards
Usar métricas para configurar Grafana, Datadog u otras herramientas de monitoreo.

---

*Documento de referencia - API Test Life Cycle*
*Última actualización: Junio 2026*
