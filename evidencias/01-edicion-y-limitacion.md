# Actividad 1 — Lo que no existe en esta edición
**Estudiante:** Ricardo Alonso Villalba

## Respuestas e Interpretación
1. **¿Qué edición de MongoDB estás usando? ¿Qué evidencia lo demuestra?** MongoDB 7 Community Edition. Se demuestra con `db.adminCommand({ buildInfo: 1 }).modules`, que devuelve un arreglo vacío `[]` (en Enterprise incluiría `"enterprise"`).
2. **¿A dónde escribe tu servidor su bitácora? ¿Cómo lo sabes?** A la salida estándar (`stdout`) del contenedor Docker, verificable con `db.serverCmdLineOpts().parsed`, donde no existe sección `systemLog.destination: file`.
3. **¿Qué cuatro categorías de eventos registraría la auditoría nativa?** Cambios de esquema (DDL), eventos de conjuntos de réplicas y clústeres fragmentados, autenticación/autorización y operaciones CRUD.
4. **¿Qué tendría que evaluar una organización antes de licenciar Enterprise o migrar a Atlas solo para obtener auditoría?** El costo de licenciamiento frente al riesgo operativo, los requisitos normativos de cumplimiento, la viabilidad de controles compensatorios y el impacto de migración.
