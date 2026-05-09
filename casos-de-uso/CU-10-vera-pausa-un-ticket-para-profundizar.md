# CU-10. Vera marca un ticket en pausa para profundizar después

## Identidad

- **Actor**: Vera (Dev Lead)
- **Pantalla principal**: D1. Sprint actual — backlog enriquecido
- **Pantallas secundarias**: ninguna en el flujo principal (Vera vuelve al sprint o cierra D1).
- **Pre-condición**: Vera está en D1 escaneando el sprint y ve un ticket que no quiere delegar todavía pero tampoco descartar.
- **Post-condición**: El ticket queda en estado "⏸️ Pausado para profundizar" con una nota opcional. NO arranca análisis ni squad. Vera puede retomarlo más tarde y delegarlo o descartarlo.
- **Etapa del journey**: Etapa 2 — Selección y priorización
- **Frecuencia esperada**: 1–3 veces por sprint (los tickets que necesitan más data antes de decidir).

## Disparador

Vera ve un ticket donde la spec es ambigua, las dependencias no están claras, o necesita confirmar algo con otro humano antes de comprometerse a delegarlo.

## Flujo principal (happy path)

1. Vera está en D1 sobre la card de PROM-5678 (un ticket bloqueado por dependencia y con spec ⚠️ ambigua).
2. Vera lee la card: chip ⚠️ "spec ambigua", banner "depende de PROM-1233 sin resolver", riesgo histórico 📈.
3. Vera no quiere delegarlo así (Rob va a frenar en el muro pidiendo aclaraciones), pero tampoco quiere sacarlo del radar.
4. Click en "Pausar, profundizar".
5. ROBOK abre un mini-input opcional: "Nota libre — ¿qué falta antes de delegar?". Vera escribe: "confirmar con Pablo si esto va al sprint o se posterga".
6. Vera click en "Pausar".
7. ROBOK marca la card en estado "⏸️ Pausado para profundizar" con la nota visible al hover. La acción NO escribe nada en Jira (queda solo en ROBOK como flag para Vera).
8. Vera sigue escaneando el resto del sprint o cierra D1.

## Variantes

### V1. Pausa sin nota

Vera salta el input y solo aprieta "Pausar". El ticket queda en "⏸️ Pausado para profundizar" sin nota adicional.

### V2. Vera retoma el ticket más tarde

En una sesión posterior (mismo sprint o el siguiente), Vera abre D1 y ve la card en estado "⏸️". Click en la card (o en "Ver detalle") la lleva a D2 (CU-11). Desde D2 puede delegarlo (CU-09) o cambiar a "No es para Rob".

### V3. Pausa anticipando que el ticket va a salir del sprint

Vera pausa porque sospecha que el ticket no va a entrar en este sprint. La nota lo registra ("posiblemente para el próximo sprint, esperar planning"). El estado "⏸️" la ayuda a no perderlo cuando el sprint cambie.

## Caminos alternativos / errores

- **Si Vera quiere despausar sin delegar ni descartar**: en la card pausada aparece un menú secundario con "Despausar" que la vuelve al estado anterior. Útil cuando ya consiguió la info y quiere volver a evaluarlo de cero.

## Decisiones del usuario en este flujo

- Pausar con o sin nota.
- Si volver más tarde y delegar, descartar, o despausar.

## Componentes UI involucrados

- 2. Card de ticket en el backlog enriquecido
- 14. Pill de estado (la card pasa a "⏸️ Pausado para profundizar")

## Notas para el prototipo HTML

- Estados sugeridos:
  - `d1-card-pausada.html` (card en ⏸️ con nota visible al hover)
  - `d1-modal-pausa-nota.html` (input opcional de nota antes de confirmar pausa)
- Para el prototipo, el "hover muestra la nota" puede simularse con un tooltip CSS o con un popover al click.

## Referencias

- Journey: ROBOK_v5.md §1.6 — Etapa 2, "Dos decisiones distintas en esta etapa: Pausa para profundizar".
- Inventario: 03-inventario-pantallas.md §D1.
- Componentes UI: 04-componentes-ui.md §2, §14.
- Principios involucrados: P3 (el humano decide — pausar también es decisión, no solo delegar), P8 (Vera elige el trade-off entre actuar ahora y conseguir más info).
