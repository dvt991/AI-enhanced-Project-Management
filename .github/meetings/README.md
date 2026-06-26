# Meetings

Flujo recomendado:

1. Guarda notas crudas en `raw\`.
2. Genera un resumen revisado en `summaries\`.
3. Extrae acciones segun el criterio backlog / action-tracker (ver abajo).
4. Registra decisiones formales en `.github\decisions\`.
5. Actualiza `.github\context\` y `.github\tracking\` si la reunion cambia alcance, riesgos, coste o planning.
6. Si hay cambio de alcance, registralo tambien en `.github\tracking\scope-changes.md`.

## Criterio: backlog vs action-tracker

| Destino | Tipo de item | Ejemplo |
|---------|--------------|---------|
| `.github\planning\backlog.md` | Tareas de proyecto planificables, asociadas a un milestone u objetivo | Completar ingenieria, preparar FAT, enviar entregable |
| `.github\tracking\action-tracker.csv` | Acciones criticas puntuales que el PM humano debe cerrar personalmente | Reclamar PO al cliente, escalar proveedor, confirmar fecha de envio |
