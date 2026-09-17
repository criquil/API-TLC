---
name: rest-assured-api-workflow
description: Usa esta skill para diseñar, generar y revisar pruebas de API con REST Assured, incluyendo Given/When/Then, matchers, filters y reportes Allure.
---

# REST Assured API Workflow

## Referencias
- [DOCs/05_Herramientas_API/04_REST_Assured_Guia_Completa.md](../../../DOCs/05_Herramientas_API/04_REST_Assured_Guia_Completa.md)
- [DOCs/04_Metricas_y_KPIs_API/01_Metricas_API_Exhaustivas.md](../../../DOCs/04_Metricas_y_KPIs_API/01_Metricas_API_Exhaustivas.md)

## Flujo
1. Define el alcance: endpoints, métodos, body y dependencias.
2. Configura el proyecto Maven/Gradle con REST Assured y Allure.
3. Escribe tests con Given/When/Then, validación de schema y matchers.
4. Ejecuta con `mvn test` y revisa el reporte Allure.
5. Resume hallazgos: cobertura, fallos, tiempos de respuesta y contract validation.
