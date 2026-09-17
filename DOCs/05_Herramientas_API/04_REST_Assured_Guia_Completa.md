# REST Assured - Guía Completa para API Testing

## Visión General

**REST Assured** es una librería Java para testing de APIs REST que proporciona una sintaxis BDD (Given/When/Then) limpia y expressiva. Es ideal para equipos Java que buscan una solución madura y bien documentada.

---

## Características Principales

| Característica | Descripción |
|----------------|-------------|
| **Lenguaje** | Java |
| **Tipo** | Library |
| **HTTP** | Full support |
| **gRPC** | Via extensions |
| **GraphQL** | Manual (via HTTP) |
| **Auth** | JWT, OAuth, API Key, Basic |
| **Reportes** | Allure integración |
| **CI/CD** | Maven/Gradle |

---

## Instalación

### Maven
```xml
<dependency>
    <groupId>io.rest-assured</groupId>
    <artifactId>rest-assured</artifactId>
    <version>5.4.0</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>io.rest-assured</groupId>
    <artifactId>json-schema-validator</artifactId>
    <version>5.4.0</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>io.qameta.allure</groupId>
    <artifactId>allure-rest-assured</artifactId>
    <version>2.25.0</version>
    <scope>test</scope>
</dependency>
```

### Gradle
```groovy
testImplementation 'io.rest-assured:rest-assured:5.4.0'
testImplementation 'io.rest-assured:json-schema-validator:5.4.0'
testImplementation 'io.qameta.allure:allure-rest-assured:2.25.0'
```

---

## Configuración Base

### Configuración Global
```java
import io.restassured.RestAssured;

@BeforeAll
public static void setup() {
    RestAssured.baseURI = "https://api.example.com";
    RestAssured.basePath = "/v1";
    RestAssured.requestSpecification = new RequestSpecBuilder()
        .setContentType(ContentType.JSON)
        .setAccept(ContentType.JSON)
        .build();
}
```

### Configuración por Test
```java
given()
    .baseUri("https://api.example.com")
    .header("Accept", "application/json")
    .header("X-Custom-Header", "value")
.when()
    .get("/users")
.then()
    .statusCode(200);
```

---

## Requests Básicos

### GET Request
```java
@Test
public void testGetUsers() {
    given()
        .when()
            .get("/users")
        .then()
            .statusCode(200)
            .body("size()", greaterThan(0));
}

@Test
public void testGetUserById() {
    given()
        .pathParam("id", 1)
    .when()
        .get("/users/{id}")
    .then()
        .statusCode(200)
        .body("name", equalTo("John"));
}
```

### POST Request
```java
@Test
public void testCreateUser() {
    String requestBody = """
        {
            "name": "John",
            "email": "john@test.com"
        }
        """;
    
    given()
        .body(requestBody)
    .when()
        .post("/users")
    .then()
        .statusCode(201)
        .body("name", equalTo("John"));
}
```

### PUT Request
```java
@Test
public void testUpdateUser() {
    String requestBody = """
        {
            "name": "John Updated"
        }
        """;
    
    given()
        .pathParam("id", 1)
        .body(requestBody)
    .when()
        .put("/users/{id}")
    .then()
        .statusCode(200)
        .body("name", equalTo("John Updated"));
}
```

### DELETE Request
```java
@Test
public void testDeleteUser() {
    given()
        .pathParam("id", 1)
    .when()
        .delete("/users/{id}")
    .then()
        .statusCode(204);
}
```

---

## Parámetros

### Path Parameters
```java
given()
    .pathParam("userId", 1)
    .pathParam("postId", 10)
.when()
    .get("/users/{userId}/posts/{postId}")
```

### Query Parameters
```java
given()
    .queryParam("page", 1)
    .queryParam("limit", 10)
    .queryParam("sort", "name")
.when()
    .get("/users")
```

### Form Parameters
```java
given()
    .formParam("username", "admin")
    .formParam("password", "secret")
.when()
    .post("/login")
```

### Multi-Value Parameters
```java
given()
    .queryParam("tag", "java", "testing", "api")
.when()
    .get("/posts")
```

---

## Headers y Auth

### Headers
```java
given()
    .header("Accept", "application/json")
    .header("X-API-Key", "your-api-key")
    .header("Accept-Language", "es-ES")
.when()
    .get("/users")
```

### Basic Auth
```java
given()
    .auth().basic("username", "password")
.when()
    .get("/users")
```

### Bearer Token
```java
given()
    .auth().oauth2("your-bearer-token")
.when()
    .get("/users")
```

### Digest Auth
```java
given()
    .auth().digest("username", "password")
.when()
    .get("/users")
```

---

## Assert y Validación

### Status Code
```java
.then()
    .statusCode(200)
    .statusCode(lessThan(400));
```

### Response Body
```java
.then()
    .body("name", equalTo("John"))
    .body("age", greaterThan(18))
    .body("email", containsString("@"))
    .body("active", is(true));
```

### Complex Body
```java
.then()
    .body("users.size()", equalTo(10))
    .body("users[0].name", equalTo("John"))
    .body("users.find { it.id == 1 }.name", equalTo("John"));
```

### Collection Assertion
```java
.then()
    .body("name", hasItems("John", "Jane"))
    .body("age", hasItems(greaterThan(18), lessThan(65)));
```

