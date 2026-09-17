# SOURCE_MAPPING — API Testing Knowledge Base

## Skill → Doc Mapping

| Skill | Primary Doc Reference | Secondary Docs |
|-------|----------------------|----------------|
| `restsharp-api-workflow` | `DOCs/05_Herramientas_API/01_RestSharp_Guia_Completa.md` | `DOCs/04_Metricas_y_KPIs_API/01_Metricas_API_Exhaustivas.md` |
| `karate-api-workflow` | `DOCs/05_Herramientas_API/02_Karate_Guia_Completa.md` | `DOCs/04_Metricas_y_KPIs_API/01_Metricas_API_Exhaustivas.md` |
| `playwright-api-workflow` | `DOCs/05_Herramientas_API/03_Playwright_API_Guia_Completa.md` | `DOCs/04_Metricas_y_KPIs_API/01_Metricas_API_Exhaustivas.md` |
| `rest-assured-api-workflow` | `DOCs/05_Herramientas_API/04_REST_Assured_Guia_Completa.md` | `DOCs/04_Metricas_y_KPIs_API/01_Metricas_API_Exhaustivas.md` |
| `api-diagnostics-rca` | `DOCs/09_Analisis_y_Bottlenecks_API/01_RCA_y_Troubleshooting_API.md` | `DOCs/04_Metricas_y_KPIs_API/01_Metricas_API_Exhaustivas.md` |
| `api-metrics-analysis` | `DOCs/04_Metricas_y_KPIs_API/01_Metricas_API_Exhaustivas.md` | `DOCs/02_Tipos_de_Pruebas_API/` |
| `api-test-strategy` | `DOCs/02_Tipos_de_Pruebas_API/01_Tipos_de_Pruebas_API.md` | `DOCs/03_Fases_del_API_TLC/02_Planificacion_y_Diseno_API.md` |
| `api-tool-selector` | `DOCs/05_Herramientas_API/00_Comparativa_Herramientas_API.md` | `DOCs/02_Tipos_de_Pruebas_API/` |

## Agent → Doc Mapping

| Agent | Mandatory Reads |
|-------|----------------|
| `api-tlc-intake-api` | `DOCs/03_Fases_del_API_TLC/01_Recopilacion_de_Requisitos_API.md`, `DOCs/05_Herramientas_API/00_Comparativa_Herramientas_API.md` |
| `api-tlc-diagnostics-api` | `DOCs/07_Entorno_y_Monitoreo_API/01_Monitoreo_y_Observabilidad_API.md` |
| `api-tlc-procedure-plan-api` | `DOCs/02_Tipos_de_Pruebas_API/01_Tipos_de_Pruebas_API.md`, `DOCs/06_Test_Data_Management/01_Test_Data_Strategy.md` |
| `api-tlc-test-plan-api` | `DOCs/03_Fases_del_API_TLC/03_Plan_Formal_API.md`, `DOCs/04_Metricas_y_KPIs_API/01_Metricas_API_Exhaustivas.md` |
| `api-tlc-execution-api` | `DOCs/08_Desarrollo_de_Scripts_API/01_Scripting_API_Avanzado.md`, `DOCs/05_Herramientas_API/` |
| `api-tlc-analysis-api` | `DOCs/09_Analisis_y_Bottlenecks_API/01_RCA_y_Troubleshooting_API.md`, `DOCs/04_Metricas_y_KPIs_API/01_Metricas_API_Exhaustivas.md` |

## Tool Selection Matrix

| Scenario | Recommended Tool | Rationale |
|----------|-----------------|-----------|
| .NET/C# team, HTTP/REST | RestSharp | Native .NET, xUnit/NUnit integration |
| Java team, HTTP/gRPC | REST Assured | JUnit/TestNG, Allure reports |
| JS/TS team, API + browser | Playwright | Modern DX, fixtures, multi-protocol |
| Multi-language, BDD, GraphQL | Karate | Gherkin, multi-protocol, HTML reports |
| Contract-first development | Karate | Built-in contract validation |
| CI/CD pipeline | Playwright or REST Assured | Best CI integrations |
