# Métricas y KPIs de API Testing

## Métricas de Cobertura

| Métrica | Fórmula | Target |
|---------|---------|--------|
| Endpoint Coverage | endpoints_tested / total_endpoints | ≥ 80% |
| Line Coverage | lines_covered / total_lines | ≥ 70% |
| Branch Coverage | branches_covered / total_branches | ≥ 60% |
| Scenario Coverage | scenarios_tested / total_scenarios | ≥ 90% |

## Métricas de Calidad

| Métrica | Fórmula | Target |
|---------|---------|--------|
| Pass Rate | passed_tests / total_tests | ≥ 95% |
| Defect Density | defects / kLOC | ≤ 5 |
| Escape Rate | defects_found_in_prod / total_defects | ≤ 10% |
| MTTR | total_recovery_time / num_incidents | ≤ 4h |

## Métricas de Performance (API-level)

| Métrica | Fórmula | Target |
|---------|---------|--------|
| Response Time p95 | 95th percentile of response times | ≤ 200ms |
| Response Time p99 | 99th percentile of response times | ≤ 500ms |
| Throughput | requests per second | ≥ 100 req/s |
| Error Rate | errors / total_requests | ≤ 1% |
| Availability | uptime / total_time | ≥ 99.9% |

## Métricas de Contract

| Métrica | Fórmula | Target |
|---------|---------|--------|
| Schema Compliance | valid_responses / total_responses | 100% |
| Breaking Changes | breaking_changes / total_changes | 0 |
| Backward Compatibility | compatible_versions / total_versions | 100% |

## Criterios Pass/Fail

- **PASSED**: Todas las métricas dentro del target
- **CONDITIONAL**: Métricas borderline con mitigación propuesta
- **FAILED**: Alguna métrica crítica fuera del target sin mitigación
