# Biblioteca de Prompts — PM@Siemens AI Workspace

Esta carpeta contiene prompts reutilizables de IA para flujos de trabajo de gestion de proyectos PM@Siemens. Cada prompt sigue una estructura consistente: frontmatter YAML, proposito, patron de invocacion, pasos de procesamiento, plantilla de salida y checklist de validacion.

## Referencia Rapida

| Prompt | Proposito | Cuando usar | Agent |
|--------|-----------|-------------|-------|
| [session-recovery](session-recovery.prompt.md) | Recuperar contexto al inicio de sesion | **Cada inicio de sesion** | ask |
| [meeting-summary](meeting-summary.prompt.md) | Transformar notas crudas en minutas compliance | Despues de cualquier reunion | ask |
| [weekly-status](weekly-status.prompt.md) | Generar status semanal con dashboard de salud | Cadencia semanal (cada lunes) | ask |
| [financial-status](financial-status.prompt.md) | Analizar datos financieros y generar informe | Mensual o pre-gate | ask |
| [risk-analysis](risk-analysis.prompt.md) | Identificar y evaluar O&R usando proceso de 6 pasos | Revisiones periodicas, nueva informacion, pre-gate | ask |
| [task-breakdown](task-breakdown.prompt.md) | Descomponer entregables en tareas de backlog | Nuevos paquetes de trabajo, adiciones de alcance | agent |
| [stakeholder-update](stakeholder-update.prompt.md) | Redactar comunicaciones calibradas por audiencia | Actualizaciones de estado, escalaciones, aprobaciones | ask |
| [milestone-readiness](milestone-readiness.prompt.md) | Evaluar preparacion para Quality Gate | 2-4 semanas antes de cualquier gate | ask |
| [scope-impact](scope-impact.prompt.md) | Evaluar solicitudes de cambio en todas las dimensiones | Solicitudes de cambio recibidas | ask |
| [input-classifier](input-classifier.prompt.md) | Clasificar y enrutar nuevo material de entrada | Nuevo material en `.github/input/` | agent |
| [closeout-review](closeout-review.prompt.md) | Verificar requisitos de cierre PM700 | Cierre de proyecto | ask |

## Cadencia Recomendada

| Frecuencia | Prompt(s) | Disparador |
|------------|-----------|-----------|
| **Cada sesion** | `session-recovery` | Inicio de cualquier sesion de trabajo con IA |
| **Semanal** | `weekly-status` | Lunes por la manana |
| **Tras reuniones** | `meeting-summary` | Notas crudas depositadas en `meetings/raw/` |
| **Mensual** | `risk-analysis` (periodico), `financial-status` (periodico) | Revision programada |
| **Por evento** | `scope-impact`, `input-classifier`, `stakeholder-update` | Llega nuevo input |
| **Pre-gate (2-4 semanas)** | `milestone-readiness`, `financial-status` (pre-gate) | Milestone proximo |
| **En cierre** | `closeout-review` | Preparacion PM700 |

## Patron de Invocacion

Todos los prompts usan un formato de invocacion consistente:

```
Follow instructions in #prompt:<prompt-name>.prompt.md with these arguments:
  @file:<input-file-path>          (si se necesita fichero de entrada)
  --parameter-name [value]          (parametros especificos del prompt)
  --project-folder @workspace       (incluir siempre para lecturas de contexto)
```

## Agentes de IA

Los prompts declaran un valor `agent` en su frontmatter YAML que determina como se ejecuta el prompt:

| Valor de `agent` | Comportamiento | Modifica ficheros? |
|------------------|---------------|-------------------|
| **ask** | Analisis, resumen, evaluacion, comunicacion — responde en chat | No — produce solo salida en chat |
| **agent** | Autonomo multi-paso — lee, escribe, clasifica, actualiza artefactos | Si — con confirmacion del PM |
| **plan** | Planificacion estructurada | No — produce solo plan |

Los prompts de analisis, reporting y revision usan `ask`. Los prompts que necesitan mover ficheros o actualizar backlog usan `agent`.

## Estandares de Diseno de Prompts

Todos los prompts de esta biblioteca siguen estas convenciones:

1. **Frontmatter YAML**: `description` + `agent`
2. **Seccion de proposito**: Que hace el prompt y por que
3. **Seccion de invocacion**: Sintaxis exacta con parametros
4. **Pasos de procesamiento**: Que lee la IA primero (lecturas de contexto) y como procesa
5. **Estructura de salida**: Plantilla Markdown para salida consistente
6. **Checklist de validacion**: Gate de revision PM antes de confirmar resultados
7. **Notas de compliance PM@Siemens**: Alineacion con terminologia y metodologia

## Referencias de Ficheros Usadas en los Prompts

Todos los prompts referencian estos ficheros del workspace (asegurate de que existan y esten poblados):

| Fichero | Referenciado por |
|---------|-----------------|
| `context/OBJECTIVES.md` | Todos los prompts |
| `context/STAKEHOLDERS.md` | Todos los prompts |
| `context/RISKS.md` | risk-analysis, weekly-status, milestone-readiness, financial-status, scope-impact |
| `context/SOLUTION-OVERVIEW.md` | risk-analysis, task-breakdown, milestone-readiness |
| `planning/backlog.md` | weekly-status, task-breakdown, milestone-readiness, session-recovery |
| `tracking/financial-tracking.md` | financial-status, weekly-status, milestone-readiness, scope-impact |
| `tracking/action-tracker.csv` | weekly-status, session-recovery, milestone-readiness, closeout-review |
| `tracking/scope-changes.md` | scope-impact, weekly-status, milestone-readiness, financial-status |
| `tracking/ncc-log.md` | weekly-status, milestone-readiness, closeout-review |
| `meetings/summaries/` | weekly-status, session-recovery, stakeholder-update |
| `decisions/` | session-recovery, milestone-readiness, closeout-review |
| `.github/financials/` | financial-status |
| `.github/input/` | input-classifier, session-recovery |
| `PM@Siemens-Guide.md` | milestone-readiness, closeout-review |

## Documentacion de Soporte

- [MEETING-SUMMARY-USAGE-GUIDE.md](MEETING-SUMMARY-USAGE-GUIDE.md) — Guia detallada de flujo de trabajo con ejemplos (contiene datos de muestra de un proyecto anterior a modo de ilustracion)

---

*Ultima actualizacion: 2026-05-02*
