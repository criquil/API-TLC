# Contract Testing para API

## Definición
Valida que la API cumple con su contrato establecido (OpenAPI, Swagger, AsyncAPI), verificando que request/response sigan la especificación definida.

## Tipos de Contract Testing

### 1. Schema Validation
```yaml
# OpenAPI Schema
type: object
required:
  - id
  - name
  - email
properties:
  id:
    type: integer
  name:
    type: string
    minLength: 1
    maxLength: 100
  email:
    type: string
    format: email
```

### 2. Provider-Consumer Contracts
- **Provider:** La API que expone endpoints
- **Consumer:** El cliente que consume la API
- **Contract:** Acuerdo entre ambos sobre la estructura

### 3. AsyncAPI Contracts
```yaml
asyncapi: 2.0.0
channels:
  userCreated:
    subscribe:
      message:
        payload:
          type: object
          properties:
            userId:
              type: string
            timestamp:
              type: string
              format: date-time
```

## Herramientas de Contract Testing

### Pact
```javascript
// Consumer test
const { Pact } = require('@pact-foundation/pact');

const provider = new Pact({
  consumer: 'UserClient',
  provider: 'UserService',
});

describe('User API', () => {
  it('should return a user', async () => {
    await provider.addInteraction({
      state: 'user exists',
      uponReceiving: 'a request for user',
      withRequest: {
        method: 'GET',
        path: '/api/users/1',
      },
      willRespondWith: {
        status: 200,
        body: {
          id: 1,
          name: 'John',
          email: 'john@example.com',
        },
      },
    });
  });
});
```

### Specmatic
```yaml
# API Specification
openapi: 3.0.0
paths:
  /users:
    get:
      responses:
        '200':
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/User'
```

### Dredd
```bash
# Validate API against OpenAPI
dredd init
dredd test apiary.yml http://localhost:3000
```

## Estrategia de Pruebas

### Contract Validation Flow
```gherkin
Scenario: Validar contrato de usuario
  Given I have OpenAPI specification
  When I GET /api/users/1
  Then response should match User schema
  And response should have required fields
  And field types should match specification
```

### Schema Validation Code
```javascript
const Ajv = require('ajv');
const addFormats = require('ajv-formats');

const ajv = new Ajv();
addFormats(ajv);

const userSchema = {
  type: 'object',
  required: ['id', 'name', 'email'],
  properties: {
    id: { type: 'integer' },
    name: { type: 'string', minLength: 1 },
    email: { type: 'string', format: 'email' },
  },
};

const validate = ajv.compile(userSchema);
const isValid = validate(response.data);
expect(isValid).toBe(true);
```

### Contract Breaking Changes
1. **Removing field** → Breaking
2. **Changing field type** → Breaking
3. **Adding required field** → Breaking
4. **Adding optional field** → Non-breaking
5. **Changing field format** → Breaking

## Best Practices

### 1. Version Control
- Mantener contratos en repositorio
- Usar branches para cambios
- Review de contratos como PR

### 2. Backward Compatibility
- Agregar campos opcionales
- No remover campos existentes
- Mantener tipos de datos

### 3. Documentation
- Generar contratos desde código
- Mantener documentación actualizada
- Validar contratos en CI/CD

## Métricas de Éxito
| Métrica | Target |
|---------|--------|
| Contract Compliance | 100% |
| Breaking Changes Detected | 100% |
| Schema Validation Pass | ≥ 95% |
| Contract Coverage | ≥ 80% |
