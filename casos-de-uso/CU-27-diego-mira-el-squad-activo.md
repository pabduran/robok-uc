# CU-27. Diego mira el squad activo (sin permisos de aprobar) y aprende cómo Rob descompone

## Identidad

- **Actor**: Diego (Developer)
- **Pantalla principal**: F1. Vista del squad activo (con permisos de Developer — lectura)
- **Pantallas secundarias**: F2. Drill-down de agente (también accesible para Developers en lectura).
- **Pre-condición**: El squad de PROM-1234 está activo. Diego es Developer del producto Promise Engine. Tiene permisos de lectura y comentar, pero NO puede pausar, detener, ni resolver checkpoints.
- **Post-condición**: Diego miró el squad, eventualmente hizo drill-down en algún agente para aprender cómo Rob descompone, eventualmente comentó si vio algo. Cierra F1 sin haber actuado sobre el flujo principal del squad.
- **Etapa del journey**: Etapa 4 — Implementación con squad
- **Frecuencia esperada**: 1–3 veces por historia (los Developers que están atentos miran; otros no entran nunca al squad).

## Disparador

Diego entra a F1 por curiosidad (estuvo en el muro en CU-16 y quiere ver cómo va el squad), o llegó por un link en Slack/Teams ("@Diego — el squad arrancó tu historia, mirá si querés").

## Flujo principal (happy path)

1. Diego entra a F1 desde C2 (Mis historias en curso filtrada por las que tocó algo) o desde el link de Slack.
2. Diego ve F1 con la misma estructura que Vera (Status Bar + Cards + Timeline) pero con:
   - **Controles globales en gris**: [Pausar], [Detener], [Pedir actualización] visibles para entender qué se puede hacer pero deshabilitados con tooltip "Solo los Dev Leads pueden controlar el squad. Podés mirar y comentar."
   - **Caja de double-texting visible pero limitada**: Diego puede mandar mensajes (similar a un comentario), pero quedan etiquetados como "comentario de Developer" en lugar de "instrucción de Dev Lead". Vera los ve y decide si los promueve a directiva.
   - **Sin acceso a "Resolver checkpoint"**: si hay una card en 🟡, el CTA "Resolver" tampoco es clickeable para Diego — tooltip "Esperando aprobación de Vera".
3. Diego ve que el 🎨 Architect está descomponiendo T-4 (recién agregada). Le interesa cómo Rob piensa la descomposición.
4. Click en la card del Architect → F2 abre como drill-down (Diego también puede hacer drill-down en lectura).
5. Diego va a la tab "Decisiones recientes". Lee: "Decidí extraer `compute_fee` como función pura porque el ADR-2026-014 lo pide explícito. Alternativas descartadas: a) método de instancia (acoplado a Quote), b) decorador (no idiomático en este codebase)."
6. Diego aprende: ese patrón de "alternativas descartadas + razón" es algo que él podría aplicar en sus propios diseños. Cierra F2.
7. Diego ve en la timeline editorial un evento "🔨 Implementer #1 abrió PR draft #1234". Click en el evento → se abre el PR draft en GitHub en otra pestaña.
8. Diego mira el código, le parece bien. Cierra F1 sin haber comentado ni intervenido.

## Variantes

### V1. Diego comenta porque ve algo

Diego mira el squad y ve que el Implementer #2 está usando un patrón que el equipo evita (ej. raw SQL en lugar de SQLAlchemy). Click en la card → F2 → tab "Chat directo". Escribe "@Implementer #2 — en este equipo usamos SQLAlchemy ORM, no raw SQL. ¿Podés cambiarlo?". El mensaje queda etiquetado como "comentario de Diego (Developer)". El agente lo ingiere y propone diff (CU-23 mecánica) que Vera tiene que aprobar (los Developers pueden sugerir, no aprobar cambios de código).

### V2. Diego intenta detener el squad

Diego ve algo grave (ej. el squad está por modificar un módulo crítico que no debería tocar). Click en "Detener" — el botón está en gris con tooltip. Diego no puede detenerlo. Tiene que escribir a Vera por Slack o etiquetar a Vera en un comentario en la timeline ("@Vera — mirá esto, creo que el squad está fuera de scope"). Vera decide.

### V3. Diego mira squad terminado

Diego entra a F1 después de que el squad cerró (CU-26). F1 muestra "✅ Squad terminó · ver reporte de cierre" y Diego puede ir a G1 en lectura para ver el resultado.

## Caminos alternativos / errores

- **Si Diego intenta enviar un mensaje a un agente con instrucción explícita** (no comentario): ROBOK acepta el mensaje pero lo etiqueta como sugerencia. El agente lo trata como contexto, no como orden. Para que sea orden, Vera tiene que reenviarlo o promoverlo.
- **Si Diego no tiene acceso al producto** (ej. fue removido del equipo): F1 muestra "No tenés acceso a este producto" en lugar de la vista del squad.

## Decisiones del usuario en este flujo

- Solo mirar o comentar.
- Si comentar: a quién (Vera por Slack, agente por chat directo, evento en la timeline).
- Si profundizar para aprender (drill-down) o solo escanear.

## Componentes UI involucrados

- 1. Header con badge ambient (mismo que Vera, sin restricción)
- 5. Status Bar del squad (controles en gris para Diego)
- 4. Card de agente del squad (clickeable para drill-down lectura)
- 6. Timeline editorial (lectura completa)
- 7. Avatar y mensaje del muro (en F2 chat directo, los mensajes de Diego se etiquetan como Developer)

## Notas para el prototipo HTML

- Para Diego, los botones de control restringidos se modelan grises con `aria-disabled="true"` y tooltip al hover. Eso muestra el modelo de permisos sin construir backend de permisos real (consistente con lo que se hizo en CU-16 para E3).
- Estados sugeridos como archivos separados:
  - `f1-vista-developer.html` (vista de F1 desde la perspectiva de Diego — controles grises, caja de comentario en lugar de double-texting)
  - `f2-drill-down-developer.html` (vista de F2 desde Diego — chat directo etiquetado como Developer)
  - `f1-developer-intenta-detener.html` (V2: hover sobre [Detener] mostrando tooltip)

## Referencias

- Journey: ROBOK_v5.md §1.8 — Etapa 4, F1 abierta a lectura para Developers (implícito en doc 03 §F1 "Quién entra: Dev Lead, Developer (lectura)").
- Personas: 01-personas-y-arquetipos.md §Persona 2 (Diego — su frase: "Quiero ver cómo Rob piensa, no solo qué hace").
- Inventario: 03-inventario-pantallas.md §F1 y §F2 (acceso de Developer en lectura).
- Componentes UI: 04-componentes-ui.md §4, §5, §6, §7.
- Principios involucrados: P4 (gobernanza configurable — los Developers ven y comentan, los Dev Leads aprueban), P5 (mostrar trabajo — Diego aprende viendo cómo Rob descompone).
