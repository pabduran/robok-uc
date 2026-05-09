# CU-23. Vera aprueba un fix de test propuesto en checkpoint (propose-diff-then-approve)

## Identidad

- **Actor**: Vera (Dev Lead)
- **Pantalla principal**: F3. Pantalla de checkpoint (variante propose-diff-then-approve)
- **Pantallas secundarias**: F1 (entrada vía notificación interrupting).
- **Pre-condición**: El squad está implementando PROM-1234. Después del cambio en T-2 (refactor de pricing), un test existente (`test_pricing_with_promo`) empezó a fallar. El 🧪 Tester detectó la falla. El 🔨 Implementer #2 propuso un ajuste al test. El squad pausó en checkpoint con el diff listo. NO se auto-arregla.
- **Post-condición**: Vera aprobó (o rechazó) el diff explícitamente. El squad reanuda. La aprobación queda en el historial — el "verde mentiroso" no es posible porque el cambio fue revisado por humano.
- **Etapa del journey**: Etapa 4 — Implementación con squad
- **Frecuencia esperada**: 0–2 veces por historia (no en todas, pero cuando hay refactor que toca módulos con tests, sí).

## Disparador

ROBOK lanza notificación interrupting: "🟡 Squad esperándote · PROM-1234 · Fix de test requiere tu aprobación · [Ver checkpoint]". Vera click.

## Flujo principal (happy path)

1. Vera click en "Ver checkpoint" desde el toast (o desde F1).
2. F3 abre como modal bloqueante con la **variante propose-diff-then-approve**:
   - **Por qué el squad pausó**: "Test arreglado, requiere tu aprobación".
   - **Lo que pasó**: "El Tester detectó que `test_pricing_with_promo` fallaba después del cambio en T-2. El Implementer #2 propuso un ajuste al test."
   - **Diff propuesto** (destacado visualmente, no enterrado):
     ```
     - assert calculate(100, "PROMO20") == 80
     + assert calculate(100, "PROMO20") == 75
       # ahora aplica también fee de 5% post-descuento
     ```
   - **⚠️ Copy preventivo destacado**: "Aprobá solo si el cambio refleja la lógica correcta, no si 'solo querés que pase verde'."
   - **Justificación del Implementer #2**: "El refactor de T-2 introdujo el fee post-descuento (ADR-2026-014, sección 3). El test viejo asumía que PROMO20 daba 80 (100 − 20% descuento). Ahora con fee de 5%, el resultado correcto es 75 (80 − 5% fee). El test estaba congelado en el comportamiento previo al ADR."
   - Botones: **[Aprobar]** · **[Pedir cambios]** (caja de texto) · **[Detener squad]**.
3. Vera lee. Verifica el ADR-2026-014 que se referencia (link inline). Confirma que la lógica nueva es la correcta — el test estaba reflejando el comportamiento viejo.
4. Vera click en "Aprobar".
5. ROBOK marca el diff como ✅ aprobado por Vera, registra la aprobación en el historial del checkpoint y reanuda al squad. La timeline editorial registra: "✅ Test fix aprobado por Vera · diff registrado · Tester continúa".
6. El squad reanuda. El Tester re-corre la suite y T-2 cierra cuando todo pasa.

## Variantes

### V1. Vera pide cambios al diff propuesto

Vera lee el diff y se da cuenta de que el cambio es válido pero el comentario explicativo está mal redactado. Click en "Pedir cambios". Caja de texto: "el assert está bien, pero cambiá el comentario a algo tipo `# fee de 5% post-descuento según ADR-2026-014`. El comentario actual es ambiguo." Envía. El Implementer ajusta el comentario y vuelve a checkpoint con el diff revisado.

### V2. Vera rechaza porque el test estaba bien

Vera lee el diff y se da cuenta de que el test viejo estaba correcto — el problema es que el refactor introdujo un bug, no el test. Click en "Pedir cambios" con: "el test estaba bien, el refactor introdujo un bug. Ajustá el código de T-2, no el test. La lógica esperada es 80, no 75." El Implementer entiende, revierte el cambio del test, y arregla el código del refactor. Vuelve a checkpoint con el nuevo plan.

### V3. Vera detiene porque el caso revela un cambio mayor

Vera lee el diff y se da cuenta de que la decisión del fee post-descuento (que motivó el cambio del test) impacta cosas que el plan no consideró (ej. integraciones externas que esperan el cálculo viejo). Click en "Detener squad". El squad termina y vuelve a Etapa 3 para replanificar — el ADR-2026-014 hay que retrabajarlo.

## Caminos alternativos / errores

- **Si Vera aprueba sin leer el diff** (riesgo del "verde mentiroso"): el copy preventivo es la última defensa. Si igual aprueba a ciegas, la responsabilidad es suya. ROBOK registra la aprobación con timestamp y el diff completo para auditar después.
- **Si el diff es muy largo** (ej. el Implementer modificó 4 tests): F3 muestra los diffs colapsables por archivo. Vera puede expandir uno por uno o aprobar todo en bloque (un solo botón "Aprobar" cubre el set).

## Decisiones del usuario en este flujo

- Aprobar / Pedir cambios / Detener.
- Si pide cambios: ser específica (al test, al código del refactor, o a ambos).
- Si detiene: aceptar que la historia vuelve a Etapa 3.

## Componentes UI involucrados

- 10. Notificación interrupting (toast)
- 9. Pantalla de checkpoint — variante propose-diff-then-approve (con diff destacado y copy preventivo)
- 1. Header con badge ambient

## Notas para el prototipo HTML

- F3 con diff destacado: usar bloque `<pre>` con sintaxis de diff (líneas + en verde, − en rojo). El copy preventivo en banner amarillo destacado arriba del diff o entre el diff y los botones.
- Estados sugeridos como archivos separados:
  - `f3-checkpoint-fix-de-test.html` (caso central: diff de un solo test)
  - `f3-checkpoint-fix-multiples.html` (varios diffs colapsables)
  - `f3-checkpoint-fix-rechazado.html` (V2: caja de texto con feedback del rechazo)

## Referencias

- Journey: ROBOK_v5.md §1.8 — Etapa 4, "Tests rotos NO se auto-arreglan — propose-diff-then-approve" y la mención del "verde mentiroso".
- Inventario: 03-inventario-pantallas.md §F3 (estado "Checkpoint con propose-diff-then-approve").
- Componentes UI: 04-componentes-ui.md §9 (variante propose-diff).
- Principios involucrados: P3 (propose-diff-then-approve para tests — es el principio anti-"verde mentiroso"; los tests rotos NO se auto-arreglan), P5 (Rob propone el diff con justificación, no solo lo aplica).
