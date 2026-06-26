---
description: Inicializa el contexto del proyecto al inicio de una sesión de trabajo de forma ultra-eficiente (Map & Navigate).
agent: ask

---

# Generador de Inicio de Sesión Diaria — AI-Enhanced PM

**Propósito**: Cargar el estado más reciente del proyecto en la memoria de la IA sin saturar la ventana de contexto. Evita que la IA lea documentos técnicos, correos o históricos completos al arrancar, centrándose solo en "dónde estamos" y "qué toca hacer hoy".

**Alcance**: Primer prompt a ejecutar cada vez que se abre un nuevo chat o se retoma el trabajo tras un tiempo de inactividad.

## Invocación

**Usuario:** Copia y pega el texto del bloque "PROMPT" de abajo al iniciar una nueva sesión de chat con tu IA.

```
# PROMPT:
"Aplica tus instrucciones base (`copilot-instructions.md`). Acabo de iniciar una nueva sesión de trabajo.

Para recuperar el contexto del proyecto de forma eficiente (Map & Navigate), lee de forma silenciosa **únicamente** estos dos archivos:
1. `.github/context/INDEX.md`
2. `.github/context/SESSION-LOG.md`

Una vez asimilados, NO me hagas un resumen largo de ellos ni de la metodología. Dame únicamente:
1.  **Prioridades Inmediatas:** Una lista breve (3 puntos máximo) con los temas pendientes urgentes o la tarea en la que nos quedamos en la última sesión, basándote exclusivamente en la última entrada del `SESSION-LOG.md`.
2.  **Next Step:** Pregúntame: '¿Por dónde quieres empezar la sesión de hoy?'"
```

## Pasos de Procesamiento Interno (Para la IA)

1. **Validar Instrucciones:** Confirmar internamente que aplican las reglas de "Map & Navigate" y evitar la lectura masiva.

2. **Leer Índice:** Obtener el mapa del repositorio desde `INDEX.md` para saber dónde ir a buscar información *después*, si el usuario lo pide.

3. **Leer Estado:** Extraer la última entrada registrada en `SESSION-LOG.md` para conocer el estado inmediato del trabajo.

4. **Generar Salida:** Emitir una respuesta extremadamente concisa (1-3 bullets) centrada en la acción inmediata, finalizando con una pregunta de enganche.

## Notas de Uso para el PM (Humano)

- **Frecuencia:** Usa este prompt *cada vez* que inicies una nueva ventana de chat.

- **Ahorro:** Este enfoque consume un ~90% menos de tokens iniciales que cargar el proyecto entero.

- **Siguientes Pasos:** Una vez que la IA responda a este prompt, puedes empezar a pedirle análisis específicos (ej. *"Ahora ve a `.github/planning/backlog.md` y actualiza la tarea BLG-04"*).

*Versión 2.0 | Adaptado para eficiencia de tokens (Map & Navigate)*
