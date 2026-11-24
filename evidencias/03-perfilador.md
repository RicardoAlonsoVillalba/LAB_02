# Actividad 3 — Activar el perfilador y registrar operaciones
**Estudiante:** Ricardo Alonso Villalba

## Interpretación de Documentos (Tabla 9.1)
| # | op | Usuario | Cliente | Aplicación | Qué hizo | Documentos afectados |
|---|---|---|---|---|---|---|
| 1 | `query` | `app_ventas@comercial` | `127.0.0.1` | `app-ventas-web` | Consultó clientes de Culiacán | `nreturned: 3`[cite: 6] |
| 2 | `update` | `app_ventas@comercial` | `127.0.0.1` | `app-ventas-web` | Actualizó RFC de `_id: 3` a `GAMA780912XYZ` | `nModified: 1`[cite: 6] |
| 3 | `remove` | `app_ventas@comercial` | `127.0.0.1` | `app-ventas-web` | Eliminó documento `_id: 2` | `ndeleted: 1`[cite: 6] |

## Respuestas a Preguntas
1. **¿Puedes saber cuál era el RFC antes del cambio con la información de `system.profile`?** No hay evidencia con esta edición[cite: 6]. El perfilador registra únicamente la instrucción ejecutada (`$set: { rfc: "GAMA780912XYZ" }`), no el estado anterior del documento[cite: 6].
2. **¿Puedes saber qué datos tenía el documento eliminado?** No; `system.profile` solo registra el filtro de borrado (`q: { _id: 2 }`) y `ndeleted: 1`, pero no el contenido del registro eliminado[cite: 6].
3. **A diferencia de `log_statement = 'mod'` en PostgreSQL, el nivel 2 sí registró la consulta. ¿Qué ventaja y qué riesgo tiene eso?** Permite auditar lecturas de datos sensibles, pero degrada el desempeño, llena rápidamente el espacio circular de 1 MB de `system.profile` y puede exponer datos personales en los filtros[cite: 6].
4. **¿Por qué el campo `user` incluye `@comercial`?** Porque en MongoDB cada usuario pertenece a una base de datos de autenticación (`authenticationDatabase`), distinguiendo a `app_ventas@comercial` de usuarios homónimos en otras bases[cite: 6].
