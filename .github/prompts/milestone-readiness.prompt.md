---
description: Evalua la preparacion para Quality Gate verificando outputs obligatorios contra requisitos de la Guia PM@Siemens
agent: ask
---

# Verificacion de Readiness de Milestone y Quality Gate — PM@Siemens

**Proposito**: Realizar una evaluacion integral de readiness 2–4 semanas antes de un Quality Gate PM@Siemens. Verifica todos los outputs obligatorios, requisitos de compliance y items abiertos contra la Guia PM@Siemens Cap. 5 (Milestone Model). Genera un analisis de gaps y un plan de accion para lograr el paso del gate.

**Alcance**: Verificaciones de readiness pre-gate para PM040, PM100, PM200, PM600, PM700 y Quality Gates intermedios.

---

## Invocacion

```
Follow instructions in #prompt:milestone-readiness.prompt.md with these arguments:
  --milestone [PM040|PM100|PM200|PM600|PM700]
  --gate-date [YYYY-MM-DD]
  --project-type [solution|service|software-driven|category-s]
  --project-folder @workspace
```

**Parametros**:
- `--milestone`: Milestone PM@Siemens objetivo a evaluar
- `--gate-date`: Fecha planificada para la revision del Quality Gate
- `--project-type`: Determina que outputs obligatorios aplican (segun Guia PM@Siemens Cap. 5.3–5.6)
- `--project-folder`: Referencia al workspace para escaneo completo de contexto

---

## Pasos de Procesamiento

### Paso 1: Cargar Requisitos del Gate
Leer las secciones de `PM@Siemens-Guide.md` relevantes para el milestone objetivo:
- Outputs obligatorios para este gate y tipo de proyecto
- Autoridades de aprobacion (LoA)
- Requisitos de compliance (EHS, Cybersecurity, Quality Management)
- Requisitos FRG si aplica

### Paso 2: Escaneo Completo del Workspace
**Leer TODOS los artefactos del proyecto para evaluar completitud**:
1. `context/OBJECTIVES.md` — ¿Los objetivos son claros y medibles?
2. `context/STAKEHOLDERS.md` — ¿Los aprobadores estan identificados y disponibles?
3. `context/RISKS.md` — ¿El Registro O&R esta actualizado? ¿Riesgos criticos mitigados?
4. `context/SOLUTION-OVERVIEW.md` — ¿El concepto de solucion esta documentado?
5. `planning/backlog.md` — ¿Todas las tareas criticas para el gate estan completadas?
6. `tracking/financial-tracking.md` — ¿La baseline financiera esta establecida? ¿EAC saludable?
7. `tracking/scope-changes.md` — ¿Todos los cambios aprobados estan reflejados?
8. `tracking/ncc-log.md` — ¿Las no-conformidades estan resueltas o dispensadas?
9. `tracking/action-tracker.csv` — ¿Las acciones criticas estan cerradas?
10. `meetings/summaries/` — ¿Todas las reuniones pre-gate estan documentadas?
11. `decisions/` — ¿Las decisiones relevantes para el gate estan registradas?

### Paso 3: Analisis de Gaps
Para cada output obligatorio:
- **Estado**: Completo / En progreso / Ausente / No aplica
- **Evidencia**: ¿Donde esta el entregable en el workspace?
- **Gap**: ¿Que falta y que accion se necesita?
- **Responsable**: ¿Quien debe cerrar este gap?
- **Viabilidad**: ¿Se puede cerrar el gap antes de la fecha del gate?

---

## Estructura de Salida

