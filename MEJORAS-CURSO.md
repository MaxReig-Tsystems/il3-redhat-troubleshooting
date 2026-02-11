# Propuesta de mejoras pedagógicas (sesión de 4 horas)

Este documento propone mejoras para que cada sesión tenga:

- **2 horas de base teórica sólida** (sin prisas, con narrativa de troubleshooting).
- **2 horas prácticas** (demo + laboratorio + debrief).

La idea es convertir el repositorio en una guía no solo de comandos, sino de **razonamiento técnico**.

---

## 1) Estructura recomendada por sesión (4h)

### Bloque 1 — Teoría guiada (2h)

**Objetivo:** construir modelo mental antes de teclear comandos.

1. **Mapa conceptual del tema (20 min)**
   - Qué problema resuelve cada tecnología.
   - Qué piezas intervienen y cómo se relacionan.
   - Qué síntomas típicos aparecen cuando falla.

2. **Fundamentos con ejemplos (45 min)**
   - Explicar conceptos con casos reales y anti-patrones.
   - Introducir vocabulario mínimo imprescindible.
   - Diferenciar “qué hace” vs “cómo se diagnostica”.

3. **Flujo de troubleshooting (35 min)**
   - Hipótesis iniciales.
   - Evidencias que necesito recoger.
   - Comandos de verificación por prioridad.
   - Criterio para decidir la siguiente acción.

4. **Errores frecuentes y checklist (20 min)**
   - Fallos operativos más comunes.
   - Cómo evitarlos con un checklist corto.

### Bloque 2 — Práctica aplicada (2h)

1. **Demo guiada (30 min)**: el instructor ejecuta el flujo completo y verbaliza su razonamiento.
2. **Lab por parejas (70 min)**: tareas escaladas (base → intermedio → reto).
3. **Debrief final (20 min)**: qué síntomas vieron, qué señales ignoraron, qué harían distinto.

---

## 2) Mejoras de contenido para el repositorio

### A. Añadir una sección fija en cada día: “Teoría que hay que entender”

Plantilla sugerida:

- Conceptos clave del día (5-10 puntos).
- Relaciones entre conceptos (mini diagrama textual).
- Riesgos típicos en producción.
- Señales de alerta en logs/comandos.
- Decisiones operativas: cuándo usar A y cuándo B.

### B. Añadir “Preguntas de diagnóstico” por tema

En vez de memorizar comandos, entrenar preguntas:

- ¿Qué cambió?
- ¿Desde cuándo falla?
- ¿El fallo es global o parcial?
- ¿Qué evidencia objetiva tengo?
- ¿Cuál es la hipótesis más barata de comprobar?

### C. Añadir “errores intencionados” en labs

Cada lab debería incluir 2-3 fallos sembrados:

- permisos incorrectos,
- unidad de systemd no habilitada,
- entrada de `/etc/fstab` mal formada,
- repositorio deshabilitado o no alcanzable.

Así se evalúa diagnóstico real, no ejecución mecánica.

### D. Añadir rúbrica de evaluación rápida

- Diagnóstico estructurado (0-2)
- Calidad de evidencias (0-2)
- Corrección técnica (0-2)
- Validación posterior al cambio (0-2)
- Comunicación técnica (0-2)

---

## 3) Recomendaciones específicas para tus primeras 2 horas

Si quieres dedicar las 2 primeras horas a “apartados con mucha miga”, usa este orden:

1. **Modelo mental del sistema** (capas: hardware → kernel → servicios → app).
2. **Método de troubleshooting** (hipótesis, evidencia, validación).
3. **Riesgo operativo** (qué tocar primero y qué no tocar sin rollback).
4. **Casos reales breves** (3 mini-incidencias, 5-7 min cada una).

Esto reduce muchísimo el bloqueo durante la parte práctica.

---

## 4) Cambios editoriales sugeridos

- Mantener README de cada día como “vista ejecutiva”.
- Crear documentos de teoría por día (ej. `teoria-dia1.md`, `teoria-dia2.md`).
- Añadir al inicio de cada lab una sección: **“Qué vas a diagnosticar”**.
- Cerrar cada día con una sección: **“Señales que debes recordar mañana”**.

---

## 5) Resultado esperado

Con este enfoque, el alumno:

- memoriza menos comandos sueltos,
- entiende mejor causa-efecto,
- mejora la autonomía en incidencias nuevas,
- y llega al bloque práctico con menos frustración.
