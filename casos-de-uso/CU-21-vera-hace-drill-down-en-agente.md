# CU-21. Vera hace drill-down en un agente para ver qué está haciendo

## Identidad

- **Actor**: Vera (Dev Lead)
- **Pantalla principal**: F2. Drill-down de agente (panel lateral sobre F1)
- **Pantallas secundarias**: F1. Vista del squad activo (donde se invoca y donde vuelve al cerrar).
- **Pre-condición**: Vera está en F1 con un squad activo. Quiere profundizar en un agente puntual — porque ve algo curioso, porque el agente está en estado raro (🟡 esperando), o por curiosidad de aprendizaje.
- **Post-condición**: Vera vio el detalle del agente. Cierra el panel y vuelve a F1 sin perder la vista global del squad.
- **Etapa del journey**: Etapa 4 — Implementación con squad
- **Frecuencia esperada**: 2–4 veces por historia (no en todas las visitas, pero sí cuando algo demanda atención puntual).

## Disparador

Vera está en F1 y ve la card de 🔨 Implementer #1 con un avance lento o con un mensaje en la timeline ("⚠️ Implementer #1 pidió tu input sobre naming"). Click en la card.

## Flujo principal (happy path)

1. Vera click en la card de 🔨 Implementer #1 en F1.
2. ROBOK abre F2 — panel lateral derecho que se desliza sobre F1. La vista de F1 sigue visible detrás (Vera no perdió contexto).
3. F2 muestra:
   - **Header del panel**: avatar 🔨, rol "Implementer #1", tag, tarea actual ("Modificando `pricing/calculator.py`"), status visual 🟢.
   - **Tabs**:
     - **Código en curso**: archivos modificados con diff parcial visible (`pricing/calculator.py` con +12/−3 líneas).
     - **Tool calls recientes**: los últimos 10 (no todos — eso es ruido).
     - **Contexto**: qué docs y módulos está usando como referencia (ADR-2026-014, `pricing/domain/quote.py`).
     - **Decisiones recientes**: "Decidí extraer `compute_fee` como función pura porque el ADR-2026-014 lo pide explícito".
     - **Chat directo con este agente**: entrada de texto + historial.
4. Vera lee la tab "Decisiones recientes" para entender el razonamiento. Coincide con el plan, sin sorpresas.
5. Vera cierra el panel con el botón ✕ o clickeando fuera.
6. Vuelve a F1. La card del Implementer está donde estaba; la timeline en la zona derecha sigue donde estaba.

## Variantes

### V1. Vera usa el chat directo con el agente

Vera abre F2 sobre el Implementer #1 y va a la tab "Chat directo". Escribe "agregá un test específico para el caso del descuento progresivo, no aparece en los casos de prueba del plan". El agente lo ingiere en su próxima decisión (similar a double-texting pero dirigido a uno solo). Esta interacción queda en el historial del chat de F2.

### V2. Drill-down sobre un agente atascado (🟡)

Vera ve el Implementer #2 en estado 🟡 con CTA "Resolver". Click en la card. F2 abre con el agente en pausa. El header muestra "🟡 Esperándote — pidió input sobre naming del endpoint". Vera lee el contexto, decide el naming (`POST /v1/quote` vs `POST /v1/quotes`) y responde desde el chat directo. El agente reanuda con la respuesta.

### V3. Drill-down de drill-down

Desde F2 sobre el Implementer #1, Vera click en una decisión de la tab "Decisiones recientes" → se abre un sub-panel con más profundidad sobre esa decisión específica (qué archivos consultó, qué alternativas descartó). Cerrar el sub-panel vuelve a F2; cerrar F2 vuelve a F1. Tres niveles, sin perder ninguno.

## Caminos alternativos / errores

- **Si el agente terminó su tarea mientras Vera tenía F2 abierto**: el header se actualiza en vivo a "✅ Tarea cerrada — Implementer #1 ahora idle" sin cerrar el panel. Vera ve el cambio sin sorpresa.
- **Si Vera abre F2 sobre un agente con error 🔴**: el header muestra el error y la tab "Tool calls recientes" se abre por defecto para que vea qué falló.
- **Si Vera cliquea otra card en F1 mientras F2 está abierto sobre un agente diferente**: F2 cambia de contenido al nuevo agente sin cerrarse (transición fluida).

## Decisiones del usuario en este flujo

- Qué tab del panel revisar primero según motivo (decisiones, código, contexto, chat).
- Si solo leer o intervenir con chat directo.
- Si profundizar más (drill-down de drill-down) o cerrar.

## Componentes UI involucrados

- 4. Card de agente del squad (lo que se cliquea para invocar F2)
- 5. Status Bar del squad (sigue visible detrás del panel)
- 6. Timeline editorial (sigue visible detrás)
- 7. Avatar y mensaje del muro de discusión (en la tab de chat directo, mismo componente para conversación con un agente)
- 14. Pill de estado (en el header del panel y en cada decisión)

## Notas para el prototipo HTML

- F2 es un panel lateral simulado con CSS — no requiere lógica real. Slide-in desde la derecha sobre F1.
- Estados sugeridos como archivos separados:
  - `f2-drill-down-implementer.html` (panel abierto sobre Implementer #1, tab "Código en curso")
  - `f2-drill-down-decisiones.html` (tab "Decisiones recientes" activa)
  - `f2-drill-down-chat.html` (tab "Chat directo" con conversación)
  - `f2-drill-down-atascado.html` (V2: agente 🟡 con CTA visible)

## Referencias

- Journey: ROBOK_v5.md §1.8 — Etapa 4, "Drill-down sin perder contexto" y "Click en un agente abre panel lateral".
- Inventario: 03-inventario-pantallas.md §F2.
- Componentes UI: 04-componentes-ui.md §4 (lo que se cliquea), §7 (chat directo).
- Principios involucrados: P5 (demostrar entendimiento — la tab "Decisiones recientes" muestra el razonamiento, no solo el output), P6 (presencia adaptativa — Vera elige cuánto profundizar).
