# AGENTS_SPEC.md — Especificación de Agentes APEX

Referencia canónica de todos los agentes usados en el framework. Define rol, fase, inputs, outputs y notas de comportamiento.

> **Nota de implementación:** Los agentes listados aquí son **roles de análisis** que APEX despacha via el Agent tool (Claude Code) o el Codex Agents SDK (apex-oc). No son implementaciones fijas — APEX pasa el rol y el contexto al modelo, que lo ejecuta con los inputs provistos.

---

## Agentes por fase

### Fase 01 — Inicio

| Agente | Rol | Inputs | Output esperado |
|--------|-----|--------|----------------|
| `product-manager` | Genera el draft de PROJECT.md con visión, objetivo, OKRs y restricciones | Respuestas de entrevista Grupos 1-3 + datos de MODO NUEVO | `PROJECT.md` draft completo |
| `business-analyst` | Valida que el objetivo sea alcanzable con los constraints definidos | Draft de `PROJECT.md` | Lista de inconsistencias o validación positiva |

**Comportamiento esperado:**
- `product-manager`: proponer 2-3 formulaciones de OKRs para que el PM elija
- `business-analyst`: ser adversarial — buscar razones por las que el proyecto podría fallar dado el scope y restricciones

---

### Fase 02 — Investigación

| Agente | Rol | Inputs | Output esperado |
|--------|-----|--------|----------------|
| `market-researcher` | Dimensiona el mercado, segmentos y dinámicas | `PROJECT.md` + respuestas Grupo A | Tamaño de mercado, segmentación, fuerzas del mercado |
| `competitive-analyst` | Benchmarking de competidores, gaps y posicionamiento | `PROJECT.md` + lista de competidores del PM | Tabla comparativa, gaps identificados, oportunidades |
| `trend-analyst` | Tendencias emergentes con curvas de adopción y horizonte temporal | `PROJECT.md` + horizonte temporal del PM | Tendencias priorizadas por relevancia e impacto |

**Comportamiento esperado:**
- Los 3 se despachan en paralelo
- Cada agente debe incluir sus fuentes en el output
- `research-synthesizer` (ver Fase 03) sintetiza los 3 outputs en `RESEARCH.md`

---

### Fase 03 — Estrategia

| Agente | Rol | Inputs | Output esperado |
|--------|-----|--------|----------------|
| `product-strategist` | Roadmap priorizado con rationale estratégico | `PROJECT.md` + `RESEARCH.md` + respuestas Grupos A-B | Roadmap con fases, milestones y justificación |
| `risk-manager` | Risk register con probabilidad, impacto y mitigaciones | `PROJECT.md` + `RESEARCH.md` + respuestas Grupo C | Tabla de riesgos + suposiciones críticas con método de validación |
| `research-synthesizer` | Sintetiza outputs de `product-strategist` y `risk-manager` en `STRATEGY.md` | Outputs de ambos agentes | Draft completo de `STRATEGY.md` |

**Comportamiento esperado:**
- `product-strategist` y `risk-manager` se despachan en paralelo; `research-synthesizer` espera a ambos
- `risk-manager` debe ser explícito sobre suposiciones que, si fallan, invalidan toda la estrategia
- `research-synthesizer` no inventa — solo sintetiza y da coherencia

---

### Fase 04 — Ejecución

#### Gestión y coordinación

| Agente | Rol | Inputs | Output esperado |
|--------|-----|--------|----------------|
| `agile-product-owner` | Backlog priorizado, criterios de aceptación y criterios de cierre del sprint | `STRATEGY.md` + contexto del sprint | Backlog ordenado + criterios de cierre verificables |
| `project-manager` | Tracking de avance, dependencias y riesgos de ejecución | `SPRINT.md` en curso | Actualizaciones de estado, alertas de bloqueos |
| `workflow-orchestrator` | Coordinación de procesos multi-paso con múltiples componentes | Plan de ejecución + dependencias entre tareas | Secuencia de ejecución con handoffs claros |

#### Implementación técnica (tipo `software`/`mixed`)

| Agente | Activa si PROJECT.md contiene | Rol |
|--------|-------------------------------|-----|
| `senior-frontend` | Next.js, React, Vue, frontend, interfaz, componente | Implementación de UI/UX, componentes, estado |
| `senior-backend` | Python, FastAPI, Django, Node, backend, API, servidor | Implementación de APIs, lógica de negocio, BD |
| `senior-devops` | Docker, Kubernetes, Terraform, infra, infraestructura, nube | Infraestructura, CI/CD, deployments |
| `senior-prompt-engineer` | AI, LLM, Claude, Gemini, embeddings, modelo, agente, IA | Prompts, chains, RAG, integración de modelos |
| `senior-data-engineer` | Data, pipeline, ETL, SQL, datos, análisis, base de datos | Pipelines de datos, transformaciones, schemas |

