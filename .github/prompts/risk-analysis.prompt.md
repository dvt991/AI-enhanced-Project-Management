---
description: Identifica, evalua y estructura riesgos del proyecto siguiendo el proceso O&R de 6 pasos PM@Siemens
agent: ask
---

# Analisis de Riesgos y Actualizacion del Registro O&R — PM@Siemens

**Proposito**: Analizar la informacion proporcionada sobre Oportunidades y Riesgos del proyecto siguiendo el proceso de 6 pasos PM@Siemens (Identificar → Evaluar → Responder → Monitorizar → Reportar → Sostener). La salida se alinea con la Guia PM@Siemens Cap. 3.7 y se integra con `context/RISKS.md`.

**Alcance**: Revisiones periodicas de riesgos, analisis activados por eventos y evaluaciones O&R pre-gate.

---

## Invocacion

```
Follow instructions in #prompt:risk-analysis.prompt.md with these arguments:
  @file:<ruta-al-input-o-contexto>
  --trigger [periodic|event|pre-gate]
  --project-folder @workspace
```

**Parametros**:
- `@file`: Ruta al input que dispara el analisis (notas de reunion, correo, solicitud de cambio, informe financiero, etc.)
- `--trigger`: `periodic` (revision programada), `event` (nueva informacion recibida), `pre-gate` (verificacion de readiness de milestone)
- `--project-folder`: Referencia al workspace para lecturas de contexto

---

## Pasos de Procesamiento

### Paso 1: Lecturas de Contexto
Antes del analisis, **siempre leer y cruzar**:
1. `context/RISKS.md` — Registro O&R existente (evitar duplicados, actualizar entradas existentes)
2. `context/OBJECTIVES.md` — Vincular riesgos a objetivos que amenazan
3. `context/STAKEHOLDERS.md` — Identificar responsables de riesgo por area de competencia
4. `context/SOLUTION-OVERVIEW.md` — Contexto tecnico/solucion para riesgos de viabilidad
5. `planning/backlog.md` — Riesgos relacionados con plazos y dependencias
6. `tracking/financial-tracking.md` — Indicadores de riesgo financiero
7. `tracking/scope-changes.md` — Cambios aprobados/pendientes que introducen riesgo

### Paso 2: Identificar
- Escanear el input buscando menciones explicitas e indicios implicitos de riesgo
- Categorias a verificar: Tecnico, Comercial, Plazo, Recursos, Proveedor, Compliance, Externo
- Cruzar contra registro existente — ¿es una actualizacion o una nueva entrada?

### Paso 3: Evaluar (Matriz Probabilidad × Impacto)

| | Impacto Bajo | Impacto Medio | Impacto Alto |
|---|---|---|---|
| **Probabilidad Alta** | Medio | Alto | Critico |
| **Probabilidad Media** | Bajo | Medio | Alto |
| **Probabilidad Baja** | Bajo | Bajo | Medio |

### Paso 4: Responder
Para cada riesgo, determinar estrategia de respuesta:
- **Riesgos**: Evitar / Mitigar / Transferir / Aceptar
- **Oportunidades**: Explotar / Mejorar / Compartir / Aceptar

---

## Estructura de Salida

