---
name: karate-api-workflow
description: Usa esta skill para diseñar, generar y revisar pruebas de API con Karate Framework, incluyendo feature files, scenario outlines, call ods, matchers y reportes HTML.
---

# Karate API Workflow

## Referencias
- [DOCs/05_Herramientas_API/02_Karate_Guia_Completa.md](../../../DOCs/05_Herramientas_API/02_Karate_Guia_Completa.md)
- [DOCs/04_Metricas_y_KPIs_API/01_Metricas_API_Exhaustivas.md](../../../DOCs/04_Metricas_y_KPIs_API/01_Metricas_API_Exhaustivas.md)

## Flujo
1. Define el alcance: endpoints, métodos, datos de entrada y dependencias entre escenarios.
2. Escribe feature files Gherkin conScenario Outline y Examples para datos parametrizados.
3. Configura karate-config.js para entornos, variables globales y hooks.
4. Ejecuta con `mvn test` o `karate -Denv=...` y revisa el reporte HTML.
5. Resume hallazgos: cobertura, fallos, tiempos de respuesta y contract validation.
