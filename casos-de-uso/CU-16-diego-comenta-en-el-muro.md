# CU-16. Diego comenta en el muro y Rob ajusta el plan en respuesta

## Identidad

- **Actor**: Diego (Developer)
- **Pantalla principal**: E3. Muro de discusión del plan
- **Pantallas secundarias**: ninguna en el flujo principal (Diego entra desde una notificación de Slack/Teams al muro de la historia).
- **Pre-condición**: Diego es Developer del producto Promise Engine (sin permisos de aprobación). Rob publicó un plan en E3 para PROM-1234. La notificación llegó al canal del producto en Slack/Teams con el link al muro.
- **Post-condición**: Diego dejó un comentario sustantivo en el muro. Rob actualizó el plan en respuesta. Vera ve el cambio cuando vuelve al muro.
- **Etapa del journey**: Etapa 3 — Análisis profundo y plan
- **Frecuencia esperada**: 1 de cada 2–3 historias delegadas (los Developers no comentan en todas, pero sí cuando ven algo relevante de su área).

## Disparador

Diego recibe en el canal de Slack del producto una notificación: "📢 Plan publicado en ROBOK · PROM-1234 · Refactor pricing calculator · [Ver muro]". Click en el link.

## Flujo principal (happy path)

1. Diego entra al muro E3 desde el link de Slack. SSO ya estaba activo.
2. Diego ve el muro con permisos de Developer: puede leer todo y comentar, pero los botones "Cancelar plan" y "Dale" están en gris (visibles para entender el contexto, no clickeables).
3. Diego revisa la zona del plan. Lee la sección "ADRs propuestos". Ve que Rob propone usar la lib `httpx` para una nueva integración.
4. Diego sabe que en Promise Engine el equipo eligió `requests` por consistencia con el resto del codebase. Es una convención no escrita que Rob no detectó.
5. Diego click en "Citar parte del plan" sobre el ADR que menciona `httpx`. La cita aparece en su mensaje pendiente.
6. Diego escribe en el muro: "@Rob — Ojo, en este producto usamos `requests`, no `httpx`, por consistencia con el resto del codebase. Vera lo confirmó hace un par de sprints, no quedó como ADR formal pero es una convención del equipo".
7. Diego envía. Su mensaje aparece en el muro con avatar (👤 Diego — Developer) y timestamp.
8. Rob detecta la mención y responde en el muro: "Tenés razón, no detecté esa convención. Ajusto el ADR para usar `requests`. ¿Querés que lo registre como ADR formal de convenciones para el próximo onboarding?". Rob propone la mejora; Vera (o Diego como challenger) puede confirmar o corregir.
9. Diego responde "dale, registralo como ADR de convención del producto". Rob actualiza el plan: el ADR del refactor cambia a `requests`, y aparece un nuevo ADR-2026-016 "Convención de cliente HTTP en Promise Engine". Banner "Plan actualizado · ajustado por @Diego" con [Ver diff del plan].
10. Diego ve el cambio aplicado, satisfecho. Cierra el muro y vuelve a su trabajo. Vera, cuando vuelva al muro, va a ver tanto el comentario como el cambio aplicado en el feed.

## Variantes

### V1. Diego comenta sin que Rob acepte de una

Diego propone algo que Rob considera incorrecto. Rob responde justificando ("Vi `httpx` en `requirements-dev.txt` en otro repo, asumí compatibilidad. Pero me decís que no se usa en producción de Promise Engine — confirmo. Cambio a `requests`."). El intercambio queda como evidencia del razonamiento.

### V2. Diego desafía sin proponer cambio

Diego comenta "@Rob ¿probaste qué pasa si la lista de promos es vacía en T-2?". Es una pregunta, no una propuesta de cambio. Rob responde con el caso de borde detectado en su análisis ("sí, lo cubrí en el caso de prueba P-3"). Diego lee, conforme. El plan no cambia, pero el muro registra que el caso fue revisado.

### V3. Vera y Diego van y vienen sin Rob

Diego y Vera (que está mirando) discuten algo en el muro sin que Rob intervenga. Cuando llegan a un acuerdo, Vera puede mencionar "@Rob actualizá el plan para reflejar X". Rob ejecuta el cambio.

## Caminos alternativos / errores

- **Si Diego intenta apretar "Dale" o "Cancelar plan"**: los botones están deshabilitados con un tooltip "Solo los Dev Leads pueden aprobar o cancelar planes. Podés comentar y proponer cambios."
- **Si Rob no responde en N minutos** a una mención de Diego: el comentario queda visible. Vera, cuando vuelva, va a ver la mención sin respuesta y puede responder ella misma o pedirle a Rob que aborde el punto.

## Decisiones del usuario en este flujo

- Si comentar o solo leer.
- Si proponer cambio concreto, hacer pregunta, o solo dejar contexto.
- Si pedir que el cambio quede como ADR formal o solo aplicado al plan actual.

## Componentes UI involucrados

- 1. Header con badge ambient
- 7. Avatar y mensaje del muro de discusión

## Notas para el prototipo HTML

- Para el prototipo, modelar permisos restringidos como botones grises ("Dale" y "Cancelar plan" sin click). Eso muestra el modelo de permisos sin construirlo de verdad (Diego en doc 01).
- Estados sugeridos:
  - `e3-muro-diego-comentando.html` (mensaje de Diego pendiente de envío)
  - `e3-muro-rob-respondio.html` (Rob ya respondió y actualizó el plan)
  - `e3-muro-permisos-diego.html` (vista del muro con botones de aprobación grises)

## Referencias

- Journey: ROBOK_v5.md §1.7 — Etapa 3, "varios humanos debaten con varios agentes".
- Personas: 01-personas-y-arquetipos.md §Persona 2 (Diego — su frase: "Quiero ver cómo Rob piensa, no solo qué hace").
- Inventario: 03-inventario-pantallas.md §E3 (rol Developer con lectura + comentar).
- Componentes UI: 04-componentes-ui.md §7.
- Principios involucrados: P4 (gobernanza configurable — los Developers comentan, los Dev Leads aprueban), P3 (inferencia con confirmación — Rob propone el ajuste, Diego o Vera confirma).
