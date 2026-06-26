---
description: Genera comunicaciones adaptadas a stakeholders con tono, detalle y terminologia PM@Siemens apropiados
agent: ask
---

# Generador de Comunicaciones a Stakeholders — PM@Siemens

**Proposito**: Redactar comunicaciones profesionales y adaptadas a la audiencia para stakeholders o grupos especificos. Usa los perfiles de stakeholders de `context/STAKEHOLDERS.md` para calibrar tono, nivel de detalle y llamada a la accion. Se alinea con la Guia PM@Siemens Cap. 3.9 (Communication & Collaboration).

**Alcance**: Actualizaciones de estado, escalaciones, solicitudes de aprobacion, notificaciones de gate, comparticion de informacion y reporting especifico por stakeholder.

---

## Invocacion

```
Follow instructions in #prompt:stakeholder-update.prompt.md with these arguments:
  --stakeholder [STK-XX o descripcion del rol]
  --comm-type [status|escalation|approval|gate-notification|information]
  --key-points "[Puntos principales del mensaje]"
  --project-folder @workspace
```

**Parametros**:
- `--stakeholder`: ID del stakeholder objetivo de STAKEHOLDERS.md (ej., `STK-02`) o rol (ej., "Line Manager", "PM del cliente")
- `--comm-type`: Tipo de comunicacion que determina estructura y urgencia
  - `status`: Actualizacion de progreso regular
  - `escalation`: Problema que requiere decision o intervencion de un nivel superior
  - `approval`: Solicitud de firma o autorizacion (relevante para LoA)
  - `gate-notification`: Notificacion de readiness de Milestone/Quality Gate
  - `information`: Comunicacion informativa (FYI), sin accion requerida
- `--key-points`: Puntos de contenido principales a incluir (descripcion breve)
- `--project-folder`: Referencia al workspace para lecturas de contexto

---

## Pasos de Procesamiento

### Paso 1: Lecturas de Contexto
Antes de redactar, **siempre leer y cruzar**:
1. `context/STAKEHOLDERS.md` — Perfil del destinatario: interes, influencia, preferencias de comunicacion
2. `context/OBJECTIVES.md` — Enmarcar la actualizacion contra los objetivos del proyecto
3. `context/RISKS.md` — Incluir contexto de riesgo relevante si comm-type es escalation
4. `tracking/action-tracker.csv` — Acciones pendientes de/para este stakeholder
5. `planning/backlog.md` — Datos de progreso relevantes
6. `tracking/financial-tracking.md` — Contexto presupuestario si es relevante para el mensaje
7. `meetings/summaries/` — Decisiones recientes que involucran a este stakeholder

### Paso 2: Calibracion de Audiencia

| Tipo de Stakeholder | Tono | Nivel de detalle | Foco |
|---------------------|------|-----------------|------|
| Ejecutivo / Sponsor | Formal, conciso | Solo KPIs de alto nivel | Decisiones necesarias, estado de salud, riesgos |
| Line Manager | Profesional, directo | Moderado | Necesidades de recurso, presupuesto, escalaciones |
| PM del cliente | Profesional-externo | Filtrado (sin detalles internos) | Entregables, plazos, items abiertos |
| Miembro del equipo | Colaborativo, detallado | Detalle tecnico completo | Tareas, bloqueantes, expectativas |
| Comunidad de Practica PM | Informativo, estructurado | Foco en metodologia | Lecciones, practicas, templates |
| Proveedor/Subcontratista | Contractual, preciso | Limitado al alcance | Estado PO, entregables, penalizaciones |

### Paso 3: Reglas por Tipo de Comunicacion

