# API-TLC — API Test Life Cycle Orchestrator v1.0

Sistema de orquestación inteligente para el ciclo de vida completo de pruebas de API, implementado como una base de conocimiento con agentes de IA especializados sobre **Claude Code** (Anthropic).

---

## ¿Qué es API-TLC?

API-TLC es un repositorio-cerebro que define agentes de IA y habilidades especializadas para guiar, generar y ejecutar pruebas de API de extremo a extremo. No es un proyecto de código tradicional: es la *inteligencia* que los agentes de IA utilizan para tomar decisiones, seleccionar herramientas, generar scripts y producir informes profesionales para cualquier API que se le indique.

Corre sobre **Claude Code** (Anthropic) usando el sistema de agentes y skills nativos de Claude. El sistema sigue estándares de la industria (**ISTQB**, **IEEE-829**, **OpenAPI/Swagger**) y soporta cuatro herramientas de testing de API líderes.

---

## Arquitectura del Sistema

### Flujo de Orquestación

```mermaid
flowchart TD
    U([👤 Usuario]) --> APU

    APU["🤖 APU\nOrquestador Principal"]

    APU --> F1["📋 Fase 1\nAPI-TLC-intake-api\nRequisitos & selección de herramienta"]
    F1 --> F2["🔍 Fase 2\nAPI-TLC-diagnostics-api\nDiagnóstico técnico & riesgos"]
    F2 --> F3["📐 Fase 3\nAPI-TLC-procedure-plan-api\nPlan de procedimiento & datos"]
    F3 --> F4["📄 Fase 4\nAPI-TLC-test-plan-api\nPlan formal ISTQB/IEEE-829"]
    F4 --> F5["⚙️ Fase 5\nAPI-TLC-execution-api\nGeneración de scripts & ejecución"]
    F5 --> F6["📊 Fase 6\nAPI-TLC-analysis-api\nMétricas, RCA & informe final"]

    F6 --> V{{"Veredicto"}}
    V --> P(["✅ PASSED"])
    V --> C(["⚠️ CONDITIONAL"])
    V --> X(["❌ FAILED"])

    style APU fill:#1e3a5f,color:#fff,stroke:#4a90d9
    style F1 fill:#2d6a4f,color:#fff,stroke:#52b788
    style F2 fill:#2d6a4f,color:#fff,stroke:#52b788
    style F3 fill:#2d6a4f,color:#fff,stroke:#52b788
    style F4 fill:#2d6a4f,color:#fff,stroke:#52b788
    style F5 fill:#2d6a4f,color:#fff,stroke:#52b788
    style F6 fill:#2d6a4f,color:#fff,stroke:#52b788
    style P fill:#1b4332,color:#fff,stroke:#52b788
    style C fill:#7b4f00,color:#fff,stroke:#f4a261
    style X fill:#6b1a1a,color:#fff,stroke:#e63946
```

### Los 7 Agentes

| Agente | Fase | Responsabilidad |
|--------|------|-----------------|
| `api-tlc-orchestrator-api` | — | Entrada única; coordina todas las fases |
| `api-tlc-intake-api` | 1 | Levantamiento de requisitos y selección de la herramienta óptima |
| `api-tlc-diagnostics-api` | 2 | Diagnóstico técnico del entorno y evaluación de riesgos |
| `api-tlc-procedure-plan-api` | 3 | Tipos de prueba, escenarios y estrategia de datos |
| `api-tlc-test-plan-api` | 4 | Documento de plan de pruebas formal (ISTQB/IEEE-829) |
| `api-tlc-execution-api` | 5 | Generación de scripts y coordinación de ejecución |
| `api-tlc-analysis-api` | 6 | Análisis de métricas, RCA y reporte final con veredicto |

---

## Las 8 Habilidades (Skills)

```mermaid
mindmap
  root((API-TLC\nSkills))
    Estrategia
      api-test-strategy\nMatriz de cobertura
      api-tool-selector\nSelección de herramienta
    Diagnóstico
      api-diagnostics-rca\nRCA & troubleshooting
      api-metrics-analysis\nKPIs & métricas
    Herramientas
      restsharp-api-workflow\n.NET / C#
      karate-api-workflow\nJava / BDD
      playwright-api-workflow\nJS / TS
      rest-assured-api-workflow\nJava / DSL
```

