# Prompt de Resumen de Reunion — Guia de Uso

> **⚠️ AVISO DE DATOS DE EJEMPLO**: Esta guia contiene datos de ejemplo de un proyecto anterior (nombres, IDs de backlog, entradas de riesgos, cifras financieras) solo con fines ilustrativos. Al usar esta plantilla para un proyecto real, reemplaza todas las referencias de ejemplo con los datos reales de tu proyecto. Los patrones de flujo de trabajo y la estructura son reutilizables; los datos especificos no.

## Referencia Rapida

**Fichero del prompt**: `.github/prompts/meeting-summary.prompt.md`

**Invocacion**:
```
Follow instructions in #prompt:meeting-summary.prompt.md with these arguments:
  @file:meetings/raw/YYYY-MM-DD.md
  --meeting-type [operational|steering]
```

**Ubicacion de salida**: `meetings/summaries/minutes-YYYY-MM-DD.md` (para operativas) o `meetings/summaries/gate-review-YYYY-MM-DD.md` (para steering)

---

## Cuando Usar Este Prompt

| Escenario | Tipo de reunion | ¿Usar este prompt? |
|-----------|-----------------|-------------------|
| Sync semanal de equipo, revision de estado, actualizacion de proyecto | Operativa | ✓ Si |
| Revision Quality Gate PM100, PM200, PM700 | Steering | ✓ Si (variante gate) |
| Inmersion tecnica, discusion de arquitectura | Operativa | ✓ Si |
| Comite de direccion, gate de decision ejecutivo | Steering | ✓ Si (variante gate) |
| One-on-one de feedback, evaluacion de rendimiento | N/A | ✗ No aplica |
| Reunion externa con cliente/proveedor | Operativa | ✓ Si (con cautela en confidencialidad) |

---

## Flujo de Trabajo Paso a Paso

### **1. Preparar las Notas Crudas de Reunion**

Antes de invocar el prompt, captura las notas crudas en:
- `meetings/raw/YYYY-MM-DD-[nombre-reunion].md`

**Ejemplo**:
```markdown
# Notas Crudas — 2026-04-07 Team Sync

Fecha: 2026-04-07, 10:00–11:00
Ubicacion: Teams
Asistentes: Daniel Velasco, Jorge Jimenez, [otros]

## Temas Tratados
- Daniel dio actualizacion sobre BLG-06 (diseno prompt de reunion) — empezo el lunes, on track
- Jorge reporto bloqueante en BLG-07 (prompt de analisis de riesgos) — esperando clarificacion del rulebook
- Discusion: investigacion RSK-08 (pico de costes DUAGON) — necesita contacto DI Finance
- Accion: Necesario establecer baseline BAC (RSK-09) — contactar STK-02 line manager
- Decision: Priorizar completar BLG-06 para fin de semana
- Riesgo marcado: Concentracion en proveedor unico (DUAGON 72.8%) podria impactar readiness PM100

## Preguntas Abiertas
- ¿Cuanto esfuerzo requiere BLG-07 prompt de analisis de riesgos?
- ¿Cuando estara disponible STK-02 para reunion de baseline presupuestaria?
```

### **2. Determinar el Tipo de Reunion**

- **Operativa**: Actualizaciones de equipo, revisiones de estado, decisiones tacticas
  - Usar flag: `--meeting-type operational`
  - La salida incluye: Agenda, items de accion, bloqueantes, vinculacion a backlog
  
- **Steering/Gate**: Revisiones de Quality Gate, aprobaciones de milestone, decisiones ejecutivas
  - Usar flag: `--meeting-type steering`
  - La salida incluye: Checklist de readiness de gate, sign-off de aprobadores, evaluacion de compliance

### **3. Invocar el Prompt**

**Para reunion operativa**:
```
Follow instructions in #prompt:meeting-summary.prompt.md with these arguments:
  @file:meetings/raw/2026-04-07-team-sync.md
  --meeting-type operational
```

