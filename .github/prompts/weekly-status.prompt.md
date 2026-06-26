---
description: Genera un informe conciso de estado semanal del proyecto con indicadores de salud y awareness de milestones
agent: ask
---

# Generador de Informe de Estado Semanal — PM@Siemens

**Proposito**: Producir una actualizacion de estado semanal profesional y concisa analizando el estado actual de todos los artefactos del workspace. La salida esta lista para comunicacion a stakeholders y se alinea con la Guia PM@Siemens Cap. 3.9 (Communication & Collaboration).

**Alcance**: Reporting de cadencia semanal regular y snapshots de estado ad-hoc.

---

## Invocacion

```
Follow instructions in #prompt:weekly-status.prompt.md with these arguments:
  --period [YYYY-Www or "current"]
  --audience [team|management|customer]
  --project-folder @workspace
```

**Parametros**:
- `--period`: Referencia ISO de semana (ej., `2026-W18`) o `current` para auto-deteccion
- `--audience`: Ajusta nivel de detalle y tono — `team` (detallado, tecnico), `management` (resumen, KPIs), `customer` (externo, filtrado)
- `--project-folder`: Referencia al workspace para lecturas de contexto

---

## Pasos de Procesamiento

### Paso 1: Lecturas de Contexto (Escaneo Completo del Workspace)
**Siempre leer y cruzar TODOS estos**:
1. `context/OBJECTIVES.md` — Objetivos generales y alineacion con milestones
2. `planning/backlog.md` — Progreso de tareas, items completados/bloqueados esta semana
3. `context/RISKS.md` — Riesgos abiertos, cambios de estado
4. `tracking/financial-tracking.md` — Salud presupuestaria, EAC vs BAC
5. `tracking/action-tracker.csv` — Acciones criticas del PM vencidas/proximas
6. `tracking/scope-changes.md` — Cambios pendientes o recientemente aprobados
7. `tracking/ncc-log.md` — No-conformidades abiertas
8. `meetings/summaries/` — Decisiones recientes y acciones
9. `context/STAKEHOLDERS.md` — Adaptar comunicacion a la audiencia

### Paso 2: Determinar Estado de Salud

**Evaluacion con Semaforo**:
| Dimension | 🟢 Verde | 🟡 Ambar | 🔴 Rojo |
|-----------|----------|----------|---------|
| **Alcance** | Sin cambios no controlados | Cambios pendientes bajo revision | Scope creep no aprobado detectado |
| **Plazo** | On track para proximo milestone | Riesgo de retraso ≤2 semanas | Ruta critica retrasada |
| **Coste** | EAC ≤ BAC | EAC 1-10% sobre BAC | EAC >10% sobre BAC o sin baseline |
| **Calidad** | Sin NCCs abiertas | NCCs bajo control | NCCs sin resolver bloqueando gate |
| **Riesgos** | Sin riesgos Criticos/Altos abiertos | Riesgos altos con mitigacion activa | Riesgo critico sin mitigacion |

### Paso 3: Generar Informe

---

## Estructura de Salida

