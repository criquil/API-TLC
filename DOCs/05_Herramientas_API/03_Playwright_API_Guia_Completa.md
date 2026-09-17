# Playwright API Testing - Guía Completa

## Visión General

**Playwright** es un framework de testing moderno que soporta API testing además de UI testing. Ofrece una API limpia en JavaScript/TypeScript con soporte nativo para async/await, fixtures y assertions.

---

## Características Principales

| Característica | Descripción |
|----------------|-------------|
| **Lenguaje** | JavaScript/TypeScript |
| **Tipo** | Framework completo |
| **HTTP** | Full support |
| **gRPC** | No soportado nativamente |
| **GraphQL** | Via HTTP |
| **Auth** | JWT, OAuth, API Key, Basic |
| **Reportes** | HTML Playwright nativo |
| **CI/CD** | GitHub Actions, Jenkins |

---

## Instalación

### npm
```bash
npm init playwright@latest
# or
npm install -D @playwright/test
```

### Configuración
```typescript
// playwright.config.ts
import { defineConfig } from '@playwright/test';

export default defineConfig({
  testDir: './tests',
  timeout: 30000,
  expect: {
    timeout: 5000,
  },
  use: {
    baseURL: 'https://api.example.com',
    extraHTTPHeaders: {
      'Accept': 'application/json',
    },
  },
});
```

---

## API Context

### Request Context Básico
```typescript
import { test, expect } from '@playwright/test';

test('GET users', async ({ request }) => {
  const response = await request.get('/users');
  expect(response.status()).toBe(200);
  
  const users = await response.json();
  expect(Array.isArray(users)).toBeTruthy();
});
```

### Context con Autenticación
```typescript
test('GET protected resource', async ({ request }) => {
  const response = await request.get('/protected', {
    headers: {
      'Authorization': `Bearer ${token}`,
    },
  });
  expect(response.status()).toBe(200);
});
```

### Context con Base URL
```typescript
// playwright.config.ts
export default defineConfig({
  use: {
    baseURL: 'https://api.example.com',
  },
});

// En el test
test('GET users', async ({ request }) => {
  const response = await request.get('/users'); // Solo la ruta
  expect(response.status()).toBe(200);
});
```

---

## Requests

### GET Request
```typescript
test('GET user by ID', async ({ request }) => {
  const response = await request.get('/users/1');
  
  expect(response.status()).toBe(200);
  
  const user = await response.json();
  expect(user.id).toBe(1);
  expect(user.name).toBe('John');
});
```

### POST Request
```typescript
test('POST create user', async ({ request }) => {
  const response = await request.post('/users', {
    data: {
      name: 'John',
      email: 'john@test.com',
    },
  });
  
  expect(response.status()).toBe(201);
  
  const user = await response.json();
  expect(user.id).toBeDefined();
  expect(user.name).toBe('John');
});
```

### PUT Request
```typescript
test('PUT update user', async ({ request }) => {
  const response = await request.put('/users/1', {
    data: {
      name: 'John Updated',
    },
  });
  
  expect(response.status()).toBe(200);
  
  const user = await response.json();
  expect(user.name).toBe('John Updated');
});
```

### DELETE Request
```typescript
test('DELETE user', async ({ request }) => {
  const response = await request.delete('/users/1');
  expect(response.status()).toBe(204);
});
```

---

## Assert y Validación

### Status Code
```typescript
expect(response.status()).toBe(200);
expect(response.status()).toBe(201);
expect(response.status()).toBe(404);
```

### Response Body
```typescript
const user = await response.json();

expect(user.name).toBe('John');
expect(user.age).toBe(25);
expect(user.active).toBe(true);
```

### Array Validation
```typescript
const users = await response.json();

expect(Array.isArray(users)).toBeTruthy();
expect(users.length).toBeGreaterThan(0);
expect(users[0].name).toBe('John');
```

### Schema Validation
```typescript
import { z } from 'zod';

const UserSchema = z.object({
  id: z.number(),
  name: z.string(),
  email: z.string().email(),
  age: z.number().optional(),
});

test('GET user validates schema', async ({ request }) => {
  const response = await request.get('/users/1');
  const user = await response.json();
  
  const result = UserSchema.safeParse(user);
  expect(result.success).toBeTruthy();
});
```

