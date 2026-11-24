# Actividad 8 — Conservación: exportar antes de perder
**Estudiante:** Ricardo Alonso Villalba

## Respuestas e Interpretación
1. **Menciona tres formas en que la evidencia del perfilador o de la bitácora puede perderse sin que nadie la borre deliberadamente:**
   * Sobrescritura automática en `system.profile` al alcanzar su límite circular de 1 MB (*capped collection*)[cite: 6].
   * Reinicio del servicio `mongod`, lo cual vacía el búfer en memoria de `getLog` y restablece `db.setProfilingLevel()` a `0`[cite: 6].
   * Rotación o eliminación del contenedor Docker si no existe un volumen persistente externo con `systemLog.destination: file`[cite: 6].
2. **¿Por qué el tamaño de 1 MB de `system.profile` es un problema de conservación y no solo de espacio?** Porque al funcionar como colección circular (*capped*), un pico de tráfico sobrescribe silenciosamente los eventos históricos más antiguos sin generar alertas, destruyendo la evidencia forense[cite: 6].
3. **¿Por qué `getLog` sirve para interpretar pero no para conservar?** Porque únicamente retiene los últimos 1024 eventos en memoria volátil del servidor y desaparece por completo ante cualquier reinicio del proceso[cite: 6].