### Array Validation
```java
.then()
    .body("size()", equalTo(10))
    .body("[0].name", equalTo("John"))
    .body("*.name", hasItems("John", "Jane"));
```

---

## JSON Schema Validation

```java
import static io.restassured.module.jsv.JsonSchemaValidator.matchesJsonSchema;

@Test
public void testUserSchema() {
    given()
    .when()
        .get("/users/1")
    .then()
        .body(matchesJsonSchemaInClasspath("user-schema.json"));
}
```

### Schema JSON
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "required": ["id", "name", "email"],
  "properties": {
    "id": { "type": "integer" },
    "name": { "type": "string" },
    "email": { "type": "string", "format": "email" },
    "age": { "type": "integer", "minimum": 0, "maximum": 150 }
  }
}
```

---

## Extracting Values

### Extract Simple Value
```java
String name = given()
    .when()
        .get("/users/1")
    .then()
        .extract().path("name");
```

### Extract Object
```java
User user = given()
    .when()
        .get("/users/1")
    .then()
        .extract().as(User.class);
```

### Extract Header
```java
String contentType = given()
    .when()
        .get("/users")
    .then()
        .extract().header("Content-Type");
```

### Extract Cookie
```java
String sessionId = given()
    .when()
        .get("/login")
    .then()
        .extract().cookie("SESSIONID");
```

---

## Spec (Reusable Specifications)

### Request Spec
```java
RequestSpecification requestSpec = new RequestSpecBuilder()
    .setBaseUri("https://api.example.com")
    .setContentType(ContentType.JSON)
    .setAccept(ContentType.JSON)
    .addHeader("X-API-Key", "your-key")
    .build();
```

### Response Spec
```java
ResponseSpecification responseSpec = new ResponseSpecBuilder()
    .expectStatusCode(200)
    .expectContentType(ContentType.JSON)
    .build();
```

### Uso
```java
given()
    .spec(requestSpec)
.when()
    .get("/users")
.then()
    .spec(responseSpec);
```

---

## Logging

### Request Logging
```java
given()
    .log().all()  // Log everything
    // .log().uri()  // Log only URI
    // .log().method()  // Log only method
    // .log().body()  // Log only body
.when()
    .get("/users")
.then()
    .log().all();  // Log response
```

### Conditional Logging
```java
given()
    .log().ifValidationFails()  // Log only on failure
.when()
    .get("/users")
.then()
    .log().ifValidationFails();
```

---

## CI/CD con GitHub Actions

```yaml
name: REST Assured API Tests

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
    
    - name: Run tests
      run: mvn test
    
    - name: Generate Allure Report
      uses: simple-elf/allure-report-action@master
      if: always()
      with:
        allure_results: target/allure-results
    
    - name: Publish Allure Report
      uses: peaceiris/actions-gh-pages@v3
      if: always()
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        publish_branch: gh-pages
        allure_history: target/allure-history
```

---

## Allure Report Integration

### Configuración
```java
import io.qameta.allure.restassured.AllureRestAssured;

@BeforeAll
public static void setup() {
    RestAssured.filters(new AllureRestAssured());
}
```

### Anotaciones
```java
@Test
@AllureId("1")
@AllureFeature("User Management")
@AllureStory("Create User")
@Severity(SeverityLevel.CRITICAL)
@Description("Test creating a new user with valid data")
public void testCreateUser() {
    // Test code
}
```

### Attachments
```java
Allure.addAttachment("Request Body", "application/json", requestBody);
Allure.addAttachment("Response Body", "application/json", response.getBody().asString());
```

---

## Mejores Prácticas

### 1. Organización
```
src/test/java/
├── tests/
│   ├── UsersApiTest.java
│   └── OrdersApiTest.java
├── specs/
│   ├── RequestSpec.java
│   └── ResponseSpec.java
├── models/
│   ├── User.java
│   └── CreateUserRequest.java
├── helpers/
│   └── TestDataBuilder.java
└── resources/
    ├── schemas/
    │   └── user-schema.json
    └── testdata/
        └── users.json
```

### 2. Page Object Pattern
```java
public class UsersPage {
    private final RequestSpecification request;
    
    public UsersPage(RequestSpecification request) {
        this.request = request;
    }
    
    public Response getAllUsers() {
        return request.when().get("/users");
    }
    
    public Response createUser(CreateUserRequest user) {
        return request.body(user).post("/users");
    }
    
    public Response getUserById(int id) {
        return request.pathParam("id", id).get("/users/{id}");
    }
}
```

### 3. Data Builder Pattern
```java
public class UserBuilder {
    private String name = "Default User";
    private String email = "default@test.com";
    private int age = 25;
    
    public UserBuilder withName(String name) {
        this.name = name;
        return this;
    }
    
    public UserBuilder withEmail(String email) {
        this.email = email;
        return this;
    }
    
    public UserBuilder withAge(int age) {
        this.age = age;
        return this;
    }
    
    public CreateUserRequest build() {
        return new CreateUserRequest(name, email, age);
    }
}
```

---

## Referencias

- [REST Assured Official](https://rest-assured.io/)
- [REST Assured GitHub](https://github.com/rest-assured/rest-assured)
- [Allure Framework](https://docs.qameta.io/allure/)
- [JSON Schema Validator](https://github.com/rest-assured/json-schema-validator)

---

*Documento de referencia - API Test Life Cycle*
*Última actualización: Junio 2026*