**Para reunion steering/gate**:
```
Follow instructions in #prompt:meeting-summary.prompt.md with these arguments:
  @file:meetings/raw/2026-04-10-PM100-gate-review.md
  --meeting-type steering
```

### **4. Revisar la Salida de la IA**

Copilot genera una salida Markdown estructurada. **Paso critico**: Revisar el **"Checklist de Validacion de Calidad"** embebido antes de hacer commit:

- [ ] Asistentes listados y cruzados contra STAKEHOLDERS.md
- [ ] Todos los items de accion tienen responsable, fecha limite, prioridad y vinculacion a backlog
- [ ] Nuevos riesgos identificados y vinculados al proceso O&R de 6 pasos
- [ ] Todos los items de accion vinculados a OBJ-01..05
- [ ] Terminologia PM@Siemens usada correctamente
- [ ] Plan de notificacion a stakeholders definido
- [ ] Actualizaciones de ficheros identificadas

**Si ≥ 2 items del checklist fallan**:
- Editar manualmente la salida (juicio del PM)
- O re-promptear con clarificacion:
  ```
  La salida anterior necesita refinamiento. Especificamente:
  - Anadir vinculos a objetivos en items de accion AI-002 y AI-003
  - Clarificar responsable de "Investigar pico de costes DUAGON" — deberia ser Daniel + DI Finance
  - Re-promptear la seccion de Riesgos para incluir vinculo a entradas existentes del registro RSK
  ```

### **5. Commit al Workspace**

Una vez validado:
1. Guardar en `meetings/summaries/minutes-YYYY-MM-DD.md`
2. Crear un commit git:
   ```
   git add meetings/summaries/minutes-YYYY-MM-DD.md
   git commit -m "AI-draft: resumen reunion 2026-04-07 team sync

   Temas: progreso BLG-06, bloqueante BLG-07, escalacion RSK-08/RSK-09
   Items de Accion: 3 (vinculados a backlog)
   Riesgos: 1 nuevo riesgo (concentracion proveedor)
   
   Checklist: PASS (10/10 items verificados)"
   ```

3. Actualizar ficheros relacionados si los items de accion lo requieren:
   - `planning/backlog.md` (si cambios de timeline/alcance)
   - `context/RISKS.md` (si nuevos riesgos identificados)
   - `tracking/ncc-log.md` (si no-conformidades registradas)

---

## Ejemplo de Flujo de Trabajo: Reunion de Proyecto Real

### Input: Notas Crudas