**Si ningún keyword detectado:** preguntar al PM qué stack se usa.

#### Agentes Caveman (disponibles durante toda la fase 04)

| Agente | Rol | Cuándo usar |
|--------|-----|-------------|
| `caveman:cavecrew-builder` | Edits quirúrgicos, implementación táctica en 1-2 archivos | Correcciones puntuales, funciones pequeñas |
| `caveman:cavecrew-investigator` | Búsqueda de código, localización de símbolos y referencias | Encontrar dónde está definido X, qué llama a Y |
| `caveman:cavecrew-reviewer` | Review de diffs antes de merge, hallazgos con severidad | Antes de cada merge a main |

---

### Fase 04b — Auditoría

> Estos son **roles inline** — APEX los desempeña directamente, no los despacha como agents separados. Solo `caveman:caveman-review` es externo.

| Rol | Tipo | Alcance | Aplica a |
|-----|------|---------|----------|
| `security-review` | Inline | Vulnerabilidades, secrets hardcodeados, permisos, OWASP Top 10 | `software` / `mixed` |
| `code-reviewer` | Inline | Calidad de código, antipatterns, cobertura de tests, deuda técnica | `software` / `mixed` |
| `senior-qa` | Inline | Estrategia de tests, edge cases no cubiertos, regresiones posibles | `software` / `mixed` |
| `review` | Inline | Consistencia del entregable vs PROJECT.md y STRATEGY.md | `strategy` / `marketing` |
| `caveman:caveman-review` | Externo | Review del diff final antes de merge | `software` / `mixed` (si Caveman disponible) |

---

### Fase 05 — Cierre

| Agente | Rol | Inputs | Output esperado |
|--------|-----|--------|----------------|
| `report-generator` | Draft del entregable final en el formato solicitado por el PM | Todos los docs del proyecto + Grupo A de entrevista | Presentación / informe / doc técnico |
| `research-synthesizer` | Síntesis de lecciones aprendidas del proyecto completo | Todos los docs + `LOG.md` | Sección de lecciones para `CLOSE.md` |
| `data-analyst` | Análisis cuantitativo de resultados (opcional) | Métricas y resultados del proyecto | Análisis con visualizaciones o tablas |

**Comportamiento esperado:**
- `report-generator` y `research-synthesizer` se despachan en paralelo
- `data-analyst` solo si hay datos cuantitativos — no forzar si no hay métricas reales
- `report-generator` adapta tono al destinatario (Grupo A de entrevista)

---

## Skills externos (no agents)

| Skill | Plataforma | Fase | Cuándo |
|-------|-----------|------|--------|
| `/brainstorming` | Superpowers (apex-cc) / `$apex-brainstorming` (apex-oc) | 01 | Hard-gate al definir primera feature |
| `/writing-plans` | Superpowers (apex-cc) / `$apex-writing-plans` (apex-oc) | 04 | Al generar el plan de ejecución |
| `/using-git-worktrees` | Superpowers (apex-cc) / Codex nativo (apex-oc) | 04 | Antes de implementar (`software`/`mixed`) |
| `marketing-strategy-pmm` | Skills externos | 02 | Al sintetizar para posicionamiento (`marketing`/`mixed`) |
| `senior-data-scientist` | Skills externos | 02 | Si hay data cuantitativa para analizar |
| `pricing-strategy` | Skills externos | 03 | Si hay componente de monetización |
| `launch-strategy` | Skills externos | 03 | Si hay lanzamiento al mercado |
| `ceo-advisor` | Skills externos | 03 | Si hay decisiones de modelo de negocio de alto impacto |
| `anthropic-skills:pptx` | Skills externos | 05 | Entregable es presentación |
| `anthropic-skills:docx` | Skills externos | 05 | Entregable es informe formal |
| `content-research-writer` | Skills externos | 05 | Entregable es documento narrativo largo |

---

## Notas de despacho

1. **Paralelo vs secuencial** — ver instrucciones en cada fase. Nunca asumir paralelo si la fase no lo indica.
2. **Inputs mínimos** — cada agente necesita los docs de las fases previas en contexto. APEX los pasa explícitamente.
3. **Roles inline (04b)** — no se despachan como Agent tool. APEX ejecuta el análisis directamente con el rol especificado.
4. **Cavecrew agents** — requieren Caveman instalado. Si no está disponible, APEX usa `claude` / `general-purpose` como equivalente con menor eficiencia de tokens.
