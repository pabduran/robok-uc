# CU-07. Vera lee el insight inicial, encuentra errores, y los corrige conversando con Rob

## Identidad

- **Actor**: Vera (Dev Lead)
- **Pantalla principal**: B3. Onboarding — Insight inicial proactivo
- **Pantallas secundarias**: B2 (entrada), C1. Vista del producto (salida).
- **Pre-condición**: Indexado terminó en B2. Rob produjo el insight inicial pero hay gaps o errores que Vera detecta al leer.
- **Post-condición**: Insight corregido (inline o vía conversación con Rob); el producto queda listo. ROBOK lleva a Vera a C1.
- **Etapa del journey**: Etapa 1 — Onboarding del producto
- **Frecuencia esperada**: una vez por producto (one-time). Es esperable que la mayoría de los onboardings caigan acá y no en el happy path puro de CU-06, porque el primer indexado siempre tiene gaps.

## Disparador

Igual a CU-06: ROBOK transiciona automáticamente desde B2 cuando termina el indexado.

## Flujo principal (happy path)

1. Vera ve B3 con el canvas "Lo que entendí del producto".
2. Vera detecta dos errores al revisar:
   - **(a)** Rob dijo que el stack incluye Redis. El equipo migró a Memcached el sprint pasado, pero la migración vive en una rama no mergeada y Rob no la consideró.
   - **(b)** Rob dice "tests viven en `tests/` espejo del código", pero hay tests específicos del módulo de pricing que viven adyacentes al código en `pkg/promise/test_*.py`.
3. Vera click en "Corregir inline" sobre el bloque de stack. Edita "Redis → Memcached" y agrega una nota libre: "migrado en el sprint reciente, todavía no mergeado a main, Rob: tenelo en cuenta para el próximo refresh".
4. Vera click en "Corregir inline" sobre el bloque de convenciones. Agrega: "+ tests adyacentes al código en `pkg/promise/test_*.py` para el módulo de pricing".
5. Vera quiere asegurarse de un tercer punto: el módulo async de promise-worker. Click en "Preguntarle a Rob" → se abre un panel lateral con la conversación (componente 7. Avatar y mensaje del muro). Vera valida preguntando, no haciendo clic en "confirmar".
6. Vera escribe: "¿Detectaste el módulo de async tasks de promise-worker? Es importante para el equipo."
7. Rob responde mostrando qué entendió de promise-worker: estructura, tareas detectadas (`enqueue_promise`, `expire_promise`), dependencias (Redis para la cola, ahora Memcached según la corrección de Vera). Confirma que sí lo entendió pero lo había omitido del resumen visible. Rob propone: "¿querés que lo agregue al bloque de estructura del insight?". Rob propone el agregado. Vera confirma o corrige antes de que Rob actúe.
8. Vera responde "dale, agregalo". Rob actualiza el insight con el nuevo bloque y muestra el cambio destacado.
9. Vera relee el insight completo. Ahora refleja el producto correctamente.
10. Vera click en "Producto listo".
11. ROBOK lleva a Vera a C1 — Vista del producto Promise Engine.

## Variantes

### V1. Vera detecta un error grande y pide re-indexado parcial

Si Vera ve que Rob mapeó mal toda una sección (ej. confundió `promise-tools` con un repo activo cuando es deprecado), Vera puede pedir desde la conversación con Rob: "ignorá `promise-tools` para el insight, no se usa". Rob propone marcarlo como "deprecado, fuera del scope de Rob". Vera confirma. Alternativamente, Vera puede pedir un re-indexado parcial de un repo específico desde C4 — Configuración del producto (post-aprobación).

### V2. Vera no logra resolver un gap aunque pregunta

Si Vera intentó conversar con Rob varias veces y un gap sigue sin resolverse (ej. una convención del equipo que no quedó clara), Vera puede aprobar el insight con observaciones: el bloque "🤔 Cosas que no entendí bien" mantiene esa duda explícita y queda visible en C4 para que el equipo la revise más tarde.

## Caminos alternativos / errores

- **Si Rob no entiende la corrección inline**: cuando Vera edita un bloque inline, Rob detecta que el cambio puede afectar otros bloques (ej. cambiar Redis por Memcached afecta también la sección de actividad reciente que mencionó Redis). Rob pregunta en el panel lateral: "tu cambio afecta también X, ¿lo ajusto?". Vera responde, Rob ajusta.
- **Si una corrección de Vera contradice algo que Rob ve en el código actual**: Rob lo marca explícitamente ("vos decís Memcached, pero veo `redis-py` en `requirements.txt` de main. ¿Querés que lo registre como en transición?"). La negociación queda registrada y forma parte del insight final.

## Decisiones del usuario en este flujo

- Corregir inline o pedir re-indexado parcial.
- Preguntar a Rob para validar o aprobar con observaciones aceptando los gaps.
- Aceptar las propuestas de Rob (ej. agregar el bloque de promise-worker) o rechazarlas.

## Componentes UI involucrados

- 1. Header con badge ambient
- 11. Insight inicial / proactivo
- 7. Avatar y mensaje del muro de discusión (panel lateral con la conversación con Rob)

## Notas para el prototipo HTML

- B3 es la pantalla más visualmente rica del onboarding. Esta variante (con corrección y conversación) es la que mejor demuestra "validación conversacional, no wizard".
- Estados sugeridos como archivos separados (extienden los de CU-06):
  - `b3-insight-con-correcciones.html` (Vera ya corrigió 2 bloques inline, sin conversación abierta)
  - `b3-insight-conversando.html` (panel lateral abierto, hilo de mensajes con Rob — incluyendo propuesta de Rob de agregar bloque)
  - `b3-insight-corregido-listo.html` (todo arreglado, listo para "Producto listo")
- Para el panel lateral, reutilizar el componente 7 con dos avatares (👤 Vera y 🤖 Rob — Architect general, sin rol específico aún).

## Referencias

- Journey: ROBOK_v5.md §1.5 — Etapa 1, ramas H y I del flujo detallado ("¿el insight refleja bien el producto?" → "Líder corrige o agrega contexto" / "Líder le pregunta a Rob").
- Inventario: 03-inventario-pantallas.md §B3 (estados "con correcciones marcadas por el Dev Lead").
- Componentes UI: 04-componentes-ui.md §11 y §7.
- Principios involucrados: P5 (demostrar entendimiento — Rob debe poder defender lo que dijo), P3 (inferencia con confirmación humana — Rob propone agregar el bloque, Vera confirma antes de que Rob actúe), P6 (presencia adaptativa — Vera elige cuánto preguntar).
