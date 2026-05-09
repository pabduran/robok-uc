# CU-01. Vera entra a ROBOK por primera vez y elige un producto

## Identidad

- **Actor**: Vera (Dev Lead)
- **Pantalla principal**: A2. Selector de productos
- **Pantallas secundarias**: A1. Login / SSO (entrada), C1. Vista del producto (salida)
- **Pre-condición**: Marisol ya provisionó el producto Promise Engine y le asignó a Vera el rol de Dev Lead. Vera tiene cuenta SSO en la empresa.
- **Post-condición**: Vera está dentro de C1 — Vista del producto Promise Engine.
- **Etapa del journey**: Etapa 0 — Acceso
- **Frecuencia esperada**: la primera vez es única; después se reduce a un click cada vez que vuelve a ROBOK (o se salta con auto-redirect si solo tiene un producto).

## Disparador

Vera recibió un mensaje de Marisol diciéndole "ya está tu producto en ROBOK, entrá". Abre la URL en el navegador.

## Flujo principal (happy path)

1. Vera abre la URL de ROBOK en el navegador. ROBOK detecta que no está autenticada y muestra A1 — Login / SSO con el logo y un botón "Entrar con SSO".
2. Vera click en "Entrar con SSO". El proveedor de identidad de la empresa autentica a Vera y devuelve los permisos.
3. ROBOK la redirige automáticamente a A2 — Selector de productos.
4. Vera ve un grid con los productos a los que pertenece: **Promise Engine** y **Ratings & Reviews**. Cada card muestra: nombre del producto, mini-stats (historias en curso, costo del sprint en lucas), y un badge de notificación si algo demanda atención.
5. Vera ve que Promise Engine tiene 3 historias en curso y un badge amarillo "Squad esperándote · PROM-1234". Ratings & Reviews no tiene actividad pendiente.
6. Vera click sobre la card de Promise Engine.
7. ROBOK lleva a Vera a C1 — Vista del producto Promise Engine.

## Variantes

### V1. Vera tiene un solo producto

Si Vera estuviera asignada a un único producto, A2 hace auto-redirect a C1 sin mostrar el selector. La pantalla A2 sigue siendo accesible desde el header con badge ambient (componente 1) si Vera quiere volver a verla.

### V2. Vera no tiene productos asignados

A2 muestra un mensaje vacío: "Todavía no tenés productos asignados. Hablá con tu Admin de ROBOK." Sin grid, sin acciones más allá de cerrar sesión.

## Caminos alternativos / errores

- **Si SSO falla**: A1 muestra el error devuelto por el proveedor + botón "Reintentar". Sin entrar a ROBOK.
- **Si Vera tiene rol de Admin de ROBOK además de ser Dev Lead**: A2 muestra adicionalmente un acceso al "Panel del tenant" (cubierto en CU-02).

## Decisiones del usuario en este flujo

- A qué producto entrar primero (cuando tiene más de uno).
- Si entrar al producto que pide atención o priorizar otro.

## Componentes UI involucrados

- 14. Pill de estado (en los badges de notificación de cada card de producto)

## Notas para el prototipo HTML

- A1 puede ser una página simple con logo centrado y un botón "Entrar como Vera" que simula el SSO.
- A2 con grid de 2 cards (Promise Engine, Ratings & Reviews). Promise Engine con badge amarillo "Squad esperándote", Ratings & Reviews sin badge.
- Estados sugeridos como archivos separados:
  - `a2-selector-multi-producto.html` (caso normal de Vera)
  - `a2-selector-sin-productos.html` (V2)
  - `a2-selector-con-acceso-admin.html` (variante para CU-02 — Marisol)

## Referencias

- Journey: ROBOK_v5.md §1 (La plataforma — lo que ve el líder al entrar).
- Principios involucrados: P2 (el producto es la unidad de todo), P6 (presencia adaptativa — el badge ambient ya aparece desde el selector).
