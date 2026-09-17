# API Testing: Definición y Fundamentos

## ¿Qué es API Testing?

API Testing es la validación programática de Application Programming Interfaces (APIs) para verificar que cumplen con sus contratos funcionales, manejan correctamente errores, y operan de manera segura bajo diferentes condiciones.

## Diferencias con Performance Testing

| Aspecto | API Testing | Performance Testing |
|---------|-------------|---------------------|
| **Objetivo** | Funcionalidad correcta | Comportamiento bajo carga |
| **Métricas** | Status codes, schemas, business logic | Response times, throughput, error rates |
| **Herramientas** | RestSharp, Karate, Playwright, REST Assured | k6, JMeter, Gatling, Locust |
| **Enfoque** | Correctitud y contract compliance | Escalabilidad y estabilidad |

## Tipos de API Testing

1. **Functional Testing** — Validación de comportamiento esperado
2. **Integration Testing** — Interacción entre servicios
3. **Contract Testing** — Validación de contratos (OpenAPI, Swagger)
4. **Negative Testing** — Manejo de errores y casos inválidos
5. **Boundary Testing** — Límites de entrada y edge cases
6. **Security Testing** — Autenticación, autorización, inyección
7. **Regression Testing** — Validación de cambios no rompan funcionalidad existente

## Estándares y Frameworks

- **ISTQB** — International Software Testing Qualifications Board
- **IEEE 829** — Standard for Software Test Documentation
- **OpenAPI/Swagger** — Contract specification
- **JSON Schema** — Schema validation

## Referencias

- ISTQB Foundation Level Syllabus
- IEEE 829-2008 Standard
- OpenAPI Specification 3.0