```markdown
# Notas Crudas — 2026-04-07 Weekly Team Sync

Fecha: 2026-04-07, 10:00–11:00
Asistentes: Daniel Velasco (PM, STK-01), Jorge Jimenez (Equipo, STK-05)
Ubicacion: Teams
Facilitador: Daniel Velasco

## Agenda
1. Estado de BLG-06 (diseno prompt de reunion) — 20 min
2. Bloqueante en BLG-07 (prompt analisis de riesgos) — 15 min
3. Investigacion RSK-08 (pico costes DUAGON) — 15 min
4. Accion baseline presupuestaria (RSK-09) — 10 min

## Temas

### BLG-06: Diseno del Prompt de Reunion
- Estado: EN PROGRESO (inicio 2026-04-07)
- Daniel: "Estructura mejorada del prompt completa. Dos variantes: operativa + steering gate"
- Tarea: PM necesita testear prompt con notas reales (diferido a semana siguiente)
- Bloqueante: Ninguno
- Timeline: On track para gate PM100 (vence 2026-04-30)
- Objetivo vinculado: OBJ-01, OBJ-02, OBJ-03

### BLG-07: Prompt de Analisis de Riesgos
- Estado: PENDIENTE (esperando clarificacion del rulebook)
- Jorge: "Necesito clarificacion sobre como vincular nuevos riesgos al proceso O&R de 6 pasos. ¿Que capitulo?"
- Decision: Daniel revisara Guia PM@Siemens Cap. 3.7 y documentara un "Resumen del Rulebook de Analisis de Riesgos" para Jorge
- Fecha limite: 2026-04-10
- Objetivo vinculado: OBJ-01, OBJ-02

### RSK-08: Investigacion Pico de Costes DUAGON
- Problema: €14,848 gastados en Nov 2025 = 47.8% de los actuals del proyecto a fecha
- Estado: Bloqueado (esperando datos de contacto DI Finance)
- Decision: Daniel contactara a STK-02 (Line Manager) para conseguir intro a DI Finance
- Responsable: Daniel Velasco
- Fecha limite: 2026-04-14
- Riesgo: Si no se resuelve antes del gate PM100, estado RSK-08 = "En Riesgo"
- Objetivo vinculado: OBJ-01 (validar flujos, necesita datos presupuestarios limpios)

### RSK-09: Formalizacion Baseline Presupuestaria
- Problema: No hay baseline BAC formal establecida; solo EAC
- Estado: Pendiente de aprobacion
- Decision: Formalizar EAC actual (€31,040.33) como baseline BAC para habilitar tracking de desviaciones
- Responsable: Daniel Velasco (enviar), STK-02 (aprobar)
- Fecha limite: 2026-04-14
- Impacto: CRITICO — bloquea analisis de variaciones del Quality Gate
- Objetivo vinculado: OBJ-01 (validar flujos, necesita compliance governance)

## Decisiones
1. **BLG-06 a testear antes de 2026-04-14** — Daniel aplicara prompt a notas reales y evaluara readiness Quality Gate
2. **Resumen rulebook BLG-07 antes de 2026-04-10** — Daniel documentara proceso O&R Cap. 3.7 para Jorge
3. **Contacto DI Finance antes de 2026-04-14** — Daniel obtener contacto e iniciar investigacion costes DUAGON
4. **Aprobacion baseline BAC antes de 2026-04-14** — Daniel enviar €31,040.33 BAC a STK-02 para firma

## Riesgos Identificados
1. **Single-point-of-failure en aprobaciones RSK-08/RSK-09** — Ambos dependen de disponibilidad de STK-02; si no disponible, gate PM100 en riesgo
2. **Bloqueante BLG-07** — Si se retrasa clarificacion del rulebook, testing del prompt de analisis de riesgos se desliza
```

### Invocacion

```
Follow instructions in #prompt:meeting-summary.prompt.md with these arguments:
  @file:meetings/raw/2026-04-07-team-sync.md
  --meeting-type operational
```

### Salida Esperada de la IA

