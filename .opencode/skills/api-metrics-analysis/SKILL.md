---
name: api-metrics-analysis
description: Usa esta skill para analizar métricas de pruebas de API: tiempos de respuesta, throughput, error rates, SLA compliance y contract validation.
---

# API Metrics Analysis

## Objetivo
Analizar métricas de ejecuciones de API testing y generar insights accionables.

## Referencias
- [DOCs/04_Metricas_y_KPIs_API/01_Metricas_API_Exhaustivas.md](../../../DOCs/04_Metricas_y_KPIs_API/01_Metricas_API_Exhaustivas.md)
- [DOCs/02_Tipos_de_Pruebas_API/01_Tipos_de_Pruebas_API.md](../../../DOCs/02_Tipos_de_Pruebas_API/01_Tipos_de_Pruebas_API.md)

## Flujo
1. Recopila métricas: response time (p50/p95/p99), throughput (req/s), error rate, SLA compliance.
2. Compara contra criterios de aceptación definidos.
3. Identifica outliers y tendencias.
4. Genera visualizaciones si es posible (tablas, gráficos ASCII).
5. Produce resumen ejecutivo con verdicto: PASSED / CONDITIONAL / FAILED.

## Salida Esperada
- Tabla de métricas vs criterios de aceptación.
- Outliers y tendencias identificados.
- Veredicto: PASSED / CONDITIONAL / FAILED.
- Recomendaciones priorizadas para mejora.
