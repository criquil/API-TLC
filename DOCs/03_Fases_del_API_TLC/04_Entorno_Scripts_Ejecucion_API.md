# Entorno, Scripts y Ejecución de API Testing

## Configuración del Entorno

### 1. Entornos Disponibles
```yaml
environments:
  development:
    url: "http://localhost:3000"
    database: "dev_db"
    auth: "debug_mode"
  
  staging:
    url: "https://staging-api.example.com"
    database: "staging_db"
    auth: "real_tokens"
  
  production:
    url: "https://api.example.com"
    database: "prod_db"
    auth: "real_tokens"
    read_only: true
```

### 2. Variables de Entorno
```bash
# .env file
API_BASE_URL=https://staging-api.example.com
API_KEY=your-api-key-here
AUTH_TOKEN=your-auth-token-here
DB_HOST=staging-db.example.com
DB_PORT=5432
DB_NAME=staging_db
```

### 3. Configuración de Herramientas

#### RestSharp (C#)
```csharp
// appsettings.json
{
  "ApiSettings": {
    "BaseUrl": "https://staging-api.example.com",
    "Timeout": 30000,
    "Headers": {
      "Accept": "application/json",
      "X-Api-Key": "${API_KEY}"
    }
  }
}
```

#### Karate
```javascript
// karate-config.js
function config() {
  var env = karate.env;
  var config = {
    baseUrl: env == 'staging' ? 'https://staging-api.example.com' : 'http://localhost:3000',
    apiKey: karate.properties['API_KEY'] || 'test-key',
  };
  return config;
}
```

#### Playwright
```typescript
// playwright.config.ts
import { defineConfig } from '@playwright/test';

export default defineConfig({
  testDir: './tests',
  baseURL: process.env.API_BASE_URL || 'http://localhost:3000',
  timeout: 30000,
  expect: {
    timeout: 5000,
  },
  use: {
    extraHTTPHeaders: {
      'Accept': 'application/json',
      'Authorization': `Bearer ${process.env.AUTH_TOKEN}`,
    },
  },
});
```

#### REST Assured
```java
// BaseTest.java
@BeforeAll
public static void setup() {
    RestAssured.baseURI = System.getenv("API_BASE_URL");
    RestAssured.basePath = "/api";
}
```

## Generación de Scripts

### Estructura de Proyecto
```
tests/api/
├── {tool}/
│   ├── scripts/
│   │   ├── functional/
│   │   ├── integration/
│   │   ├── negative/
│   │   └── contract/
│   ├── fixtures/
│   │   ├── users.json
│   │   └── orders.json
│   ├── config/
│   │   ├── environments.json
│   │   └── credentials.json
│   └── results/
│       ├── reports/
│       └── screenshots/
└── README.md
```

### Patrones de Scripts

#### RestSharp Example
```csharp
[TestFixture]
public class UserTests
{
    private RestClient _client;
    
    [SetUp]
    public void Setup()
    {
        _client = new RestClient(Config.BaseUrl);
        _client.AddDefaultHeader("Authorization", $"Bearer {Config.AuthToken}");
    }
    
    [Test]
    public async Task GetUser_ValidId_ReturnsOk()
    {
        var request = new RestRequest("/users/1", Method.Get);
        var response = await _client.ExecuteAsync<User>(request);
        
        Assert.That(response.StatusCode, Is.EqualTo(HttpStatusCode.OK));
        Assert.That(response.Data.Id, Is.EqualTo(1));
    }
}
```

#### Karate Example
```gherkin
Feature: User API

Background:
  * url baseUrl
  * def apiKey = karate.properties['API_KEY']

Scenario: Get user by ID
  Given path '/users/1'
  And header Authorization = 'Bearer ' + apiKey
  When method GET
  Then status 200
  And match response.id == 1
  And match response.name == '#string'
```

## Ejecución de Pruebas

### 1. Smoke Test
```bash
# Ejecutar smoke test primero
dotnet test --filter "Category=Smoke"  # RestSharp
mvn test -Dtest=SmokeTest              # REST Assured
npx playwright test --grep "smoke"      # Playwright
```

### 2. Suite Completa
```bash
# Ejecución completa
dotnet test                             # RestSharp
mvn test                                # REST Assured
npx playwright test                     # Playwright
mvn test -Dfeatures=classpath:features  # Karate
```

### 3. Ejecución en CI/CD
```yaml
# .github/workflows/api-tests.yml
name: API Tests
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup .NET
        uses: actions/setup-dotnet@v3
        with:
          dotnet-version: '7.0.x'
      - name: Run Tests
        run: dotnet test --logger "trx;LogFileName=test-results.trx"
      - name: Publish Results
        uses: dorny/test-reporter@v1
        if: always()
        with:
          name: Test Results
          path: '**/*.trx'
          reporter: dotnet-trx
```

## Captura de Resultados

### Output esperado
```json
{
  "execution_id": "exec_123",
  "start_time": "2024-01-15T10:00:00Z",
  "end_time": "2024-01-15T10:15:00Z",
  "duration_seconds": 900,
  "tests_total": 50,
  "tests_passed": 48,
  "tests_failed": 2,
  "tests_skipped": 0,
  "pass_rate": 96.0,
  "results_file": "results/api-test-report.html"
}
```

## Troubleshooting

### Errores Comunes
| Error | Causa | Solución |
|-------|-------|----------|
| 401 Unauthorized | Token expirado | Renovar token |
| 403 Forbidden | Sin permisos | Verificar roles |
| Connection refused | Entorno caído | Verificar servicios |
| Timeout | API lenta | Aumentar timeout |
