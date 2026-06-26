---
description: Evalua solicitudes de cambio de alcance en dimensiones de coste, plazo, riesgo y calidad antes de su aprobacion
agent: ask
---

# Analisis de Impacto de Cambio de Alcance — PM@Siemens

**Proposito**: Analizar una solicitud de cambio de alcance (originada por cliente, interna o regulatoria) y producir una evaluacion de impacto estructurada cubriendo las dimensiones de coste, plazo, riesgo y calidad. Soporta la decision del PM de aprobar, rechazar o escalar segun requisitos LoA. La salida se integra con `tracking/scope-changes.md`.

**Alcance**: Solicitudes de cambio, ordenes de variacion, peticiones de cliente, requisitos regulatorios y ajustes internos de alcance.

---

## Invocacion

```
Follow instructions in #prompt:scope-impact.prompt.md with these arguments:
  @file:<ruta-a-solicitud-de-cambio-o-descripcion>
  --change-source [customer|internal|regulatory|supplier]
  --urgency [standard|urgent|critical]
  --project-folder @workspace
```

**Parametros**:
- `@file`: Ruta al documento de solicitud de cambio, correo o descripcion (o inline `--description "..."`)
- `--change-source`: Origen del cambio — determina el flujo de aprobacion
- `--urgency`: `standard` (proceso normal), `urgent` (revision acelerada necesaria), `critical` (decision inmediata requerida)
- `--project-folder`: Referencia al workspace para lecturas de contexto

---

## Pasos de Procesamiento

### Paso 1: Lecturas de Contexto
Antes del analisis, **siempre leer y cruzar**:
1. `context/OBJECTIVES.md` — ¿El cambio se alinea o contradice los objetivos del proyecto?
2. `context/SOLUTION-OVERVIEW.md` — Viabilidad tecnica e impacto en integracion
3. `planning/backlog.md` — Impacto en plazo, tareas afectadas, analisis de ruta critica
4. `tracking/financial-tracking.md` — Salud presupuestaria actual, contingencia disponible
5. `tracking/scope-changes.md` — Impacto acumulado de cambios anteriores
6. `context/RISKS.md` — ¿Nuevos riesgos? ¿Mitigaciones existentes afectadas?
7. `context/STAKEHOLDERS.md` — ¿Quien aprueba? ¿Quien se ve impactado?
8. `Oferta_Pedido/` — Baseline contractual, clausula de cambios, terminos de facturacion

### Paso 2: Dimensiones de Impacto

Evaluar el impacto en las cinco dimensiones:

| Dimension | Preguntas de evaluacion |
|-----------|------------------------|
| **Coste** | ¿Esfuerzo adicional? ¿Material? ¿Coste proveedor? ¿Uso de contingencia? |
| **Plazo** | ¿Ruta critica afectada? ¿Fechas de milestone desplazadas? ¿Dependencias cascada? |
| **Calidad** | ¿Criterios de aceptacion cambian? ¿Pruebas adicionales? ¿Impacto en compliance? |
| **Riesgo** | ¿Nuevos riesgos introducidos? ¿Mitigaciones existentes invalidadas? |
| **Alcance** | ¿Que se anade? ¿Que se elimina? ¿Crecimiento neto o reduccion de alcance? |

### Paso 3: Verificacion LoA y Aprobacion
- Determinar si el cambio excede la autoridad del PM (umbral LoA)
- Identificar el nivel correcto de aprobacion
- Marcar si se necesita aprobacion del cliente o modificacion contractual

---

## Estructura de Salida

