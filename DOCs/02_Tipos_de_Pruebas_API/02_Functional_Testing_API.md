# Functional Testing para APIs

## Definición
Validación de que cada endpoint retorna el resultado esperado para inputs válidos, cubriendo operaciones CRUD y flujos de negocio.

## Tipos de Functional Testing

### 1. Positive Testing
Valida que la API funciona correctamente con datos válidos.

**Ejemplo:**
```
GET /api/users/1 → 200 OK + body con user
POST /api/users → 201 Created + user creado
PUT /api/users/1 → 200 OK + user actualizado
DELETE /api/users/1 → 204 No Content
```

### 2. CRUD Operations
- **Create**: POST con datos válidos → 201 Created
- **Read**: GET existente → 200 OK + body
- **Update**: PUT existente → 200 OK + body actualizado
- **Delete**: DELETE existente → 204 No Content

### 3. Query Parameters
```
GET /api/users?status=active&page=1&limit=10
→ 200 OK + lista paginada
```

### 4. Path Parameters
```
GET /api/users/123
→ 200 OK + user con id=123
```

### 5. Request Headers
```
GET /api/users
Headers:
  Authorization: Bearer <token>
  Accept: application/json
  X-Request-ID: <uuid>
→ 200 OK
```

### 6. Request Body
```json
POST /api/users
{
  "name": "John Doe",
  "email": "john@example.com",
  "role": "admin"
}
→ 201 Created
```

## Scenarios Comunes

### Flujo Completo de Usuario
1. Registrar usuario → 201 Created
2. Login → 200 OK + token
3. Obtener perfil → 200 OK
4. Actualizar perfil → 200 OK
5. Logout → 200 OK

### Paginación
```
GET /api/users?page=1&limit=10 → 10 users
GET /api/users?page=2&limit=10 → 10 users
GET /api/users?page=3&limit=10 → 5 users (última página)
```

### Filtros y Búsqueda
```
GET /api/users?search=john
GET /api/users?status=active&role=admin
GET /api/users?created_after=2024-01-01
```

## Assertions Comunes

### Status Codes
- 200: OK
- 201: Created
- 204: No Content
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 422: Unprocessable Entity

### Response Body
```json
{
  "id": 1,
  "name": "John Doe",
  "email": "john@example.com",
  "createdAt": "2024-01-15T10:30:00Z"
}
```

### Response Headers
```
Content-Type: application/json
X-Total-Count: 100
X-Page-Count: 10
```

## Herramientas para Functional Testing

| Herramienta | Lenguaje | Ventajas |
|-------------|----------|----------|
| RestSharp | C# | Integración .NET, sintaxis fluida |
| Karate | Gherkin | BDD, multi-protocolo |
| Playwright | JS/TS | API + Browser testing |
| REST Assured | Java | Given/When/Then, Allure |

## Best Practices

1. **Test data isolation**: Cada test con datos independientes
2. **Cleanup automático**: Eliminar datos creados durante tests
3. **Assertions específicas**: Verificar status, body, headers
4. **Edge cases**: Probar límites, caracteres especiales
5. **Documentación**: Mantener casos de prueba actualizados

## Ejemplo con RestSharp

```csharp
[Test]
public async Task GetUser_ReturnsOk()
{
    // Arrange
    var client = new RestClient("https://api.example.com");
    var request = new RestRequest("/users/1", Method.GET);
    request.AddHeader("Authorization", "Bearer token");

    // Act
    var response = await client.GetAsync<User>(request);

    // Assert
    Assert.That(response, Is.Not.Null);
    Assert.That(response.Id, Is.EqualTo(1));
    Assert.That(response.Name, Is.Not.Empty);
}
```

## Ejemplo con Karate

```gherkin
Feature: User CRUD

  Scenario: Create and get user
    Given url 'https://api.example.com'
    And path '/users'
    And request { name: 'Test User', email: 'test@example.com' }
    When method POST
    Then status 201
    And def userId = response.id

    Given path '/users/' + userId
    When method GET
    Then status 200
    And match response.name == 'Test User'
```

## Referencias
- [DOCs/02_Tipos_de_Pruebas_API/01_Tipos_de_Pruebas_API.md](01_Tipos_de_Pruebas_API.md)
- [ISTQB Foundation Level - Functional Testing](https://www.istqb.org/)