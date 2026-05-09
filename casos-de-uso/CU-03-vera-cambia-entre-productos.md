# CU-03. Vera cambia entre dos productos sin volver al login

## Identidad

- **Actor**: Vera (Dev Lead)
- **Pantalla principal**: C1. Vista del producto (la del producto destino, donde aterriza después del cambio)
- **Pantallas secundarias**: cualquier pantalla del producto origen donde Vera estaba antes (típicamente C1, C2, D1, E3 o F1).
- **Pre-condición**: Vera ya está autenticada y trabajando dentro de un producto (Promise Engine). Tiene acceso a por lo menos un segundo producto (Ratings & Reviews).
- **Post-condición**: Vera está dentro de C1 — Vista del producto Ratings & Reviews. La sesión de Vera se mantiene; no vuelve a A1 ni a A2.
- **Etapa del journey**: Etapa 0 — Acceso (navegación transversal)
- **Frecuencia esperada**: varias veces al día si Vera lidera más de un producto.

## Disparador

Vera está revisando algo en Promise Engine y necesita ir a Ratings & Reviews — por ejemplo, porque alguien le mandó un link de un ticket o porque va a empezar el bloque de tiempo dedicado a ese segundo producto.

## Flujo principal (happy path)

1. Vera está en cualquier pantalla del producto Promise Engine (ej. F1 — Vista del squad activo). El header con badge ambient (componente 1) muestra "ROBOK · Promise Engine ▼ · 🟢 3 historias en curso".
2. Vera click en el selector "Promise Engine ▼" del header.
3. ROBOK despliega un menú con la lista de productos a los que Vera pertenece, incluyendo un mini-resumen por producto (estado global, número de historias).
4. Vera click en "Ratings & Reviews".
5. ROBOK navega a C1 — Vista del producto Ratings & Reviews. El header se actualiza a "ROBOK · Ratings & Reviews ▼ · 🟡 Squad esperándote · RR-512".
6. Vera ya está orientada al nuevo producto.

## Variantes

### V1. El producto destino tiene una notificación interrupting pendiente

Al cambiar a Ratings & Reviews, además de C1 aparece la notificación interrupting (componente 10) sobre el checkpoint pendiente de RR-512. Vera puede resolverlo o postergarlo a "Más tarde".

### V2. Vera cambia desde el medio de una operación que no quería abandonar

Si Vera estaba escribiendo un mensaje no enviado en E3 (muro de discusión) de Promise Engine y cambia de producto, ROBOK le muestra un confirm "Tenés un mensaje sin publicar. ¿Salir igual?". Vera elige seguir o quedarse.

## Caminos alternativos / errores

- **Si Vera intenta cambiar a un producto que ya no tiene acceso** (ej. Marisol le revocó el rol mientras Vera estaba conectada): ROBOK muestra un mensaje "No tenés acceso a este producto" y refresca el selector.

## Decisiones del usuario en este flujo

- A qué producto cambiar.
- Si abandonar contexto sin guardar (variante V2).
- Si responder al checkpoint del producto destino o postergarlo (variante V1).

## Componentes UI involucrados

- 1. Header con badge ambient (con el selector "Producto ▼")
- 10. Notificación interrupting (en variante V1)
- 14. Pill de estado (en el mini-resumen del menú desplegado)

## Notas para el prototipo HTML

- El selector se simula como un `<details>` o un dropdown CSS con la lista de productos.
- Estados sugeridos:
  - `header-selector-cerrado.html` (estado normal)
  - `header-selector-abierto.html` (dropdown desplegado con dos productos)
- Como el cambio de producto navega entre archivos, basta con que el dropdown linkee a `c1-ratings-and-reviews.html`.

## Referencias

- Journey: ROBOK_v5.md §1 (el selector de productos como parte del modelo conceptual).
- Componentes UI: 04-componentes-ui.md §1 (el selector "Promise Engine ▼" es parte del header con badge ambient).
- Principios involucrados: P2 (el producto es la unidad de todo — pero Vera puede pertenecer a varios), P6 (presencia adaptativa — el badge ambient se actualiza al cambiar).