```markdown
# Minutas de Reunion — Weekly Team Sync

## Metadatos de la Reunion
| Propiedad | Valor |
|-----------|-------|
| **Fecha** | 2026-04-07, 10:00–11:00 |
| **Tipo** | Operativa |
| **Responsable** | Daniel Velasco (STK-01, PM) |
| **Asistentes** | Daniel Velasco (PM), Jorge Jimenez (Miembro de Equipo, STK-05) |
| **Ubicacion/Plataforma** | Teams |
| **Objetivo(s) vinculado(s)** | OBJ-01, OBJ-02, OBJ-03 |

## Resumen Ejecutivo
- **BLG-06 on track**: Diseno del prompt de reunion completo; testing programado para semana siguiente
- **BLG-07 bloqueado**: Esperando clarificacion del rulebook sobre proceso O&R de 6 pasos; Daniel entregara resumen antes de 2026-04-10
- **RSK-08 escalacion**: Pico de costes DUAGON (€14,848) requiere investigacion de Finance; contacto pendiente via STK-02
- **RSK-09 critico**: Aprobacion de baseline presupuestaria necesaria antes de 2026-04-14 para desbloquear analisis de variaciones del Quality Gate PM100

## Agenda y Temas Cubiertos

| Tema | Duracion | Responsable |
|------|----------|-------------|
| BLG-06: Estado Diseno Prompt Reunion | 20 min | Daniel Velasco |
| BLG-07: Bloqueante Prompt Analisis Riesgos | 15 min | Jorge Jimenez |
| RSK-08: Investigacion Costes DUAGON | 15 min | Daniel Velasco |
| RSK-09: Formalizacion Baseline Presupuestaria | 10 min | Daniel Velasco |

## Decisiones Clave

| Decision | Responsable | Impacto | Objetivo vinculado | Fecha limite |
|----------|-------------|---------|-------------------|--------------|
| Priorizar testing BLG-06 con notas reales | Daniel Velasco | Valida calidad de output IA para evaluacion OBJ-02 | OBJ-02 | 2026-04-14 |
| Documentar proceso O&R Cap. 3.7 PM@Siemens para BLG-07 | Daniel Velasco | Desbloquea diseno de prompt de analisis de riesgos de Jorge | OBJ-01, OBJ-02 | 2026-04-10 |
| Iniciar investigacion costes DUAGON via contacto Finance de STK-02 | Daniel Velasco | Resuelve riesgo proveedor RSK-08; habilita readiness PM100 | OBJ-01 | 2026-04-14 |
| Enviar baseline BAC (€31,040.33) para aprobacion de STK-02 | Daniel Velasco | CRITICO: Habilita tracking de variaciones segun PM@Siemens Cap. 3.5 | OBJ-01 | 2026-04-14 |

## Items de Accion

| ID | Tarea | Responsable | Fecha limite | Prioridad | Estado | Backlog vinculado | Dependencias |
|----|-------|-------------|--------------|-----------|--------|-------------------|--------------|
| AI-001 | Testear prompt BLG-06 con notas reales de reunion | Daniel Velasco | 2026-04-14 | Alta | Pendiente | BLG-06 | BLG-05 (arquitectura completa) |
| AI-002 | Documentar resumen proceso O&R Cap. 3.7 PM@Siemens | Daniel Velasco | 2026-04-10 | Alta | Pendiente | BLG-07 | Ninguna |
| AI-003 | Obtener contacto DI Finance de STK-02; iniciar investigacion costes DUAGON | Daniel Velasco | 2026-04-14 | Alta | Pendiente | RSK-08-ACTION | Ninguna |
| AI-004 | Preparar documentacion baseline BAC (€31,040.33) para aprobacion STK-02 | Daniel Velasco | 2026-04-14 | Alta | Pendiente | RSK-09-ACTION | BLG-04b (financial tracking completo) |

## Riesgos / Incidencias / No-Conformidades Identificados

| ID | Descripcion | Tipo | Responsable | Vinculo al Registro O&R | Respuesta recomendada |
|----|-------------|------|-------------|------------------------|----------------------|
| RSK-08-FOLLOW | Concentracion proveedor DUAGON (72.8% del EAC); dependencia de proveedor unico | Riesgo | Daniel Velasco | Coincide con RSK-08 (abierto) | Monitorizar: Investigar pico €14,848 Nov; considerar proveedores alternativos para fase 2 |
| RSK-09-FOLLOW | Sin baseline BAC formal bloquea tracking de variaciones del Quality Gate PM100 | Riesgo | Daniel Velasco | Coincide con RSK-09 (abierto) | Mitigar: Formalizar €31,040.33 como baseline BAC antes de 2026-04-14 |
| RSK-NEW-001 | Single-point-of-failure: RSK-08 y RSK-09 ambos dependen de disponibilidad de STK-02 | Riesgo | Daniel Velasco | Nuevo — evaluar probabilidad e impacto | Mitigar: Definir ruta de escalacion si STK-02 no disponible; identificar aprobador backup |

## Actualizaciones de Milestones y Timeline

- **Readiness PM100**: En Riesgo — RSK-08 y RSK-09 deben resolverse antes de fecha del gate (est. 2026-04-30)
- **Cambios de timeline**: Ninguno al planning PM; todos los items de accion vencen 2026-04-14 (antes del gate PM100)
- **Cambios de backlog**: Ninguno; todos los items de accion vinculados a backlog existente (BLG-06, BLG-07, RSK-08-ACTION, RSK-09-ACTION)

## Notificacion a Stakeholders Requerida

| Stakeholder | Resumen del mensaje | Metodo de notificacion | Responsable |
|-------------|--------------------|-----------------------|-------------|
| STK-02 (Line Manager) | Necesita: (1) Proporcionar contacto DI Finance para investigacion RSK-08, (2) Aprobar baseline BAC €31,040.33. Items de ruta critica vencen 2026-04-14. | Email + solicitud de reunion | Daniel Velasco |
| STK-05 (Jorge Jimenez) | Info de desbloqueo (resumen Cap. 3.7) sera entregada antes de 2026-04-10. Sin mas accion necesaria. | Actualizacion workspace | Daniel Velasco |

## Proximos Pasos
1. **AI-002 (Vence 2026-04-10)**: Daniel completar resumen rulebook O&R para Jorge → desbloquear BLG-07
2. **AI-001, AI-003, AI-004 (Vencen 2026-04-14)**: Los tres items de ruta critica deben completarse antes del gate PM100
3. **Escalacion de riesgo**: Si STK-02 no disponible antes de 2026-04-10, escalar inmediatamente a liderazgo DI

---

## Ficheros a Actualizar (Revision PM)
- [x] `meetings/summaries/minutes-2026-04-07.md` (este fichero) — **LISTO PARA COMMIT**
- [ ] `planning/backlog.md` (sin cambios; todos los items de accion ya vinculados)
- [x] `context/RISKS.md` (RSK-NEW-001 identificado; anotar dependencia de STK-02)
- [ ] `tracking/ncc-log.md` (sin no-conformidades registradas en esta reunion)

## Checklist de Validacion de Calidad (El PM Debe Verificar Antes del Commit)
- [x] **Asistentes**: Daniel (STK-01, PM), Jorge (STK-05, Equipo) — todos los roles validados contra STAKEHOLDERS.md
- [x] **Decisiones**: 4 decisiones documentadas; todas claramente con responsable y fecha
- [x] **Items de accion**: 4 items de accion; todos con responsable (Daniel), fecha limite (2026-04-14), prioridad (Alta) y vinculo a backlog
- [x] **Riesgos**: 3 riesgos identificados (RSK-08, RSK-09, RSK-NEW-001); vinculados al proceso O&R de 6 pasos
- [x] **Objetivos**: Todos los items de accion vinculados a OBJ-01, OBJ-02 u OBJ-03
- [x] **Terminologia**: Terminologia PM@Siemens usada (Quality Gate, Registro O&R, baseline BAC, etc.)
- [x] **Notificacion stakeholders**: Plan de escalacion STK-02 definido; Jorge notificado del desbloqueo
- [x] **Precision temporal**: Readiness gate PM100 evaluada como "En Riesgo" por ruta critica RSK-08/RSK-09
- [x] **Actualizaciones de ficheros**: Recomendaciones revisadas; actualizacion RISKS.md pendiente para RSK-NEW-001
- [x] **Tono profesional**: Las minutas son objetivas y listas para audit trail

**Resultado del Checklist**: ✅ **10/10 PASS — Listo para Commit**
```

