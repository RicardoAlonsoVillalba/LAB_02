# Actividad 6 — Correlacionar un cambio de configuración con su autor
**Estudiante:** Ricardo Alonso Villalba

## Respuestas e Interpretación
1. **¿Qué usuario cambió la configuración del perfilador? ¿Qué dos eventos lo demuestran?** El usuario `admin@admin`[cite: 6]. Lo demuestran el evento `Profiler settings changed` (que registra el cambio y su hilo `ctx`) y el evento previo `Successfully authenticated` con ese mismo identificador `ctx`[cite: 6].
2. **¿Qué condición debe cumplirse para que esta correlación sea válida?** Debe ocurrir dentro del mismo ciclo de ejecución del proceso `mongod` (sin reinicios intermedios), ya que los identificadores `connX` vuelven a empezar desde `conn1` al reiniciar el servicio[cite: 6].
3. **¿Por qué es relevante que quede registrado un cambio de configuración del propio mecanismo de registro?** Porque permite detectar si un administrador o atacante desactivó o alteró el nivel de perfilado para ocultar operaciones posteriores[cite: 6].
