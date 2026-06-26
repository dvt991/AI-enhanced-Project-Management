---
description: Transforma notas crudas de reunion en minutas conformes con governance, con acciones, decisiones y vinculacion a riesgos
agent: ask
---

# Generador de Resumen de Reunion y Acciones — PM@Siemens Enhanced v2.0

**Proposito**: Transformar notas crudas de reunion en minutas profesionales conformes con governance, alineadas con la Guia PM@Siemens Cap. 3.9 (Communication & Collaboration). La salida se integra con los artefactos del proyecto (OBJECTIVES.md, STAKEHOLDERS.md, RISKS.md, backlog.md).

**Alcance**: Reuniones operativas (team sync, revisiones de estado) y reuniones estrategicas (revisiones Quality Gate, steering committee, decision gates).

---

## Instrucciones de Invocacion

**Como usar este prompt**:
```
Follow instructions in #prompt:meeting-summary.prompt.md with these arguments:
  @file:meetings/raw/YYYY-MM-DD.md
  --meeting-type [operational|steering]
  --project-folder @workspace
```

**Parametros**:
- `@file`: Ruta a las notas crudas de reunion (ej., `meetings/raw/2026-04-07-team-sync.md`)
- `--meeting-type`: `operational` (revisiones de estado, team sync) o `steering` (Quality Gates, decisiones ejecutivas)
- `--project-folder`: Referencia al workspace del proyecto para lecturas de contexto (@workspace auto-detecta)

**Ejemplo de invocacion**:
```
Follow instructions in #prompt:meeting-summary.prompt.md with these arguments:
  @file:meetings/raw/2026-04-07-pm100-kick.md
  --meeting-type steering
  --project-folder @workspace
```

---

## Pasos de Procesamiento

### Paso 1: Extraer Metadatos Principales
- **Fecha y Hora**: YYYY-MM-DD, HH:MM-HH:MM (o N/A si no se especifica)
- **Titulo de Reunion**: Tipo operativo (ej., "Weekly Team Sync") o estrategico (ej., "PM100 Concept Gate Review")
- **Asistentes**: Lista con titulos/roles; cruzar con STAKEHOLDERS.md
- **Ubicacion/Plataforma**: Presencial, Teams, etc.
- **Responsable/Facilitador**: Quien dirigio la reunion

### Paso 2: Lecturas de Contexto (Capa de Governance)
Antes de estructurar la salida, **siempre leer y referenciar**:
1. `context/OBJECTIVES.md` — Vincular todas las acciones a objetivos del proyecto (OBJ-XX)
2. `context/STAKEHOLDERS.md` — Validar roles de asistentes e identificar quien necesita notificacion
3. `context/RISKS.md` — Senalar nuevos riesgos identificados; anotar riesgos existentes discutidos
4. `planning/backlog.md` — Referenciar items de backlog relacionados; anotar cambios de plazo/alcance

### Paso 3: Extraer Contenido de Reunion
- **Agenda/Temas Tratados**: Lista de items discutidos con duracion aproximada
- **Decisiones Clave**: Extraer con responsable claro, justificacion, impacto
- **Acciones**: Responsable, descripcion, fecha limite, prioridad, dependencias
- **Riesgos/Issues Identificados**: Nuevos riesgos → anotar para proceso O&R de 6 pasos
- **Preguntas Abiertas**: Items sin resolver que requieren seguimiento
- **Actualizaciones de Plazo/Milestone**: Cambios a gates PM040/100/200/700 o readiness

### Paso 4: Validar y Aplicar Reglas PM@Siemens

**Verificacion de Terminologia**:
- Usar "Non-conformance" no "defecto" o "issue"
- Usar "Quality Gate" no "revision de gate" o "aprobacion"
- Usar "O&R Register" no "log de riesgos" o "tracker de riesgos"
- Usar "SPMP" al referirse al Siemens Project Management Plan
- Usar nombres correctos de milestones: PM040, PM100, PM200, PM700

