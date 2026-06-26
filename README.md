# PM@Siemens AI Workspace Template

Plantilla reutilizable para gestionar proyectos con ayuda de IA siguiendo PM@Siemens. El modelo de este workspace es **hibrido**:

- **Fuera de `.github\`** solo quedan las carpetas de material fuente del proyecto: `Correos\`, `Documentacion\`, `Oferta_Pedido\` y `Tecnico\`, ademas de este `README.md`.
- **Dentro de `.github\`** vive el workspace gestionado por IA: contexto, planning, tracking, meetings, financials, decisions, input, output, instrucciones y PM@Siemens Guide.

El workspace debe arrancar sin hechos de proyecto confirmados; sustituye los `_TBD_`, adapta los ficheros base y elimina cualquier resto especifico antes de usarlo en un proyecto real.

## Ficha inicial del proyecto

| Campo | Valor |
|------|------|
| Project Name | _TBD_ |
| Customer / Sponsor | _TBD_ |
| PM / CPM | _TBD_ |
| Project Type / Category | _TBD_ |
| Milestone actual | _TBD_ |
| Baseline documental cargada | _TBD_ |

## Secuencia recomendada de inicializacion

1. Actualiza esta ficha y define el alcance base del proyecto.
2. Carga las fuentes reales en `Correos\`, `Documentacion\`, `Oferta_Pedido\` y `Tecnico\`.
3. Usa `.github\input\` solo para material temporal o no clasificado que deba procesar la IA antes de archivarlo.
4. Sustituye los `_TBD_` en `.github\context\`.
5. Reconstruye `.github\planning\backlog.md` con WBS, hitos, responsables y dependencias reales.
6. Activa `.github\tracking\` y carga exportes reales en `.github\financials\`.
7. Usa `.github\meetings\`, `.github\decisions\` y `.github\output\` durante la ejecucion.

## Reglas de uso

- `.github\PM@Siemens-Guide.md` es la fuente metodologica principal para hitos, Quality Gates, LoA, FRG, EHS, Cybersecurity y Quality Management.
- No guardes backlog, contexto, tracking ni entregables de trabajo fuera de `.github\`.
- Manten separados hechos confirmados, supuestos y gaps de adaptacion.
- Marca la informacion no disponible como `_TBD_`.
- No asumas contexto software si el proyecto no lo requiere.
- No conserves datos de un proyecto anterior cuando reutilices esta plantilla.

## Estructura del workspace

| Path | Uso |
|------|-----|
| `.github\` | Workspace interno gestionado por IA, instrucciones y PM@Siemens Guide |
| `Correos\` | Correos y trazabilidad de mensajes |
| `Documentacion\` | Documentacion de cliente, ingenieria y referencias |
| `Oferta_Pedido\` | Oferta, pedido, contrato y baseline comercial |
| `Tecnico\` | Soporte tecnico, ejecucion y seguimiento de materiales |
| `.github\CADENCE.md` | Ritmo de interaccion del PM con la IA (cadencias y protocolo de sesion) |
| `.github\prompts\` | Biblioteca de prompts reutilizables para cada actividad PM |
| `.github\context\` | Objetivos, stakeholders, riesgos, concepto de solucion y session-log |
| `.github\planning\` | Backlog, WBS y preparacion de hitos (tareas de proyecto planificables) |
| `.github\tracking\` | Seguimiento financiero, NCC, cambios de alcance y acciones criticas del PM |
| `.github\financials\` | Exportes brutos para actuals y forecast |
| `.github\meetings\raw\` | Notas crudas de reunion |
| `.github\meetings\summaries\` | Minutas revisadas y salidas de gate |
| `.github\decisions\` | Registro de decisiones y aprobaciones |
| `.github\input\` | Material temporal de entrada pendiente de clasificar o procesar |
| `.github\output\` | Entregables revisados y compartibles |

Si una carpeta no aplica en un proyecto concreto, deja la `README.md` y anota claramente `Not applicable` o `_TBD_`.