### Revision y Validacion del PM

**Checklist**: Los 10 items pasan ✅

**Commit**:
```bash
git add meetings/summaries/minutes-2026-04-07.md
git commit -m "feat: resumen reunion 2026-04-07 team sync

Temas: progreso BLG-06, bloqueante BLG-07, ruta critica RSK-08/RSK-09
Items de Accion: 4 (AI-001 test BLG-06, AI-002 rulebook, AI-003 Finance, AI-004 BAC)
Riesgos: RSK-NEW-001 (STK-02 single-point-of-failure)
Objetivos: OBJ-01, OBJ-02, OBJ-03

Checklist: 10/10 PASS — listo para alineacion gate PM100

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>"
```

**Seguimiento**:
1. Actualizar `context/RISKS.md` para anadir RSK-NEW-001
2. Enviar email a STK-02 con solicitudes AI-003 y AI-004 (vencen 2026-04-14)

---

## Solucion de Problemas Comunes

### Problema: "El item de accion no tiene responsable"
**Causa**: Las notas crudas no aclaran quien es responsable
**Solucion**: Re-promptear con clarificacion o asignar manualmente antes del commit
```
AI-001 dice "Investigar pico costes DUAGON" pero el responsable es ambiguo. Por favor clarificar:
- ¿Daniel es responsable? ¿O deberia ir a Finance?
- Sugerencia: Responsable = Daniel (coordinador) + DI Finance (ejecutor)
```

