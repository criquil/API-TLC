# Scripting Avanzado para API Testing

## Patrones de Scripts

### 1. Setup/Teardown
```csharp
// RestSharp
[SetUp]
public void Setup() {
    _client = new RestClient("https://api.example.com");
    _client.Authenticator = new JwtAuthenticator(GetToken());
}

[TearDown]
public void TearDown() {
    CleanupTestData();
}
```

### 2. Data-Driven Tests
```gherkin
# Karate
Scenario Outline: Validar usuario
  Given url baseUrl + '/api/users'
  And request { name: '<name>', email: '<email>' }
  When method post
  Then status 201

  Examples:
    | name    | email              |
    | Juan    | juan@test.com      |
    | María   | maria@test.com     |
```

### 3. Contract Validation
```typescript
// Playwright
test('validate user schema', async ({ request }) => {
  const response = await request.get('/api/users/1');
  expect(response.ok()).toBeTruthy();
  const body = await response.json();
  expect(body).toHaveProperty('id');
  expect(body).toHaveProperty('name');
  expect(typeof body.name).toBe('string');
});
```

### 4. Authentication Patterns
```java
// REST Assured
given()
  .auth().oauth2(getAccessToken())
  .when()
  .get("/api/users")
  .then()
  .statusCode(200);
```

## Mejores Prácticas

1. **Page Object Pattern** — Abstraer endpoints en clases
2. **Retry Logic** — Manejar transient failures
3. **Timeout Configuration** — Configurar timeouts apropiados
4. **Logging** — Log request/response para debugging
5. **Parallel Execution** — Ejecutar tests en paralelo cuando sea seguro
