---
description: Guia el cierre de proyecto PM700 verificando todos los requisitos de cierre, lecciones aprendidas y completitud de archivado
agent: ask
---

# Revision de Cierre de Proyecto — PM@Siemens (PM700)

**Proposito**: Guiar el proceso de cierre de proyecto en PM700 verificando que todos los requisitos de cierre se cumplen, capturando lecciones aprendidas, asegurando el cierre financiero y confirmando el archivado correcto. Se alinea con la Guia PM@Siemens Cap. 5 (Milestone Model — requisitos PM700).

**Alcance**: Cierre de proyecto, traspaso de garantia, captura de lecciones aprendidas, reconciliacion financiera y archivado del workspace.

---

## Invocacion

```
Follow instructions in #prompt:closeout-review.prompt.md with these arguments:
  --project-folder @workspace
  --closure-type [standard|early-termination|partial]
```

**Parametros**:
- `--project-folder`: Referencia al workspace para evaluacion completa de cierre
- `--closure-type`: `standard` (fin normal del proyecto), `early-termination` (proyecto cancelado/detenido), `partial` (cierre de fase, proyecto continua)

---

## Pasos de Procesamiento

### Paso 1: Auditoria Completa del Workspace
**Leer TODOS los artefactos para verificar completitud del cierre**:
1. `context/OBJECTIVES.md` — ¿Se cumplieron todos los objetivos? ¿Cual es el estado final?
2. `planning/backlog.md` — ¿Todas las tareas estan en Done o formalmente canceladas?
3. `tracking/financial-tracking.md` — Reconciliacion final EAC vs BAC
4. `tracking/action-tracker.csv` — ¿Todas las acciones cerradas?
5. `tracking/scope-changes.md` — ¿Todos los cambios formalmente resueltos?
6. `tracking/ncc-log.md` — ¿Todas las NCC cerradas o transferidas a garantia?
7. `context/RISKS.md` — ¿Todos los riesgos cerrados, transferidos o aceptados?
8. `context/STAKEHOLDERS.md` — Estado de sign-off de stakeholders
9. `decisions/` — ¿Todas las decisiones pendientes resueltas?
10. `Oferta_Pedido/` — ¿Obligaciones contractuales cumplidas?
11. `meetings/summaries/` — ¿Reunion final de gate documentada?

### Paso 2: Verificacion de Cierre
Para cada output obligatorio de PM700:
- ¿Esta completo?
- ¿Donde esta la evidencia?
- ¿Quien firmo?

### Paso 3: Captura de Lecciones Aprendidas
- ¿Que fue bien?
- ¿Que hariamos diferente?
- ¿Que debe compartirse con la comunidad PM?
- ¿Que mejoras de template/proceso surgieron?

---

## Estructura de Salida

