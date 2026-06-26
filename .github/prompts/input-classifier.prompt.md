---
description: Clasifica nuevo material de entrada y lo enruta a la ubicacion correcta del workspace con el procesamiento adecuado
agent: agent
---

# Clasificador y Enrutador de Material de Entrada — PM@Siemens

**Proposito**: Procesar nuevo material dejado en `.github/input/` (o proporcionado directamente). Clasificar por tipo, extraer informacion accionable, enrutar a la carpeta correcta del workspace y actualizar los artefactos relevantes del proyecto. Asegura que nada se pierda y que la trazabilidad se mantenga.

**Alcance**: Correos, documentos, exportes, notas de reunion, solicitudes de cambio, comunicaciones con proveedores y cualquier input no clasificado del proyecto.

---

## Invocacion

```
Follow instructions in #prompt:input-classifier.prompt.md with these arguments:
  @file:<ruta-al-fichero-de-entrada>
  --project-folder @workspace
```

**Parametros**:
- `@file`: Ruta al fichero de entrada en `.github/input/` o en otra ubicacion
- `--project-folder`: Referencia al workspace para decisiones de enrutamiento

**Procesamiento por lotes** (multiples ficheros):
```
Follow instructions in #prompt:input-classifier.prompt.md with these arguments:
  --scan-folder .github/input/
  --project-folder @workspace
```

---

## Pasos de Procesamiento

### Paso 1: Identificar Tipo de Material

| Tipo | Indicadores | Ubicacion destino |
|------|------------|-------------------|
| **Correo / correspondencia** | .msg, .eml, firma de email, Para/De/CC | `Correos/` |
| **Documentacion de cliente** | Specs, planos, manuales, datasheets | `Documentacion/` (subcarpeta apropiada) |
| **Oferta / pedido / contrato** | Cotizacion, PO, terminos y condiciones | `Oferta_Pedido/` |
| **Documentacion de proveedor** | Oferta proveedor, albaran, factura | `Oferta_Pedido/Proveedores/` |
| **Ejecucion tecnica** | Docs de instalacion, puesta en marcha, material de obra | `Tecnico/` |
| **Seguimiento de materiales** | Programas de entrega, packing lists | `Tecnico/Seguimiento_Materiales/` |
| **Notas de reunion (crudas)** | Notas no estructuradas, transcripciones | `.github/meetings/raw/` |
| **Exporte financiero** | Exporte SAP, informe de costes, datos CSV | `.github/financials/` |
| **Registro de decision / aprobacion** | Aprobacion firmada, memo de decision | `.github/decisions/` |
| **No clasificable** | No se puede determinar el tipo | Mantener en `.github/input/` + marcar para PM |

### Paso 2: Extraer Contenido Accionable

Para cada input, verificar si contiene:
- [ ] **Items de accion** → Anadir a `tracking/action-tracker.csv`
- [ ] **Decisiones** → Registrar en `.github/decisions/`
- [ ] **Riesgos / incidencias** → Marcar para actualizar `context/RISKS.md`
- [ ] **Cambios de alcance** → Marcar para `tracking/scope-changes.md`
- [ ] **Datos financieros** → Marcar para actualizar `tracking/financial-tracking.md`
- [ ] **Info de stakeholders** → Marcar para actualizar `context/STAKEHOLDERS.md`
- [ ] **Cambios de timeline** → Marcar para actualizar `planning/backlog.md`

### Paso 3: Aplicar Convencion de Nombres

**Formato de nombre destino**: `YYYY-MM-DD_fuente_tema.[ext]`

Ejemplos:
- `2026-04-28_cliente_spec-update-rev3.pdf`
- `2026-05-01_SAP_cost-export-W18.csv`
- `2026-04-30_proveedor_DUAGON-programa-entrega.xlsx`

### Paso 4: Generar Informe de Procesamiento

---

## Estructura de Salida

