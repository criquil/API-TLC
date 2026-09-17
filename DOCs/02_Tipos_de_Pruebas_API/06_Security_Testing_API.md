# Security Testing para API

## Definición
Valida que la API sea segura contra vulnerabilidades conocidas, protegiendo datos sensibles y previniendo accesos no autorizados.

## OWASP API Security Top 10 (2023)

### API1: Broken Object Level Authorization
```gherkin
Scenario: Acceder a objeto de otro usuario
  Given I am authenticated as user A
  When I GET /api/users/user-B-id
  Then I should receive 403 Forbidden
```

### API2: Broken Authentication
```gherkin
Scenario: Token expirado
  Given I have expired JWT token
  When I GET /api/users with expired token
  Then I should receive 401 Unauthorized
```

### API3: Broken Object Property Level Authorization
```gherkin
Scenario: Acceder a propiedades sensibles
  Given I am authenticated as regular user
  When I GET /api/users/me
  Then response should not contain password hash
  And response should not contain internal fields
```

### API4: Unrestricted Resource Consumption
```gherkin
Scenario: Rate limiting
  Given I send 100 requests in 1 minute
  When I send 101st request
  Then I should receive 429 Too Many Requests
```

### API5: Broken Function Level Authorization
```gherkin
Scenario: Acceder a función admin
  Given I am authenticated as regular user
  When I DELETE /api/admin/users/1
  Then I should receive 403 Forbidden
```

## Tipos de Security Testing

### 1. Authentication Testing
```javascript
// Test token generation and validation
const authTests = [
  { name: 'Valid token', token: validToken, expected: 200 },
  { name: 'Expired token', token: expiredToken, expected: 401 },
  { name: 'Invalid token', token: 'invalid', expected: 401 },
  { name: 'Missing token', token: null, expected: 401 },
];
```

### 2. Authorization Testing
```javascript
// Test role-based access
const rbacTests = [
  { role: 'admin', endpoint: '/api/admin', expected: 200 },
  { role: 'user', endpoint: '/api/admin', expected: 403 },
  { role: 'guest', endpoint: '/api/admin', expected: 401 },
];
```

### 3. Input Validation
```javascript
// SQL Injection prevention
const sqlInjectionTests = [
  "' OR '1'='1",
  "'; DROP TABLE users; --",
  "1' UNION SELECT * FROM users --",
];

// XSS prevention
const xssTests = [
  '<script>alert("xss")</script>',
  '"><img src=x onerror=alert(1)>',
  'javascript:alert(1)',
];
```

### 4. Data Protection
```javascript
// Sensitive data exposure
const sensitiveFields = [
  'password',
  'password_hash',
  'credit_card',
  'ssn',
  'internal_id',
];

// Verify sensitive data is not in response
sensitiveFields.forEach(field => {
  expect(response.data).not.toHaveProperty(field);
});
```

## Security Testing Tools

### OWASP ZAP
```bash
# Automated security scan
zap-cli quick-scan -s all -r https://api.example.com
```

### Burp Suite
- Proxy interception
- Vulnerability scanning
- Manual testing

### Postman Security Tests
```javascript
// Postman test for security headers
pm.test("Security headers present", () => {
  pm.response.to.have.header("X-Content-Type-Options");
  pm.response.to.have.header("X-Frame-Options");
  pm.response.to.have.header("X-XSS-Protection");
});
```

## Security Headers
```javascript
const requiredHeaders = [
  'X-Content-Type-Options',
  'X-Frame-Options',
  'X-XSS-Protection',
  'Strict-Transport-Security',
  'Content-Security-Policy',
  'Referrer-Policy',
];

requiredHeaders.forEach(header => {
  expect(response.headers).toHaveProperty(header.toLowerCase());
});
```

## Best Practices

### 1. Never Trust User Input
- Validate all input server-side
- Use parameterized queries
- Escape output

### 2. Authentication
- Use strong password policies
- Implement MFA
- Rotate secrets regularly

### 3. Authorization
- Principle of least privilege
- Validate permissions per request
- Audit access logs

### 4. Data Protection
- Encrypt sensitive data at rest
- Use HTTPS only
- Mask sensitive data in logs

## Métricas de Éxito
| Métrica | Target |
|---------|--------|
| Critical Vulnerabilities | 0 |
| High Vulnerabilities | 0 |
| Security Headers | 100% |
| Input Validation | 100% |
