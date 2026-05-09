# CU-34. Vera revisa una versión antigua del mapa para auditar una decisión de hace 6 meses

## Identidad

- **Actor**: Vera (Dev Lead)
- **Pantalla principal**: C3. Mapa de componentes (modo versión antigua)
- **Pantallas secundarias**: ADR específico (vista externa o panel lateral con el documento del ADR).
- **Pre-condición**: Hace 6 meses se tomó una decisión arquitectónica (ADR-2025-088) que afectó al producto. Vera necesita revisar por qué se tomó — alguien le pidió contexto, o ella misma lo cuestiona ahora ante un cambio nuevo. El plan/ADR de ese momento quedó anclado a un snapshot del mapa de ese día.
- **Post-condición**: Vera ve el mapa como estaba al momento del ADR. Entiende el contexto histórico — qué componentes existían, cuáles no, qué dependencias había. Sale del modo histórico y vuelve a la vista actual.
- **Etapa del journey**: Vista auxiliar (transversal — auditoría de decisiones)
- **Frecuencia esperada**: 1–2 veces por mes (no es frecuente, pero crítico cuando hace falta).

## Disparador

Vera está revisando ADR-2025-088 ("Estrategia de migrations de Postgres") porque alguien propuso una nueva migration y quiere entender por qué la regla actual existe. Click en el link del ADR → ROBOK detecta que el ADR tiene snapshot anclado y ofrece "Ver el mapa al momento del ADR".

## Flujo principal (happy path)

1. Vera está leyendo ADR-2025-088 (vista del documento del ADR, accesible desde el muro o desde C3 → panel del nodo Postgres).
2. Al final del ADR, Vera ve un link "📎 Ver el mapa del producto al momento de este ADR (2025-11-14)".
3. Click. ROBOK la lleva a C3 en **modo versión antigua**:
   - Banner amarillo destacado en el tope: "Viendo snapshot del 14 nov 2025 · este mapa es histórico, NO refleja el estado actual · [Volver al actual]".
   - El dropdown del banner superior cambia de "v actual ▼" a "14 nov 2025 ▼".
   - El mapa renderea como estaba ese día.
4. Vera compara mentalmente con el mapa actual:
   - Hace 6 meses NO existía `promise-cache`.
   - `billing-api` consumía `redis` directamente, ahora va vía `promise-worker`.
   - Había un módulo `legacy-billing` que ya fue eliminado.
5. Vera entiende el contexto del ADR — la regla de migrations se diseñó cuando el grafo era más simple. Ahora con el nuevo nodo y los cambios de dependencias, la regla puede haber quedado obsoleta o necesitar refinamiento.
6. Vera click en "Volver al actual" para salir del modo histórico. El mapa vuelve a renderear el estado actual.

## Variantes

### V1. Vera compara dos snapshots side-by-side

ROBOK ofrece (V1 simple, V2 complejo) una vista comparada: dropdown "Comparar con" y selecciona "v actual". El mapa muestra dos vistas en columnas con diff visual (nodos en verde si nuevos, en rojo si eliminados, en azul si solo cambió la posición). Para V1 esta variante puede ser solo un toggle "ver diff" que destaca cambios.

### V2. Vera entra al modo histórico desde el dropdown directamente

Vera no viene de un ADR. Está en C3 y quiere ver "cómo era el producto hace un año". Click en "v actual ▼" → selecciona una fecha del dropdown (ROBOK ofrece snapshots cada N tiempo o cada decisión grande). El mapa renderea esa versión.

### V3. Vera audita una decisión del muro (no de un ADR formal)

Vera tiene una nota en una historia donde Rob propuso usar Memcached. Quiere entender el contexto: ¿qué cache existía cuando se tomó esa propuesta? Click en el link del muro → snapshot del mapa al momento de la propuesta.

## Caminos alternativos / errores

- **Si no hay snapshot disponible para esa fecha** (ej. el producto se onboardeó después): ROBOK muestra "No hay snapshot disponible para 14 nov 2025 · el snapshot más antiguo es 5 ene 2026". Vera puede elegir una fecha posterior.
- **Si Vera intenta hacer click en un nodo en modo histórico**: el panel lateral abre con la información del nodo en ese snapshot. Si el nodo ya no existe en el actual, indica "Este nodo fue eliminado el X — ver ADR Y donde se justificó".
- **Si Vera intenta editar algo en modo histórico** (ej. agregar nota a un nodo): no se puede — banner "Modo lectura histórica". Vera tiene que volver al actual para hacer cambios.

## Decisiones del usuario en este flujo

- Qué fecha/snapshot mirar.
- Si comparar con el actual o solo ver el histórico.
- Si la diferencia entre histórico y actual amerita acciones (ej. revisar si el ADR sigue válido, abrir nuevo ADR para superseder).

## Componentes UI involucrados

- 1. Header con badge ambient
- 8. Mapa de componentes (modo versión antigua — banner amarillo, dropdown de versión)
- 14. Pill de estado (en el banner: "Histórico")

## Notas para el prototipo HTML

- Estados sugeridos como archivos separados:
  - `c3-mapa-historico-2025-11-14.html` (mapa de noviembre 2025 con banner amarillo)
  - `c3-mapa-comparado.html` (V1: side-by-side con diff visual)
  - `c3-mapa-historico-nodo-eliminado.html` (panel lateral sobre un nodo que ya no existe)
- Para el prototipo, basta con tener 2–3 snapshots fechados como archivos distintos del mismo SVG con diferencias visuales.

## Referencias

- Journey: ROBOK_v5.md §1.7 — Etapa 3 + §Principios P7 ("El mapa de componentes está versionado · cada decisión queda anclada al snapshot").
- Inventario: 03-inventario-pantallas.md §C3 (estado "Vista versionada antigua").
- Componentes UI: 04-componentes-ui.md §8 (modo versión antigua).
- Principios involucrados: P7 (trabajo informado por contexto fresco Y versionado · cada decisión queda anclada al snapshot, auditar a 6 meses requiere reconstruir el mundo en que se tomó).
