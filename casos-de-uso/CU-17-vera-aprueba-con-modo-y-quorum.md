# CU-17. Vera aprueba el plan: elige modo y se cumple el quórum

## Identidad

- **Actor**: Vera (Dev Lead)
- **Pantalla principal**: E4. Modo + quórum (modal)
- **Pantallas secundarias**: E3 (entrada), F1. Vista del squad activo (salida).
- **Pre-condición**: Vera apretó "Dale" en E3 (CU-15). El plan está en estado "listo para aprobar".
- **Post-condición**: Plan aprobado en un modo concreto. Si hay quórum extra, queda en espera; cuando se cumple, el squad arranca y ROBOK lleva a F1.
- **Etapa del journey**: Etapa 3 — Análisis profundo y plan
- **Frecuencia esperada**: una vez por historia delegada que se aprueba (las que se cancelan son CU-18).

## Disparador

Vera apretó "Dale" en el muro de discusión E3.

## Flujo principal (happy path) — sin quórum extra

1. ROBOK abre E4 — modal "Aprobar plan · PROM-1234".
2. Vera ve el selector de modo con los 4 pills:
   - ⚡ **Express** USD 95 · 4h
   - 🚀 **Estándar** USD 55 · 1d (sugerido por Rob, destacado con badge "Sugerido por Rob")
   - 🌱 **Económico** USD 28 · 3d
   - ⏳ **Sprint-pace** USD 20 · 2 sem
3. Vera elige el modo (Express / Estándar / Económico / Sprint-pace). Click en "Estándar". El pill se selecciona con fondo del color del modo.
4. La sección "Quórum requerido para esta historia" muestra: "Sin requerimiento adicional — basta tu aprobación como Dev Lead".
5. Vera click en "Aprobar plan".
6. ROBOK confirma la aprobación, marca el plan como "✅ Aprobado · Estándar", cierra E4 y lanza el squad. Transición automática a F1 — Vista del squad activo (cubierto en CU-20).

## Variantes

### V1. Quórum extra requerido (módulo de auth)

La historia toca el módulo de auth, que tiene una regla de quórum configurada por Vera en C4: "historias que toquen módulo de auth requieren aprobación adicional de un Security Officer".

1. Vera ve el modal con el selector de modo + una sección extra "Quórum requerido": "💡 Esta historia toca el módulo de auth — requiere también aprobación de Pablo (Security Officer)".
2. Lista de aprobadores:
   - ✅ Vera (vos · Dev Lead)
   - ⏳ Pablo (Security Officer · invitado al muro)
3. Vera elige modo Estándar y aprieta "Aprobar plan".
4. El botón cambia de texto a "Esperar quórum". El plan queda en estado "🟡 En espera de quórum · esperando a Pablo".
5. ROBOK manda notificación a Pablo (Slack/Teams + ROBOK) — cubierto en CU-19.
6. Cuando Pablo aprueba (CU-19), el quórum se completa, el squad arranca y Vera recibe notificación "Quórum cumplido · squad arrancado". Si Vera está conectada, ROBOK la lleva a F1; si no, la próxima vez que entre a la historia ya verá F1.

### V2. Quórum por costo (>USD 500)

La historia se aprueba en modo Express por USD 600 (>USD 500). La regla de quórum del producto requiere 2 Dev Leads. Mismo flujo que V1, pero el quórum es a otro Dev Lead del producto.

### V3. Vera cambia el modo después de seleccionar y vuelve a abrir E4

Vera aprobó en Económico pero antes de que el squad arranque (mientras está en espera de quórum), se da cuenta de que necesita más rápido. Vera puede cambiar el modo desde la pantalla de "Mis historias en curso" mientras siga "🟡 En espera de quórum". Cambiar a Express dispara una nueva pasada por E4 (mismo modal) con el costo actualizado, y los aprobadores ya cumplidos se mantienen (no requiere re-aprobar a Pablo si solo cambia el modo).

## Caminos alternativos / errores

- **Si Vera intenta aprobar sin elegir modo**: el botón "Aprobar plan" está deshabilitado. Tooltip: "Elegí un modo de ejecución."
- **Si el quórum requerido es a alguien que no está disponible** (Pablo de vacaciones): la regla del producto puede definir un suplente, o Vera tiene que escalar el caso a Marisol para resolver. Para V1, el plan queda en espera hasta que el aprobador requerido entre.
- **Si Vera quiere aprobar saltándose el quórum**: no se puede en V1. Las reglas de quórum son no-negociables (P3 — el humano lidera, los gates humanos son no-negociables; los gates configurados por el equipo también).

## Decisiones del usuario en este flujo

- Qué modo elegir (Express / Estándar / Económico / Sprint-pace).
- Si la sugerencia de Rob coincide con su propia evaluación.
- Si el quórum está muy lento, decidir esperar o escalar.

## Componentes UI involucrados

- 15. Selector de modo + quórum (modal — la pantalla central)
- 3. Pill de modo costo × tiempo (los 4 pills del selector)
- 14. Pill de estado (en cada aprobador del quórum)

## Notas para el prototipo HTML

- E4 es modal limpio, no requiere mucha sofisticación visual.
- Estados sugeridos como archivos separados:
  - `e4-modal-sin-quorum.html` (caso simple, basta Vera)
  - `e4-modal-con-quorum.html` (V1: requiere Pablo además)
  - `e4-modal-en-espera.html` (después de aprobar, esperando a Pablo)
- El modal puede usar `<dialog>` HTML5 o un `<div>` con backdrop CSS.

## Referencias

- Journey: ROBOK_v5.md §1.7 — Etapa 3, "Quórum configurable" y la rama K del flujo detallado.
- Inventario: 03-inventario-pantallas.md §E4.
- Componentes UI: 04-componentes-ui.md §15 (selector de modo + quórum).
- Principios involucrados: P3 (gates humanos no-negociables — el quórum es uno), P4 (gobernanza configurable — Vera definió la regla del producto, ROBOK la respeta), P8 (decisión bidimensional — Vera elige el modo costo × tiempo).
