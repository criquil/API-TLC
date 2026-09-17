# Karate Framework - Guía Completa para API Testing

## Visión General

**Karate** es un framework de API testing basado en Gherkin que permite escribir pruebas en un lenguaje simple y legible. Soporta HTTP, gRPC, GraphQL, WebSocket y UI testing en una sola herramienta.

---

## Características Principales

| Característica | Descripción |
|----------------|-------------|
| **Lenguaje** | Gherkin (BDD) |
| **Tipo** | Framework completo |
| **HTTP** | Full support |
| **gRPC** | Soportado |
| **GraphQL** | Soportado |
| **WebSocket** | Soportado |
| **BDD** | Nativo |
| **Contract** | Nativo |
| **Reportes** | HTML nativo |
| **CI/CD** | Maven/Gradle |

---

## Instalación

### Maven
```xml
<dependency>
    <groupId>com.intuit.karate</groupId>
    <artifactId>karate-junit5</artifactId>
    <version>1.4.1</version>
    <scope>test</scope>
</dependency>
```

### Gradle
```groovy
testImplementation 'com.intuit.karate:karate-junit5:1.4.1'
```

### Proyecto de Ejemplo
```
src/test/java/
├── examples/
│   ├── users.feature
│   └── orders.feature
├── runners/
│   └── TestRunner.java
└── karate-config.js
```

---

## Feature Files

### Estructura Básica
```gherkin
Feature: User API

  Background:
    * url baseUrl
    * def token = call read('classpath:auth.feature')
    * header Authorization = 'Bearer ' + token

  Scenario: Get user by ID
    Given path '/users/1'
    When method GET
    Then status 200
    And match response.name == 'John'

  Scenario: Create new user
    Given path '/users'
    And request { name: 'Jane', email: 'jane@test.com' }
    When method POST
    Then status 201
    And match response.id == '#number'
```

### Scenario Outline (Data-Driven)
```gherkin
Scenario Outline: Validate user creation with different data
  Given path '/users'
  And request { name: '<name>', email: '<email>', age: <age> }
  When method POST
  Then status <status>

  Examples:
    | name  | email           | age | status |
    | John  | john@test.com   | 25  | 201    |
    |       | invalid-email   | 25  | 400    |
    | Jane  | jane@test.com   | -1  | 400    |
```

### Background (Setup)
```gherkin
Feature: Protected API

  Background:
    * url baseUrl
    * def authResponse = call read('classpath:auth.feature')
    * def token = authResponse.token
    * header Authorization = 'Bearer ' + token
```

---

## Parámetros y Variables

### URL Segments
```gherkin
Given path '/users', userId
When method GET
```

### Query Parameters
```gherkin
Given path '/users'
And params { page: 1, limit: 10 }
When method GET
```

### Headers
```gherkin
Given header Accept = 'application/json'
And header X-Custom-Header = 'value'
When method GET
```

### Request Body
```gherkin
# JSON
Given request { name: 'John', email: 'john@test.com' }

# From file
Given request read('user.json')

# From variable
* def userData = { name: 'John', email: 'john@test.com' }
Given request userData
```

---

## Assert y Validación

### Response Status
```gherkin
Then status 200
Then status 404
```

### Response Body
```gherkin
And match response.name == 'John'
And match response.age == 25
And match response.email == '#regex .+@.+'
```

### Response Array
```gherkin
And match response == '#array'
And match response[0].name == 'John'
And match response.length == 10
```

### Schema Validation
```gherkin
And match response ==
  """
  {
    id: '#number',
    name: '#string',
    email: '#string',
    age: '#number'
  }
  """
```

### Negative Assertions
```gherkin
And match response.error == '#notnull'
And match response.message == '#contains invalid'
```

---

## Variables y Defs

### Definir Variables
```gherkin
* def userId = 1
* def userName = 'John'
* def user = { name: 'John', email: 'john@test.com' }
```

### Capturar de Respuesta
```gherkin
When method POST
Then def newUserId = response.id
And def newUserName = response.name
```

### Uso de Variables
```gherkin
Given path '/users', newUserId
When method GET
Then match response.name == newUserName
```

---

## Call y Reutilización

### Call Feature
```gherkin
# auth.feature
Feature: Authentication

  Scenario: Get token
    Given url baseUrl
    And path '/auth/login'
    And request { username: 'admin', password: 'password' }
    When method POST
    Then status 200
    And def token = response.token
```

