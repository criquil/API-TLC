# Tipos de Pruebas de API

## 1. Functional Testing
Valida que cada endpoint retorna el resultado esperado para inputs válidos.

**Ejemplo:**
```
GET /api/users/1 → 200 OK + body con user
POST /api/users → 201 Created + user creado
PUT /api/users/1 → 200 OK + user actualizado
DELETE /api/users/1 → 204 No Content
```

## 2. Integration Testing
Valida la interacción entre múltiples servicios o módulos.

**Escenarios:**
- Flujo completo: crear order → procesar pago → enviar notificación
- Dependencias: servicio A llama a servicio B
- Transacciones distribuidas

## 3. Contract Testing
Valida que la API cumple con su contrato (OpenAPI, Swagger, AsyncAPI).

**Herramientas:**
- Pact (consumer-driven contracts)
- Dredd (OpenAPI validation)
- Schemathesis (fuzz testing de schemas)

## 4. Negative Testing
Valida el manejo correcto de errores y inputs inválidos.

**Casos:**
- Status codes incorrectos (400, 401, 403, 404, 500)
- Body malformado
- Campos faltantes o con tipos incorrectos
- Rate limiting

## 5. Boundary Testing
Valida límites de entrada y edge cases.

**Casos:**
- Strings vacíos, máximo de caracteres
- Números en límites (0, -1, MAX_INT)
- Arrays vacíos, con un elemento, con máximo de elementos
- Fechas límite (epoch, futuro lejano)

## 6. Security Testing
Valida mecanismos de seguridad de la API.

**Casos:**
- Autenticación (JWT, OAuth, API keys)
- Autorización (roles, permisos)
- Inyección (SQL, NoSQL, command injection)
- OWASP API Security Top 10

## 7. Regression Testing
Valida que cambios no rompan funcionalidad existente.

**Estrategia:**
- Suite de regresión automática
- Ejecución en CI/CD
- Comparación de contratos previos vs actuales
