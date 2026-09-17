# RCA y Troubleshooting para API Testing

## Técnicas de Root Cause Analysis

### 1. 5 Whys
Preguntar "¿por qué?" sucesivamente hasta llegar a la causa raíz.

**Ejemplo:**
1. ¿Por qué falló el test? → El endpoint retornó 500
2. ¿Por qué retornó 500? → La DB no respondió
3. ¿Por qué la DB no respondió? → Connection pool agotado
4. ¿Por qué se agotó? → Queries lentas sin índice
5. ¿Por qué no hay índice? → Migration no se ejecutó

### 2. Fishbone (Ishikawa)
Clasificar causas potenciales en categorías:
- **Personas:** Falta de capacitación, errors humanos
- **Proceso:** Flawed test design, missing edge cases
- **Herramientas:** Bug en herramienta, configuración incorrecta
- **Entorno:** Network issues, DB state, third-party API down
- **Datos:** Test data corrompido, datos insuficientes

### 3. 8D Report
8 disciplinas para resolver problemas recurrentes:
1. Formar equipo
2. Definir el problema
3. Contener el problema
4. Identificar causa raíz
5. Elegir acciones correctivas
6. Implementar acciones
7. Prevenir recurrencia
8. Reconocer equipo

## Troubleshooting Común

| Problema | Causa Común | Solución |
|----------|-------------|----------|
| 401 Unauthorized | Token expirado | Refresh token, re-autenticar |
| 403 Forbidden | Permisos insuficientes | Verificar roles y permisos |
| 404 Not Found | Endpoint incorrecto | Verificar URL y versionado |
| 429 Too Many Requests | Rate limiting | Implementar backoff, reducir frecuencia |
| 500 Internal Server Error | Bug en backend | Escalar a equipo de desarrollo |
| Timeout | Server lento o caído | Verificar disponibilidad, ajustar timeout |
| Connection Refused | Servicio no disponible | Verificar que el servicio esté corriendo |