```markdown
# Revision de Cierre de Proyecto — [Nombre del Proyecto] — [Fecha: YYYY-MM-DD]

## Metadatos del Cierre
| Propiedad | Valor |
|-----------|-------|
| **Nombre del proyecto** | [De OBJECTIVES.md] |
| **ID Proyecto / WBS** | [De financial-tracking.md] |
| **Cliente** | [De OBJECTIVES.md] |
| **PM** | [De STAKEHOLDERS.md] |
| **Tipo de cierre** | Estandar / Terminacion anticipada / Parcial |
| **Fecha de cierre** | YYYY-MM-DD |
| **Readiness de cierre** | 🟢 Preparado / 🟡 Condicional / 🔴 No preparado |

## Resumen Ejecutivo
[3–5 frases: Resultado del proyecto, logros clave, items pendientes, recomendacion de cierre]

## Consecucion de Objetivos

| ID | Objetivo | Criterio de exito | Estado final | Evidencia |
|----|----------|-------------------|--------------|-----------|
| OBJ-01 | [Objetivo] | [Criterio] | Conseguido / Parcialmente / No conseguido | [Referencia] |
| OBJ-02 | [Objetivo] | [Criterio] | Conseguido / Parcialmente / No conseguido | [Referencia] |

**Global**: [X] de [Y] objetivos conseguidos ([%])

## Checklist de Outputs Obligatorios PM700

| # | Output obligatorio | Estado | Evidencia | Responsable | Sign-Off |
|---|-------------------|--------|-----------|-------------|----------|
| 1 | Aceptacion cliente / handover documentado | ✓/⚠/✗ | [Fichero/ubicacion] | [Nombre] | [Fecha] |
| 2 | Cierre financiero completo | ✓/⚠/✗ | [Fichero/ubicacion] | [Nombre] | [Fecha] |
| 3 | Lecciones aprendidas capturadas | ✓/⚠/✗ | [Fichero/ubicacion] | [Nombre] | [Fecha] |
| 4 | Terminos de garantia documentados y traspasados | ✓/⚠/✗ | [Fichero/ubicacion] | [Nombre] | [Fecha] |
| 5 | Todos los claims / NCC resueltos o transferidos | ✓/⚠/✗ | [Fichero/ubicacion] | [Nombre] | [Fecha] |
| 6 | Registro O&R cerrado | ✓/⚠/✗ | [Fichero/ubicacion] | [Nombre] | [Fecha] |
| 7 | Documentacion del proyecto archivada | ✓/⚠/✗ | [Fichero/ubicacion] | [Nombre] | [Fecha] |
| 8 | Notificacion de cierre a stakeholders | ✓/⚠/✗ | [Fichero/ubicacion] | [Nombre] | [Fecha] |
| 9 | Recursos liberados / reasignados | ✓/⚠/✗ | [Fichero/ubicacion] | [Nombre] | [Fecha] |
| 10 | Contratos de proveedores cerrados | ✓/⚠/✗ | [Fichero/ubicacion] | [Nombre] | [Fecha] |

## Cierre Financiero

| Metrica | Baseline (BAC) | Final (EAC) | Desviacion | Notas |
|---------|----------------|-------------|-----------|-------|
| Ingresos totales | [€] | [€] | [+/-] | [Explicacion] |
| Coste total | [€] | [€] | [+/-] | [Explicacion] |
| Margen bruto | [%] | [%] | [+/- pp] | [Explicacion] |
| Cuentas por cobrar abiertas | — | [€] | — | [Estado de cobro] |
| Cuentas por pagar abiertas | — | [€] | — | [Estado de liquidacion] |
| Provision de garantia | — | [€] | — | [Adecuacion] |

**Veredicto financiero**: En presupuesto / Por debajo / Por encima en [%]

## Items Abiertos Transferidos a Garantia / Operaciones

| ID | Item | Tipo | Transferido a | Fecha limite | Notas |
|----|------|------|--------------|--------------|-------|
| [ID] | [Descripcion] | NCC/Claim garantia/Punch item | [Nombre/Equipo] | [Fecha] | [Contexto] |

## Resumen de Cambios de Alcance (Vida del Proyecto)

| Metrica | Valor |
|---------|-------|
| Total cambios de alcance procesados | [N] |
| Aprobados | [N] |
| Rechazados | [N] |
| Impacto neto en coste | [€ +/-] |
| Impacto neto en plazo | [+/- dias/semanas] |

## Lecciones Aprendidas

### Que Fue Bien
| # | Categoria | Leccion | Reutilizable para |
|---|-----------|---------|-------------------|
| 1 | [Proceso/Tecnico/Comercial/Comunicacion] | [Descripcion] | [Futuros proyectos/templates] |
| 2 | [Categoria] | [Descripcion] | [Aplicabilidad] |

### Que Hariamos Diferente
| # | Categoria | Problema | Causa raiz | Mejora |
|---|-----------|----------|-----------|--------|
| 1 | [Categoria] | [Que ocurrio] | [Por que] | [Como prevenirlo la proxima vez] |
| 2 | [Categoria] | [Que ocurrio] | [Por que] | [Como prevenirlo la proxima vez] |

### Recomendaciones para la Comunidad PM
| # | Recomendacion | Aplicabilidad | Compartir con |
|---|---------------|---------------|---------------|
| 1 | [Recomendacion] | [Tipo de proyecto/contexto] | [CoP PM / Actualizacion template / Formacion] |

## Feedback del Template
| # | Elemento del template | Feedback | Cambio sugerido |
|---|----------------------|----------|-----------------|
| 1 | [Fichero/prompt/estructura] | [Que funciono o no] | [Mejora] |

## Checklist de Archivado

| Item | Estado | Ubicacion | Notas |
|------|--------|-----------|-------|
| Workspace del proyecto (este repo) | ✓/✗ | [Ubicacion del archivo] | [Solo lectura tras cierre] |
| Entregables al cliente | ✓/✗ | [Ubicacion] | [Entregados en fecha] |
| Registros financieros | ✓/✗ | [SAP / Archivo] | [Periodo de retencion] |
| Contratos y adendas | ✓/✗ | [Ubicacion] | [Periodo de retencion] |
| Archivo de correspondencia | ✓/✗ | [Ubicacion] | [Correos clave preservados] |

## Sign-Off de Cierre

| Aprobador | Rol | Aprobacion | Fecha | Condiciones |
|-----------|-----|------------|-------|-------------|
| [Nombre] | PM | ✓/✗ | YYYY-MM-DD | [Ninguna / condiciones] |
| [Nombre] | Line Manager / Sponsor | ✓/✗ | YYYY-MM-DD | [Ninguna / condiciones] |
| [Nombre] | Cliente (si aplica) | ✓/✗ | YYYY-MM-DD | [Ninguna / condiciones] |

## Acciones Restantes (Post-Cierre)
| # | Accion | Responsable | Fecha limite | Notas |
|---|--------|-------------|--------------|-------|
| 1 | [Tarea post-cierre] | [Nombre] | [Fecha] | [ej., seguimiento garantia, factura final] |
```

---

## Checklist de Validacion (El PM Debe Verificar)
- [ ] **Todos los objetivos evaluados**: Cada OBJ-XX tiene un estado final
- [ ] **Financiero reconciliado**: BAC vs EAC final documentado; sin desviaciones sin explicar
- [ ] **Items abiertos transferidos**: Nada queda sin responsable (garantia, operaciones o formalmente cerrado)
- [ ] **Lecciones capturadas**: Significativas y accionables (no generalidades)
- [ ] **Stakeholders notificados**: Todos los stakeholders clave informados del cierre
- [ ] **Archivado completo**: Todos los documentos en ubicaciones de archivo designadas
- [ ] **Sign-off obtenido**: Los aprobadores requeridos han firmado formalmente
- [ ] **Feedback del template**: Sugerencias de mejora documentadas para evolucion del template

---

## Notas de Compliance PM@Siemens

- PM700 cierra el proyecto formalmente — no se permite mas imputacion de costes tras el cierre
- Las obligaciones de garantia se transfieren a operaciones/servicio; el PM debe documentar el traspaso
- Las lecciones aprendidas son un output obligatorio de PM700 segun la Guia PM@Siemens
- El cierre financiero debe reconciliarse con la liquidacion del proyecto SAP
- La terminacion anticipada requiere documentacion adicional de razones e impacto
- Si el proyecto forma parte de un programa, notificar al program management del cierre

---

*Version 1.0 | Alineado con PM@Siemens Cap. 5 (PM700 — Cierre de Proyecto)*
