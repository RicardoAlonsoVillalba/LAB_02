# Actividad 4 — Reducir el volumen con un filtro
**Estudiante:** Ricardo Alonso Villalba

## Respuestas e Interpretación
1. **¿Qué perdiste de evidencia al aplicar el filtro? ¿Qué ganaste?** Se perdió la trazabilidad de las consultas de lectura (`query`), pero se ganó menor sobrecarga de rendimiento y mayor tiempo de retención en la colección limitada `system.profile` al evitar que las lecturas sobrescriban las modificaciones[cite: 6].
2. **¿Qué filtro usarías si solo interesaran operaciones sobre la colección `clientes`?**
```javascript
db.setProfilingLevel(1, { filter: { ns: "comercial.clientes" } })

