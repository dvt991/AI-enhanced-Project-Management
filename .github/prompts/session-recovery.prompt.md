---
description: Recupera el contexto del proyecto al inicio de sesion escaneando todos los artefactos del workspace y produciendo un resumen del estado actual
agent: ask
---

# Recuperacion de Sesion y Briefing de Contexto — PM@Siemens

**Proposito**: Al inicio de cada sesion de trabajo con IA, escanear todos los artefactos del proyecto y producir un briefing conciso del estado actual. Compensa la falta de estado de la IA entre sesiones reconstruyendo el contexto del proyecto, identificando cambios recientes y resaltando lo que requiere atencion inmediata.

**Alcance**: Inicializacion de sesion, recuperacion de contexto tras pausas, onboarding de un nuevo colaborador.

---

## Invocacion

```
Follow instructions in #prompt:session-recovery.prompt.md with these arguments:
  --project-folder @workspace
```

**Parametros**:
- `--project-folder`: Referencia al workspace para escaneo completo
- No se necesitan otros parametros — este prompt es autocontenido

---

## Pasos de Procesamiento

### Paso 1: Escaneo Completo del Workspace
**Leer TODOS los artefactos sistematicamente**:

| Prioridad | Fichero | Buscar |
|-----------|---------|--------|
| 1 | `context/OBJECTIVES.md` | Identidad del proyecto, objetivos, milestone actual |
| 2 | `planning/backlog.md` | Tareas activas, bloqueos, estado de completitud |
| 3 | `tracking/action-tracker.csv` | Acciones criticas vencidas o proximas a vencer |
| 4 | `context/RISKS.md` | Riesgos Criticos/Altos, cambios recientes |
| 5 | `tracking/financial-tracking.md` | Salud presupuestaria, EAC vs BAC |
| 6 | `tracking/scope-changes.md` | Cambios pendientes de decision |
| 7 | `tracking/ncc-log.md` | No-conformidades abiertas |
| 8 | `meetings/summaries/` | Reunion mas reciente (decisiones, acciones) |
| 9 | `context/STAKEHOLDERS.md` | Contactos clave y necesidades de comunicacion |
| 10 | `context/SOLUTION-OVERVIEW.md` | Contexto tecnico/solucion |
| 11 | `.github/input/` | Material de entrada sin procesar |
| 12 | `decisions/` | Decisiones recientes |

### Paso 2: Identificar Cambios / Urgencias
- Ficheros modificados en los ultimos 7 dias (si es detectable)
- Acciones vencidas o que vencen en 3 dias
- Riesgos con calificacion Critica/Alta
- Material de entrada sin procesar
- Cambios de alcance pendientes sin aprobacion
- Milestone gate proximo (dentro de 4 semanas)

### Paso 3: Generar Briefing

---

## Estructura de Salida

```markdown
# Briefing de Sesion — [Nombre del Proyecto] — [Fecha: YYYY-MM-DD]

## Identidad del Proyecto (Referencia Rapida)
| Campo | Valor |
|-------|-------|
| Nombre del Proyecto | [De OBJECTIVES.md] |
| Cliente | [De OBJECTIVES.md] |
| PM | [De STAKEHOLDERS.md] |
| Milestone Actual | [PM040/100/200/600/700] |
| Salud del Proyecto | 🟢/🟡/🔴 (evaluacion general) |

## Resumen del Estado Actual (5-7 puntos)
- [Item mas critico que requiere atencion]
- [Estado del backlog: X completadas, X en progreso, X bloqueadas, X no iniciadas]
- [Salud financiera: EAC vs BAC, cualquier desviacion]
- [Postura de riesgo principal]
- [Decisiones o aprobaciones pendientes]
- [Proximo milestone / fecha limite acercandose]
- [Inputs sin procesar esperando en .github/input/]

## Atencion Urgente Requerida

### Acciones Vencidas
| ID | Accion | Responsable | Vencia | Dias vencida |
|----|--------|-------------|--------|--------------|
| [ID] | [Accion] | [Nombre] | [Fecha] | [N] |

### Riesgos Criticos/Altos (Activos)
| ID | Riesgo | Calificacion | Estado mitigacion |
|----|--------|--------------|-------------------|
| RSK-XX | [Descripcion] | Critico/Alto | [Estado] |

### Decisiones Pendientes
| Item | Esperando a | Desde | Impacto del retraso |
|------|------------|-------|---------------------|
| [Decision necesaria] | [Quien] | [Fecha] | [Consecuencia] |

### Inputs Sin Procesar
| Fichero | Tipo (estimado) | En `.github/input/` desde |
|---------|----------------|---------------------------|
| [nombre-fichero] | [Tipo probable] | [Fecha o desconocido] |

## Fechas Limite Proximas (14 Dias)
| Fecha | Item | Responsable | Tipo |
|-------|------|-------------|------|
| YYYY-MM-DD | [Tarea/Accion/Gate] | [Nombre] | Backlog/Accion/Milestone |

## Foco Sugerido para la Sesion
Basado en el estado actual, prioridades recomendadas para esta sesion:
1. **[Prioridad 1]**: [Por que — que es urgente o bloquea]
2. **[Prioridad 2]**: [Por que — que vence pronto]
3. **[Prioridad 3]**: [Por que — que avanza el proyecto]

## Prompts Disponibles para Esta Sesion
| Si quieres... | Usa el prompt |
|---------------|--------------|
| Procesar notas de reunion | `#prompt:meeting-summary.prompt.md` |
| Generar status semanal | `#prompt:weekly-status.prompt.md` |
| Analizar datos financieros | `#prompt:financial-status.prompt.md` |
| Revisar riesgos | `#prompt:risk-analysis.prompt.md` |
| Evaluar cambio de alcance | `#prompt:scope-impact.prompt.md` |
| Verificar readiness de gate | `#prompt:milestone-readiness.prompt.md` |
| Descomponer una tarea | `#prompt:task-breakdown.prompt.md` |
| Redactar comunicacion | `#prompt:stakeholder-update.prompt.md` |
| Clasificar nuevo input | `#prompt:input-classifier.prompt.md` |
| Cerrar proyecto | `#prompt:closeout-review.prompt.md` |
```

---

## Notas

- Este prompt no requiere **ningun input** mas alla del workspace
- Debe ser el **primer prompt invocado** en cada nueva sesion
- La salida es solo informativa — no se modifican ficheros
- Si el proyecto no esta inicializado (todo `_TBD_`), el briefing indicara "template aun no adaptado"
- El PM puede usar el "Foco Sugerido para la Sesion" como punto de partida del dia

---

## Checklist de Validacion (El PM Debe Verificar)
- [ ] **Identidad del proyecto correcta**: Nombre, cliente, PM coinciden con la realidad actual
- [ ] **Evaluacion de salud razonable**: El semaforo refleja el estado real
- [ ] **Items vencidos precisos**: Cruzar con action-tracker.csv
- [ ] **Sin datos obsoletos**: El briefing refleja el estado mas reciente del workspace
- [ ] **Prioridades sensatas**: El foco sugerido se alinea con el criterio del PM

---

## Notas de Compliance PM@Siemens

- La recuperacion de sesion respeta el modelo de milestones — siempre identifica el milestone actual
- Las acciones vencidas y riesgos criticos se muestran prominentemente segun PM@Siemens Cap. 3.8 (Monitoring & Controlling)
- Los cambios de alcance pendientes se senalan para prevenir ejecucion no aprobada
- La plantilla usa terminologia PM@Siemens: Quality Gate, O&R, NCC, EAC/BAC, LoA

---

*Version 1.0 | Soporte de Cadencia del Workspace*