```markdown
# Analisis O&R — [Disparador: periodic/event/pre-gate] — [Fecha: YYYY-MM-DD]

## Metadatos del Analisis
| Propiedad | Valor |
|-----------|-------|
| **Fecha** | YYYY-MM-DD |
| **Disparador** | Periodico / Evento / Pre-Gate |
| **Fuente de input** | [Fichero o contexto analizado] |
| **Analista** | Asistido por IA (requiere revision del PM) |

## Resumen Ejecutivo
[2–3 frases: numero de nuevos riesgos/oportunidades identificados, items criticos, cambio en la postura de riesgo]

**Postura de Riesgo**: 🟢 Estable / 🟡 Elevada / 🔴 Critica

## Nuevos Riesgos Identificados

| ID | Descripcion | Categoria | Objetivo vinculado | Probabilidad | Impacto | Calificacion | Responsable | Estrategia de respuesta | Fecha limite |
|----|-------------|-----------|-------------------|--------------|---------|--------------|-------------|------------------------|--------------|
| RSK-NEW-XX | [Descripcion clara] | [Categoria] | OBJ-XX | A/M/B | A/M/B | Critico/Alto/Medio/Bajo | [De STAKEHOLDERS.md] | [Evitar/Mitigar/Transferir/Aceptar] | YYYY-MM-DD |

## Nuevas Oportunidades Identificadas

| ID | Descripcion | Categoria | Objetivo vinculado | Probabilidad | Impacto | Calificacion | Responsable | Estrategia de respuesta | Fecha limite |
|----|-------------|-----------|-------------------|--------------|---------|--------------|-------------|------------------------|--------------|
| OPP-NEW-XX | [Descripcion clara] | [Categoria] | OBJ-XX | A/M/B | A/M/B | [Calificacion] | [Responsable] | [Explotar/Mejorar/Compartir/Aceptar] | YYYY-MM-DD |

## Actualizaciones a Entradas Existentes del Registro

| ID | Cambio | Estado anterior | Nuevo estado | Justificacion |
|----|--------|-----------------|--------------|---------------|
| RSK-XX | [Que cambio] | [P×I / estado anterior] | [Nuevo P×I / estado] | [Por que] |

## Interdependencias de Riesgos

| Riesgo A | Riesgo B | Relacion | Impacto combinado |
|----------|----------|----------|-------------------|
| RSK-XX | RSK-YY | [Dispara / amplifica / bloquea mitigacion] | [Descripcion] |

## Recomendaciones (Priorizadas)
1. **[Critico]**: [Accion] — Responsable: [Nombre], Fecha: [Fecha]
2. **[Alto]**: [Accion] — Responsable: [Nombre], Fecha: [Fecha]
3. **[Medio]**: [Accion] — Responsable: [Nombre], Fecha: [Fecha]

## Estado del Proceso de 6 Pasos

| Paso | Estado | Notas |
|------|--------|-------|
| 1. Identificar | ✓ Hecho (este analisis) | [N] nuevos items identificados |
| 2. Evaluar | ✓ Hecho (este analisis) | Matriz P×I aplicada |
| 3. Responder | ✓ Propuesto | Requiere confirmacion del responsable |
| 4. Monitorizar | → Siguiente | Definir cadencia de revision y KRIs |
| 5. Reportar | → Siguiente | Incluir en proximo informe de estado |
| 6. Sostener | En curso | Actualizar registro tras revision del PM |

## Ficheros a Actualizar
- [ ] `context/RISKS.md` — Anadir nuevas entradas, actualizar existentes
- [ ] `tracking/action-tracker.csv` — Si se identifican acciones criticas para el PM
- [ ] `planning/backlog.md` — Si tareas de mitigacion requieren planificacion
- [ ] `tracking/financial-tracking.md` — Si la contingencia financiera se ve afectada
```

---

## Checklist de Validacion (El PM Debe Verificar)
- [ ] **Sin duplicados**: Los nuevos riesgos no duplican entradas existentes del registro
- [ ] **Vinculacion a objetivos**: Cada riesgo enlaza al menos un OBJ-XX que amenaza
- [ ] **Responsable asignado**: Cada riesgo tiene un responsable identificable de STAKEHOLDERS.md
- [ ] **Respuesta accionable**: La mitigacion/respuesta es especifica, no generica
- [ ] **Evaluacion justificada**: Las calificaciones de Probabilidad e Impacto tienen justificacion clara
- [ ] **Interdependencias anotadas**: Riesgos relacionados estan cruzados
- [ ] **Terminologia**: Usa terminos PM@Siemens (Registro O&R, No-conformidad, Quality Gate)
- [ ] **Escalacion marcada**: Riesgos Criticos/Altos que requieren escalacion LoA estan senalados

---

## Notas de Compliance PM@Siemens

- Usar "Oportunidad y Riesgo" (O&R), no solo "riesgo"
- Seguir el **proceso de 6 pasos**: Identificar → Evaluar → Responder → Monitorizar → Reportar → Sostener
- Usar "No-conformidad" para desviaciones de calidad/proceso, no "incidencia" ni "defecto"
- Referenciar umbrales de escalacion **LoA** para riesgos con calificacion Critica
- Los analisis pre-gate deben mapear a outputs obligatorios del gate segun Guia PM@Siemens Cap. 5

---

*Version 2.0 | Alineado con PM@Siemens Cap. 3.7*