### Problema: "Este item de accion no vincula a ningun objetivo"
**Causa**: La tarea esta fuera de alcance o no esta claramente alineada
**Solucion**: Eliminarlo o re-promptear para establecer vinculacion a objetivo
```
La tarea AI-002 "Actualizar web" no esta vinculada a OBJ-01..05.
¿Esta fuera de alcance? ¿O deberia vincularse a OBJ-04 (templates reutilizables)?
```

### Problema: "Falta la justificacion de la decision"
**Causa**: Las notas crudas no explican el "por que"
**Solucion**: Anadir justificacion manualmente o re-promptear con contexto
```
La decision de "priorizar BLG-06" necesita justificacion para la revision del gate.
Anadir: "Priorizado porque BLG-06 es ruta critica para validacion OBJ-02 (evaluar calidad documentacion),
que bloquea la evaluacion de readiness PM100."
```

### Problema: "El riesgo esta identificado pero no vinculado al proceso O&R de 6 pasos"
**Causa**: El prompt no reconocio el formato de riesgo
**Solucion**: Re-promptear con lenguaje explicito de riesgo
```
Se menciono un riesgo: "Single-point-of-failure en STK-02".
Por favor reestructurar como una entrada propia del registro O&R y vincular al proceso de 6 pasos:
Identificar → Evaluar (probabilidad/impacto) → Responder (mitigacion) → Monitorizar → Reportar → Sostener
```

---

## Buenas Practicas

1. **Preparar notas el mismo dia** — No esperar dias para transcribir; capturar notas crudas inmediatamente despues de la reunion
2. **Usar formato de fecha consistente** — Siempre `YYYY-MM-DD` para ordenacion y vinculacion
3. **Nombrar asistentes con titulo, no solo nombre de pila** — Habilita cruce con STAKEHOLDERS.md
4. **Capturar decisiones explicitamente** — No enterrar decisiones en narrativa; usar formato de decision
5. **Vincular cada item de accion a un item del backlog** — Previene tareas huerfanas
6. **Marcar nuevos riesgos inmediatamente** — No diferir la identificacion de riesgos
7. **Revisar el Checklist de Validacion de Calidad antes del commit** — Este es tu gate de QA
8. **Actualizar ficheros dependientes consistentemente** — Si la reunion actualiza backlog, comitear backlog.md en el mismo push
9. **Usar mensajes de commit claros** — Incluir temas, numero de acciones, numero de riesgos, resultado del checklist
10. **Escalar pronto si items de ruta critica estan en riesgo** — No esperar hasta la revision del gate

---

## Tips para Reuniones Steering/Gate

- Usar `--meeting-type steering` para revisiones de Quality Gate
- La salida incluye un checklist de "Evaluacion de Readiness de Gate" vinculado a outputs obligatorios de la Guia PM@Siemens
- Siempre incluir seccion de sign-off de aprobadores
- Marcar explicitamente cualquier aprobacion condicional o dispensa
- Usar nombre de fichero gate-review: `gate-review-[MILESTONE]-[FECHA].md`

---

## Contacto y Soporte

¿Preguntas sobre el prompt?
- Consultar el prompt en si: `.github/prompts/meeting-summary.prompt.md`
- Revisar Guia PM@Siemens Cap. 3.9 para estandares de Communication & Collaboration
- Escalar preguntas de governance a Daniel Velasco (experto PM@Siemens en el proyecto)

---

*Guia de Uso v2.0 | 2026-04-07 | PM@Siemens Cap. 3.9*
