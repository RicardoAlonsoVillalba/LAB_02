# Actividad 2 — El incidente sin perfilado
**Estudiante:** Ricardo Alonso Villalba

## Respuestas e Interpretación
1. **¿Puedes demostrar qué cambió? ¿Y quién se conectó a esa hora?** No hay evidencia con esta edición sobre qué documento o campo cambió (`system.profile` tiene `0` registros). En la bitácora de `mongod` sí consta que `app_ventas` (`app-ventas-web`) se conectó y autenticó, pero no qué operación ejecutó[cite: 6].
2. **¿Por qué `mongosh` abre varias conexiones (`connX`, `connY`, ...) para una sola instrucción? ¿Cuál es la que se autentica?** El driver moderno abre un pool de conexiones (monitoreo de topología y ejecución)[cite: 6]. La primera negocia metadatos sin autenticar (`Connection not authenticating`) y la segunda realiza la autenticación SCRAM (`Successfully authenticated`) para ejecutar la operación[cite: 6].
