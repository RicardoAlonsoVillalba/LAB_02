# Actividad 5 — Accesos en la bitácora de mongod
**Estudiante:** Ricardo Alonso Villalba

## Respuestas e Interpretación
1. **¿Qué campo distingue un acceso fallido de uno exitoso?** El campo `msg` (`"Failed to authenticate"` frente a `"Successfully authenticated"`) y `attr.result` (`0` para éxito, códigos distintos de cero como `18` u `11` para fallo)[cite: 6].
2. **¿Qué diferencia hay entre el error de contraseña incorrecta y el de usuario inexistente? ¿Por qué el cliente recibe el mismo mensaje?** Internamente el servidor registra `AuthenticationFailed: SCRAM authentication failed` frente a `UserNotFound: Could not find user`[cite: 6]. Al cliente se le devuelve un mensaje genérico `Authentication failed` para impedir la enumeración de usuarios válidos[cite: 6].
3. **¿Por qué el nombre de la aplicación del intento fallido es `mongosh` y no `app-ventas-web`?** Porque en los comandos de prueba fallidos no se incluyó el parámetro `?appName=app-ventas-web` en la cadena de conexión, enviando el nombre por defecto del cliente[cite: 6].