```markdown
# Evaluacion de Readiness de Quality Gate — [Milestone] — [Fecha]

## Metadatos de la Evaluacion
| Propiedad | Valor |
|-----------|-------|
| **Fecha de evaluacion** | YYYY-MM-DD |
| **Gate objetivo** | PM040 / PM100 / PM200 / PM600 / PM700 |
| **Fecha planificada del gate** | YYYY-MM-DD |
| **Dias hasta el gate** | [N] dias |
| **Tipo de proyecto** | Solution / Service / Software-driven / Category S |
| **Readiness general** | 🟢 Preparado / 🟡 Preparado con condiciones / 🔴 No preparado |

## Resumen Ejecutivo
[3–5 frases: postura general de readiness, gaps criticos, nivel de confianza para paso del gate]

**Recomendacion**: Proceder / Proceder con condiciones / Posponer — [Justificacion]

## Checklist de Outputs Obligatorios

| # | Output obligatorio | Estado | Ubicacion de evidencia | Gap / Accion necesaria | Responsable | ¿Viable antes del gate? |
|---|-------------------|--------|------------------------|------------------------|-------------|------------------------|
| 1 | [Output segun Guia PM@Siemens] | ✓/⚠/✗ | [Ruta de fichero o N/A] | [Descripcion del gap] | [Nombre] | Si/No/En riesgo |
| 2 | [Output segun Guia PM@Siemens] | ✓/⚠/✗ | [Ruta de fichero o N/A] | [Descripcion del gap] | [Nombre] | Si/No/En riesgo |
| ... | ... | ... | ... | ... | ... | ... |

**Leyenda**: ✓ = Completo y verificado | ⚠ = En progreso / condicional | ✗ = Ausente / bloqueado

**Puntuacion**: [X] de [Y] outputs obligatorios completos ([%])

## Verificacion de Compliance y Governance

| Area | Requisito | Estado | Notas |
|------|-----------|--------|-------|
| **LoA** | Autoridad de aprobacion confirmada | ✓/✗ | [¿Quien aprueba este gate?] |
| **FRG** | Financial Review Gate pasado | ✓/✗/N/A | [Si aplica] |
| **EHS** | Medio ambiente, Salud y Seguridad revisado | ✓/✗/N/A | [Estado] |
| **Cybersecurity** | Evaluacion de seguridad completada | ✓/✗/N/A | [Estado] |
| **Quality** | No-conformidades resueltas | ✓/⚠/✗ | [Numero de NCC abiertas] |
| **O&R** | Registro de riesgos actualizado, criticos mitigados | ✓/⚠/✗ | [Estado] |

## Riesgos Abiertos que Amenazan el Paso del Gate

| ID | Riesgo | Calificacion | Impacto en gate | Estado de mitigacion | ¿Cerrable antes del gate? |
|----|--------|--------------|-----------------|---------------------|--------------------------|
| RSK-XX | [Descripcion] | Critico/Alto | [Como bloquea el gate] | [Estado actual] | Si/No |

## Acciones Criticas Requeridas Antes del Gate

| Prioridad | Accion | Responsable | Fecha limite | Dependencia | Estado |
|-----------|--------|-------------|--------------|-------------|--------|
| 1 (Critica) | [Accion para cerrar gap] | [Nombre] | [Fecha < fecha-gate] | [Si alguna] | No iniciada/En progreso |
| 2 (Critica) | [Accion para cerrar gap] | [Nombre] | [Fecha < fecha-gate] | [Si alguna] | No iniciada/En progreso |
| 3 (Importante) | [Accion para cerrar gap] | [Nombre] | [Fecha < fecha-gate] | [Si alguna] | No iniciada/En progreso |

## Readiness Financiera

| Metrica | Valor | Requisito del gate | Estado |
|---------|-------|-------------------|--------|
| EAC vs BAC | [Variacion %] | Dentro del umbral LoA | 🟢/🟡/🔴 |
| Reales a la fecha | € [importe] | Alineado con plan | 🟢/🟡/🔴 |
| Claims/cambios abiertos | [N] | Resueltos o documentados | 🟢/🟡/🔴 |
| Facturacion on track | [Estado] | Segun hitos contractuales | 🟢/🟡/🔴 |

## Preparacion de la Reunion de Gate

| Item | Estado | Notas |
|------|--------|-------|
| Reunion programada | ✓/✗ | [Fecha, ¿asistentes confirmados?] |
| Aprobadores confirmados disponibles | ✓/✗ | [Nombres de STAKEHOLDERS.md] |
| Presentacion de gate preparada | ✓/✗ | [Ubicacion en output/] |
| Pre-read distribuido | ✓/✗ | [¿A quien, cuando?] |

## Recomendaciones

1. **[Critico]**: [Accion necesaria antes del gate] — Responsable: [Nombre], Fecha: [Fecha]
2. **[Importante]**: [Paso de preparacion] — Responsable: [Nombre], Fecha: [Fecha]
3. **[Orientativo]**: [Sugerencia de buena practica] — Responsable: [Nombre]

## Ficheros a Actualizar Antes del Gate
- [ ] [Ruta de fichero] — [Que necesita actualizacion]
- [ ] [Ruta de fichero] — [Que necesita actualizacion]
- [ ] [Ruta de fichero] — [Que necesita actualizacion]
```

---

## Checklist de Validacion (El PM Debe Verificar)
- [ ] **Completitud**: Todos los outputs obligatorios para este milestone y tipo de proyecto estan listados
- [ ] **Basado en evidencia**: Cada estado esta respaldado por una ubicacion de artefacto, no por supuestos
- [ ] **Gaps accionables**: Cada gap tiene un responsable, accion y evaluacion de viabilidad
- [ ] **LoA correcto**: La autoridad de aprobacion coincide con los requisitos LoA de PM@Siemens
- [ ] **Areas de compliance**: EHS, Cybersecurity, Quality, FRG evaluados donde aplica
- [ ] **Consciente del riesgo**: Riesgos abiertos mapeados explicitamente al impacto en gate
- [ ] **Realista**: La evaluacion refleja honestamente la readiness (sin falsos verdes)
- [ ] **Viable en tiempo**: Las acciones criticas pueden completarse realistamente antes de la fecha del gate

---

## Notas de Compliance PM@Siemens

- Los outputs obligatorios varian segun tipo de proyecto — siempre referenciar la seccion correcta de la Guia PM@Siemens Cap. 5
- **PM040** (Oportunidad Cualificada): Foco en business case, evaluacion inicial de riesgos, verificacion LoA
- **PM100** (Concepto Aprobado): Concepto de solucion, baseline presupuestaria, Registro O&R, mapa de stakeholders
- **PM200** (Listo para Ejecucion): Plan detallado, POs de proveedores, verificaciones de compliance, SPMP
- **PM600** (Handover): Protocolo de aceptacion, inicio de garantia, cierre de punch list
- **PM700** (Proyecto Cerrado): Lecciones aprendidas, cierre financiero, archivado, traspaso de garantia
- Usar aprobacion condicional ("proceder con dispensas") solo cuando las condiciones estan documentadas y acotadas en tiempo

---

*Version 1.0 | Alineado con PM@Siemens Cap. 5 (Milestone Model)*
