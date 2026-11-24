# Actividad 7 — Cambios de privilegios: el límite más importante
**Estudiante:** Ricardo Alonso Villalba

## Evaluación del Control Compensatorio
| Pregunta | ¿La instantánea responde? |
|---|---|
| ¿Cambiaron los privilegios de `app_ventas`? | **Sí**[cite: 6] |
| ¿Qué rol se agregó? | **Sí** (`dbAdmin` en `comercial`)[cite: 6] |
| ¿Quién lo agregó? | **No**[cite: 6] |
| ¿Cuándo exactamente? | **No**, solo que ocurrió entre dos instantáneas[cite: 6] |

## Respuestas a Preguntas
1. **¿Por qué un cambio de privilegios es uno de los eventos auditables más importantes?** Porque una escalada de privilegios permite a un usuario o atacante tomar control administrativo del servidor o acceder a datos confidenciales fuera de su rol[cite: 6].
2. **¿Con qué frecuencia tomarías instantáneas? ¿Qué factores influyen?** De forma diaria automatizada (o cada hora en sistemas críticos) y antes/después de ventanas de mantenimiento[cite: 6]. Influyen la criticidad de los datos, la frecuencia de cambios administrativos y los requisitos regulatorios[cite: 6].
3. **¿Quién debería poder escribir en `auditoria.instantaneas_privilegios`?** Exclusivamente una cuenta automatizada de auditoría o el administrador de seguridad (`root`), revocando permisos de escritura y borrado a usuarios de aplicación[cite: 6].
4. **Comunicación a Dirección (3 líneas):**
> En MongoDB Community Edition no existe registro nativo de cambios de permisos; el motor no audita comandos como `grantRolesToUser`[cite: 6]. Como control compensatorio implementamos instantáneas periódicas de roles que detectan *qué* privilegios cambiaron entre dos cortes, aunque no permiten atribuir *quién* ejecutó el cambio ni el segundo exacto[cite: 6]. Para trazabilidad forense completa en tiempo real se requiere migrar a MongoDB Enterprise o Atlas[cite: 6].
