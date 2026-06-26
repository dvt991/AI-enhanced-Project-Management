# .github - workspace PM@Siemens gestionado por IA

Esta carpeta contiene el espacio de trabajo interno que la IA utiliza para estructurar y mantener el proyecto.

## Contenido esperado

- `copilot-instructions.md` — reglas de comportamiento para la IA
- `CADENCE.md` — ritmo de interaccion del PM con la IA (cadencias diaria, semanal, mensual, pre-gate)
- `PM@Siemens-Guide.md` / `.pdf` — guia metodologica PM@Siemens
- `prompts/` — biblioteca de prompts reutilizables para cada actividad PM
- `context/` — objetivos, stakeholders, riesgos, solucion y session-log
- `planning/` — backlog y preparacion de hitos
- `tracking/` — seguimiento financiero, NCC, scope changes y acciones criticas
- `financials/` — exportes brutos financieros
- `meetings/` — notas crudas (`raw/`) y minutas revisadas (`summaries/`)
- `decisions/` — registro de decisiones y aprobaciones
- `input/` — material temporal pendiente de clasificar
- `output/` — entregables finalizados

## Regla estructural

Fuera de `.github\` solo deben quedar las carpetas de material fuente del proyecto:

- `Correos\`
- `Documentacion\`
- `Oferta_Pedido\`
- `Tecnico\`
- `README.md`

Todo el backlog, contexto, seguimiento, notas de reunion, decisiones y entregables gestionados por IA debe quedarse dentro de `.github\`.
