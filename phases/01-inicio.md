# Fase 01 — Inicio

## Contexto

Establece la base del proyecto. Produce `PROJECT.md` validado con visión, objetivo, restricciones y definición de éxito. Configura el calendario de trabajo. La aprobación del PM sobre este documento es el gate obligatorio antes de investigar o estrategizar.

**Tipo de proyecto**: todos (`software` / `strategy` / `marketing` / `mixed`)

---

## Entrevista

> Si el proyecto llegó aquí desde MODO NUEVO, los datos de identidad (nombre, tipo, industria), equipo (PM, roles, cliente) y alcance (objetivo, restricciones, éxito) ya están disponibles en apex.config.json y PROJECT.md. Pre-llenar con esos datos — solo preguntar Grupos A, B y C si falta información específica de mercado/usuario. El Grupo D (calendario) siempre se hace completo.

### Grupo A — Contexto de mercado
1. ¿En qué mercado o industria opera este proyecto?
2. ¿Quiénes son los competidores o referencias más relevantes?
3. ¿Hay alguna investigación previa o dato de contexto disponible?

### Grupo B — Usuarios y valor
4. ¿Quién es el usuario/beneficiario final?
5. ¿Cuál es el problema central que este proyecto resuelve?
6. ¿Qué valor genera y para quién?

### Grupo C — Constraints y éxito
7. ¿Cuáles son las restricciones más críticas? (tiempo, presupuesto, tecnología, regulación)
8. ¿Cómo se mide el éxito? ¿Qué métricas o entregables lo confirman?
9. ¿Hay decisiones ya tomadas que no están a discusión?

### Grupo D — Calendario de trabajo
> Siempre preguntar, incluso si el proyecto viene de MODO NUEVO.

10. ¿Cuál es tu zona horaria? (default: `America/Guayaquil`)
11. ¿Cuáles son tus días hábiles? (default: lunes a viernes)
12. ¿Cuál es tu horario de trabajo? (default: 09:00–18:00)
13. ¿Hay feriados o días bloqueados en el horizonte del proyecto? (opcional)

> Guardar respuestas en `apex.config.json → workCalendar`. Crear `apex-time.log` usando `templates/apex-time.log` como base con el nombre del proyecto y timezone.

---

## Agents a despachar

**Despachar en paralelo** — hacer ambas llamadas al Agent tool en el mismo mensaje:

| Agent | Input | Output esperado |
|-------|-------|----------------|
| `product-manager` | Respuestas de entrevista + datos de MODO NUEVO | Draft de `PROJECT.md` con OKRs |
| `business-analyst` | Draft de PROJECT.md | Validación: ¿el objetivo es alcanzable con los constraints? |

---

## Skills a invocar

### `/brainstorming` — HARD-GATE (con Superpowers)

Invocar obligatoriamente al definir la primera feature, workstream o módulo.

- APEX invoca `/brainstorming`, no lo reemplaza
- El PM pasa por el flujo completo de Superpowers
- APEX espera que `/brainstorming` termine y produzca su spec aprobada
- Sin esta aprobación no se avanza a Fase 02

### Modo degradado (sin Superpowers)

Reemplazar `/brainstorming` con proceso interno:
1. APEX pregunta: ¿Cuál es la primera feature o workstream a definir?
2. APEX genera spec estructurada (contexto, decisiones, tasks, criterios de aceptación)
3. PM aprueba la spec generada
4. Gate cumplido con spec interna aprobada — avanzar a Fase 02

---

## Output

| Documento | Path | Contenido |
|-----------|------|-----------|
| `PROJECT.md` | raíz del proyecto | Visión, objetivo, constraints, definición de éxito, OKRs |
| `apex-time.log` | raíz del proyecto | Inicializado con DAY_START y PHASE_START |
| Spec de brainstorming | `docs/plans/` | Producida por Superpowers o APEX — archivada aquí |

---

## Gate de aprobación

**Con Superpowers:**
- [ ] PM aprueba `PROJECT.md` explícitamente
- [ ] Spec de brainstorming aprobada
- [ ] `apex.config.json` actualizado: `currentPhase → 02-investigacion`, fase 01 en `completedPhases`, `phases.01-inicio.closedAt` con timestamp actual
- [ ] `apex-time.log` creado con entradas iniciales

**Sin Superpowers (modo degradado):**
- [ ] PM aprueba `PROJECT.md` explícitamente
- [ ] PM aprueba spec interna generada por APEX
- [ ] `apex.config.json` actualizado igual que arriba

> **Regla: una fase por sesión.** Al completar este gate, informar al PM y cerrar la fase. No iniciar Fase 02 en la misma sesión.

---

## Qué escribir a memoria al cerrar

**`~/.claude/apex/patterns.md`:**
- Tipo de proyecto + industria
- Iteraciones de PROJECT.md antes de aprobación
- Decisiones más debatidas en Grupo C

**`~/.claude/apex/pm-profile.md`:**
- Velocidad observada (¿aprobó rápido o iteró mucho?)
- Nivel de detalle pedido en PROJECT.md
- ¿Aceptó los OKRs propuestos o los reformuló?
- Zona horaria y patrón de horario de trabajo