**Vinculacion a Objetivos**:
- Cada accion debe vincularse a al menos un objetivo definido en `context/OBJECTIVES.md` (ej., OBJ-01, OBJ-02)
- Si una accion no se vincula a ningun objetivo, senalar como fuera de alcance o sugerir reformulacion

**Compliance de Stakeholders**:
- Cruzar asistentes con STAKEHOLDERS.md
- Si un stakeholder critico (ej., STK-02 Line Manager) esta ausente pero se tomaron decisiones que le afectan, anadir nota en "Notificacion a Stakeholders"

**Integracion del Proceso de Riesgos**:
- Todo riesgo identificado debe mencionar el proceso O&R de 6 pasos: Identificar → Evaluar → Responder → Monitorear → Reportar → Sostener
- Vincular nuevos riesgos a entradas existentes del registro si aplica
- Cruzar con `context/RISKS.md` para IDs de riesgos existentes; senalar si un riesgo nuevo se relaciona con entradas existentes

---

## Estructura de Salida

### Tipo de Salida: REUNION OPERATIVA

**Ubicacion recomendada del fichero**: `meetings/summaries/minutes-YYYY-MM-DD.md`

```markdown
# Minuta de Reunion — [Titulo de Reunion]

## Metadatos de Reunion
| Propiedad | Valor |
|-----------|-------|
| **Fecha** | YYYY-MM-DD, HH:MM-HH:MM |
| **Tipo** | Operativa |
| **Responsable** | [Nombre y Rol] |
| **Asistentes** | [Nombre y Rol], [Nombre y Rol], ... |
| **Ubicacion/Plataforma** | [Presencial / Teams / etc.] |
| **Objetivo(s) Vinculado(s)** | OBJ-01, OBJ-03 (ejemplo) |

## Resumen Ejecutivo (3-5 puntos)
- [Resumen conciso de temas clave, decisiones e impacto]
- [Bloqueos criticos o escalaciones]
- [Progreso contra backlog/milestones]

## Agenda y Temas Tratados

| Tema | Duracion | Responsable |
|------|----------|-------------|
| Tema 1: [Descripcion] | 15 min | [Nombre] |
| Tema 2: [Descripcion] | 20 min | [Nombre] |
| ... | ... | ... |

## Decisiones Clave

| Decision | Owner | Impact | Linked Objective | Implementation Date |
|----------|-------|--------|------------------|---------------------|
| [Enunciado de decision] | [Nombre] | [Impacto en plazo/alcance/presupuesto] | OBJ-XX | [Fecha] |
| ... | ... | ... | ... | ... |

## Acciones

| ID | Tarea | Responsable | Fecha limite | Prioridad | Estado | Backlog vinculado | Dependencias |
|----|-------|-------------|-------------|-----------|--------|-------------------|--------------|
| AI-001 | [Descripcion de tarea] | [Nombre] | YYYY-MM-DD | Alta/Media/Baja | Pendiente | BLG-XX | [IDs de tareas relacionadas] |
| AI-002 | [Descripcion de tarea] | [Nombre] | YYYY-MM-DD | Alta/Media/Baja | Pendiente | BLG-XX | [IDs de tareas relacionadas] |
| ... | ... | ... | ... | ... | ... | ... | ... |

## Riesgos / Issues / No-Conformidades Identificadas

| ID | Descripcion | Tipo | Responsable | Vinculacion a O&R Register | Respuesta recomendada |
|----|-------------|------|-------------|---------------------------|----------------------|
| RSK-NEW | [Descripcion] | Riesgo | [Nombre] | Coincide RSK-XX / Nuevo | Identificar → Evaluar → Responder per proceso 6 pasos |
| NCC-001 | [Descripcion de no-conformidad/retrabajo] | No-Conformidad | [Nombre] | Vincular a NCC log | Documentar impacto coste en ncc-log.md |
| ... | ... | ... | ... | ... | ... |

## Actualizaciones de Milestones y Plazos

- [ ] **Readiness PM100**: [Actualizacion de estado — on track / at risk / blocked]
- [ ] **Cambios de Plazo**: [Ajustes a fechas planificadas? Describir impacto]
- [ ] **Cambios de Backlog**: [Nuevos items identificados / cambios de alcance / dependencias]

## Notificacion a Stakeholders Requerida

| Stakeholder | Resumen del mensaje | Metodo de notificacion | Responsable |
|-------------|--------------------|-----------------------|-------------|
| STK-XX ([Nombre PM]) | [Puntos clave relevantes para el PM] | Actualizacion workspace | — |
| STK-XX ([Sponsor/Manager]) | [Si se necesita escalacion] | Email / Reunion | [Nombre] |
| STK-XX ([Otro stakeholder]) | [Si resultados compartibles] | [Metodo per STAKEHOLDERS.md] | [Nombre] |
| ... | ... | ... | ... |

## Proximos Pasos
1. [Tarea]: Responsable [Nombre], Fecha [Fecha]
2. [Tarea]: Responsable [Nombre], Fecha [Fecha]
3. [Tarea]: Responsable [Nombre], Fecha [Fecha]

---

## Ficheros a Actualizar (Revision PM)
- [ ] `meetings/summaries/minutes-YYYY-MM-DD.md` (este fichero)
- [ ] `planning/backlog.md` (si hay cambios de plazo/alcance)
- [ ] `context/RISKS.md` (si se identificaron nuevos riesgos)
- [ ] `tracking/ncc-log.md` (si se registraron no-conformidades)
- [ ] [Otros ficheros segun aplique]

## Checklist de Validacion de Calidad (El PM Debe Verificar Antes de Confirmar)
- [ ] **Asistentes**: Todos listados con titulos/roles correctos (cruzado con STAKEHOLDERS.md)
- [ ] **Decisiones**: Cada decision claramente asignada, fechada y con evaluacion de impacto
- [ ] **Acciones**: Todas las acciones tienen responsable, fecha limite, prioridad y vinculacion a backlog
- [ ] **Riesgos**: Nuevos riesgos identificados y anotados para proceso O&R de 6 pasos; riesgos existentes cruzados
- [ ] **Objetivos**: Todas las acciones vinculadas a al menos un objetivo de `context/OBJECTIVES.md`
- [ ] **Terminologia**: Terminologia PM@Siemens usada correctamente (NCC, Quality Gate, O&R Register, SPMP)
- [ ] **Notificacion Stakeholders**: Plan establecido para stakeholders ausentes de la reunion
- [ ] **Precision de Plazos**: Cambios de milestone/calendario claramente documentados con evaluacion de impacto
- [ ] **Actualizacion de Ficheros**: Ficheros recomendados identificados y listos para actualizar
- [ ] **Tono Profesional**: Minutas neutrales, objetivas y aptas para trail de auditoria

---
```