```markdown
# Analisis de Impacto de Cambio de Alcance — [Titulo del Cambio] — [Fecha]

## Resumen de la Solicitud de Cambio
| Propiedad | Valor |
|-----------|-------|
| **Fecha de solicitud** | YYYY-MM-DD |
| **ID del cambio** | SCR-XXX (secuencial desde scope-changes.md) |
| **Origen** | Cliente / Interno / Regulatorio / Proveedor |
| **Urgencia** | Estandar / Urgente / Critica |
| **Solicitante** | [Nombre, Rol, STK-XX] |
| **Descripcion** | [Declaracion clara y concisa de lo que se cambia] |

## Descripcion del Cambio
[Descripcion detallada de lo que se solicita, incluyendo aspectos tecnicos y funcionales]

**Dentro del alcance de este cambio**:
- [Item 1]
- [Item 2]

**Explicitamente fuera de alcance**:
- [Item que podria confundirse como incluido]

## Evaluacion de Impacto

### Impacto en Coste

| Item | Baseline actual | Tras el cambio | Delta | Financiado por |
|------|----------------|----------------|-------|----------------|
| Esfuerzo interno | [horas/€] | [horas/€] | [+/- horas/€] | [Contingencia / Cliente / Margen] |
| Externo/proveedor | [€] | [€] | [+/- €] | [Fuente] |
| Material/licencias | [€] | [€] | [+/- €] | [Fuente] |
| **Impacto total en coste** | — | — | **[+/- €]** | — |

**Cambio en EAC**: [EAC actual] → [Nuevo EAC] ([+/- %])
**Contingencia restante tras el cambio**: [€ / % del BAC]
**Impacto en margen**: [Margen actual %] → [Nuevo margen %]

### Impacto en Plazo

| Milestone | Fecha actual | Nueva fecha | Desplazamiento | ¿Ruta critica? |
|-----------|-------------|-------------|----------------|----------------|
| [PM100/200/600/700] | YYYY-MM-DD | YYYY-MM-DD | [+X dias/semanas] | Si/No |

**Ruta critica afectada**: Si / No
**Dependencias en cascada**: [Listar tareas downstream afectadas de backlog.md]

### Impacto en Calidad y Compliance

| Area | Impacto | Descripcion |
|------|---------|-------------|
| Criterios de aceptacion | Cambiados / Sin cambios | [Detalle] |
| Alcance de pruebas | Ampliado / Sin cambios / Reducido | [Detalle] |
| EHS | Impacto / Sin impacto | [Detalle] |
| Cybersecurity | Impacto / Sin impacto | [Detalle] |
| Compliance/regulatorio | Impacto / Sin impacto | [Detalle] |

### Impacto en Riesgos

| ID | Nuevo/Existente | Descripcion | Probabilidad | Impacto | Respuesta |
|----|-----------------|-------------|--------------|---------|-----------|
| RSK-NEW-XX | Nuevo | [Riesgo introducido por el cambio] | A/M/B | A/M/B | [Estrategia] |
| RSK-XX | Existente (modificado) | [Como cambia el riesgo existente] | [Nueva P] | [Nuevo I] | [Respuesta actualizada] |

## Estado Acumulado de Cambios de Alcance

| Metrica | Antes de este cambio | Despues de este cambio |
|---------|---------------------|------------------------|
| Total cambios aprobados | [N] | [N+1] |
| Impacto acumulado en coste | [€] | [€] |
| Impacto acumulado en plazo | [dias/semanas] | [dias/semanas] |
| Crecimiento neto de alcance | [%] | [%] |

## Analisis de Opciones

| Opcion | Descripcion | Coste | Plazo | Riesgo | Recomendacion |
|--------|-------------|-------|-------|--------|---------------|
| A: Aceptar completo | [Implementacion completa] | [€] | [+X dias] | [Impacto] | [Pro/contra] |
| B: Aceptacion parcial | [Alternativa de alcance reducido] | [€] | [+X dias] | [Impacto] | [Pro/contra] |
| C: Rechazar | [Mantener alcance actual] | [€0] | [Sin cambio] | [Contra-riesgo] | [Pro/contra] |

**Opcion recomendada**: [A/B/C] — [Justificacion]

## Requisitos de Aprobacion

| Verificacion | Estado | Notas |
|--------------|--------|-------|
| Dentro del LoA del PM | Si / No | [Si no: ¿escalacion a quien?] |
| Aprobacion del cliente necesaria | Si / No | [Referencia de clausula contractual] |
| Modificacion de contrato necesaria | Si / No | [Implicaciones comerciales] |
| Reasignacion de presupuesto necesaria | Si / No | [Contingencia / financiacion adicional] |
| Re-evaluacion FRG necesaria | Si / No | [Si impacto financiero excede umbral] |

**Autoridad de aprobacion**: [Nombre/Rol de STAKEHOLDERS.md]
**Fecha limite de aprobacion**: [Fecha — determinada por urgencia y dependencias]

## Recomendaciones
1. [Recomendacion principal con justificacion]
2. [Condicion o accion de seguimiento si se aprueba]
3. [Mitigacion de riesgo para acompanar la aprobacion]

## Ficheros a Actualizar (Tras Aprobacion)
- [ ] `tracking/scope-changes.md` — Registrar este cambio
- [ ] `tracking/financial-tracking.md` — Actualizar EAC/ETC
- [ ] `planning/backlog.md` — Anadir/modificar tareas
- [ ] `context/RISKS.md` — Anadir nuevos riesgos si se identifican
- [ ] `context/OBJECTIVES.md` — Si objetivos o criterios de exito cambian
- [ ] `Oferta_Pedido/` — Si se requiere modificacion contractual
```

---

## Checklist de Validacion (El PM Debe Verificar)
- [ ] **Descripcion completa**: El cambio es inequivoco; dentro y fuera de alcance estan claros
- [ ] **Todas las dimensiones evaluadas**: Coste, plazo, calidad, riesgo, alcance — ninguna omitida
- [ ] **Baseline referenciada**: Impacto medido contra la baseline aprobada actual
- [ ] **Vista acumulada**: Impacto total de cambios de alcance (no solo este cambio aislado)
- [ ] **Opciones presentadas**: Al menos 2 alternativas para el decisor
- [ ] **LoA respetado**: Autoridad de aprobacion correctamente identificada
- [ ] **Conciencia contractual**: Implicaciones cliente/comerciales abordadas
- [ ] **Reversibilidad anotada**: ¿Puede deshacerse el cambio si falla?
- [ ] **Terminologia**: Terminos PM@Siemens (LoA, FRG, Quality Gate, NCC)

---

## Notas de Compliance PM@Siemens

- Todos los cambios de alcance deben registrarse formalmente en `tracking/scope-changes.md` segun PM@Siemens Cap. 3.4
- Cambios que exceden umbrales LoA requieren escalacion al siguiente nivel de autoridad
- Cambios originados por cliente pueden requerir orden de variacion formal y modificacion de contrato
- Crecimiento acumulado de alcance mas alla del umbral dispara revision obligatoria
- El impacto financiero debe reflejarse en actualizacion del EAC y reportarse en el proximo FRG (si aplica)
- Cambios de alcance no aprobados constituyen una No-conformidad (NCC)

---

*Version 1.0 | Alineado con PM@Siemens Cap. 3.4 (Scope & Change Management)*
