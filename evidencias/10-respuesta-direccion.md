# Actividad 10 — Responder a Dirección
**Estudiante:** Ricardo Alonso Villalba

1. **¿Quién cambió el RFC de María García en la Actividad 2? ¿Y en la Actividad 3?**
   * *Actividad 2:* **No hay evidencia con esta edición** de quién ejecutó la modificación[cite: 6]. Aunque la bitácora de `mongod` muestra una conexión autenticada de `app_ventas` (`app-ventas-web`), el perfilador estaba apagado (`was: 0`), por lo que no se registró ninguna operación de escritura[cite: 6].
   * *Actividad 3:* El usuario **`app_ventas@comercial`**, registrado en `comercial.system.profile` bajo `op: 'update'` sobre `_id: 3`[cite: 6].
2. **¿Cuándo y desde qué aplicación?**
   * En la Actividad 3, en la marca temporal UTC registrada en el campo `ts` de `system.profile`, desde la IP **`127.0.0.1`** utilizando la aplicación **`app-ventas-web`**[cite: 6].
3. **¿Qué valor tenía antes?**
   * **No hay evidencia con esta edición.** El perfilador de MongoDB Community únicamente almacena la operación ejecutada (`$set: { rfc: "GAMA780912XYZ" }`), pero no conserva una copia del estado anterior del documento antes de ser modificado[cite: 6].
4. **¿Cambió alguien los privilegios de `app_ventas`?**
   * **No hay evidencia con esta edición** en los registros nativos del motor (`mongod.log` y `system.profile` devolvieron `0` eventos para `grantRolesToUser`)[cite: 6]. Mediante el control compensatorio de instantáneas (`auditoria.instantaneas_privilegios`) se detectó que entre dos cortes de tiempo se agregó y posteriormente revocó el rol `dbAdmin`, sin poder identificar al autor ni el instante exacto[cite: 6].
