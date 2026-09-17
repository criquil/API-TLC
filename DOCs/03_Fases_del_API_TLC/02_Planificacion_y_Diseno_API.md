# Planificación y Diseño de Pruebas de API

## Fase de Planificación

### 1. Definición del Alcance
- Endpoints a probar (CRUD completo)
- Flujos de negocio críticos
- Dependencias entre servicios
- Restricciones de tiempo y recursos

### 2. Selección de Tipos de Prueba
- Functional Testing (happy path)
- Integration Testing (flujos multi-endpoint)
- Contract Testing (validación OpenAPI/Swagger)
- Negative Testing (errores y edge cases)
- Boundary Testing (límites de entrada)
- Security Testing (autenticación, autorización)

### 3. Diseño de Escenarios
Para cada endpoint:
- Happy path (200/201/204)
- Edge cases (vacíos, nulos, límites)
- Error cases (400, 401, 403, 404, 500)
- Contract validation (schema match)

### 4. Estrategia de Datos
- Fixtures estáticos (JSON)
- Factories (generadores)
- Seeds (inicialización de DB)
- Mock data (Faker, Mockaroo)

### 5. Orden de Ejecución
1. Smoke test
2. Functional tests
3. Contract tests
4. Integration tests
5. Negative tests
6. Boundary tests
7. Security tests

## Entregables
- Procedure plan documentado
- Matrix de cobertura endpoint × tipo
- Estimación de esfuerzo
- Estrategia de datos definida