```markdown
# Informe de Estado Semanal — [Nombre del Proyecto] — [Semana YYYY-Www]

## Dashboard de Salud

| Dimension | Estado | Tendencia | Comentario |
|-----------|--------|-----------|-----------|
| **Alcance** | 🟢/🟡/🔴 | ↑/→/↓ | [Explicacion en una linea] |
| **Plazo** | 🟢/🟡/🔴 | ↑/→/↓ | [Explicacion en una linea] |
| **Coste** | 🟢/🟡/🔴 | ↑/→/↓ | [Explicacion en una linea] |
| **Calidad** | 🟢/🟡/🔴 | ↑/→/↓ | [Explicacion en una linea] |
| **Riesgos** | 🟢/🟡/🔴 | ↑/→/↓ | [Explicacion en una linea] |

**Salud General del Proyecto**: 🟢/🟡/🔴
**Milestone Actual**: [PM040/PM100/PM200/PM600/PM700] — [Estado: On Track / At Risk / Blocked]

## Resumen Ejecutivo (3-5 puntos)
- [Logro clave de esta semana]
- [Bloqueo principal o desarrollo de riesgo]
- [Fecha limite critica proxima]
- [Accion de stakeholder necesaria, si aplica]

## Progreso Esta Semana

| ID | Tarea | Milestone | Cambio de estado | Responsable | Notas |
|----|-------|-----------|-----------------|-------------|-------|
| BLG-XX | [Tarea] | PM100 | No iniciada → En progreso | [Nombre] | [Nota breve] |
| BLG-YY | [Tarea] | PM100 | En progreso → Completada | [Nombre] | [Nota breve] |

**Resumen Backlog**: [X] total | [X] completadas | [X] en progreso | [X] bloqueadas | [X] no iniciadas

## Foco Proximo (Semana Siguiente)

| Prioridad | Tarea | Responsable | Fecha | Milestone | Dependencias |
|-----------|-------|-------------|-------|-----------|--------------|
| 1 | [Tarea] | [Nombre] | [Fecha] | PM100 | [Bloqueos] |
| 2 | [Tarea] | [Nombre] | [Fecha] | PM100 | [Bloqueos] |
| 3 | [Tarea] | [Nombre] | [Fecha] | PM200 | [Bloqueos] |

## Top Riesgos e Issues (Max 5)

| ID | Descripcion | Calificacion | Responsable | Estado mitigacion | Tendencia |
|----|-------------|-------------|-------------|-------------------|-----------|
| RSK-XX | [Descripcion] | Critico/Alto/Medio | [Nombre] | [Activa/Planificada/Estancada] | ↑/→/↓ |

## Acciones Criticas (de action-tracker.csv)

| ID | Accion | Responsable | Fecha | Estado | Vencida? |
|----|--------|-------------|-------|--------|----------|
| [ID] | [Accion] | [Nombre] | [Fecha] | [Estado] | Si/No |

## Snapshot Financiero (si audience = management)

| Metrica | Valor | vs Baseline | Estado |
|---------|-------|-------------|--------|
| EAC | € [importe] | [+/- vs BAC] | 🟢/🟡/🔴 |
| Costes reales a fecha | € [importe] | [% de EAC consumido] | — |
| Proximo hito facturacion | [M-XX] | [Estado del disparador] | — |

## Bloqueos y Soporte Necesario

| Bloqueo | Impacto | Que se necesita | De quien | Urgencia |
|---------|---------|----------------|----------|----------|
| [Descripcion] | [Impacto en Plazo/Coste/Calidad] | [Peticion especifica] | [Stakeholder] | Alta/Media |

## Decisiones Tomadas Esta Semana

| Decision | Fecha | Responsable | Impacto | Fuente |
|----------|-------|-------------|---------|--------|
| [Decision] | [Fecha] | [Nombre] | [Impacto breve] | [Ref reunion/email] |

---

*Informe generado: YYYY-MM-DD | Proximo informe: [+7 dias]*
```

---

## Reglas de Adaptacion por Audiencia

| Seccion | Team | Management | Customer |
|---------|------|------------|----------|
| Dashboard de Salud | Detalle completo | Detalle completo | Simplificado (solo Alcance/Plazo/Calidad) |
| Progreso Esta Semana | Todas las tareas | Solo resumen (conteos + highlights) | Solo logros de alto nivel |
| Snapshot Financiero | Omitir | Incluir | Omitir (salvo contractual) |
| Riesgos | Todos | Top 3 Criticos/Altos | Solo riesgos visibles para cliente |
| Action Tracker | Completo | Solo vencidos | Omitir |
| Bloqueos | Detalle tecnico completo | Resumen con peticion de escalacion | Solo items accionables por cliente |

---

## Checklist de Validacion (El PM Debe Verificar)
- [ ] **Precision**: Estado refleja el estado real del workspace, no supuestos
- [ ] **Completitud**: Las cinco dimensiones de salud evaluadas
- [ ] **Awareness de milestone**: Milestone actual y proximo claramente indicados
- [ ] **Accionable**: Bloqueos tienen "peticion" especifica y persona objetivo
- [ ] **Apropiado para audiencia**: Nivel de detalle coincide con destinatarios
- [ ] **Terminologia**: Terminos PM@Siemens usados (Quality Gate, O&R, NCC, EAC/BAC)
- [ ] **Tono**: Profesional, factual, positivo-pero-realista

---

## Notas de Compliance PM@Siemens

- Las dimensiones del dashboard de salud se alinean con KPIs de project controlling PM@Siemens
- Usar nombres de milestones del modelo aplicable (PM040/100/200/600/700)
- Referenciar readiness de **Quality Gate** explicitamente cuando estes a 4 semanas de un gate
- Senalar desviaciones que disparen LoA en el snapshot financiero
- No-conformidades usan terminologia "NCC" per PM@Siemens

---

*Version 2.0 | PM@Siemens Cap. 3.9 (Reporting & Communication) Alineado*