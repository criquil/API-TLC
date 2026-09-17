# AGENTS.md

## What This Repo Is

This is a **knowledge base + agent configuration repository** for the **Intelligent API Test Orchestrator (API-TLC v1.0)**. It is NOT a traditional code project — there are no build commands, tests, or CI pipelines to run.

The repo defines:
- **7 API-TLC agents** (`.opencode/agents/`) that orchestrate API testing end-to-end
- **8 skills** (`.opencode/skills/`) for tool-specific workflows
- **Documentation knowledge base** (`DOCs/`) covering functional, integration, contract, negative, security testing
- **Training data** (`DOCs/batch*_api.jsonl`) for LLM fine-tuning

## Agent Architecture (API-TLC v1.0)

The orchestrator (`api-tlc-orchestrator-api`) delegates to 6 specialized agents in strict order:

```
1. api-tlc-intake-api         → Requirements gathering + single tool selection
2. api-tlc-diagnostics-api    → Environment readiness, risks, contract validation
3. api-tlc-procedure-plan-api → Test types, scenarios, test data strategy
4. api-tlc-test-plan-api      → Formal test plan document (ISTQB/IEEE-829)
5. api-tlc-execution-api      → Script generation + test execution
6. api-tlc-analysis-api       → Metrics, RCA, report with verdict (PASSED/CONDITIONAL/FAILED)
```

Each agent has mandatory pre-execution reads — it MUST read specific `DOCs/` files before operating. Do not skip these.

## Key Conventions

### v1.0 Defaults
- **Single tool** by default: RestSharp, Karate, Playwright, or REST Assured — never multi-tool unless explicitly requested
- **Sequential execution** — parallel only if `parallel: true` is explicitly requested
- **3 analysis modes**: `individual` (current run), `vs_baseline` (compare to saved baseline), `vs_other_runs` (compare to prior runs)
- Plan state persists in `docs/plan/{plan_id}/plan.yaml`

### File Locations
- **Plan state**: `docs/plan/{plan_id}/plan.yaml`
- **Test plan doc**: `docs/api-test-plan.md`
- **Test scripts**: `tests/api/{tool}/scripts/`
- **Test data**: `tests/api/{tool}/data/`
- **Test results**: `tests/api/{tool}/results/`
- **Analysis report**: `docs/api-test-report.md`

### Knowledge Base Structure (`DOCs/`)
- `01_Introduccion_API_Testing/` — API testing definition, fundamentals, standards
- `02_Tipos_de_Pruebas_API/` — Functional, integration, contract, negative, boundary, security testing
- `03_Fases_del_API_TLC/` — Requirements, planning, environment, analysis/closure
- `04_Metricas_y_KPIs_API/` — Coverage, quality, performance, contract metrics
- `05_Herramientas_API/` — Tool-specific guides (RestSharp, Karate, Playwright, REST Assured)
- `06_Test_Data_Management/` — Fixtures, factories, seeds, mock data
- `07_Entorno_y_Monitoreo_API/` — Observability, CI/CD integration
- `08_Desarrollo_de_Scripts_API/` — Advanced scripting patterns per tool
- `09_Analisis_y_Bottlenecks_API/` — RCA techniques (5 Whys, Fishbone)
- `10_Mejores_Practicas_API/` — CI/CD, test pyramid, contract-first, trends

### Tool Selection Rules
When an agent selects a tool, it follows this decision matrix:
- **RestSharp**: .NET/C# teams, HTTP/REST, xUnit/NUnit
- **Karate**: BDD, multi-protocol (HTTP/gRPC/GraphQL), reporting
- **Playwright**: JS/TS teams, API + browser testing
- **REST Assured**: Java teams, Given/When/Then, Allure reports

## Training Data Generation

The `DOCs/batch*_api.jsonl` files are ChatML-format training data. Regenerate with:
- `DOCs/batch1_api/generate_batch1_api.ps1` (PowerShell) — API-TLC fundamentals + test types
- `DOCs/generate_batch5_api.py` (Python) — Environment & monitoring

The merged dataset is in `DOCs/11_APITLC_ChatML_Unsloth_merged_api.jsonl`.

## Pitfalls

- **Agents must read DOCs before operating.** Every subagent has mandatory pre-execution reads. Do not generate content from memory.
- **Never skip the smoke test.** Execution always runs smoke first; if it fails, stop.
- **Never mark a phase complete without user confirmation** for execution-related decisions (especially phase 5).
- **Plan state is in `docs/plan/`**, not in `.opencode/`. The `.opencode/todo.md` is for OpenCode session tracking, not API-TLC plan state.
- **NFRs must be numeric.** No "fast" or "acceptable" — use concrete thresholds (p95 ≤ 200ms, error rate ≤ 1%).
- **This is a Spanish-language project.** Agent prompts and docs are primarily in Spanish. Maintain this convention.
