# Informe Técnico — LAB-02: Trazabilidad en MongoDB Community

## 1. Despliegue, edición y limitación
El laboratorio se desplegó sobre un contenedor Docker con la imagen oficial `mongo:7` arrancado con la bandera `--auth` (`security.authorization: enabled`)[cite: 6]. Mediante `db.adminCommand({ buildInfo: 1 }).modules` se verificó que el arreglo de módulos está vacío (`[]`), confirmando que se trata de **MongoDB Community Edition**[cite: 6]. Esta edición **carece de auditoría nativa** (`--auditDestination`), característica exclusiva de MongoDB Enterprise y MongoDB Atlas[cite: 6]. En consecuencia, cualquier evento no cubierto por el perfilador de rendimiento o por la bitácora básica del servidor carece de evidencia nativa[cite: 6].

## 2. Perfilador (`Database Profiler`)
El perfilador es una herramienta diseñada para diagnóstico de rendimiento, no para auditoría de seguridad[cite: 6]. Se activó inicialmente en **nivel 2** (`db.setProfilingLevel(2)`), registrando todas las operaciones (`query`, `update`, `remove`) en la colección `comercial.system.profile` con usuario (`user`), IP (`client`), aplicación (`appName`) y marca temporal (`ts`)[cite: 6]. Debido al alto costo de desempeño y al riesgo de saturación, se reconfiguró a **nivel 1 con filtro** (`filter: { op: { $in: ["insert", "update", "remove"] } }`) para excluir lecturas[cite: 6]. Su principal limitación forense es que registra el comando ejecutado, pero **no guarda el valor anterior** (`OLD`) del documento modificado[cite: 6].

## 3. Bitácora de `mongod`
En este despliegue en contenedor sin `systemLog.destination: file`, `mongod` escribe sus eventos en formato JSON estructurado hacia la salida estándar, consultable en memoria reciente mediante `db.adminCommand({ getLog: "global" })`[cite: 6]. Permitió auditar intentos de acceso fallidos (`Failed to authenticate` por contraseña errónea y usuario inexistente) y exitosos (`Successfully authenticated`)[cite: 6]. Asimismo, aunque el evento `Profiler settings changed` no incluye el nombre de usuario, fue posible atribuir el cambio a `admin@admin` correlacionando el identificador de conexión `ctx` (`connX`) con su autenticación previa[cite: 6].

## 4. Privilegios: evidencia disponible y control compensatorio
MongoDB Community **no registra cambios de privilegios** (`grantRolesToUser`, `revokeRolesFromUser`, `createUser`) ni en `system.profile` ni en `mongod.log`[cite: 6]. Para mitigar este vacío se implementó un **control compensatorio** basado en instantáneas periódicas almacenadas en `auditoria.instantaneas_privilegios`[cite: 6]. Si bien este mecanismo demuestra *qué* roles cambiaron entre dos cortes de tiempo, **no equivale a una auditoría nativa** porque no registra *quién* realizó la modificación ni la marca temporal exacta del cambio[cite: 6].

## 5. Conservación
La evidencia en MongoDB Community es altamente volátil si no se exporta proactivamente[cite: 6]:
1. `system.profile` es una colección circular (*capped collection*) de **1 MB** por defecto; al llenarse sobrescribe los documentos más antiguos[cite: 6].
2. `getLog` solo retiene los últimos **1024 eventos** en memoria RAM y se vacía al reiniciar el servicio[cite: 6].
3. El nivel de perfilado configurado por consola vuelve a `0` tras un reinicio si no se persiste en `mongod.conf`[cite: 6].
Por ello, se exportaron `system-profile.json` y `bitacora-mongod.log` antes de reiniciar el contenedor y se aseguraron mediante sumas criptográficas en `evidencias/hashes.sha256`[cite: 6].

## 6. Comparación (PostgreSQL vs. MongoDB Community vs. Enterprise/Atlas)
Mientras que PostgreSQL (con `logging_collector`, `log_statement = 'mod'` y disparadores `SECURITY DEFINER`) permite auditar accesos, DDL, privilegios, errores y reconstruir el valor anterior exacto (`OLD` en `JSONB`)[cite: 6], **MongoDB Community** solo ofrece trazabilidad parcial orientada a desempeño (sin valores anteriores y sin registro de cambios de privilegios)[cite: 6]. Por su parte, **MongoDB Enterprise / Atlas** incorpora auditoría nativa capaz de registrar eventos DDL, autenticación, autorización y CRUD en archivos JSON/BSON o `syslog`[cite: 6].

## 7. Recomendación
Para una organización que requiera auditar cambios de privilegios y operaciones críticas en MongoDB con validez forense, se recomienda **migrar a MongoDB Enterprise o MongoDB Atlas (clústeres M10+)** habilitando la auditoría nativa hacia un servidor externo centralizado (SIEM)[cite: 6].
* **Hechos verificados en este laboratorio:** MongoDB Community no registra `grantRolesToUser`, `system.profile` sobrescribe datos al superar 1 MB y `getLog` se pierde al reiniciar[cite: 6].
* **Supuestos operativos:** Se asume que la organización cuenta con presupuesto para licenciamiento Enterprise/Atlas y que los desarrolladores no tienen acceso administrativo (`root`) en producción[cite: 6].

---

## Preguntas de Reflexión
1. **¿Por qué el perfilador no debe presentarse como un mecanismo de auditoría de seguridad?** Porque fue diseñado para diagnosticar consultas lentas, no protege contra alteraciones ni borrados (`db.system.profile.drop()`), sobrescribe registros al llegar a 1 MB, no guarda valores anteriores y omite cambios de privilegios[cite: 6].
2. **¿Por qué el perfilador sí aporta trazabilidad aunque no sea auditoría?** Porque vincula cada operación ejecutada (`op`, `command`) con el usuario autenticado (`user`), la dirección IP (`client`), la aplicación (`appName`) y el instante exacto (`ts`), permitiendo investigar incidentes operativos recientes[cite: 6].
3. **¿Qué riesgo introduce el nivel 2 al registrar filtros de consultas que pueden contener datos personales?** Convierte a `system.profile` en un repositorio secundario no cifrado de datos sensibles (RFC, correos, teléfonos incluidos en cláusulas `find`), exponiendo información confidencial a cualquiera que pueda leer la base[cite: 6].
4. **¿Qué diferencia hay entre detectar un cambio y atribuirlo a alguien?** Detectar un cambio significa saber que un dato o permiso fue alterado (como ocurre con las instantáneas de privilegios); atribuirlo implica vincular de forma irrefutable esa acción con una identidad, origen y momento específicos[cite: 6].
5. **¿Por qué reconocer una limitación técnica es parte del trabajo profesional del DBA?** Porque ocultarla o simular controles inexistentes genera una falsa sensación de seguridad ante auditorías internas o incidentes legales; documentar el límite permite diseñar controles compensatorios reales o justificar inversiones en infraestructura[cite: 6].
