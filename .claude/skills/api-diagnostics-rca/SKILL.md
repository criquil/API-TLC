---
name: api-diagnostics-rca
description: "Usa esta skill para diagnosticar fallos en pruebas de API, identificar la causa raíz (RCA) y proponer acciones correctivas. Cubre: validación de contratos, error handling, timeout analysis y dependency mapping."
---

# API Diagnostics & RCA

## Objetivo
Diagnosticar fallos en pruebas de API y determinar la causa raíz con evidencia.

## Referencias
- `DOCs/09_Analisis_y_Bottlenecks_API/01_RCA_y_Troubleshooting_API.md`
- `DOCs/04_Metricas_y_KPIs_API/01_Metricas_API_Exhaustivas.md`

## Pre-ejecución

Antes de operar, leer:
```
Read("DOCs/09_Analisis_y_Bottlenecks_API/01_RCA_y_Troubleshooting_API.md")
Read("DOCs/04_Metricas_y_KPIs_API/01_Metricas_API_Exhaustivas.md")
```

## Flujo
1. Clasifica el fallo: contrato, HTTP, timeout, autenticación, datos, dependencia.
2. Recopila evidencia: logs, traces, request/response bodies, status codes.
3. Aplica técnicas RCA: 5 Whys, Fishbone, Fault Tree.
4. Identifica causa raíz con evidencia vinculada.
5. Propón acciones correctivas priorizadas (P1/P2/P3).

## Salida Esperada
- Clasificación del tipo de fallo.
- Evidencia recopilada (logs, traces, requests).
- Causa raíz identificada con técnica RCA aplicada.
- Acciones correctivas priorizadas (P1/P2/P3) con responsable estimado.