### Uso en Otro Feature
```gherkin
Feature: Protected API

  Scenario: Get protected resource
    * def auth = call read('classpath:auth.feature')
    * header Authorization = 'Bearer ' + auth.token
    Given url baseUrl
    And path '/protected/resource'
    When method GET
    Then status 200
```

### Call con Parámetros
```gherkin
# Definir función
* def createUser =
  """
  function(data) {
    var result = karate.call('classpath:create-user.feature', data);
    return result;
  }
  """

# Usar función
* def newUser = createUser({ name: 'Jane', email: 'jane@test.com' })
```

---

## Matchers

### Built-in Matchers
```gherkin
# Type matchers
And match response.id == '#number'
And match response.name == '#string'
And match response.active == '#boolean'
And match response.tags == '#array'
And match response.address == '#object'
And match response.deleted == '#null'

# Value matchers
And match response.name == 'John'
And match response.age == 25
And match response.active == true

# Regex matchers
And match response.email == '#regex .+@.+'
And match response.phone == '#regex \\d{10}'

# Contains matchers
And match response.name == '#contains John'
And match response.tags == '#contains vip'

# Negation
And match response.error == '#notnull'
And match response.deleted == '#notpresent'
```

---

## Variables Ambientales

### karate-config.js
```javascript
function fn() {
    var env = karate.env;
    var config = {};
    
    if (env == 'dev') {
        config.baseUrl = 'https://dev.api.example.com';
    } else if (env == 'staging') {
        config.baseUrl = 'https://staging.api.example.com';
    } else {
        config.baseUrl = 'https://api.example.com';
    }
    
    return config;
}
```

### Uso en Features
```gherkin
Given url baseUrl
And path '/users'
When method GET
Then status 200
```

---

## Ejecución de Tests

### Con Maven
```bash
# Ejecutar todos los tests
mvn test

# Ejecutar.feature específico
mvn test -Dkarate.options="classpath:examples/users.feature"

# Ejecutar con tag
mvn test -Dkarate.options="--tags @smoke"
```

### Con Gradle
```bash
# Ejecutar todos los tests
gradle test

# Ejecutar con tag
gradle test -Dkarate.options="--tags @smoke"
```

### Con Java Directamente
```java
public class TestRunner {
    @Test
    public void testAll() {
        Results results = Runner.path("classpath:examples")
            .parallel(1);
        assertTrue(results.getFailCount() == 0);
    }
    
    @Test
    public void testUsers() {
        Results results = Runner.path("classpath:examples/users.feature")
            .parallel(1);
        assertTrue(results.getFailCount() == 0);
    }
}
```

---

## Reportes

### Reporte HTML
```bash
# Generar reporte
mvn test

# Ubicación: target/karate-reports/karate-summary.html
```

### Reporte JUnit
```java
@Karate.Test
karate.tags("@smoke");
```

---

## CI/CD con GitHub Actions

```yaml
name: Karate API Tests

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Set up JDK 17
      uses: actions/setup-java@v3
      with:
        java-version: '17'
        distribution: 'temurin'
        cache: maven
    
    - name: Run Karate tests
      run: mvn test
    
    - name: Upload report
      uses: actions/upload-artifact@v3
      if: always()
      with:
        name: karate-report
        path: target/karate-reports/
```

---

## Mejores Prácticas

### 1. Organización
```
src/test/java/
├── features/
│   ├── users/
│   │   ├── users-crud.feature
│   │   └── users-validation.feature
│   └── orders/
│       └── orders-flow.feature
├── helpers/
│   ├── auth.feature
│   └── utils.feature
└── runners/
    ├── UsersTestRunner.java
    └── OrdersTestRunner.java
```

### 2. Naming Convention
```gherkin
Scenario: Get user by valid ID returns user data
Scenario: Create user with invalid email returns 400
Scenario: Delete non-existent user returns 404
```

### 3. No Hardcode Values
```gherkin
# Bad
* def baseUrl = 'https://api.example.com'

# Good
* def baseUrl = karate.properties['baseUrl']
```

---

## Referencias

- [Karate Official Documentation](https://github.com/karatelabs/karate)
- [Karate GitHub](https://github.com/karatelabs/karate)
- [Karate Examples](https://github.com/karatelabs/karate/tree/master/examples)

---

*Documento de referencia - API Test Life Cycle*
*Última actualización: Junio 2026*
