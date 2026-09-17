# Frameworks y Estándares en API Testing

## Visión General

El API Testing se apoya en múltiples frameworks, estándares y mejores prácticas que garantizan la calidad, consistencia y automatización de las pruebas de APIs. Este documento describe los principales frameworks, estándares y herramientas utilizados en la industria.

---

## Estándares Internacionales

### ISTQB (International Software Testing Qualifications Board)

#### Fundamentos aplicados a API Testing
- **Niveles de testing**: Unit → Integration → System → Acceptance
- **Tipos de testing**: Functional, Non-functional, Regression, Maintenance
- **Proceso de testing**: Plan → Design → Implementation → Execution → Closure

#### Certificaciones relevantes
| Certificación | Enfoque | Relevancia para API Testing |
|--------------|---------|----------------------------|
| ISTQB Foundation Level | Fundamentos generales | Alta |
| ISTQB Advanced Level - Technical Test Analyst | Aspectos técnicos | Muy Alta |
| ISTQB Advanced Level - Test Analyst | Diseño de pruebas | Alta |

### IEEE 829 - Standard for Software Test Documentation

#### Documentos estándar aplicables
1. **Test Plan** — Alcance, recursos, cronograma
2. **Test Design** — Criterios de diseño de pruebas
3. **Test Case** — Especificaciones de casos de prueba
4. **Test Procedure** — Pasos de ejecución
5. **Test Report** — Resultados y análisis

#### Plantilla para API Test Plan
```markdown
# API Test Plan

## 1. Introducción
- Objetivo del testing
- Alcance de las APIs a probar
- Stakeholders

## 2. Estrategia de Testing
- Tipos de testing aplicables
- Herramientas a utilizar
- Criterios de aceptación

## 3. Diseño de Pruebas
- Matriz de cobertura
- Casos de prueba por endpoint
- Datos de prueba

## 4. Ejecución
- Cronograma
- Responsabilidades
- Entorno de testing

## 5. Reporte
- Métricas a recolectar
- Formato de reportes
- Proceso de comunicación
```

---

## Estándares de Contratos API

### OpenAPI Specification (Swagger)

#### Versión actual: 3.0.3
```yaml
openapi: 3.0.3
info:
  title: API de Ejemplo
  version: 1.0.0
paths:
  /users:
    get:
      summary: Obtener lista de usuarios
      responses:
        '200':
          description: Éxito
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/User'
```

#### Uso en API Testing
- **Contract Validation**: Verificar que la API cumple con el contrato
- **Test Generation**: Generar casos de prueba automáticamente
- **Documentation**: Generar documentación interactiva
- **Mock Servers**: Crear servidores mock basados en el contrato

### JSON Schema

#### Validación de estructura de datos
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "properties": {
    "id": { "type": "integer" },
    "name": { "type": "string" },
    "email": { "type": "string", "format": "email" }
  },
  "required": ["id", "name", "email"]
}
```

#### Uso en API Testing
- **Response Validation**: Validar estructura de respuestas
- **Request Validation**: Validar estructura de requests
- **Test Data Generation**: Generar datos de prueba válidos
- **Contract Testing**: Verificar compliance de contratos

### AsyncAPI

#### Para APIs event-driven
```yaml
asyncapi: 2.0.0
info:
  title: Event API
  version: 1.0.0
channels:
  userCreated:
    publish:
      message:
        payload:
          type: object
          properties:
            userId:
              type: string