### Tipo de Salida: REUNION STEERING / REVISION DE GATE

**Ubicacion recomendada del fichero**: `meetings/summaries/gate-review-YYYY-MM-DD.md` (o `milestone-PM100-gate-review.md`)

```markdown
# Minuta de Revision de Gate — [Nombre del Milestone Gate]

## Metadatos de Reunion
| Propiedad | Valor |
|-----------|-------|
| **Fecha** | YYYY-MM-DD, HH:MM-HH:MM |
| **Gate** | PM100 / PM200 / PM700 (especificar) |
| **Tipo** | Steering / Quality Gate Review |
| **Facilitador** | [Nombre y Rol] |
| **Aprobadores Presentes** | [Nombres de decisores] |
| **Stakeholders Clave** | [STK-01, STK-02, STK-04, ...] |
| **Ubicacion/Plataforma** | [Presencial / Teams / etc.] |

## Resumen Ejecutivo
- **Readiness del Gate**: Ready / At Risk / Not Ready (indicar y justificar)
- **Decisiones Clave**: [2-3 decisiones criticas de este gate]
- **Aprobaciones Otorgadas**: [Que se aprobo? Que es condicional?]
- **Escalaciones**: [Issues que requieren escalacion ejecutiva]

## Evaluacion de Readiness del Gate

### Checklist de Outputs Obligatorios (Per Guia PM@Siemens, Capitulo 5.3-5.6)

| Entregable | Estado | Responsable | Notas |
|------------|--------|-------------|-------|
| [Output obligatorio 1] | ✓ / ⚠ / ✗ | [Nombre] | [Estado de compliance] |
| [Output obligatorio 2] | ✓ / ⚠ / ✗ | [Nombre] | [Estado de compliance] |
| ... | ... | ... | ... |

**Leyenda**: ✓ = Completo y aprobado; ⚠ = En progreso / aprobacion condicional; ✗ = Faltante / bloqueado

### Revision de Compliance Quality Gate

| Criterio | Pasa/Falla | Evidencia | Responsable |
|----------|-----------|----------|-------------|
| [Criterio per PM@Siemens] | ✓ / ⚠ / ✗ | [Artefacto de soporte] | [Aprobador] |
| [Criterio per PM@Siemens] | ✓ / ⚠ / ✗ | [Artefacto de soporte] | [Aprobador] |
| ... | ... | ... | ... |

## Decisiones y Aprobaciones del Gate

| Decision | Responsable | Estado aprobacion | Condicion / Notas | Fecha efectiva |
|----------|-------------|-------------------|-------------------|----------------|
| Aprobar avance a PM100 | STK-02 | ✓ Aprobado / ⚠ Condicional / ✗ No aprobado | [Condiciones si hay] | [Fecha] |
| [Decision 2] | [Aprobador] | [Estado] | [Condiciones] | [Fecha] |
| ... | ... | ... | ... | ... |

## Revision de Riesgos e Issues (Nivel Gate)

| Item | Descripcion | Impacto | Mitigacion | Responsable | Estado |
|------|-------------|---------|-----------|-------------|--------|
| RSK-XX | [Descripcion del riesgo desde O&R Register] | Alto/Medio/Bajo | [Estrategia de mitigacion actual] | [Nombre] | Abierto/Cerrado |
| RSK-XX | [Descripcion del riesgo desde O&R Register] | Alto/Medio/Bajo | [Estrategia de mitigacion actual] | [Nombre] | Abierto/Cerrado |
| ... | ... | ... | ... | ... | ... |

## Acciones para la Siguiente Fase

| ID | Tarea | Responsable | Fecha limite | Fase vinculada | Prioridad |
|----|-------|-------------|-------------|----------------|-----------|
| [Tarea] | [Descripcion] | [Nombre] | [Fecha] | PM200 (siguiente) | Alta/Media/Baja |
| ... | ... | ... | ... | ... | ... |

## Firma del Gate

| Aprobador | Rol | Firma / Aprobacion | Fecha |
|-----------|-----|---------------------|-------|
| [Nombre] | [ej., STK-02 Line Manager] | Aprobado / No Aprobado | YYYY-MM-DD |
| [Nombre] | [ej., STK-01 PM] | Aprobado / No Aprobado | YYYY-MM-DD |
| ... | ... | ... | ... |

---

## Checklist de Validacion de Calidad (El PM Debe Verificar Antes de Enviar al Gate)
- [ ] **Readiness**: Evaluacion de readiness clara y justificada (Ready / At Risk / Not Ready)
- [ ] **Compliance**: Todos los outputs obligatorios per Guia PM@Siemens Capitulo 5 listados y verificados
- [ ] **Aprobadores**: Todos los aprobadores requeridos (per LoA) presentes y documentados
- [ ] **Decisiones**: Decisiones del gate claramente documentadas; estado y condiciones de aprobacion explicitos
- [ ] **Riesgos**: Todos los riesgos abiertos documentados con responsable y estrategia de mitigacion
- [ ] **Actualizacion de Ficheros**: Artefactos referenciados y listos para archivo (ej., tag repo como `v-PM040-approved`)
- [ ] **Escalaciones**: Escalaciones a STK-02 o STK-03 claramente anotadas con responsable y fecha limite
- [ ] **Tono Profesional**: Minutas aptas para trail de auditoria y revision externa
- [ ] **Listo para Firma**: Aprobadores han revisado y aceptado firmar

---
```

