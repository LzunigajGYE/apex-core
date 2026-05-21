# APEX Core

Framework de gestión de proyectos con IA — plataforma-agnóstico.

Contiene las fases, templates de documentos y schemas de memoria que comparten todos los adapters de APEX. No se instala directamente — es consumido como git submodule por los adapters.

---

## Contenido

```
apex-core/
├── phases/
│   ├── 01-inicio.md          ← Fase 01: PROJECT.md + brainstorming
│   ├── 02-investigacion.md   ← Fase 02: RESEARCH.md
│   ├── 03-estrategia.md      ← Fase 03: STRATEGY.md
│   ├── 04-ejecucion.md       ← Fase 04: SPRINT.md (bifurcación por tipo)
│   ├── 04b-auditoria.md      ← Fase 04b: entrada en LOG.md
│   └── 05-cierre.md          ← Fase 05: CLOSE.md + entregable final
├── templates/
│   ├── apex.config.json      ← config de proyecto (estado, fases, aprobaciones)
│   ├── PROJECT.md            ← visión, objetivo, constraints, OKRs
│   ├── TEAM.md               ← PM, equipo, stakeholders, cliente
│   └── LOG.md                ← trazabilidad desde el primer día
├── memory/
│   ├── pm-profile.md         ← schema: perfil del PM (cross-proyecto)
│   └── patterns.md           ← schema: patrones aprendidos (cross-proyecto)
└── Docs/
    └── apex-skill-design.md  ← spec maestra del framework
```

---

## Adapters disponibles

| Adapter | Plataforma | Repo |
|---------|-----------|------|
| APEX CC | Claude Code | [LzunigajGYE/apex-cc](https://github.com/LzunigajGYE/apex-cc) |
| APEX OC | Codex (Desktop + CLI) | [LzunigajGYE/apex-oc](https://github.com/LzunigajGYE/apex-oc) |

---

## Uso como submodule

```bash
git submodule add https://github.com/LzunigajGYE/apex-core core
git submodule update --init --recursive
```

---

## Créditos

APEX Framework — Luis Zúñiga — [github.com/LzunigajGYE](https://github.com/LzunigajGYE)
