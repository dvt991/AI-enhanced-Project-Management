# Cadencia del Proyecto — Ritmo de Interaccion del PM con la IA

> Define la frecuencia y disparadores para cada tipo de actividad asistida por IA. Adapta fechas y frecuencias al ritmo de tu proyecto.

## Cadencia Diaria

| Actividad | Disparador | Prompt | Salida |
|-----------|-----------|--------|--------|
| Procesamiento de inputs | Material nuevo en `.github/input/` | `#prompt:input-classifier.prompt.md` | Ficheros clasificados + informe |
| Check rapido de contexto | Inicio de sesion de trabajo | `#prompt:session-recovery.prompt.md` | Briefing de sesion |

## Cadencia Semanal (Cada Lunes)

| Actividad | Disparador | Prompt | Salida |
|-----------|-----------|--------|--------|
| Informe de estado semanal | Programado (lunes) | `#prompt:weekly-status.prompt.md` | Dashboard de salud + status |
| Revision de action-tracker | Programado (lunes) | Manual / session-recovery | Items vencidos marcados |
| Grooming del backlog | Cambios de estado detectados | Manual | `planning/backlog.md` actualizado |

## Tras Cada Reunion

| Actividad | Disparador | Prompt | Salida |
|-----------|-----------|--------|--------|
| Minutas de reunion | Notas crudas en `meetings/raw/` | `#prompt:meeting-summary.prompt.md` | Minutas + acciones + decisiones |
| Notificaciones a stakeholders | Decisiones que afectan a ausentes | `#prompt:stakeholder-update.prompt.md` | Borradores de comunicacion |

## Cadencia Mensual

| Actividad | Disparador | Prompt | Salida |
|-----------|-----------|--------|--------|
| Revision O&R | Programado (1ª semana del mes) | `#prompt:risk-analysis.prompt.md --trigger periodic` | RISKS.md actualizado |
| Reconciliacion financiera | Export SAP disponible | `#prompt:financial-status.prompt.md --report-type periodic` | Informe financiero |
| Revision del plan de comunicacion | Trimestral o mensual | Manual | STAKEHOLDERS.md actualizado |

## Por Evento (Bajo Demanda)

| Actividad | Disparador | Prompt | Salida |
|-----------|-----------|--------|--------|
| Analisis de cambio de alcance | Change request recibido | `#prompt:scope-impact.prompt.md` | Evaluacion de impacto |
| Descomposicion de tareas | Nuevo paquete de trabajo identificado | `#prompt:task-breakdown.prompt.md` | Entradas de backlog |
| Borrador de escalacion/aprobacion | Accion critica requiere input de stakeholder | `#prompt:stakeholder-update.prompt.md --comm-type escalation` | Borrador de comunicacion |

## Cadencia Pre-Gate (2–4 Semanas Antes del Milestone)

| Actividad | Disparador | Prompt | Salida |
|-----------|-----------|--------|--------|
| Check de readiness (T-4 semanas) | Primera evaluacion | `#prompt:milestone-readiness.prompt.md` | Gap analysis + plan de accion |
| Revision financiera pre-gate | Alineacion presupuestaria | `#prompt:financial-status.prompt.md --report-type pre-gate` | Informe financiero pre-gate |
| Re-check de readiness (T-1 semana) | Verificacion final | `#prompt:milestone-readiness.prompt.md` | Evaluacion actualizada |
| Minuta de gate meeting | Tras la revision de gate | `#prompt:meeting-summary.prompt.md --meeting-type steering` | Minutas de gate + sign-off |

## Cadencia de Cierre (PM700)

| Actividad | Disparador | Prompt | Salida |
|-----------|-----------|--------|--------|
| Revision de cierre | Proyecto acercandose a PM700 | `#prompt:closeout-review.prompt.md` | Checklist de cierre + lecciones aprendidas |
| Reconciliacion financiera final | Liquidacion completada | `#prompt:financial-status.prompt.md --report-type variance` | Informe financiero final |
| Feedback del template | Reflexion post-cierre | Manual | Sugerencias de mejora para el template |

---

## Protocolo de Sesion

Cada sesion de trabajo con IA debe seguir este patron:

```
1. INICIO   → Ejecutar prompt session-recovery → Obtener briefing
2. PROCESO  → Gestionar la(s) tarea(s) principales del dia con los prompts apropiados
3. VALIDAR  → Asegurar consistencia de todos los ficheros modificados (validacion cruzada)
4. REGISTRAR → La IA anade resumen de sesion a context/SESSION-LOG.md
5. FIN      → Verificar que no quedan acciones huerfanas ni decisiones sin registrar
```

## Notas de Adaptacion

- Ajusta frecuencias segun la fase del proyecto (arranque = mas frecuente, crucero = menos)
- En periodos criticos (pre-gate, reclamaciones, escalaciones) aumenta la cadencia
- Para proyectos pequenos/simples, combina semanal + mensual en una revision quincenal
- Si el proyecto no tiene seguimiento financiero activo, omite la reconciliacion mensual
- Marca actividades como `No aplica` si no encajan con tu tipo de proyecto

---

*Ultima actualizacion: 2026-05-01*
