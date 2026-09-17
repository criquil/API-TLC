---
name: playwright-api-workflow
description: Usa esta skill para diseñar, generar y revisar pruebas de API con Playwright (APITesting), incluyendo request context, fixtures, assertions y test generation.
---

# Playwright API Workflow

## Referencias
- [DOCs/05_Herramientas_API/03_Playwright_API_Guia_Completa.md](../../../DOCs/05_Herramientas_API/03_Playwright_API_Guia_Completa.md)
- [DOCs/04_Metricas_y_KPIs_API/01_Metricas_API_Exhaustivas.md](../../../DOCs/04_Metricas_y_KPIs_API/01_Metricas_API_Exhaustivas.md)

## Flujo
1. Define el alcance: endpoints, métodos, body fixtures y dependencias.
2. Configura playwright.config.ts con baseURL, timeouts y retries.
3. Escribe tests usando `request.newContext()` y fixtures reutilizables.
4. Ejecuta con `npx playwright test` y revisa el HTML report.
5. Resume hallazgos: cobertura, fallos, tiempos de respuesta y contract validation.
