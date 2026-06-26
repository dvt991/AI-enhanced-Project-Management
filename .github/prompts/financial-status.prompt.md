---
description: Analiza datos financieros y genera un informe de estado financiero conforme a PM@Siemens
agent: ask
---

# Generador de Informe de Estado Financiero — PM@Siemens

**Proposito**: Analizar datos financieros (exportes CSV o entrada manual) y generar un informe de estado financiero conforme a governance, alineado con la Guia PM@Siemens Cap. 3.5 (Cost & Finance Management). La salida se integra con `tracking/financial-tracking.md` y alimenta las revisiones de gate.

**Alcance**: Revisiones financieras periodicas (mensual, pre-gate) y analisis de desviaciones ad-hoc.

---

## Invocacion

```
Follow instructions in #prompt:financial-status.prompt.md with these arguments:
  @file:<ruta-al-csv-financiero-o-exporte>
  --report-type [periodic|pre-gate|variance]
  --project-folder @workspace
```

**Parametros**:
- `@file`: Ruta al CSV/exporte financiero en `.github/financials/` (ej., `financials/SAP_export_W18.csv`)
- `--report-type`: `periodic` (revision mensual), `pre-gate` (preparacion de milestone), `variance` (analisis de desviaciones)
- `--project-folder`: Referencia al workspace para lecturas de contexto

---

## Pasos de Procesamiento

### Paso 1: Lecturas de Contexto
Antes del analisis, **siempre leer y cruzar**:
1. `tracking/financial-tracking.md` — Baseline actual (BAC, EAC, ETC, hitos de facturacion)
2. `context/OBJECTIVES.md` — Vincular drivers de coste a objetivos del proyecto
3. `context/RISKS.md` — Cruzar riesgos financieros (entradas RSK existentes)
4. `tracking/scope-changes.md` — Cambios aprobados que afectan al presupuesto
5. `planning/backlog.md` — Trabajo pendiente que genera gasto futuro

### Paso 2: Analisis de Datos Financieros
Extraer del fichero proporcionado:
- **Reales (Actuals)**: Dinero ya gastado (columnas con meses historicos)
- **Forecast**: Gasto futuro esperado (columnas con meses futuros)
- **Desglose por recurso**: Por elemento de coste, persona o proveedor
- **Fecha de datos**: Ultimo mes con Actuals (determina el periodo de reporte)

### Paso 3: Generar Informe

---

## Estructura de Salida