| Tipo | Elementos requeridos | Senal de urgencia | Patron de CTA |
|------|---------------------|-------------------|---------------|
| Status | Resumen de salud, progreso, proximos pasos | Baja | "Para su informacion" / "Revisar antes de [fecha]" |
| Escalation | Enunciado del problema, impacto, opciones, recomendacion | Alta | "Decision necesaria antes de [fecha]" / "Intervencion requerida" |
| Approval | Que se aprueba, criterios cumplidos, consecuencia del retraso | Media–Alta | "Por favor aprobar antes de [fecha]" / "Requisito LoA" |
| Gate Notification | Readiness del gate, estado de outputs obligatorios, convocatoria | Media | "Revision de gate programada para [fecha]" |
| Information | Contexto, referencia, sin accion necesaria | Baja | "No se requiere accion" / "Para su conocimiento" |

---

## Estructura de Salida

```markdown
# Borrador de Comunicacion — [Stakeholder: STK-XX / Nombre] — [Fecha]

## Metadatos de la Comunicacion
| Propiedad | Valor |
|-----------|-------|
| **Para** | [Nombre, Rol] |
| **CC** | [Nombres si aplica] |
| **Tipo** | Status / Escalacion / Aprobacion / Notificacion de Gate / Informacion |
| **Urgencia** | Alta / Media / Baja |
| **Respuesta necesaria antes de** | [Fecha o "No se necesita respuesta"] |
| **Linea de asunto** | [Asunto profesional y claro] |

---

## Cuerpo del Mensaje

[Saludo apropiado a la relacion]

[Parrafo de apertura: Contexto y proposito de esta comunicacion — 2-3 frases max]

### [Seccion 1: Actualizacion clave / Enunciado del problema / Solicitud]
[Contenido calibrado a la audiencia — puntos para ejecutivos, detalle para equipo]

### [Seccion 2: Impacto / Datos / Informacion de soporte]
[Metricas relevantes, estado de riesgo, impacto temporal — solo lo que este stakeholder necesita]

### [Seccion 3: Llamada a la Accion]
[Solicitud clara, especifica y acotada en tiempo — o declaracion explicita de "no se requiere accion"]

[Cierre profesional]

---

## Adjuntos / Referencias (si aplica)
- [Documento o artefacto a incluir]
- [Enlace a fichero relevante del workspace]

## Notas Internas (NO incluidas en la comunicacion)
- **Sensibilidad**: [Que filtrar para esta audiencia]
- **Seguimiento necesario**: [Que debe monitorizar el PM tras enviar]
- **Relevancia LoA**: [¿Esta comunicacion dispara requisitos LoA?]
- **Acciones vinculadas**: [Entradas de action-tracker.csv relacionadas con esta comunicacion]
```

---

## Checklist de Validacion (El PM Debe Verificar Antes de Enviar)
- [ ] **Apropiado para la audiencia**: Tono y detalle coinciden con el perfil del stakeholder
- [ ] **Filtrado**: Sin informacion interna/sensible expuesta a stakeholders externos
- [ ] **CTA claro**: El destinatario sabe exactamente que se espera y para cuando
- [ ] **Factual**: Basado en hechos confirmados del proyecto, no en supuestos
- [ ] **Terminos PM@Siemens**: Terminologia correcta (Quality Gate, O&R, NCC, LoA, FRG)
- [ ] **Trazabilidad**: La comunicacion puede trazarse a una decision, reunion o disparador
- [ ] **Verificacion LoA**: Si es aprobacion/escalacion, el nivel de autoridad LoA es correcto
- [ ] **Tono profesional**: Adecuado para entorno corporativo Siemens

---

## Notas de Compliance PM@Siemens

- **LoA (Limits of Authority)**: Escalaciones y aprobaciones deben respetar los umbrales LoA
- **Notificaciones de Quality Gate**: Deben incluir estado de outputs obligatorios segun Guia PM@Siemens Cap. 5
- **Comunicaciones con cliente**: Nunca exponer registro interno de riesgos, log de NCC o detalles financieros salvo requisito contractual
- **Ruta de escalacion**: Seguir la jerarquia de stakeholders definida en STAKEHOLDERS.md
- Usar "No-conformidad" (no "incidencia/bug") y "Quality Gate" (no "revision/aprobacion") en todas las comunicaciones

---

*Version 2.0 | Alineado con PM@Siemens Cap. 3.9 (Communication & Collaboration)*