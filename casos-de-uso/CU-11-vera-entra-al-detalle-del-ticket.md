# CU-11. Vera entra al detalle de un ticket anotado para ver más contexto

## Identidad

- **Actor**: Vera (Dev Lead)
- **Pantalla principal**: D2. Detalle de ticket anotado
- **Pantallas secundarias**: D1. Sprint actual (entrada y posible vuelta), E1. Confirmación de scope (salida si delega).
- **Pre-condición**: Vera está en D1 y quiere más contexto sobre un ticket antes de decidir si lo delega, lo pausa, o lo descarta.
- **Post-condición**: Vera tomó una decisión (delegar / pausar / no es para Rob) o vuelve a D1 sin tomar ninguna.
- **Etapa del journey**: Etapa 2 — Selección y priorización
- **Frecuencia esperada**: 1–3 veces por sprint, sobre los tickets más complejos o con más historial.

## Disparador

Vera ve una card en D1 con muchas anotaciones interesantes (similitud histórica relevante, riesgo alto, dependencias múltiples) y quiere ver el detalle expandido antes de decidir.

## Flujo principal (happy path)

1. Vera está en D1. Click en cualquier parte de la card de PROM-892 (un ticket parecido al recordado por Rob como similar a uno resuelto en mar-2026).
2. ROBOK lleva a D2 — Detalle de ticket anotado.
3. Vera ve la pantalla expandida con secciones:
   - **Spec original** (lectura desde Jira, sin edición desde ROBOK).
   - **Análisis de Rob**: módulos detallados con justificación, dependencias listadas con estado, gaps de info marcados explícitamente.
   - **Matriz costo × tiempo completa**: cada modo (Express / Estándar / Económico / Sprint-pace) con costo, tiempo y justificación de la diferencia entre modos. Rob muestra qué entendió: el desglose por etapas del trabajo, NO solo dice "USD X".
   - **Historial de tickets parecidos**: "Similar a PROM-892 resuelto en mar-2026 por USD 42 estándar — el squad invirtió más tiempo en tests por el riesgo histórico de pricing". Lista de hasta 3 tickets pasados con resultado (costo real, tiempo real, ADRs producidos).
4. Vera lee. Confirma que entiende el scope y los trade-offs.
5. Vera decide: aprieta directamente "Delegar de una" / "Pausar, profundizar" / "No es para Rob" desde D2 (los mismos botones que en D1), o vuelve a D1 con "← Volver al sprint" para seguir explorando.

## Variantes

### V1. Sin similitudes históricas

Si no hay tickets similares en el historial del producto (ej. el producto se onboardeó hace poco y no hay base), la sección "Historial de tickets parecidos" muestra "Sin tickets similares para mostrar todavía". El resto de las secciones se mantiene.

### V2. Vera detecta que falta info y vuelve a Jira

Vera abre D2 y al leer la spec original confirma que falta criterio de aceptación. Sale de ROBOK, va a Jira a completar la spec con el resto del equipo, y vuelve más tarde — el ticket vuelve a aparecer enriquecido con la nueva spec.

### V3. Vera delega directo desde D2

Vera no necesita volver a D1 para delegar. Aprieta "Delegar de una" desde D2 y el flujo continúa igual que CU-09.

## Caminos alternativos / errores

- **Si Jira no devuelve la spec original**: la sección "Spec original" muestra "No se pudo cargar desde Jira — [Reintentar] o [Ver en Jira ↗]". El resto del análisis de Rob sigue disponible.
- **Si Rob no detectó dependencias pero Vera sabe que existen**: en V1 no hay edición de las anotaciones; Vera tiene que ajustar el ticket en Jira o agregar la dependencia más tarde en el muro de discusión (E3).

## Decisiones del usuario en este flujo

- Volver al sprint o decidir desde acá.
- Si delega: con o sin preselección de modo.
- Si pausa: con o sin nota.

## Componentes UI involucrados

- 1. Header con badge ambient
- 3. Pill de modo costo × tiempo (matriz completa, no miniatura)
- 14. Pill de estado (en el ticket y en cada modo)

## Notas para el prototipo HTML

- D2 puede ser una pantalla simple en una columna, con las 4 secciones colapsables.
- Estados sugeridos:
  - `d2-detalle-con-historial.html` (con sección de tickets similares poblada)
  - `d2-detalle-sin-historial.html` (V1: sin similitudes históricas)
- Reutilizar el componente 3 (pill de modo) en su versión completa con justificación.

## Referencias

- Journey: ROBOK_v5.md §1.6 — Etapa 2, "Anatomía del ticket anotado".
- Inventario: 03-inventario-pantallas.md §D2.
- Componentes UI: 04-componentes-ui.md §3.
- Principios involucrados: P5 (demostrar entendimiento — el detalle muestra la justificación, no solo el número), P7 (trabajo informado por contexto fresco — el historial de tickets pasados ancla la similitud), P8 (decisión bidimensional — la matriz completa permite ver el trade-off con detalle).