### Response Headers
```typescript
expect(response.headers()['content-type']).toContain('application/json');
```

### Response Time
```typescript
const startTime = Date.now();
const response = await request.get('/users');
const duration = Date.now() - startTime;

expect(duration).toBeLessThan(2000);
```

---

## Fixtures

### Custom Fixtures
```typescript
// fixtures.ts
import { test as base } from '@playwright/test';

type Fixtures = {
  apiToken: string;
  authenticatedRequest: APIRequestContext;
};

export const test = base.extend<Fixtures>({
  apiToken: async ({}, use) => {
    const token = await getAuthToken();
    await use(token);
  },
  
  authenticatedRequest: async ({ request, apiToken }, use) => {
    const authRequest = request.clone({
      headers: {
        ...request.context().options.extraHTTPHeaders,
        'Authorization': `Bearer ${apiToken}`,
      },
    });
    await use(authRequest);
  },
});
```

### Uso de Fixtures
```typescript
import { test } from './fixtures';

test('GET protected resource', async ({ authenticatedRequest }) => {
  const response = await authenticatedRequest.get('/protected');
  expect(response.status()).toBe(200);
});
```

---

## Data-Driven Tests

### Con Array de Datos
```typescript
const testData = [
  { input: { name: 'John' }, expected: 201 },
  { input: { name: '' }, expected: 400 },
  { input: { name: 'A'.repeat(101) }, expected: 400 },
];

for (const data of testData) {
  test(`Create user with name length ${data.input.name.length}`, async ({ request }) => {
    const response = await request.post('/users', {
      data: data.input,
    });
    expect(response.status()).toBe(data.expected);
  });
}
```

### Con CSV/JSON
```typescript
import { readFileSync } from 'fs';
import { parse } from 'csv-parse/sync';

const records = parse(readFileSync('test-data.csv'), {
  columns: true,
  skip_empty_lines: true,
});

for (const record of records) {
  test(`Test with ${record.name}`, async ({ request }) => {
    const response = await request.post('/users', {
      data: { name: record.name },
    });
    expect(response.status()).toBe(parseInt(record.expectedStatus));
  });
}
```

---

## Manejo de Errores

### Try-Catch
```typescript
test('Handle API error', async ({ request }) => {
  const response = await request.get('/users/999999');
  
  if (response.status() === 404) {
    const error = await response.json();
    expect(error.message).toContain('not found');
  } else {
    expect(response.status()).toBe(404);
  }
});
```

### Custom Error Handling
```typescript
async function expectStatus(
  response: APIResponse, 
  expectedStatus: number
) {
  if (response.status() !== expectedStatus) {
    const body = await response.text();
    throw new Error(
      `Expected status ${expectedStatus}, got ${response.status()}\nBody: ${body}`
    );
  }
}
```

---

## CI/CD con GitHub Actions

```yaml
name: API Tests

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
    
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Install Playwright Browsers
      run: npx playwright install --with-deps
    
    - name: Run API tests
      run: npx playwright test --project=api
    
    - name: Upload report
      uses: actions/upload-artifact@v3
      if: always()
      with:
        name: playwright-report
        path: playwright-report/
```

---

## Mejores Prácticas

### 1. Organización
```
tests/
├── api/
│   ├── users/
│   │   ├── users.spec.ts
│   │   └── users-fixtures.ts
│   └── orders/
│       └── orders.spec.ts
├── fixtures/
│   ├── auth.ts
│   └── data.ts
└── helpers/
    └── api-helpers.ts
```

### 2. Naming Convention
```typescript
test('GET /users/:id - returns user when exists', async ({ request }) => {});
test('POST /users - returns 400 when email invalid', async ({ request }) => {});
```

### 3. Parallel Execution
```typescript
// playwright.config.ts
export default defineConfig({
  projects: [
    { name: 'api', testDir: './tests/api' },
  ],
});
```

---

## Referencias

- [Playwright API Testing](https://playwright.dev/docs/api-testing)
- [Playwright GitHub](https://github.com/microsoft/playwright)
- [Playwright Test](https://playwright.dev/docs/test-configuration)

---

*Documento de referencia - API Test Life Cycle*
*Última actualización: Junio 2026*
