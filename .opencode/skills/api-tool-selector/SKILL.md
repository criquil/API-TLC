---
name: api-tool-selector
description: Usa esta skill cuando necesites elegir entre RestSharp, Karate Framework, Playwright o REST Assured para un caso de API testing. Aplica criterios de stack tecnológico, tipo de prueba, CI/CD y curva de aprendizaje.
---

# API Tool Selector

## Objetivo
Elegir la herramienta adecuada para un escenario de pruebas de API y justificar la decisión con criterios técnicos.

## Referencias
- [DOCs/05_Herramientas_API/00_Comparativa_Herramientas_API.md](../../../DOCs/05_Herramientas_API/00_Comparativa_Herramientas_API.md)
- [DOCs/02_Tipos_de_Pruebas_API/](../../../DOCs/02_Tipos_de_Pruebas_API/)

## Flujo
1. Identifica el objetivo principal: functional, integration, contract, E2E, BDD.
2. Confirma stack tecnológico y restricciones (.NET, Java, JavaScript, multi-lenguaje).
3. Evalúa restricciones del equipo: lenguaje, curva de aprendizaje, integración CI/CD.
4. Compara opciones con una matriz corta (pros, contras, riesgos).
5. Devuelve recomendación final y alternativa de respaldo.

## Salida Esperada
- Herramienta recomendada.
- Razones técnicas en bullets.
- Riesgos de implementación.
- Siguiente documento a leer en `DOCs/05_Herramientas_API/`.
