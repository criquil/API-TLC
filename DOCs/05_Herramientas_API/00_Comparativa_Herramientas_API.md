# Comparativa de Herramientas de API Testing

## Matriz de Decisión

| Criterio | RestSharp | Karate | Playwright | REST Assured |
|----------|-----------|--------|------------|--------------|
| **Lenguaje** | C# / .NET | Gherkin (multi) | JS/TS | Java |
| **Tipo** | Library | Framework | Framework | Library |
| **HTTP** | ✅ | ✅ | ✅ | ✅ |
| **gRPC** | ❌ | ✅ | ❌ | ✅ |
| **GraphQL** | Manual | ✅ | ❌ | Manual |
| **BDD** | ❌ | ✅ | ❌ | ❌ |
| **Contract** | ❌ | ✅ | ❌ | ❌ |
| **Reportes** | xUnit/NUnit | HTML nativo | HTML Playwright | Allure |
| **CI/CD** | GitHub Actions | Maven/Gradle | GitHub Actions | Maven/Gradle |
| **Curva Aprendizaje** | Baja (familiar .NET) | Media | Baja (familiar JS) | Media |
| **Ideal para** | Equipos .NET | BDD, multi-API | JS/TS, E2E | Equipos Java |

## Recomendaciones por Stack

### Equipo .NET/C# → RestSharp
- Integración natural con xUnit/NUnit
- Familiaridad con ecosistema .NET
- Soporte completo de HTTP/REST

### Equipo Java → REST Assured
- Integración con JUnit/TestNG
- Reportes Allure
- Soporte gRPC y GraphQL

### Equipo JavaScript/TypeScript → Playwright
- API testing + browser testing en una herramienta
- Fixtures modernos y test generation
- Excelente DX

### Multi-lenguaje / BDD / Multi-protocolo → Karate
- Gherkin legible para stakeholders
- HTTP, gRPC, GraphQL, WebSocket
- Reportes HTML nativos
- Mock server integrado