---

## Resumen de Funcionalidades Context-Aware

1. **Integracion OBJECTIVES.md**: Cada accion se vincula a objetivos del proyecto (OBJ-XX); items huerfanos se senalan
2. **Cruce con STAKEHOLDERS.md**: Asistentes validados contra registro; stakeholders criticos ausentes notados
3. **Integracion RISKS.md**: Nuevos riesgos identificados; vinculados al proceso O&R de 6 pasos
4. **Vinculacion a Backlog.md**: Acciones vinculadas a items del backlog; cambios de plazo anotados
5. **Terminologia PM@Siemens**: Vocabulario obligatorio (NCC, Quality Gate, O&R Register, SPMP)
6. **Readiness Quality Gate**: Salida disenada para alimentar directamente revisiones de milestone gate
7. **Notificacion a Stakeholders**: Plan claro de a quien informar de las decisiones

---

## Errores Comunes y Como Evitarlos

| Error | Impacto | Prevencion |
|-------|---------|-----------|
| Acciones sin responsable | Accountability poco clara; tareas se retrasan | Siempre extraer nombre del responsable; senalar si confuso |
| Decisiones sin justificacion | Revisores de gate no pueden evaluar riesgo | Siempre preguntar "por que esta decision?" y documentar razonamiento |
| Nuevos riesgos no vinculados al proceso O&R | Gap de governance; no-conformidad no controlada | Senalar todos los riesgos para proceso de 6 pasos; sugerir actualizacion RSK |
| Fechas vagas ("ASAP", "pronto") | Scope creep; fechas incumplidas | Siempre extraer fecha especifica (YYYY-MM-DD) o senalar como TBD |
| Asistentes faltantes | Gaps de accountability/RACI | Cruzar con STAKEHOLDERS.md; notar si stakeholders clave ausentes |
| Vinculacion a objetivos omitida | Scope creep; desconexion de estrategia | Forzar que cada accion se vincule a un objetivo (OBJ-XX) o senalar como fuera de alcance |
| Sin evaluacion de impacto en plazos/milestones | Sorpresas en gates | Documentar explicitamente si decisiones de reunion afectan readiness PM100/200/700 |

---

## Validacion: Antes de Confirmar en el Workspace

**La salida del prompt NO esta finalizada hasta que el PM verifique el checklist**. Revisar el "Checklist de Validacion de Calidad" antes de guardar en `meetings/summaries/`.

**Si el checklist tiene >= 2 fallos**:
- Re-invocar Copilot con clarificacion
- O editar manualmente y senalar items como "PM-edited" en mensaje de commit

---

## Notas de Compliance PM@Siemens

- Usar "Non-conformance" (NCC) no "defecto", "issue" o "bug"
- Usar "Quality Gate" no "revision de gate" o "reunion de aprobacion"
- Usar "O&R Register" no "log de riesgos" o "tracker de riesgos"
- Usar "SPMP" al referirse al Siemens Project Management Plan
- Usar nombres correctos de milestones: PM040, PM100, PM200, PM600, PM700
- Las minutas de reunion se consideran artefactos de audit-trail per PM@Siemens Cap. 3.9
- Decisiones tomadas en reuniones pueden disparar requisitos LoA — senalar explicitamente
- Acciones de reuniones de gate/steering son compromisos vinculantes del proyecto

---

*Enhanced: 2026-05-01 | Version 2.1 | PM@Siemens Cap. 3.9 Alineado*