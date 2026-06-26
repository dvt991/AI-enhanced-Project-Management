# Tracking

Usa esta carpeta, dentro de `.github\`, para seguimiento economico, NCC, cambios de alcance, desviaciones y acciones vivas.

| Fichero | Uso |
|---------|-----|
| `financial-tracking.md` | Baseline comercial, costes, billing, EAC/ETC y observaciones |
| `ncc-log.md` | Registro de Non-conformance Costs y watchlist |
| `action-tracker.csv` | Acciones criticas puntuales que el PM humano debe cerrar (delimitador: `;`) |
| `scope-changes.md` | Registro explicito de cambios de alcance, claims y desviaciones contractuales |

Los exportes brutos deben guardarse en `.github\financials\`.

## Criterio: backlog vs action-tracker

- **Backlog** (`.github\planning\backlog.md`): tareas de proyecto planificables, asociadas a milestones u objetivos. Las gestiona la IA junto con el PM.
- **Action-tracker** (`.github\tracking\action-tracker.csv`): acciones criticas, puntuales y con fecha limite que el PM humano debe cerrar personalmente (reclamaciones, escalaciones, confirmaciones urgentes, aprobaciones pendientes).

## Formato del CSV

- Delimitador: punto y coma (`;`), compatible con Excel en locales europeos.
- Encoding esperado: UTF-8.
- Primera fila = cabecera de columnas.
