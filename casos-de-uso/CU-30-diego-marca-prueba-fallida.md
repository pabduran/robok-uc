# CU-30. Diego marca una prueba manual como fallida y se abre mini-muro

## Identidad

- **Actor**: Diego (Developer)
- **Pantalla principal**: G3. Mini-muro de discusión (fallo en prueba manual)
- **Pantallas secundarias**: Jira (donde Diego marcó la prueba como fallida con el reporte estructurado).
- **Pre-condición**: Diego ejecutó la prueba manual P-3 ("Cancelar promesa pendiente") siguiendo el checklist que Rob posteó en Jira. La prueba falló — el comportamiento observado no coincide con el esperado. Diego completó el template de Jira con pasos para reproducir, screenshot, comportamiento esperado vs observado.
- **Post-condición**: ROBOK detecta el cambio de estado del item en Jira (pasó de ⏸️ a ❌). Abre automáticamente G3 — Mini-muro de discusión con el reporte estructurado y el diagnóstico inicial de Rob. Vera recibe notificación interrupting.
- **Etapa del journey**: Etapa 5 — Pre-merge Gate
- **Frecuencia esperada**: 0–1 vez por historia (no en todas las historias falla una prueba, pero cuando pasa el flujo debe ser claro).

## Disparador

Diego está ejecutando P-3 en la app interna de Promise Engine. Al cancelar una promesa pendiente, observa que el estado queda en `cancelling` indefinidamente en lugar de pasar a `cancelled`. Diego documenta y marca la prueba como fallida en Jira.

## Flujo principal (happy path)

1. Diego va a Jira al ticket PROM-1234. Encuentra el comentario de Rob con el checklist de pruebas manuales. Ubica el item P-3 ("Cancelar promesa pendiente").
2. Diego marca P-3 como ❌ Falló y completa el template predefinido:
   - **Pasos para reproducir**:
     1. Crear promesa nueva con código demo.
     2. Marcar como pendiente.
     3. Cancelar.
   - **Comportamiento esperado**: La promesa pasa a estado `cancelled` en <2s y aparece el banner "Promesa cancelada".
   - **Comportamiento observado**: La promesa queda en estado `cancelling` indefinidamente, sin transición a `cancelled`. Banner no aparece.
   - **Screenshot adjunto**: imagen del estado bloqueado.
   - **Notas libres**: "Pasó dos veces seguidas. Refresh no resuelve. La promesa queda zombie en la DB."
3. Diego guarda el comentario en Jira.
4. ROBOK detecta el cambio de estado del item (P-3: ⏸️ → ❌) vía polling o webhook. En segundos:
   - El item en G2 ("Plan de pruebas manuales") cambia a ❌ con CTA "Ver mini-muro →".
   - ROBOK abre automáticamente G3 — Mini-muro de discusión.
   - El 🛡️ Security Reviewer y el 🔨 Implementer involucrado en la tarea relacionada (T-2 del refactor) son convocados al mini-muro.
   - Vera recibe notificación interrupting + Slack/Teams: "❌ Prueba manual falló · PROM-1234 · P-3 · [Ver mini-muro]".
5. Diego, si está conectado, también recibe link al mini-muro como participante (es el reportero del fallo). Click en el link.
6. Diego entra a G3. Ve:
   - **Header**: ❌ "Fallo en prueba manual · P-3 · Cancelar promesa pendiente".
   - **Reporte estructurado del humano** (lo que Diego escribió en Jira, espejado).
   - **Diagnóstico inicial de Rob**:
     - **Causa probable**: "El refactor de T-2 modificó el flujo de cancelación. La transición `cancelling → cancelled` se hace en un job async del worker. Probablemente el worker no fue redeployado o la cola está atascada."
     - **Scope del fix**: "Pequeño — verificar el job del worker, asegurar que el evento se publique correctamente."
     - **Alternativas**: a) sync inline (más simple, menos performance), b) reintentar el job con backoff, c) verificar que el worker está procesando la cola.
   - **Clasificación propuesta** por Rob: 🔧 **Fix directo** (cambio acotado, no afecta el plan).
   - **Hilo de discusión**: vacío al abrir; Diego, Rob, Vera, Implementer pueden comentar.
