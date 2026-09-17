---
name: restsharp-api-workflow
description: "Usa esta skill para diseñar, generar y revisar pruebas de API con RestSharp, incluyendo configuración de cliente, autenticación, manejo de tokens, assertions y manejo de errores HTTP."
---

# RestSharp API Workflow

## Referencias
- `DOCs/05_Herramientas_API/01_RestSharp_Guia_Completa.md`
- `DOCs/04_Metricas_y_KPIs_API/01_Metricas_API_Exhaustivas.md`

## Pre-ejecución

Antes de operar, leer:
```
Read("DOCs/05_Herramientas_API/01_RestSharp_Guia_Completa.md")
Read("DOCs/04_Metricas_y_KPIs_API/01_Metricas_API_Exhaustivas.md")
```

## Flujo
1. Define el alcance de la prueba: endpoints, métodos HTTP, datos de entrada.
2. Configura el cliente RestSharp con base URL, headers y autenticación.
3. Diseña los escenarios de prueba: happy path, edge cases, error handling.
4. Ejecuta las pruebas y valida status codes, response bodies y tiempos de respuesta.
5. Resume hallazgos y genera reporte con cobertura de endpoints.
