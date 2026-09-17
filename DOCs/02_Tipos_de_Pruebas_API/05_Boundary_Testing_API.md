# Boundary Testing de APIs

## Definición

El **Boundary Testing** valida el comportamiento de las APIs en los límites de los rangos aceptables de entrada. Detecta errores comunes como off-by-one, underflow/overflow y manejo incorrecto de valores extremos.

---

## Tipos de Boundaries

### 1. Numeric Boundaries

```
Min value:     -2147483648 (int32)
Max value:      2147483647 (int32)
Zero:           0
Negative:       -1
Positive:       1
```

### 2. String Boundaries

```
Empty:          ""
Min length:     "a"
Max length:     "a" × 255
Null:           null
Whitespace:     "   "
Special chars:  "!@#$%^&*()"
```

### 3. Collection Boundaries

```
Empty:          []
Single item:    [item]
Max items:      [item1, item2, ..., itemN]
Null:           null
```

### 4. Date/Time Boundaries

```
Min date:       1970-01-01T00:00:00Z
Max date:       2038-01-19T03:14:07Z
Null:           null
Invalid format: "not-a-date"
```

---

## Escenarios de Boundary Testing

### Numeric Fields

```gherkin
Scenario: Integer at minimum boundary
  Given the API accepts age between 0 and 150
  When I send a request with age = 0
  Then the response status should be 200

Scenario: Integer below minimum boundary
  Given the API accepts age between 0 and 150
  When I send a request with age = -1
  Then the response status should be 400
  And the error should indicate value below minimum

Scenario: Integer at maximum boundary
  Given the API accepts age between 0 and 150
  When I send a request with age = 150
  Then the response status should be 200

Scenario: Integer above maximum boundary
  Given the API accepts age between 0 and 150
  When I send a request with age = 151
  Then the response status should be 400
  And the error should indicate value above maximum
```

### String Fields

```gherkin
Scenario: Empty string
  Given the API accepts name with max 100 characters
  When I send a request with name = ""
  Then the response status should be 400
  And the error should indicate name is required

Scenario: String at maximum length
  Given the API accepts name with max 100 characters
  When I send a request with name = "a" × 100
  Then the response status should be 200

Scenario: String above maximum length
  Given the API accepts name with max 100 characters
  When I send a request with name = "a" × 101
  Then the response status should be 400
  And the error should indicate name exceeds maximum length
```

### Date Fields

```gherkin
Scenario: Valid date
  Given the API accepts dates
  When I send a request with date = "2024-01-15"
  Then the response status should be 200

Scenario: Date before minimum
  Given the API accepts dates after 2020-01-01
  When I send a request with date = "2019-12-31"
  Then the response status should be 400
  And the error should indicate date before minimum

Scenario: Date after maximum
  Given the API accepts dates before 2030-01-01
  When I send a request with date = "2030-01-02"
  Then the response status should be 400
  And the error should indicate date after maximum
```

---

## Tablas de Boundary Values

### Integer Field (0-150)

| Value | Boundary | Expected | Status |
|-------|----------|----------|--------|
| -1 | Below min | 400 | ✅ |
| 0 | Min | 200 | ✅ |
| 1 | Above min | 200 | ✅ |
| 75 | Midpoint | 200 | ✅ |
| 149 | Below max | 200 | ✅ |
| 150 | Max | 200 | ✅ |
| 151 | Above max | 400 | ✅ |

### String Field (1-100 chars)

| Value | Boundary | Expected | Status |
|-------|----------|----------|--------|
| "" | Empty | 400 | ✅ |
| "a" | Min length | 200 | ✅ |
| "a" × 50 | Midpoint | 200 | ✅ |
| "a" × 100 | Max length | 200 | ✅ |
| "a" × 101 | Above max | 400 | ✅ |

### Array Field (1-10 items)

| Value | Boundary | Expected | Status |
|-------|----------|----------|--------|
| [] | Empty | 400 | ✅ |
| [item] | Min items | 200 | ✅ |
| [item] × 5 | Midpoint | 200 | ✅ |
| [item] × 10 | Max items | 200 | ✅ |
| [item] × 11 | Above max | 400 | ✅ |

---

## Herramientas para Boundary Testing

### REST Assured

```java
@Test
public void testBoundaryValues() {
    // Test minimum boundary
    given()
        .baseUri(baseUrl)
        .body("{ \"age\": 0 }")
    .when()
        .post("/users")
    .then()
        .statusCode(200);
    
    // Test below minimum
    given()
        .baseUri(baseUrl)
        .body("{ \"age\": -1 }")
    .when()
        .post("/users")
    .then()
        .statusCode(400);
    
    // Test maximum boundary
    given()
        .baseUri(baseUrl)
        .body("{ \"age\": 150 }")
    .when()
        .post("/users")
    .then()
        .statusCode(200);
    
    // Test above maximum
    given()
        .baseUri(baseUrl)
        .body("{ \"age\": 151 }")
    .when()
        .post("/users")
    .then()
        .statusCode(400);
}
```

### Karate

```gherkin
Scenario Outline: Test boundary values for age
  Given url baseUrl
  And path '/users'
  And request { age: <age> }
  When method POST
  Then status <status>

  Examples:
    | age | status |
    | -1  | 400    |
    | 0   | 200    |
    | 1   | 200    |
    | 150 | 200    |
    | 151 | 400    |
```

### Playwright

```typescript
test('Boundary value testing', async ({ request }) => {
  // Test minimum boundary
  const minResponse = await request.post('/users', {
    data: { age: 0 }
  });
  expect(minResponse.status()).toBe(200);
  
  // Test below minimum
  const belowMinResponse = await request.post('/users', {
    data: { age: -1 }
  });
  expect(belowMinResponse.status()).toBe(400);
  
  // Test maximum boundary
  const maxResponse = await request.post('/users', {
    data: { age: 150 }
  });
  expect(maxResponse.status()).toBe(200);
  
  // Test above maximum
  const aboveMaxResponse = await request.post('/users', {
    data: { age: 151 }
  });
  expect(aboveMaxResponse.status()).toBe(400);
});
```

---

## Métricas de Boundary Testing

| Métrica | Fórmula | Target |
|---------|---------|--------|
| Boundary Coverage | boundaries_tested / total_boundaries | ≥ 90% |
| Boundary Defect Rate | boundary_defects / total_boundary_tests | ≤ 5% |
| Edge Case Detection | edge_cases_found / total_edge_cases | ≥ 80% |

---

## Anti-Patrones

### 1. Solo test happy path
```
❌ Test age = 25 (valid)
✅ Test age = -1, 0, 150, 151 (boundaries)
```

### 2. Ignorar off-by-one errors
```
❌ Assume max length is handled correctly
✅ Test exact max length and max+1
```

### 3. No testear null/empty
```
❌ Skip null and empty values
✅ Always test null, empty, and whitespace
```

### 4. Sin data-driven testing
```
❌ Hardcode single boundary test
✅ Use parameterized tests for all boundaries
```

---

*Documento de referencia - API Test Life Cycle*
*Última actualización: Junio 2026*