7. Diego no tiene más info que aportar — el reporte ya estaba en Jira. Espera a que Vera revise (CU-31).

## Variantes

### V1. Diego ya intuye la causa al reportar

Diego conoce el código y al reportar agrega en notas libres "creo que el worker no está procesando la cola, vi un log similar la semana pasada". Cuando G3 abre, Diego comenta en el mini-muro confirmando esa intuición y linkeando al log histórico. Rob lo ingiere y refina su diagnóstico.

### V2. Diego marca múltiples pruebas como fallidas

Diego ejecuta P-3, P-4 y ambas fallan. ROBOK abre **un mini-muro por cada fallo** (G3 puede coexistir con varias instancias para historias separadas — cada fallo es su propio hilo). Vera tiene que resolver ambos.

### V3. La prueba "falló" pero en realidad es un problema del entorno de Diego

Diego ejecuta P-3, falla, reporta. Cuando se abre G3 y discute con Rob, queda claro que el problema era que el worker estaba detenido en su entorno local. Ver CU-31 V3 para resolución como "no es bug".

## Caminos alternativos / errores

- **Si Diego no completa todos los campos del template** (ej. olvidó screenshot): Jira deja guardar igual; G3 abre con los campos disponibles. Rob puede pedir más info en el mini-muro ("@Diego — ¿podés agregar un screenshot?").
- **Si Rob no logra inferir causa probable** (caso raro): el diagnóstico inicial dice "No tengo hipótesis clara — necesito que el squad investigue. ¿Querés que reabra al squad para reproducir?". Vera decide.
- **Si Diego marca como fallida pero no abrió Jira recientemente** (lag de polling): ROBOK puede tardar segundos a minutos en detectar. Diego puede entrar directo a G2 y forzar refresh.

## Decisiones del usuario en este flujo

- **Diego**: qué nivel de detalle dar en el reporte. Cuanto más rico, mejor diagnóstico inicial de Rob.
- **Diego**: si quedarse en el mini-muro a participar o cerrar y dejar que Vera resuelva.

## Componentes UI involucrados

- 13. Plan de pruebas manuales (en Jira y en su espejo en G2)
- 7. Avatar y mensaje del muro de discusión (componente del mini-muro G3, mismo que el muro grande E3 pero más compacto)
- 10. Notificación interrupting (a Vera)
- 14. Pill de estado (la prueba pasa a ❌ en G2 y abre con estado "abierto" en G3)

## Notas para el prototipo HTML

- G3 reusa componentes del muro grande (E3) en versión compacta. Una sola columna de discusión, sin la zona izquierda del plan.
- Estados sugeridos como archivos separados:
  - `g3-mini-muro-recien-abierto.html` (reporte de Diego + diagnóstico inicial de Rob, sin discusión todavía)
  - `g3-mini-muro-con-conversacion.html` (Diego, Vera, Rob, Implementer han comentado)
  - `g3-mini-muro-multiples-fallos.html` (V2: navegación entre múltiples G3 abiertos)
- El template de Jira se simula en el prototipo como bloque de texto estructurado dentro del primer mensaje de Diego.

## Referencias

- Journey: ROBOK_v5.md §1.9 — Etapa 5, "Mini-muro de discusión cuando algo falla" y "El loop de fix usa el mismo patrón que external signal listening".
- Personas: 01-personas-y-arquetipos.md §Persona 2 (Diego — los Developers ejecutan pruebas manuales).
- Inventario: 03-inventario-pantallas.md §G3.
- Componentes UI: 04-componentes-ui.md §7 (avatar y mensaje del muro), §13 (plan de pruebas manuales).
- Principios involucrados: P5 (demostrar entendimiento — Rob propone diagnóstico inicial con causa probable, no solo "falló"), P4 (los Developers reportan, los Dev Leads deciden el camino).
