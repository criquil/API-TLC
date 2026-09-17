# Integration Testing para APIs

## Definición
Validación de la interacción entre múltiples servicios, módulos o sistemas a través de sus APIs.

## Tipos de Integration Testing

### 1. Service-to-Service
Valida comunicación directa entre dos servicios.

**Ejemplo:**
```
Servicio A → Servicio B
POST /orders → Servicio de inventario → Servicio de pagos
```

### 2. End-to-End (E2E)
Valida flujos completos que involucran múltiples servicios.

**Ejemplo:**
```
1. Crear usuario → Auth Service
2. Crear orden → Order Service
3. Procesar pago → Payment Service
4. Enviar notificación → Notification Service
```

### 3. Contract Testing
Valida que la API cumple con su contrato (OpenAPI, Swagger).

**Herramientas:**
- Pact (Consumer-driven)
- Spring Cloud Contract (Provider-driven)
- Dredd (OpenAPI validation)

### 4. Data Integration
Valida consistencia de datos entre sistemas.

**Escenarios:**
- Sincronización de datos maestros
- Migración de datos
- Replicación entre bases de datos

## Scenarios Comunes

### Flujo de Compra E-Commerce
```
1. GET /products → Lista productos
2. POST /cart → Agregar producto
3. POST /checkout → Iniciar checkout
4. POST /payments → Procesar pago
5. PUT /orders/{id}/confirm → Confirmar orden
6. GET /orders/{id}/status → Verificar estado
```

### Microservicios
```
API Gateway → User Service
           → Order Service
           → Inventory Service
           → Payment Service
           → Notification Service
```

### Third-Party Integration
```
Tu Sistema → Stripe API (pagos)
           → SendGrid API (emails)
           → Twilio API (SMS)
           → Google Maps API (geolocalización)
```

## Assertions para Integration Testing

### Status Codes
- 200: Operación exitosa
- 201: Recurso creado
- 202: Aceptado (proceso asíncrono)
- 400: Error en request
- 500: Error interno del servidor

### Response Validation
```json
{
  "orderId": "ORD-12345",
  "status": "confirmed",
  "items": [
    {
      "productId": "PROD-001",
      "quantity": 2,
      "price": 29.99
    }
  ],
  "total": 59.98,
  "paymentId": "PAY-67890"
}
```

### Idempotency
```
POST /payments (mismo request 2 veces)
→ Primer request: 201 Created
→ Segundo request: 200 OK (misma respuesta)
```

### Timeout Handling
```
GET /slow-service/data
→ Timeout después de 30 segundos
→ Retry automático
→ Fallback a cache
```

## Patrones de Integration Testing

### 1. Happy Path
Escenario exitoso principal del flujo.

### 2. Error Scenarios
- Servicio no disponible
- Timeout
- Datos inválidos
- Autorización fallida

### 3. Recovery Scenarios
- Retry automático
- Fallback a servicios alternativos
- Circuit breaker activation

### 4. Performance Scenarios
- Tiempo de respuesta bajo carga
- Throughput mínimo
- Resource utilization

## Herramientas

| Herramienta | Uso | Ventajas |
|-------------|-----|----------|
| Karate | Multi-protocolo | BDD, reportes nativos |
| RestSharp | .NET | Integración ecosistema |
| Playwright | JS/TS | API + UI testing |
| REST Assured | Java | Given/When/Then |

## Ejemplo con Karate

```gherkin
Feature: Purchase Flow Integration

  Scenario: Complete purchase flow
    # 1. Get products
    Given url 'https://api.example.com'
    And path '/products'
    When method GET
    Then status 200
    And def productId = response[0].id

    # 2. Add to cart
    Given path '/cart'
    And request { productId: '#(productId)', quantity: 1 }
    When method POST
    Then status 201

    # 3. Checkout
    Given path '/checkout'
    When method POST
    Then status 202
    And def orderId = response.orderId

    # 4. Process payment
    Given path '/payments'
    And request { orderId: '#(orderId)', method: 'credit_card' }
    When method POST
    Then status 201

    # 5. Verify order status
    Given path '/orders/' + orderId + '/status'
    When method GET
    Then status 200
    And match response.status == 'confirmed'
```

## Best Practices

1. **Test isolation**: Cada test independiente
2. **Cleanup automático**: Eliminar datos de prueba
3. **Mock externo**: Usar mocks para servicios de terceros
4. **Retry logic**: Implementar reintentos para servicios inestables
5. **Monitoring**: Monitorear servicios durante tests
6. **Documentation**: Documentar contratos y dependencias

## Referencias
- [DOCs/02_Tipos_de_Pruebas_API/01_Tipos_de_Pruebas_API.md](01_Tipos_de_Pruebas_API.md)
- [Pact Documentation](https://docs.pact.io/)
- [Karate Documentation](https://github.com/karatelabs/karate)