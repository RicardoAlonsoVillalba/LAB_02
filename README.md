# LAB-02 — Trazabilidad en MongoDB Community

## Despliegue y Versión del Servidor
* **Motor:** MongoDB 7 Community Edition (`buildInfo.modules` -> `[]`)[cite: 6]
* **Despliegue:** Contenedor Docker oficial (`docker run --name dba-u3-mongo -p 27017:27017 -d mongo:7 --auth`)[cite: 6]
* **Base de datos y roles:** Base `comercial` con colección `clientes`; roles `admin` (`root`) y `app_ventas` (`readWrite`)[cite: 6]

## Estructura del Repositorio
* `scripts/comandos-mongosh.js`: Funciones auxiliares (`bitacora()`), configuración de perfilado filtrado y toma de instantáneas de privilegios[cite: 6].
* `evidencias/`: Documentación técnica de las Actividades 1 a 10, volcado `system-profile.json`, bitácora exportada `bitacora-mongod.log` y sumas de verificación `hashes.sha256`[cite: 6].
* `informe.md`: Reporte técnico completo, análisis comparativo y preguntas de reflexión[cite: 6].