```

---

## Frameworks de Testing

### Para API Testing Funcional

#### RestSharp (.NET/C#)
- **Tipo**: Library
- **Lenguaje**: C#
- **Uso principal**: APIs REST en ecosistema .NET
- **Ventajas**: Integración nativa con .NET, syntax clean

#### Karate Framework
- **Tipo**: Framework BDD
- **Lenguaje**: Gherkin (multi-lenguaje)
- **Uso principal**: APIs REST, GraphQL, gRPC, SOAP
- **Ventajas**: BDD nativo, reporting HTML, multi-protocolo

#### Playwright (API Testing)
- **Tipo**: Framework
- **Lenguaje**: JavaScript/TypeScript
- **Uso principal**: APIs REST con browser context
- **Ventajas**: Fixtures, parallel execution, tracing

#### REST Assured (Java)
- **Tipo**: Library
- **Lenguaje**: Java
- **Uso principal**: APIs REST en ecosistema Java
- **Ventajas**: Given/When/Then syntax, Allure reports

### Para Contract Testing

#### Pact
- **Enfoque**: Consumer-driven contracts
- **Ventajas**: Desacoplamiento entre equipos
- **Uso**: Validar contratos entre servicios

#### Spring Cloud Contract
- **Enfoque**: Producer-driven contracts
- **Ventajas**: Integración nativa con Spring
- **Uso**: Microservices en ecosistema Java

### Para API Performance Testing

#### k6
- **Enfoque**: Performance testing con scripts JS
- **Ventajas**: CI-first, threshold-based pass/fail

#### JMeter
- **Enfoque**: Performance testing multi-protocolo
- **Ventajas**: GUI, plugins, múltiples protocolos

---

## Mejores Prácticas

### 1. Test Pyramid para APIs

```
           ┌─────────────┐
           │   E2E API   │  ← Pocos, lentos, costosos
           │   Tests     │
           ├─────────────┤
           │ Integration │  ← Moderados, prueban contratos
           │   Tests     │
           ├─────────────┤
           │   Unit      │  ← Muchos, rápidos, baratos
           │   Tests     │
           └─────────────┘
```

### 2. Contract-First Development

1. Definir contrato (OpenAPI) primero
2. Generar código del contrato
3. Desarrollar implementación
4. Ejecutar contract tests
5. Validar compliance

### 3. Test Data Management

- **Fixtures**: Datos predefinidos y repetibles
- **Factories**: Generación dinámica de datos
- **Seeds**: Datos iniciales para entornos
- **Mocks**: Simulación de dependencias externas

### 4. CI/CD Integration

```yaml
# Ejemplo: GitHub Actions
- name: Run API Tests
  run: |
    npm run test:api
    npm run test:contract
    npm run test:security
  env:
    API_BASE_URL: ${{ secrets.API_BASE_URL }}
```

---

## Métricas de Calidad

### Cobertura de Testing

| Métrica | Fórmula | Meta |
|---------|---------|------|
| Endpoint Coverage | (Endpoints testeados / Total endpoints) × 100 | ≥ 90% |
| Method Coverage | (Métodos HTTP testeados / Total métodos) × 100 | ≥ 85% |
| Code Coverage | (Líneas cubiertas / Total líneas) × 100 | ≥ 80% |

### Calidad de Pruebas

| Métrica | Fórmula | Meta |
|---------|---------|------|
| Pass Rate | (Tests exitosos / Total tests) × 100 | ≥ 95% |
| Defect Detection Rate | (Bugs encontrados / Total bugs) × 100 | ≥ 90% |
| False Positive Rate | (Falsos positivos / Total positivos) × 100 | ≤ 5% |

---

## Herramientas de Soporte

### Documentación
- **Swagger UI**: Interfaz interactiva para APIs
- **Redoc**: Documentación OpenAPI elegante
- **Stoplight**: Design y documentation平台

### Mocking
- **WireMock**: Mock server para APIs
- **MockServer**: Mock de APIs y servicios
- **Postman Mock Servers**: Mocks en la nube

### Monitoring
- **Postman Monitor**: Monitoreo de APIs
- **Runscope**: API monitoring y testing
- **Pingdom**: Uptime y performance monitoring

---

## Referencias

1. ISTQB Foundation Level Syllabus 2018
2. IEEE 829-2008 Standard
3. OpenAPI Specification 3.0.3
4. JSON Schema Draft 07
5. AsyncAPI Specification 2.0.0
6. Martin Fowler - Test Pyramid
7. Contract Testing Best Practices (Pact.io)

---

*Documento de referencia - API Test Life Cycle (API-TLC v1.0)*
*Última actualización: Septiembre 2026*