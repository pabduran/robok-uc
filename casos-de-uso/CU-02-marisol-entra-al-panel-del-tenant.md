# CU-02. Marisol entra como Admin y accede al panel del tenant

## Identidad

- **Actor**: Marisol (Admin de ROBOK)
- **Pantalla principal**: H1. Panel del tenant
- **Pantallas secundarias**: A1. Login / SSO (entrada), A2. Selector de productos (paso intermedio)
- **Pre-condición**: Marisol es la dueña del tenant. Tiene credenciales de admin reconocidas por el SSO de la empresa.
- **Post-condición**: Marisol está dentro de H1 con la vista agregada del tenant (productos, costos, salud, configuración global).
- **Etapa del journey**: Admin
- **Frecuencia esperada**: una vez por semana (no es la herramienta del día a día de Marisol).

## Disparador

Marisol abre ROBOK como parte de su review semanal del tenant: quiere ver costos del mes, revisar salud de los productos y eventualmente provisionar un producto nuevo.

## Flujo principal (happy path)

1. Marisol abre la URL de ROBOK. ROBOK la lleva a A1 — Login / SSO.
2. Click en "Entrar con SSO". El proveedor de identidad devuelve el rol de Admin de ROBOK además de cualquier producto donde Marisol esté mapeada.
3. ROBOK la redirige a A2 — Selector de productos. A diferencia de Vera, Marisol ve dos zonas: el grid de productos (vacío o con los productos donde es Dev Lead, si aplica) y un acceso destacado al **Panel del tenant**.
4. Marisol click en "Panel del tenant".
5. ROBOK lleva a H1. Marisol ve:
   - Lista de productos del tenant (Promise Engine, Ratings & Reviews) con costo del mes y salud (🟢 / 🟡 / 🔴).
   - Botón "Provisionar nuevo producto".
   - Sección "Asignar admins de productos".
   - Reporte de costos agregado del tenant.
   - Configuración global (límites de presupuesto, integraciones SSO).
6. Marisol revisa que Promise Engine gastó USD 1.200 esta semana y Ratings & Reviews USD 800. Ambos dentro de presupuesto. Salud 🟢 en los dos.
7. Marisol cierra ROBOK sin haber tocado nada (solo vino a mirar).

## Variantes

### V1. Marisol también es Dev Lead de algún producto

A2 muestra el grid de sus productos + el acceso al Panel del tenant. Marisol elige a dónde entrar primero según el motivo de su sesión.

### V2. Provisionamiento de un producto nuevo

Desde H1, Marisol click en "Provisionar nuevo producto" → flujo cubierto en CU-35.

## Caminos alternativos / errores

- **Si SSO falla o no devuelve el rol de admin**: Marisol entra como cualquier humano (al selector de productos sin acceso al panel del tenant). Tiene que escalar a soporte interno.
- **Si un producto está en 🔴 (algo bloqueado o budget sobrepasado)**: la card del producto en H1 destaca en rojo y Marisol puede hacer click para ver el detalle agregado (costo, historias bloqueadas).

## Decisiones del usuario en este flujo

- Entrar al Panel del tenant o entrar a un producto donde es Dev Lead (si aplica).
- Acción a tomar al ver los costos: nada, ajustar límites, o investigar un producto específico.

## Componentes UI involucrados

- 14. Pill de estado (salud por producto, badges de presupuesto)

## Notas para el prototipo HTML

- H1 es **opcional para el MVP visual** según la priorización del doc 03. Si se construye, basta con una pantalla simple con tabla de productos + 4 secciones (provisionar, admins, costos, configuración).
- Estados sugeridos:
  - `h1-tenant-saludable.html` (todos en 🟢)
  - `h1-tenant-con-alerta.html` (un producto en 🟡 por costo cerca del límite)

## Referencias

- Journey: ROBOK_v5.md §1 (la unidad de tenant aparece implícitamente en el selector de productos y en la gobernanza P4).
- Inventario: 03-inventario-pantallas.md §H1.
- Principios involucrados: P2 (el producto es la unidad de todo, el tenant es la unidad superior), P4 (gobernanza configurable en capas — Marisol opera en el nivel ROBOK).
