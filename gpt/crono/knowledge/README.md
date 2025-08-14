# Cronos Knowledge

**Propósito**: Centralizar el conocimiento y recursos para que **Cronos** ejecute **priorización de tareas** y **estimación de tiempos** de forma consistente, trazable y optimizada según el contexto del usuario o equipo.

## Contenido recomendado

* Principios y objetivos de priorización y estimación (orientación a decisión, trazabilidad, no alucinación).
* Esquemas de entrada (JSON por tarea, lote, lenguaje natural).
* Reglas de normalización y validación de datos.
* Funciones de cálculo (WSJF modificado, ponderación lineal, severidad × alcance, dependencias).
* Reglas de desempate y modos de ajuste (Incendios, Roadmap, Throughput).
* Modelos de estimación de tiempo (por historia, por referencia histórica, por técnica PERT).
* Formatos de salida (resumen ejecutivo, explicación detallada, JSON).
* Ejemplos aplicados y métricas para evaluación.

---

## Plantilla — Solicitud de priorización y estimación rápida

```markdown


**Objetivo del análisis:**
Definir una priorización objetiva y accionable de las tareas del sprint/proyecto, optimizando recursos y maximizando el valor entregado al negocio, reduciendo riesgos de cuellos de botella y entregas fuera de plazo. - Ejemplo

**Contexto (equipo, proyecto, sprint):**
Equipo de desarrollo backend y frontend trabajando en el Proyecto *Orion* (sprint actual: #24, duración: 2 semanas, enfoque en release preproducción). - Ejemplo

**Número de tareas a evaluar:**
7 tareas en backlog activo. - Ejemplo

**Modo de priorización:**
**Roadmap** — alineado con objetivos de release, priorizando impacto en funcionalidades críticas y dependencias. - Ejemplo

**Definición de éxito (criterio observable):**
Todas las tareas críticas del release completadas antes del *freeze* de preproducción (día 9 del sprint) con un mínimo de 90% de cumplimiento en estimaciones de tiempo. - Ejemplo

---

**Formato de entrada (JSON/lenguaje natural):**
Ejemplo JSON:

```json
[
  {
    "id": "T-145",
    "titulo": "Implementar endpoint de facturación",
    "dependencias": ["T-122"],
    "deadline": "2025-08-19",
    "estimacion": "2 días",
    "impacto": "alto"
  },
  {
    "id": "T-146",
    "titulo": "Optimizar query de inventario",
    "dependencias": [],
    "deadline": null,
    "estimacion": "1 día",
    "impacto": "medio"
  }
]

Ejemplo en lenguaje natural:

1. Implementar endpoint de facturación (depende de T-122, deadline 19/08, alto impacto, estimado 2 días)
2. Optimizar query de inventario (sin dependencias, impacto medio, estimado 1 día)
3. Crear script de migración de clientes legacy (sin deadline, impacto alto, estimado 3 días)
```



---

**Preguntas guía:**

* ¿Hay *deadlines* próximos que condicionen la priorización?
* ¿Existen dependencias entre tareas?
* ¿Faltan datos clave para calcular el *score*?
* ¿Se cuenta con datos históricos de duraciones para estimar tiempos?

---

**Observaciones (riesgos, datos faltantes, notas del equipo):**

* Falta definición de criterios de aceptación para T-148.
* El equipo de QA está al 60% de capacidad esta semana.
* Dependencia externa con API de terceros para T-145 (riesgo de retraso).

---

**Resultados esperados:**

* **Lista ordenada por prioridad** con *score*, ETA y justificación breve.
* **Estimación de duración por tarea** (en horas, días o sprints según contexto).
* **Sugerencia de siguientes pasos** para hoy y esta semana.

**Ejemplo de salida:**

```json
[
  {
    "id": "T-145",
    "prioridad": 1,
    "score": 95,
    "ETA": "2 días",
    "justificacion": "Alta criticidad para release, deadline cercano, dependencia resuelta."
  },
  {
    "id": "T-148",
    "prioridad": 2,
    "score": 85,
    "ETA": "1.5 días",
    "justificacion": "Impacto alto, sin dependencia crítica, pero falta detalle de criterios de aceptación."
  }
]
``

```

---

## Matriz de severidad de impacto (para bugs/incidencias)

* **S1 (Crítico)**: bloquea función clave o afecta a la mayoría de usuarios.
* **S2 (Importante)**: degrada la experiencia o retrasa entregables.
* **S3 (Menor)**: impacto limitado, mejora cosmética o técnica.

---

## Registro de resultados sugerido

```markdown
| ID   | Título                 | Score | Orden | ETA estimada | Justificación breve                 | Bloquea |
|------|------------------------|-------|-------|--------------|--------------------------------------|---------|
| T-01 | Corregir bug de login  | 8.6   | 1     | 0.5 días     | Alto impacto, urgencia alta, esfuerzo bajo | T-05    |
```

---

## Guía breve de microcopy para comunicación de resultados

* Usar frases directas y accionables (“Atender hoy”, “En el próximo sprint”).
* Mostrar siempre el **porqué** de la prioridad y el **cómo** de la estimación.
* Evitar tecnicismos innecesarios en la explicación al usuario final.
* Mantener consistencia en formato: `# — score — ETA — razón`.

---

## Checklist de validación de datos antes de calcular

* Impacto presente y en rango 1–10.
* Urgencia presente y en rango 1–5.
* Esfuerzo presente y ≥ 1.
* Dependencias validadas contra el lote de tareas.
* Deadlines convertidos a formato estándar `YYYY-MM-DD`.
* Datos para estimación presentes: esfuerzo, velocidad del equipo, referencias históricas.

---

## Ejemplo aplicado al contexto de un sprint

### Entrada (modo natural)

> "Tengo: (1) bug login caído (afecta 30% usuarios), (2) nueva métrica en dashboard, (3) refactor auth; estimaciones: 3, 5 y 8 puntos; deadline del dashboard en 10 días. Velocidad de equipo: 10 puntos/sprint."

### Salida (resumen ejecutivo con ETA)

```markdown
1. T-1 Bug login — score 8.6 — ETA: 0.5 días — Hoy
   Razón: Alto impacto (30% usuarios), urgencia alta, esfuerzo 3 ⇒ valor/tiempo superior.
2. T-2 Dashboard métrica — score 6.9 — ETA: 2.5 días — Esta semana
   Razón: Deadline 10 días, valor medio, esfuerzo 5.
3. T-3 Refactor auth — score 4.1 — ETA: 4 días — Próximo sprint
   Razón: Mejora técnica sin urgencia; esfuerzo 8.
```
---

## Integración con el flujo de trabajo
* En caso de conflicto, prevalece la guía en `instructions_crono.md`.
* Mantener ejemplos actualizados y métricas de aceptación del modelo.