```markdown
# Informe de Estado Financiero — [Fecha de Datos: YYYY-MM]

## Metadatos del Informe
| Propiedad | Valor |
|-----------|-------|
| **Fecha del informe** | YYYY-MM-DD |
| **Fecha de datos** | [Ultimo mes de Actuals] |
| **Tipo de informe** | Periodico / Pre-Gate / Desviaciones |
| **Moneda** | [De financial-tracking.md] |
| **ID Proyecto SAP / WBS** | [De financial-tracking.md] |

## Resumen Ejecutivo
[2–3 frases: salud financiera general, desviaciones clave, acciones recomendadas]

**Semaforo**: 🟢 En presupuesto / 🟡 En riesgo / 🔴 Sobrepresupuesto

## Resumen de Rendimiento de Costes

| Metrica | Baseline (BAC) | Actual (EAC) | Desviacion | Estado |
|---------|----------------|--------------|------------|--------|
| Coste total del proyecto | [De financial-tracking.md] | [Actuals + Forecast] | [EAC − BAC] | 🟢/🟡/🔴 |
| Reales a la fecha | — | [Suma Actuals] | — | — |
| Estimado para completar (ETC) | — | [Suma Forecast] | — | — |
| Margen bruto | [Baseline %] | [Actual %] | [Delta] | 🟢/🟡/🔴 |

## Desglose de Costes por Categoria

| Categoria / Recurso | Reales | Forecast (ETC) | Total (EAC) | % del Total | Tendencia |
|---------------------|--------|----------------|-------------|-------------|-----------|
| [Recurso/Categoria 1] | € | € | € | % | ↑/→/↓ |
| [Recurso/Categoria 2] | € | € | € | % | ↑/→/↓ |
| ... | ... | ... | ... | ... | ... |
| **TOTAL** | **€** | **€** | **€** | **100%** | — |

## Principales Drivers de Coste (Top 3)
1. **[Recurso/Proveedor]**: [Importe] ([%] del EAC) — [Razon/explicacion]
2. **[Recurso/Proveedor]**: [Importe] ([%] del EAC) — [Razon/explicacion]
3. **[Recurso/Proveedor]**: [Importe] ([%] del EAC) — [Razon/explicacion]

## Estado de Facturacion y Cobros

| Hito | Disparador | Planificado | Facturado | Cobrado | Estado |
|------|-----------|-------------|-----------|---------|--------|
| M-01 | [Evento] | € | € | € | Hecho/Pendiente/Bloqueado |
| M-02 | [Evento] | € | € | € | Hecho/Pendiente/Bloqueado |

## Analisis de Desviaciones (si report-type = variance o se detectan desviaciones)

| Item | Planificado | Real/Forecast | Desviacion | Causa raiz | Impacto | Accion recomendada |
|------|-------------|---------------|------------|-----------|---------|-------------------|
| [Item] | € | € | € (+/-) | [Causa] | [Plazo/Alcance/Calidad] | [Accion] |

## Riesgos y Oportunidades Financieras

| ID | Descripcion | Probabilidad | Impacto | RSK vinculado | Respuesta recomendada |
|----|-------------|--------------|---------|--------------|----------------------|
| FIN-RSK-01 | [Descripcion basada en analisis de tendencia] | A/M/B | € [importe] | RSK-XX o Nuevo | [Mitigacion segun O&R 6 pasos] |
| FIN-OPP-01 | [Oportunidad de ahorro identificada] | A/M/B | € [importe] | OPP-XX o Nuevo | [Estrategia de explotacion/mejora] |

## Recomendaciones
1. [Accion con responsable y fecha limite]
2. [Accion con responsable y fecha limite]
3. [Accion con responsable y fecha limite]

## Ficheros a Actualizar
- [ ] `tracking/financial-tracking.md` — Actualizar EAC/ETC si ha cambiado
- [ ] `context/RISKS.md` — Anadir nuevos riesgos financieros si se identifican
- [ ] `tracking/scope-changes.md` — Si la desviacion es causada por un cambio de alcance no aprobado
- [ ] `planning/backlog.md` — Si restricciones de coste requieren repriorizacion
```

---

## Checklist de Validacion (El PM Debe Verificar)
- [ ] **Precision de datos**: Los Actuals coinciden con SAP/sistema fuente; sin doble conteo
- [ ] **Referencia baseline**: BAC de `financial-tracking.md` usado correctamente
- [ ] **Cambios de alcance**: Cambios aprobados reflejados en baseline ajustada
- [ ] **Vinculacion a riesgos**: Riesgos financieros vinculados a entradas existentes del Registro O&R
- [ ] **Terminologia**: Terminos PM@Siemens usados (EAC, ETC, BAC, FRG, LoA)
- [ ] **Estado de facturacion**: Hitos de facturacion cruzados con terminos contractuales
- [ ] **Recomendaciones**: Accionables, con responsable y fecha

---

## Notas de Compliance PM@Siemens

- Usar **EAC** (Estimate at Completion), no "presupuesto total" ni "coste proyectado"
- Usar **BAC** (Budget at Completion) para la baseline aprobada
- Referenciar **FRG** (Financial Review Gate) si aplica a la categoria del proyecto
- Umbrales de desviacion que disparan escalacion **LoA** deben marcarse explicitamente
- Vincular riesgos de concentracion de proveedores al proceso O&R de 6 pasos

---

*Version 2.0 | Alineado con PM@Siemens Cap. 3.5*