---

## Herramientas de Testing Soportadas

### Árbol de Selección de Herramienta

```mermaid
flowchart TD
    S([Inicio: ¿Cuál es el stack?]) --> Q1{Ecosistema}

    Q1 -->|.NET / C#| RS["🔷 RestSharp\nCliente HTTP liviano\nxUnit / NUnit"]
    Q1 -->|JavaScript / TypeScript| PW["🎭 Playwright\nAPI + Browser testing\nEn un solo framework"]
    Q1 -->|Java / JVM| Q2{¿Necesita BDD\no multi-protocolo?}

    Q2 -->|Sí — Gherkin, gRPC,\nGraphQL, WebSocket| KA["🥋 Karate\nBDD / Gherkin\nReporting HTML nativo"]
    Q2 -->|No — DSL fluido\ny Allure reporting| RA["✅ REST Assured\nGiven / When / Then\nIntegra con Allure"]

    style RS fill:#512bd4,color:#fff,stroke:#7c5cbf
    style PW fill:#2b5797,color:#fff,stroke:#4a90d9
    style KA fill:#c0392b,color:#fff,stroke:#e74c3c
    style RA fill:#27ae60,color:#fff,stroke:#2ecc71
```

| Herramienta | Ecosistema | Características clave |
|------------|------------|----------------------|
| **RestSharp** | .NET / C# | Cliente HTTP liviano; integra con xUnit/NUnit |
| **Karate** | Java / JVM | BDD con Gherkin; soporta HTTP, gRPC, GraphQL, WebSocket; reporting HTML nativo |
| **Playwright** | JavaScript / TypeScript | Testing API + browser en un solo framework |
| **REST Assured** | Java / JVM | DSL Given/When/Then; integra con Allure para reporting |

---

## Base de Conocimiento (`DOCs/`)

```mermaid
graph LR
    KB[("📚 DOCs\nBase de\nConocimiento")]

    KB --> M1["01 Introducción\nFundamentos, roles\nestándares"]
    KB --> M2["02 Tipos de Pruebas\nFuncional, Integración\nContrato, Seguridad"]
    KB --> M3["03 Fases del TLC\nDetalle de cada\nfase del ciclo"]
    KB --> M4["04 Métricas & KPIs\nCobertura, rendimiento\ncalidad contractual"]
    KB --> M5["05 Herramientas\nComparativa y guías\ncompletas"]
    KB --> M6["06 Test Data\nFixtures, factories\ndatos sintéticos"]
    KB --> M7["07 Entorno & Monitoreo\nObservabilidad\nCI/CD"]
    KB --> M8["08 Scripts\nPatrones avanzados\npor herramienta"]
    KB --> M9["09 Análisis\n5 Whys, Fishbone\ntroubleshooting"]
    KB --> M10["10 Mejores Prácticas\nPirámide de pruebas\ncontract-first"]

    style KB fill:#1e3a5f,color:#fff,stroke:#4a90d9
    style M1 fill:#2d3748,color:#e2e8f0,stroke:#4a5568
    style M2 fill:#2d3748,color:#e2e8f0,stroke:#4a5568
    style M3 fill:#2d3748,color:#e2e8f0,stroke:#4a5568
    style M4 fill:#2d3748,color:#e2e8f0,stroke:#4a5568
    style M5 fill:#2d3748,color:#e2e8f0,stroke:#4a5568
    style M6 fill:#2d3748,color:#e2e8f0,stroke:#4a5568
    style M7 fill:#2d3748,color:#e2e8f0,stroke:#4a5568
    style M8 fill:#2d3748,color:#e2e8f0,stroke:#4a5568
    style M9 fill:#2d3748,color:#e2e8f0,stroke:#4a5568
    style M10 fill:#2d3748,color:#e2e8f0,stroke:#4a5568
```

El sistema se respalda en más de 30 documentos en español organizados en 10 módulos temáticos.

---

## Artefactos Generados en Ejecución

