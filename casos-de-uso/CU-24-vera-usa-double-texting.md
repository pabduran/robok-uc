# CU-24. Vera usa double-texting para dar instrucciones al squad sin reiniciarlo

## Identidad

- **Actor**: Vera (Dev Lead)
- **Pantalla principal**: F1. Vista del squad activo
- **Pantallas secundarias**: F2. Drill-down de agente (alternativa cuando el mensaje es para un agente puntual).
- **Pre-condición**: El squad está activo trabajando. Vera detecta algo que quiere comunicar — un cambio de criterio, una aclaración, una instrucción que no fue parte del plan original. NO es un checkpoint; el squad no pausa para esto.
- **Post-condición**: El mensaje queda inyectado al destinatario (squad entero o agente específico). El agente espera a terminar su tool call actual, ingiere el mensaje, ajusta y sigue. La timeline editorial registra el evento.
- **Etapa del journey**: Etapa 4 — Implementación con squad
- **Frecuencia esperada**: 0–3 veces por historia. Es la diferencia entre supervisar un equipo y debuggear un proceso — algunos squads no necesitan ningún double-text, otros varios.

## Disparador

Vera está mirando F1 y observa algo que quiere ajustar al vuelo. Por ejemplo: un cambio de naming que el equipo decidió en una reunión paralela, una preferencia de estilo, un constraint que no estaba en el plan.

## Flujo principal (happy path) — mensaje al squad entero

1. Vera está en F1. Ve la caja de double-texting en el lateral o abajo: "Mensaje al squad o a un agente específico".
2. Vera escribe en la caja: "🎙️ Para todos: el equipo decidió en standup que el campo `currency` del endpoint use enum `Currency` (no string libre). Aplicalo cuando llegues a esa parte."
3. Vera mantiene "destinatario: todo el squad" (default).
4. Vera click en "Enviar".
5. ROBOK inyecta el mensaje al contexto compartido del squad. Cada agente activo lo recibe en su próxima decisión — espera a terminar su tool call actual, lo ingiere, ajusta. NO se reinicia ni se interrumpe a la mitad de una operación.
6. La timeline editorial registra el evento: "💬 Vera mandó mensaje al squad: 'Para todos: usar enum `Currency`...' · ver mensaje completo".
7. Cuando el agente correspondiente (Implementer #2 que está en T-3) llega al punto donde aplica, ajusta el código y registra en su drill-down: "Decidí usar enum `Currency` por mensaje de Vera (timestamp X)".

## Variantes

### V1. Mensaje a un agente específico

Vera ve que el 🎨 Architect está borradoreando la descomposición de T-4 (recién agregada). Quiere darle una directiva específica sin notificar a todo el squad.

1. Vera click en la caja, cambia destinatario a "🎨 Architect".
2. Escribe "@Architect — para T-4, NO uses pattern matching de Python 3.10, el equipo todavía soporta 3.9 en producción".
3. Envía. Solo el Architect recibe el mensaje. La timeline lo registra como "💬 Vera → 🎨 Architect: ..."
4. El Architect lo ingiere y ajusta su descomposición.

### V2. Mensaje desde el drill-down de un agente

Vera está en F2 sobre Implementer #1 (CU-21). En la tab "Chat directo" escribe "agregá un comentario en `compute_fee` explicando por qué el orden de operaciones importa". Es la misma mecánica que double-texting V1, pero invocada desde dentro del panel del agente.

### V3. Mensaje que llega tarde (agente ya pasó por el punto)

Vera escribe "usar enum `Currency`" pero el Implementer ya escribió esa parte hace 5 minutos como string. ROBOK detecta el desfase y le marca al agente "este mensaje llega después de que tocaste el código relacionado — ¿revisar y ajustar?". El agente revisa, propone diff (CU-23 mecánica) o ajusta directamente según severidad. Vera recibe el feedback en la timeline.

## Caminos alternativos / errores

- **Si el mensaje contradice el plan aprobado**: ROBOK lo detecta y avisa antes de enviar: "Este mensaje cambia algo aprobado en el plan (ADR-2026-014 dice X). ¿Querés enviar igual? Va a quedar como ajuste post-aprobación." Vera decide.
- **Si el mensaje es para un agente que terminó**: ROBOK avisa "El Architect ya cerró sus tareas. ¿Mandar a otro rol o al squad entero?". Vera elige.
- **Si Vera quiere cancelar un mensaje recién enviado**: dentro de unos segundos ROBOK permite "Deshacer" (similar a Gmail undo send). Después no — el mensaje ya fue ingerido por al menos un agente.

## Decisiones del usuario en este flujo

- A quién dirigir el mensaje (squad entero o agente puntual).
- Cuándo enviar (en cualquier momento; ROBOK gestiona la sincronización).
- Si el mensaje contradice algo aprobado: enviar igual o ajustar el plan formalmente.

## Componentes UI involucrados

- 1. Header con badge ambient
- 5. Status Bar del squad
- 4. Card de agente (cambia visualmente cuando ingiere el mensaje — ej. micro-indicador "💬 mensaje recibido")
- 6. Timeline editorial (registra el evento)
- 7. Avatar y mensaje del muro de discusión (en la variante V2 desde el drill-down)

## Notas para el prototipo HTML

- La caja de double-texting puede ser un input fijo abajo de F1 (estilo composer de Slack) o un panel lateral abrible.
- Estados sugeridos como archivos separados:
  - `f1-double-texting-vacio.html` (caja con placeholder)
  - `f1-double-texting-con-mensaje.html` (Vera escribiendo, destinatario "todo el squad")
  - `f1-double-texting-a-un-agente.html` (V1: destinatario "🎨 Architect")
  - `f1-double-texting-enviado.html` (mensaje en la timeline, micro-indicador en la card del agente)

## Referencias

- Journey: ROBOK_v5.md §1.8 — Etapa 4, "Double-texting habilitado" y "Patrones clave que diferencian a ROBOK".
- Inventario: 03-inventario-pantallas.md §F1 (caja de double-texting).
- Componentes UI: 04-componentes-ui.md §6 (timeline registra el evento), §7 (mensaje en la variante V2).
- Principios involucrados: P3 (el humano lidera — el double-texting es la forma de dirigir al equipo en vivo), P6 (presencia adaptativa — Vera puede intervenir sin tener que reiniciar nada).
