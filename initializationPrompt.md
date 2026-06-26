# Initialization Prompt

Usa este prompt para inicializar el template como un proyecto real PM@Siemens.

```text
Inicializa este workspace como un proyecto real PM@Siemens.

Datos confirmados:
- Project name: <...>
- Customer / sponsor: <...>
- Scope summary: <...>
- Project type / category: <... o _TBD_>
- Current milestone: <... o _TBD_>
- PM / CPM / Sales / key contacts: <...>
- Constraints / deadlines / special rules: <...>

Fuentes y reglas:
- Usa los hechos confirmados de `Correos\`, `Documentacion\`, `Oferta_Pedido\` y `Tecnico\`.
- Usa `.github\PM@Siemens-Guide.md` como referencia metodologica principal.
- Todo el trabajo vivo debe quedar dentro de `.github\`.
- Si falta informacion, marca `_TBD_`.
- No inventes datos.

Tareas:
1. Actualiza `README.md` con la identidad y alcance del proyecto.
2. Completa `.github\context\OBJECTIVES.md`, `STAKEHOLDERS.md`, `RISKS.md` y `SOLUTION-OVERVIEW.md`.
3. Reconstruye `.github\planning\backlog.md`.
4. Inicializa `.github\tracking\financial-tracking.md`, `ncc-log.md` y `action-tracker.csv`.
5. Revisa `.github\CADENCE.md` y adapta frecuencias al ritmo del proyecto.
6. Deja claros los gaps de adaptacion pendientes.
7. Registra esta sesion de inicializacion en `.github\context\SESSION-LOG.md`.
8. No borres ni muevas documentacion fuente salvo que te lo pida o haya que clasificar algo desde `.github\input\`.
```