```mermaid
flowchart LR
    EX["⚙️ Agentes\nen ejecución"]

    EX --> D["docs/"]
    EX --> T["tests/api/{herramienta}/"]

    D --> PL["plan/{plan_id}/\nplan.yaml\nEstado en vivo"]
    D --> TP["api-test-plan.md\nPlan formal\nISTQB/IEEE-829"]
    D --> TR["api-test-report.md\nInforme final\ncon veredicto"]

    T --> SC["scripts/\nScripts generados"]
    T --> DA["data/\nDatos de prueba"]
    T --> RE["results/\nResultados de\nejecución"]

    style EX fill:#1e3a5f,color:#fff,stroke:#4a90d9
    style D fill:#2d6a4f,color:#fff,stroke:#52b788
    style T fill:#7b4f00,color:#fff,stroke:#f4a261
```

---

## Cómo Usar

### Requisitos

- **Claude Code** instalado (`npm install -g @anthropic-ai/claude-code` o descarga desde claude.ai/code).
- El archivo `.claude/settings.json` configura APU como agente por defecto del proyecto.

### Inicio rápido

```mermaid
sequenceDiagram
    actor U as Usuario
    participant APU as APU (Orquestador)
    participant S1 as intake-api
    participant S2 as diagnostics-api
    participant S3 as procedure-plan-api
    participant S4 as test-plan-api
    participant S5 as execution-api
    participant S6 as analysis-api

    U->>APU: "Necesito pruebas para mi API..."
    APU->>S1: Delega Fase 1
    S1-->>APU: Requisitos + herramienta seleccionada
    APU->>S2: Delega Fase 2
    S2-->>APU: Diagnóstico + mapa de riesgos
    APU->>S3: Delega Fase 3
    S3-->>APU: Plan de procedimiento + datos
    APU->>S4: Delega Fase 4
    S4-->>APU: Documento ISTQB/IEEE-829
    APU->>S5: Delega Fase 5
    S5-->>APU: Scripts generados + resultados
    APU->>S6: Delega Fase 6
    S6-->>APU: Métricas + RCA + veredicto
    APU-->>U: Informe final (PASSED / CONDITIONAL / FAILED)
```

1. Abre el directorio `API-TLC/` en Claude Code.
2. El agente APU se cargará automáticamente como agente por defecto (`.claude/settings.json`).
3. Describe la API que deseas probar y APU guiará el proceso completo de forma interactiva.

---

## Estructura del Repositorio

```mermaid
graph TD
    ROOT["📁 API-TLC/"]

    ROOT --> README["📄 README.md"]
    ROOT --> CLAUDE["📄 CLAUDE.md\nConvenciones del repo"]
    ROOT --> CLD["📁 .claude/"]
    ROOT --> DOCS["📁 DOCs/\n30+ archivos .md"]

    CLD --> SETTINGS["📄 settings.json\nAgente por defecto: APU"]
    CLD --> AG["📁 agents/\n7 agentes + 8 skills"]

    style ROOT fill:#1e3a5f,color:#fff,stroke:#4a90d9
    style CLD fill:#2d3748,color:#e2e8f0,stroke:#4a5568
    style DOCS fill:#2d3748,color:#e2e8f0,stroke:#4a5568
    style AG fill:#2d6a4f,color:#fff,stroke:#52b788
```

---

## Estándares y Metodologías

```mermaid
graph LR
    APITLC["API-TLC"] --> ISTQB["ISTQB\nTerminología y\nniveles de prueba"]
    APITLC --> IEEE["IEEE-829\nEstructura del\nplan formal"]
    APITLC --> OAS["OpenAPI / Swagger\nEspecificación\nde contratos"]
    APITLC --> CF["Contract-First\nDevelopment"]
    APITLC --> CICD["CI/CD Integration\nGitHub Actions\nAzure DevOps\nJenkins"]

    style APITLC fill:#1e3a5f,color:#fff,stroke:#4a90d9
```

---

## Idioma

Toda la documentación, prompts de agentes y conocimiento base están en **español**, siguiendo la convención del proyecto definida en `AGENTS.md`.

---

## Convenciones del Proyecto

Antes de modificar agentes, habilidades o documentación, leer [`CLAUDE.md`](CLAUDE.md). Contiene:
- Reglas de naming para archivos generados
- Orden de ejecución de fases y dependencias entre agentes
- Reglas de selección de herramientas
- Pitfalls conocidos del sistema
