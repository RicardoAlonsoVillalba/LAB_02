# Actividad 9 — Comparar los tres escenarios
**Estudiante:** Ricardo Alonso Villalba

| Evento auditable | PostgreSQL (bitácora + disparador) | MongoDB Community (perfilador + bitácora) | MongoDB Enterprise / Atlas (auditoría nativa) |
|---|---|---|---|
| **Acceso exitoso** | **Sí** (observado con `log_connections = on`)[cite: 6] | **Sí** (observado en `mongod.log` como `Successfully authenticated`)[cite: 6] | Documentado, no practicado (`auditAuthorizationSuccess`)[cite: 6] |
| **Acceso fallido** | **Sí** (observado como `FATAL: password authentication failed`)[cite: 6] | **Sí** (observado en `mongod.log` como `Failed to authenticate`)[cite: 6] | Documentado, no practicado[cite: 6] |
| **Cambio de privilegios** | **Sí** (observado con `log_statement = 'ddl'` o `'mod'`)[cite: 6] | **No** (no se registra en log ni perfilador; requiere instantáneas compensatorias)[cite: 6] | Documentado, no practicado (audita `grantRolesToUser`, `createUser`, etc.)[cite: 6] |
| **Cambio de estructura** | **Sí** (observado `ALTER TABLE` con `ddl`/`mod`)[cite: 6] | **Parcial** (solo comandos como `create`/`drop` si el perfilador está activo)[cite: 6] | Documentado, no practicado (audita eventos DDL nativamente)[cite: 6] |
| **Modificación de datos** | **Sí** (observado en bitácora y en `auditoria.cambios_clientes`)[cite: 6] | **Sí** (observado en `system.profile` con nivel 1 filtrado o nivel 2)[cite: 6] | Documentado, no practicado (audita CRUD mediante filtros)[cite: 6] |
| **Valor anterior del dato** | **Sí** (observado mediante `to_jsonb(OLD)` en trigger)[cite: 6] | **No** (`system.profile` solo guarda la sentencia `command`, no el documento previo)[cite: 6] | Documentado, no practicado (audita eventos, tampoco guarda `OLD` por defecto)[cite: 6] |
| **Lectura de datos sensibles** | **Parcial** (no con `mod`; requiere `log_statement = 'all'` o `pgaudit`)[cite: 6] | **Parcial** (observado solo con perfilador en nivel 2, con alto costo)[cite: 6] | Documentado, no practicado (audita `find` con filtros de objeto)[cite: 6] |
| **Cambio de configuración del registro** | **Sí** (observado en PID principal `parameter ... changed to ...`)[cite: 6] | **Sí** (observado en `mongod.log` como `Profiler settings changed`)[cite: 6] | Documentado, no practicado[cite: 6] |
