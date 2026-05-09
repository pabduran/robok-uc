# CU-25. Vera ve el costo desbordarse y el squad pausa automáticamente

## Identidad

- **Actor**: Vera (Dev Lead)
- **Pantalla principal**: F1. Vista del squad activo (con dialog modal de costo)
- **Pantallas secundarias**: ninguna en el flujo principal.
- **Pre-condición**: El squad está activo trabajando en PROM-1234 con presupuesto Estándar de USD 55. El costo real está creciendo más rápido de lo estimado (ej. el Implementer #2 está iterando mucho en una zona compleja). El costo cruza el umbral configurado (default 100% del presupuesto).
- **Post-condición**: ROBOK pausa el squad automáticamente y abre un dialog modal preguntando a Vera qué hacer. Vera decide: continuar (con o sin nuevo cap), o detener.
- **Etapa del journey**: Etapa 4 — Implementación con squad
- **Frecuencia esperada**: 1 de cada 5–8 historias delegadas (no es la mayoría, pero sí pasa lo suficiente como para que el flujo deba ser claro).

## Disparador

ROBOK monitorea el costo real vs el presupuesto aprobado. Cuando supera el umbral (configurable, default 100%), pausa el squad automáticamente y lanza el dialog. En paralelo manda notificación interrupting.

## Flujo principal (happy path)

1. El status bar de F1 viene mostrando 💵 USD 48 / USD 55 (en amarillo, >80% del budget) hace un rato. Vera lo ve pero no actúa.
2. El costo cruza USD 55 (100% del budget). ROBOK pausa el squad automáticamente.
3. La barra de costo en el status bar pasa a rojo: 💵 USD 56 / USD 55 (>100%).
4. ROBOK lanza un **dialog modal**: "💸 Costo desbordado · PROM-1234 — el squad pausó porque cruzaste el presupuesto aprobado (USD 55 Estándar). Costo actual: USD 56. ¿Continuar igual o pausar?"
   - Opciones:
     - **[Continuar con nuevo cap]** — input para definir nuevo cap (ej. USD 80).
     - **[Continuar sin cap]** — peligroso, requiere confirmación adicional.
     - **[Pausar el squad]** — el squad queda pausado pero no termina; Vera decide más tarde.
     - **[Detener el squad]** — el squad termina, workspace preservado.
5. En paralelo Vera recibe notificación interrupting + Slack/Teams: "💸 Costo desbordado · PROM-1234 · [Resolver]".
6. Vera lee el dialog. Quiere ver por qué se desbordó antes de decidir. Cierra el dialog (con [Cerrar] sin acción) y revisa el drill-down del Implementer #2 (CU-21) para entender el patrón de iteración.
7. Vera ve que el agente está atascado en una zona compleja del refactor donde el ADR original no era preciso. Decide ampliar el cap a USD 80 para cerrar la historia y retomar el muro después para registrar la lección.
8. Vera vuelve a abrir el dialog desde el status bar (el dialog aparece nuevamente al click sobre la barra roja de costo).
9. Click en "Continuar con nuevo cap". Input: "USD 80". Click en confirmar.
10. ROBOK reanuda el squad con el nuevo cap. La timeline editorial registra: "💸 Cap ampliado por Vera de USD 55 a USD 80 · razón inferida: complejidad no anticipada en T-2".

## Variantes

### V1. Vera detiene el squad

Vera lee el dialog y decide que el cost overrun no se justifica — la historia debería replanificarse. Click en "Detener el squad". Confirmación obligatoria. Squad termina, workspace preservado. Vera vuelve a Etapa 3 a replanificar (o cancela el plan, CU-18).

### V2. Vera ya tenía un cap configurado distinto del 100%

Algunos productos configuran (en C4) un umbral diferente — ej. "pausar al 120% del budget", para dar margen sin alarmar a 100%. El comportamiento es idéntico, solo cambia el momento del trigger.

### V3. Vera quiere continuar sin cap (raro)

Vera click en "Continuar sin cap". ROBOK pide confirmación con copy explícito: "Sin cap el squad puede consumir cualquier monto. Esto no es recomendado para historias con presupuesto definido. ¿Seguro?". Vera tiene que escribir "SIN CAP" en una caja para confirmar. Solo entonces ROBOK reanuda. Queda registrado en el ADR del cierre como decisión de Vera.

### V4. Costo se desborda durante la noche

El squad está corriendo overnight (Sprint-pace). El costo cruza el umbral a las 3 AM. ROBOK pausa el squad y manda notificación a Slack/Teams. Vera la ve a la mañana. El squad NO avanza solo durante la noche — coherente con "el squad espera al humano en checkpoints", aplicado al cap de costo.

## Caminos alternativos / errores

- **Si Vera no responde al dialog en N horas** (ej. 24h): el squad sigue pausado. La notificación interrupting se vuelve más insistente (recordatorios). El badge ambient del producto pasa a 🟡 "Squad esperando hace mucho". El squad NO reanuda solo.
- **Si Vera amplía el cap pero el costo lo vuelve a cruzar**: misma mecánica, otro dialog. Cada ampliación queda en el historial.

## Decisiones del usuario en este flujo

- Continuar (con cap nuevo o sin cap) o detener.
- Si entender por qué se desbordó antes de decidir (drill-down primero) o decidir directo.
- Si la historia se debe replanificar (vuelta a Etapa 3) o solo necesita más presupuesto.

## Componentes UI involucrados

- 5. Status Bar del squad (la barra de costo cambia a roja cuando supera 100%)
- 10. Notificación interrupting
- 9. Pantalla de checkpoint (variante "dialog de costo desbordado", reusa la mecánica modal de F3)
- 1. Header con badge ambient (pasa a 🟡 mientras el squad está pausado)
- 14. Pill de estado

## Notas para el prototipo HTML

- El dialog de costo es similar a un checkpoint pero con su propio copy y opciones específicas.
- Estados sugeridos como archivos separados:
  - `f1-status-bar-amarillo.html` (costo entre 80–100%, todavía sin pausa)
  - `f1-status-bar-rojo-pausado.html` (costo >100%, squad pausado, dialog cerrado pero barra roja)
  - `f1-dialog-costo-desbordado.html` (modal abierto con las opciones)
  - `f1-dialog-sin-cap-confirmacion.html` (V3: confirmación con caja "SIN CAP")
- Reutilizar el componente 9 (modal bloqueante) con copy de costo en lugar de copy de contrato.

## Referencias

- Journey: ROBOK_v5.md §1.8 — Etapa 4, "Costo se desboca" en Fricciones a manejar.
- Inventario: 03-inventario-pantallas.md §F1 (estado "squad bloqueado" cubre también este caso).
- Componentes UI: 04-componentes-ui.md §5 (Status Bar — comportamiento ante costo >100% incluye dialog modal).
- Principios involucrados: P3 (el squad espera al humano · cap es un gate), P8 (decisión bidimensional — Vera decide cuándo el trade-off costo × tiempo cambia mid-historia).
