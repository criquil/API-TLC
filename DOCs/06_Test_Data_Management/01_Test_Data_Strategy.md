# Test Data Management para API Testing

## Estrategias de Datos

### 1. Fixtures Estáticos
Archivos JSON/YAML con datos predefinidos.

**Ventajas:** Simple, repetible
**Riesgos:** Fragile, hard to maintain

### 2. Factories
Generadores programáticos de datos.

**Ventajas:** Flexibles, parametrizables
**Complejidad:** Requiere mantenimiento

### 3. Seeds
Scripts de inicialización de base de datos.

**Ventajas:** Estado conocido, reproducible
**Riesgos:** Acoplamiento a DB

### 4. Mock Data
Datos generados por herramientas mock (Faker, Mockaroo).

**Ventajas:** Realistas, variados
**Riesgos:** Pueden no reflejar casos edge

## Mejores Prácticas

1. **Separar datos de tests** — No hardcodear datos en scripts
2. **Usar Environment Variables** — Para datos sensibles (API keys, tokens)
3. **Data Cleanup** — Implementar limpieza después de cada test
4. **Isolation** — Cada test suite con sus propios datos
5. **Version Control** — Mantener fixtures en control de versiones
