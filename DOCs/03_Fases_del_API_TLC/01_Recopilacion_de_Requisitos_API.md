# Recopilación de Requisitos para API Testing

## Dimensiones de Requisitos

### 1. Sistema Bajo Prueba (SUT)
- Nombre y descripción del API
- Arquitectura (monolito, microservicios, serverless)
- Endpoints críticos a probar
- Protocolo (REST, GraphQL, gRPC, WebSocket)
- Especificación OpenAPI/Swagger disponible

### 2. Objetivos de Testing
- Tipo de prueba requerida
- Cobertura mínima objetivo (%)
- Criterios de aceptación numéricos
- SLAs o contratos de servicio

### 3. Contexto del Equipo
- Lenguaje de programación preferido
- Experiencia previa con herramientas de testing
- Plataforma CI/CD
- Restricciones de infraestructura

### 4. Datos de Prueba
- Disponibilidad de datos
- Necesidad de fixtures/factories
- Datos sensibles (PII) que requieren anonymización

## Preguntas Tipo

1. ¿Cuántos endpoints tiene el API?
2. ¿Cuál es la especificación OpenAPI?
3. ¿Qué autenticación utiliza?
4. ¿Cuál es el ambiente de testing disponible?
5. ¿Qué nivel de cobertura se espera?
