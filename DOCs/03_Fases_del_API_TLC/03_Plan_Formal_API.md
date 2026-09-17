## Plan Formal de Pruebas de API (IEEE-829/ISTQB)

### Estructura del Documento

1. **Introducción**
   - Alcance del plan
   - Objetivos de testing
   - Referencias (OpenAPI, requisitos)

2. **Componentes a Probar**
   - Endpoints listados
   - Métodos HTTP soportados
   - Protocolos (REST, GraphQL, gRPC)

3. **Funciones a Probar**
   - Functional: CRUD por endpoint
   - Integration: Flujos de negocio
   - Contract: Validación de schema
   - Negative: Manejo de errores
   - Boundary: Límites de entrada

4. **Funciones NO a Probar**
   - Endpoints excluidos y razón
   - Tipos de prueba no aplicables

5. **Criterios de Prueba**
   - Entrada: API desplegada, datos disponibles
   - Salida: 100% tests ejecutados, coverage ≥ 80%
   - Suspendida: Smoke test falla
   - Reanudación: Después de fix

6. **Plan de Pruebas**
   - Cronograma por tipo
   - Dependencias
   - Entregables

7. **Requerimientos**
   - Ambiente: staging, dev
   - Herramienta: RestSharp/Karate/Playwright/REST Assured
   - Personal: roles y responsabilidades

8. **Riesgos y Mitigación**
   - Riesgos identificados
   - Acciones mitigadoras

9. **Aprobaciones**
   - Firmas y fechas