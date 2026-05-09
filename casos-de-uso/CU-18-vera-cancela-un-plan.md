# CU-18. Vera cancela un plan; queda guardado para retomar después

## Identidad

- **Actor**: Vera (Dev Lead)
- **Pantalla principal**: E3. Muro de discusión del plan
- **Pantallas secundarias**: D1. Sprint actual (vuelta) o C2. Mis historias en curso.
- **Pre-condición**: Vera está en E3 con el plan ya publicado. Después de leer y/o discutir, decide no avanzar — el costo se desbordó vs lo esperado, o se da cuenta de que el ticket se debe partir, o necesita más info antes de comprometerse.
- **Post-condición**: El plan queda en estado "Cancelado guardado". El ticket vuelve a Jira sin acción de implementación. El plan queda persistido — si Vera lo retoma en otro sprint, Rob hace refresh sobre el plan existente, no análisis desde cero.
- **Etapa del journey**: Etapa 3 — Análisis profundo y plan
- **Frecuencia esperada**: 1 de cada 5–8 historias delegadas (cancelación es minoría pero no excepcional).

## Disparador

Vera está en E3 leyendo el plan o discutiéndolo y decide cancelar — típicamente porque el costo refinado quedó muy distinto a la pre-estimación, o el alcance es más grande de lo esperado, o la conversación reveló que falta info.

## Flujo principal (happy path)

1. Vera está en E3, en medio o al final de la discusión del plan. La matriz costo × tiempo refinada quedó en USD 180 estándar (vs USD 50 pre-estimado en D1). Rob justificó el delta ("el módulo histórico de pricing no tiene tests, hay que crearlos primero").
2. Vera decide que ese costo no es razonable para esta historia este sprint — va a partirla en dos.
3. Vera click en "Cancelar plan" (botón top-right de E3).
4. ROBOK abre un confirm: "Cancelar plan de PROM-1234 — el plan queda guardado, podés retomarlo en otro sprint. Opcional: dejá una nota para tu próximo yo."
5. Vera escribe en el input de nota: "Costo se desbordó por falta de tests en módulo histórico — partir en dos: primero crear tests del módulo histórico, después el refactor".
6. Vera click en "Cancelar".
7. ROBOK marca el plan como "🛑 Cancelado guardado · 2026-05-09" con la nota visible. El ticket vuelve a Jira en el estado original (sin acción de implementación). En D1 la card de PROM-1234 vuelve a su estado pre-delegación, pero con un indicador "📎 Plan cancelado guardado · ver historial".
8. Vera vuelve a D1 o C2 según donde quiera ir.

## Variantes

### V1. Vera retoma el plan en otro sprint

Dos sprints después, Vera vuelve a D1 y ve la card de PROM-1234. Click en el indicador "📎 Plan cancelado guardado". ROBOK ofrece dos caminos:
- **Refresh sobre el plan existente** — Rob revisa los cambios del producto desde el plan original y muestra el delta. NO se rehace el análisis desde cero.
- **Empezar de cero** — descarta el plan guardado.

Vera elige refresh. Rob compara el snapshot del producto al momento del plan original con el snapshot actual y muestra: "Cambios desde el plan original: módulo de auth refactoreado, dependencias actualizadas. Tu plan sigue mayormente válido, pero ADR-2026-014 cambió porque el módulo de auth ya no requiere el wrapper que asumía". Vera revisa el delta y decide seguir o cancelar de nuevo.

### V2. Cancelación sin nota

Vera salta el input de nota y solo aprieta "Cancelar". El plan queda guardado sin nota. Vale para casos rápidos.

### V3. Cancelación durante el quórum

Vera ya apretó "Dale" en E3 y aprobó el plan en E4 (Estándar), pero el quórum está pendiente (esperando a Pablo). Vera decide cancelar antes de que Pablo apruebe. Click en "Cancelar plan" desde C2 (Mis historias en curso) o E3. ROBOK retira la solicitud de aprobación a Pablo (notificación "Aprobación cancelada · PROM-1234") y marca el plan como "🛑 Cancelado guardado". El squad nunca arranca.

## Caminos alternativos / errores

- **Si la cancelación es por costo**: Vera puede en lugar de cancelar volver a E1 y ajustar el scope (CU-13) para reducir el alcance. ROBOK no fuerza cancelar — la cancelación es decisión.
- **Si Pablo ya aprobó pero el squad no arrancó todavía**: Vera puede cancelar igual. ROBOK notifica a Pablo "Plan cancelado a pesar de tu aprobación · ver muro para razones".

## Decisiones del usuario en este flujo

- Cancelar o ajustar (volver a E1 / E3 con ediciones).
- Con o sin nota libre.
- Cuándo retomar: nunca, próximo sprint, o más adelante.

## Componentes UI involucrados

- 1. Header con badge ambient
- 7. Avatar y mensaje del muro de discusión (la conversación queda visible cuando se retoma)
- 14. Pill de estado (cambio a "🛑 Cancelado guardado")

## Notas para el prototipo HTML

- Estados sugeridos como archivos separados:
  - `e3-cancelar-confirm.html` (modal de confirmación con input de nota)
  - `e3-plan-cancelado.html` (vista del muro con plan en estado cancelado, botones grises, nota visible)
  - `d1-card-con-plan-guardado.html` (card del ticket con indicador "📎 Plan cancelado guardado")
  - `e3-plan-retomado-con-delta.html` (V1: vista del muro mostrando el delta vs el plan original)

## Referencias

- Journey: ROBOK_v5.md §1.7 — Etapa 3, "Plan cancelado se guarda".
- Inventario: 03-inventario-pantallas.md §E3 (estados "plan cancelado y guardado").
- Componentes UI: 04-componentes-ui.md §7, §14.
- Principios involucrados: P3 (el humano decide — cancelar también es decisión válida), P7 (trabajo informado por contexto fresco — al retomar, Rob compara snapshots y muestra el delta).