```markdown
# Informe de Procesamiento de Inputs — [Fecha: YYYY-MM-DD]

## Ficheros Procesados

| # | Nombre original | Tipo | Enrutado a | Renombrado como | Acciones extraidas |
|---|-----------------|------|------------|-----------------|-------------------|
| 1 | [original.ext] | [Tipo] | [Carpeta destino] | [YYYY-MM-DD_fuente_tema.ext] | [Cantidad] |
| 2 | [original.ext] | [Tipo] | [Carpeta destino] | [YYYY-MM-DD_fuente_tema.ext] | [Cantidad] |

## Items de Accion Extraidos

| Fichero fuente | Accion | Responsable | Fecha limite | Prioridad | Enrutar a |
|----------------|--------|-------------|--------------|-----------|-----------|
| [Fichero #] | [Descripcion de la accion] | [Nombre/TBD] | [Fecha/TBD] | A/M/B | action-tracker.csv |

## Riesgos / Incidencias Extraidos

| Fichero fuente | Descripcion | Categoria | Urgencia | Enrutar a |
|----------------|-------------|-----------|----------|-----------|
| [Fichero #] | [Descripcion del riesgo/incidencia] | [Tipo] | A/M/B | context/RISKS.md |

## Decisiones Extraidas

| Fichero fuente | Decision | Decisor | Fecha | Enrutar a |
|----------------|----------|---------|-------|-----------|
| [Fichero #] | [Enunciado de la decision] | [Nombre] | [Fecha] | decisions/ |

## Implicaciones Financieras Detectadas

| Fichero fuente | Tipo | Importe | Impacto en | Enrutar a |
|----------------|------|---------|-----------|-----------|
| [Fichero #] | [Coste/Ingreso/Cambio] | [€ importe] | [EAC/Facturacion/Contingencia] | financial-tracking.md |

## Senales de Cambio de Alcance

| Fichero fuente | Descripcion | Origen | Impacto | Enrutar a |
|----------------|-------------|--------|---------|-----------|
| [Fichero #] | [Descripcion del cambio] | [Cliente/Interno] | [Evaluacion preliminar] | scope-changes.md |

## No Clasificados / Requiere Decision del PM

| # | Nombre de fichero | Problema | Accion del PM necesaria |
|---|-------------------|----------|------------------------|
| 1 | [fichero.ext] | [Por que no se puede auto-clasificar] | [Que debe decidir el PM] |

## Actualizaciones Recomendadas al Workspace

- [ ] Mover [fichero] a [carpeta destino]
- [ ] Anadir [accion] a `tracking/action-tracker.csv`
- [ ] Actualizar `context/RISKS.md` con [nuevo riesgo]
- [ ] Marcar [cambio de alcance] en `tracking/scope-changes.md`
- [ ] Ejecutar `#prompt:scope-impact.prompt.md` para [solicitud de cambio]
- [ ] Ejecutar `#prompt:risk-analysis.prompt.md` para [indicador de riesgo]

## Resumen de Procesamiento
| Metrica | Valor |
|---------|-------|
| Ficheros procesados | [N] |
| Ficheros enrutados con exito | [N] |
| Acciones extraidas | [N] |
| Riesgos marcados | [N] |
| No clasificados (revision PM) | [N] |
```

---

## Checklist de Validacion (El PM Debe Verificar)
- [ ] **Enrutamiento correcto**: Ficheros colocados en la carpeta correcta segun estructura del template
- [ ] **Nombres consistentes**: Convencion `YYYY-MM-DD_fuente_tema` seguida
- [ ] **Sin perdida de datos**: Todos los ficheros de entrada estan contabilizados (ninguno descartado silenciosamente)
- [ ] **Acciones capturadas**: Items de accion importantes no omitidos
- [ ] **Confidencialidad respetada**: Material sensible marcado apropiadamente
- [ ] **Originales preservados**: Ficheros fuente no modificados durante el procesamiento

---

## Notas de Compliance PM@Siemens

- Toda correspondencia con el cliente debe ser trazable en `Correos/`
- Documentos relevantes para el contrato pertenecen a `Oferta_Pedido/`, nunca en `.github/`
- Los exportes financieros deben ir a `.github/financials/` para procesamiento por IA
- Material con implicaciones LoA debe marcarse inmediatamente
- Las senales de cambio de alcance deben procesarse formalmente antes de ejecutarse (sin cambios implicitos)

---

*Version 1.0 | Alineado con la Estructura del Workspace PM@Siemens*
