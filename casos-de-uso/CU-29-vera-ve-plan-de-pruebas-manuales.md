# CU-29. Vera ve el plan de pruebas manuales en Jira (espejado en ROBOK)

## Identidad

- **Actor**: Vera (Dev Lead)
- **Pantalla principal**: G2. Pre-merge Gate dashboard (sección "Plan de pruebas manuales")
- **Pantallas secundarias**: Jira (externo, accesible vía link "Ver en Jira ↗").
- **Pre-condición**: Las validaciones automatizadas pasaron (CU-28). El PR draft está abierto. Rob posteó automáticamente un comentario estructurado en Jira con el checklist de pruebas manuales — un item por caso, con template predefinido. Los humanos del equipo (típicamente Diego, Sofía, otros Developers) tienen N horas (configurable: 24h, 48h, 1 semana) para ejecutar las pruebas.
- **Post-condición**: Vera entiende el estado de las pruebas manuales en G2 sin tener que saltar a Jira. Ve qué pasó, qué falló, qué queda pendiente. Decide si esperar, reasignar, o intervenir.
- **Etapa del journey**: Etapa 5 — Pre-merge Gate
- **Frecuencia esperada**: varias veces durante la ventana de pruebas manuales (Vera mira el progreso sin necesidad de actuar en cada visita).

## Disparador

Vera entra a G2 después de que el PR draft abrió (CU-28). O recibe notificación summary cuando un humano marca un item ("Diego marcó P-2 como pasada").

## Flujo principal (happy path)

1. Vera ve la sección "Plan de pruebas manuales · PROM-1234" en G2 con link "[Ver en Jira ↗]".
2. La sección muestra el espejo del checklist que vive en Jira:
   - ✅ 1. Calcular promesa con cliente nuevo (Sofía · ayer)
   - ✅ 2. Calcular promesa con código de descuento (Diego · 2h)
   - ⏸️ 3. Cancelar promesa pendiente (sin asignar todavía)
   - ⏸️ 4. Validar timeout en cola Redis (sin asignar)
   - N/A 5. Compatibilidad navegador antiguo (no aplica — el cambio es backend)
3. Estado agregado: "2/5 pasados · 0 fallaron · 2 pendientes · 1 N/A".
4. Vera entiende que va bien, faltan dos items por ejecutar. La ventana del producto es 48h y todavía quedan 24h.
5. Vera no necesita actuar todavía. Cierra G2 o sigue con otra historia.
6. Más tarde, vuelve a G2 y ve que los items 3 y 4 también pasaron. El estado agregado: "4/5 pasados · 0 fallaron · 0 pendientes · 1 N/A". Banner verde: "Pruebas manuales completas — listo para code review humano del PR".

## Variantes

### V1. Una prueba manual falló

Diego marcó P-3 como ❌ Falló (cubierto en CU-30). La sección muestra:
- ❌ 3. Cancelar promesa pendiente (Diego · hace 30 min) · "Falla observada — [Ver mini-muro →]"
- Estado agregado: "2/5 pasados · 1 falló · 1 pendiente · 1 N/A".
- Banner amarillo "Una prueba falló — mini-muro abierto, ver detalle". Click lleva a G3 (CU-31).

### V2. Vera click en un item pasado para ver detalle

Vera quiere chequear cómo Diego ejecutó el test 2 (caso del descuento). Click en el item ✅ 2. Panel lateral muestra el reporte estructurado del humano: pasos ejecutados, screenshot adjunto si lo subió a Jira, observaciones libres. Vera lee, conforme.

### V3. Ventana de pruebas manuales por vencer

La ventana del producto era 48h. Quedan 6h y todavía hay 2 items pendientes. ROBOK manda recordatorio a Slack/Teams del canal del producto: "PROM-1234 · 2 pruebas manuales pendientes · ventana cierra en 6h". Si pasan las 48h sin ejecución, ROBOK NO avanza solo (coherente con principio P3) — manda nuevo recordatorio escalado y el squad sigue parado. Vera puede extender la ventana manualmente desde G2 si lo cree razonable.

## Caminos alternativos / errores

- **Si Jira no responde** (token expirado, caída del proveedor): la sección muestra "No se pudo cargar desde Jira · [Reintentar] · [Ver en Jira ↗]". El estado en G2 queda con la última snapshot conocida + banner amarillo "Sin sincronizar — última actualización hace X min".
- **Si los Developers ejecutan pruebas pero no las marcan en Jira**: Vera lo ve como "⏸️ pendiente" en G2. Tiene que escalar manualmente al equipo para que actualicen, o asumir que el silencio significa que no se ejecutó.
- **Si Vera quiere reasignar un item**: en V1 Vera tiene que ir a Jira para reasignar (el espejo es de lectura). V2 puede agregar reasignación directa desde ROBOK.

## Decisiones del usuario en este flujo

- Esperar el flujo normal o intervenir (recordatorio manual, reasignar en Jira, extender ventana).
- Si una prueba falló: ir al mini-muro inmediatamente o esperar a Diego.
- Si la ventana se vence: extender o decidir avanzar sin esa prueba (Vera puede aprobar excepción).

## Componentes UI involucrados

- 1. Header con badge ambient
- 13. Plan de pruebas manuales (espejo de Jira) — la pieza central de esta sección
- 14. Pill de estado (en cada item)

## Notas para el prototipo HTML

- G2 ya tiene la sección como bloque dentro del dashboard. Reusar el componente 13.
- Estados sugeridos como archivos separados:
  - `g2-pruebas-manuales-en-progreso.html` (2/5 pasados, 2 pendientes, 1 N/A)
  - `g2-pruebas-manuales-completas.html` (4/5 pasados, banner verde "listo para code review")
  - `g2-pruebas-manuales-con-falla.html` (V1: 1 ❌ con CTA al mini-muro)
  - `g2-pruebas-manuales-vencidas.html` (V3: ventana cerca de vencer, banner amarillo)
- Para el panel lateral del item (V2), reusar la mecánica del drill-down (deslizable desde la derecha).

## Referencias

- Journey: ROBOK_v5.md §1.9 — Etapa 5, "Plan de pruebas manuales en Jira" y "Pruebas manuales que tardan demasiado".
- Inventario: 03-inventario-pantallas.md §G2.
- Componentes UI: 04-componentes-ui.md §13 (plan de pruebas manuales).
- Principios involucrados: P3 (squad espera al humano · ROBOK no avanza sin las pruebas marcadas), P5 (mostrar trabajo · espejo de Jira sin tener que saltar afuera).
