# CU-26. El squad termina, Vera recibe el reporte de cierre

## Identidad

- **Actor**: Vera (Dev Lead)
- **Pantalla principal**: G1. Reporte de cierre del squad
- **Pantallas secundarias**: F1 (entrada — el squad transiciona a G1 al cerrar todas las tareas), G2. Pre-merge Gate dashboard (salida).
- **Pre-condición**: El squad de PROM-1234 cerró todas las tareas del plan. Todos los checkpoints fueron resueltos. El último Implementer cerró su PR draft local; el Tester corrió la suite y todo pasa.
- **Post-condición**: Vera leyó el reporte (capa Summary del status en 4 capas). Se inicia Etapa 5 con la transición a G2.
- **Etapa del journey**: Etapa 4 — Implementación con squad (cierre) → puente a Etapa 5
- **Frecuencia esperada**: una vez por historia que se completó (las que se cancelaron son CU-18, las que se detuvieron por costo o checkpoint son CU-22 V2 / CU-25 V1).

## Disparador

El squad cerró la última tarea del plan. ROBOK genera el reporte de cierre y notifica a Vera (capa Summary — voluntaria, no interrupting).

## Flujo principal (happy path)

1. Vera ve la notificación summary en su badge ambient: 🟢 "Squad terminó · PROM-1234 · ver reporte". También llega a Slack/Teams como mensaje normal (no urgente, sin requerir acción).
2. Vera click en el badge o en el link de Slack. ROBOK la lleva a G1 — Reporte de cierre del squad.
3. Vera ve el reporte completo:
   - **Header**: ✅ "Squad terminó · PROM-1234 · 6h 12min · USD 52 / USD 55 estándar".
   - **PRs abiertos**: 1 PR draft `rob/PROM-1234` con link al GitHub draft.
   - **ADRs producidos**: ADR-2026-014 (uso de Decimal para precisión monetaria), con link al doc.
   - **Tests escritos**: 12 tests nuevos (8 unitarios, 4 integración), cobertura del módulo `pricing` subió de 78% a 91%.
   - **Costo real vs estimado**: USD 52 (estimado USD 55, 6% bajo presupuesto).
   - **Tiempo total vs estimado**: 6h 12min (estimado 8h, 23% más rápido).
   - **Lista de checkpoints superados**: 3 checkpoints (contrato del endpoint, fix de test post-refactor, naming del endpoint), cada uno con timestamp y decisión.
   - **Notas notables**: "1 cambio de plan post-aprobación: Diego sugirió usar `requests` en lugar de `httpx`, ADR-2026-016 quedó como convención del producto."
4. Vera lee. Confirma que el resultado refleja lo que esperaba.
5. Vera click en "Continuar a Pre-merge Gate" (o ROBOK auto-transiciona si Vera tarda en actuar).
6. ROBOK la lleva a G2 — Pre-merge Gate dashboard (cubierto en CU-28).

## Variantes

### V1. Reporte con desviaciones grandes destacadas

Si el costo o tiempo se desbordó significativamente, el reporte destaca esos números en rojo:
- "💵 Costo real USD 78 (estimado USD 55, +42%) — ver historial de ampliaciones de cap"
- "⏱️ Tiempo real 14h (estimado 8h, +75%) — el checkpoint del contrato tomó 4h por ida y vuelta con Pablo"

Vera lee, registra mentalmente la lección, y decide si revisar el plan original para mejorar futuras estimaciones.

### V2. Reporte de squad detenido por humano

Si el squad fue detenido (CU-22 V2 o CU-25 V1) en lugar de terminado, G1 se llama "Reporte de detención" en lugar de "Reporte de cierre". Muestra qué se hizo, qué quedó pendiente, y ofrece dos caminos: "Volver a Etapa 3 para replanificar" o "Cancelar definitivamente · plan guardado".

### V3. Auto-transición sin lectura

Si Vera no entra al reporte en N horas (configurable), ROBOK auto-transiciona a G2 y el reporte queda accesible desde C2 ("Mis historias en curso") o desde G2 (link "Ver reporte de cierre"). El reporte nunca se pierde.

## Caminos alternativos / errores

- **Si el squad terminó pero hay tests rojos no resueltos**: G1 muestra un banner amarillo "El squad cerró pero tiene 2 tests pendientes de propose-diff-then-approve. Resolverlos antes de continuar a Pre-merge Gate." Botón "Ver checkpoints pendientes" lleva a F3 (CU-23).
- **Si el reporte falla en generar** (caso raro de error interno): ROBOK muestra G1 con la información disponible y un mensaje "Algunos datos del reporte no pudieron consolidarse — ver detalle en C2".

## Decisiones del usuario en este flujo

- Continuar a Pre-merge Gate ahora o postergar.
- Revisar el reporte vs auto-transicionar.
- Si hay desviaciones grandes: registrar la lección en el muro o solo internamente.

## Componentes UI involucrados

- 1. Header con badge ambient (cambia a "Squad terminó · ver reporte" en capa Summary)
- 14. Pill de estado (en cada sección del reporte: PR, ADRs, tests, costo, tiempo)

## Notas para el prototipo HTML

- G1 es pantalla simple, una sola columna. Vale por la legibilidad, no por la sofisticación visual.
- Estados sugeridos como archivos separados:
  - `g1-reporte-cierre-feliz.html` (caso bajo presupuesto y dentro de tiempo)
  - `g1-reporte-cierre-con-desviaciones.html` (V1: rojo en costo y tiempo)
  - `g1-reporte-detencion.html` (V2: squad detenido)
  - `g1-reporte-con-tests-pendientes.html` (banner amarillo de checkpoints sin resolver)

## Referencias

- Journey: ROBOK_v5.md §1.8 — Etapa 4, "Status en 4 capas — Summary" y la transición de cierre con "Reporte de cierre · PRs · ADRs · tests · costo real vs estimado".
- Inventario: 03-inventario-pantallas.md §G1.
- Principios involucrados: P5 (mostrar trabajo — el reporte muestra evidencia, no solo "✓ listo"), P6 (Summary es voluntaria, no demanda atención).
