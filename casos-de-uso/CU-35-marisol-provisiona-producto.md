# CU-35. Marisol provisiona un nuevo producto en el panel del tenant y asigna un Dev Lead

## Identidad

- **Actor**: Marisol (Admin de ROBOK)
- **Pantalla principal**: H1. Panel del tenant
- **Pantallas secundarias**: A2. Selector de productos (entrada típica), eventualmente C4 cuando el Dev Lead asignado entra a configurar el producto.
- **Pre-condición**: Un equipo nuevo de la empresa quiere usar ROBOK para un producto que todavía no existe en el tenant. Marisol recibió el pedido (por email, por ticket interno o por reunión).
- **Post-condición**: El producto queda provisionado en el tenant. Marisol asignó al menos un Dev Lead inicial. El producto está vacío de contexto — el Dev Lead asignado va a tener que onboardearlo (CU-04). Marisol notifica al Dev Lead.
- **Etapa del journey**: Admin
- **Frecuencia esperada**: 1–2 veces por mes (no es la herramienta del día a día de Marisol — el provisioning es esporádico).

## Disparador

Marisol recibe el pedido del equipo de "Inventory" (un producto nuevo): "queremos usar ROBOK, ¿podés provisionarnos?". Marisol entra a ROBOK como Admin (CU-02).

## Flujo principal (happy path)

1. Marisol está en H1 — Panel del tenant. Click en "Provisionar nuevo producto" (botón visible en la sección de productos del tenant).
2. ROBOK abre un formulario (modal o pantalla intermedia) con los campos necesarios:
   - **Nombre del producto**: "Inventory".
   - **Descripción corta**: "Sistema de inventario · ownership equipo Inventory".
   - **Dev Lead inicial**: dropdown con personas del directorio SSO. Marisol selecciona a Andrés (que va a liderar el equipo).
   - **Co-Dev Leads (opcional)**: input para sumar otros Dev Leads. Marisol deja vacío — Andrés decidirá si suma a otros.
   - **Límite de presupuesto inicial** (configurable a nivel tenant): "USD 1.000 / mes" (default tomado de la configuración del tenant; Marisol lo puede ajustar).
   - **Configuración por defecto**: ROBOK ofrece templates ("Producto estándar", "Producto regulado · más quórum", "Producto experimental · cap bajo") o "Configurar manualmente después" (que es lo recomendado — el Dev Lead asignado configura desde C4).
3. Marisol completa los campos. Elige el template "Producto estándar" como default.
4. Click en "Provisionar producto".
5. ROBOK ejecuta:
   - Crea la entidad "producto Inventory" en el tenant.
   - Crea el Context Manager aislado (sin contexto todavía — vacío).
   - Asigna a Andrés como Dev Lead inicial.
   - Aplica la configuración del template "Producto estándar".
   - Manda notificación a Andrés (Slack/Teams + email): "Marisol te asignó como Dev Lead del producto 'Inventory' en ROBOK · [Empezar onboarding]".
6. ROBOK muestra confirmación en H1: "✅ Producto Inventory provisionado · Dev Lead: Andrés · presupuesto inicial USD 1.000/mes". La sección "Productos del tenant" se actualiza con el nuevo item.
7. Marisol queda satisfecha. Cierra ROBOK.
8. (Continuación off-CU): Andrés recibe la notificación, entra a A2, ve el producto Inventory listado, click → ROBOK lo lleva a B1 (CU-04 onboardea desde cero).

## Variantes

### V1. Marisol asigna múltiples Dev Leads desde el inicio

El equipo Inventory ya tiene definidos dos co-líderes (Andrés y otra persona). Marisol los selecciona ambos en el campo Dev Leads. Ambos reciben notificación. Cualquiera de los dos puede arrancar el onboarding (CU-04); el otro entra cuando quiere.

### V2. Marisol provisiona con template "Producto regulado"

Para un producto que toca compliance (ej. un producto financiero), Marisol elige el template "Producto regulado · más quórum". Este template viene con reglas de quórum más estrictas pre-configuradas (ej. "todas las historias requieren 2 Dev Leads", "historias que toquen módulo X requieren Security Officer"). El Dev Lead asignado puede ajustar después en C4 pero la base ya cubre lo crítico.

### V3. Marisol revoca un Dev Lead más tarde

No es CU-35 estrictamente, pero relacionado: Marisol entra a H1 → panel del producto Inventory → "Asignar / revocar Dev Leads" → quita a alguien que dejó el equipo. La persona pierde acceso al producto en su próxima sesión.

## Caminos alternativos / errores

- **Si el nombre del producto ya existe en el tenant**: ROBOK avisa "Ya existe un producto con ese nombre. ¿Querés llamarlo distinto o ver el existente?". Sin permitir provisioning duplicado.
- **Si el Dev Lead seleccionado no existe en el directorio SSO**: ROBOK avisa "Esa persona no está en el directorio · invitá primero al directorio antes de asignar como Dev Lead". Marisol tiene que escalar a IT.
- **Si Marisol intenta provisionar pero el tenant alcanzó su límite de productos** (configurable a nivel contractual): ROBOK avisa "Tu tenant tiene N productos · alcanzaste el límite del plan · contactá a Anthropic para ampliar". Sin permitir.

## Decisiones del usuario en este flujo

- Qué nombre y descripción dar al producto.
- A quién asignar como Dev Lead inicial (uno o varios).
- Qué template aplicar (estándar / regulado / experimental / manual).
- Qué límite de presupuesto inicial dejar.

## Componentes UI involucrados

- (H1 reusa el componente 14 — Pill de estado — en la sección de productos. Otros componentes son específicos de la pantalla y no aparecen en otras pantallas según doc 04.)

## Notas para el prototipo HTML

- H1 es **opcional para el MVP visual** según la priorización del doc 03. Si se construye, basta con la pantalla simple que ya describe el inventario.
- Estados sugeridos como archivos separados:
  - `h1-provisionar-formulario-vacio.html` (modal/pantalla con el formulario sin completar)
  - `h1-provisionar-formulario-completo.html` (campos completados, template "estándar" seleccionado)
  - `h1-producto-recien-provisionado.html` (vista de H1 con el nuevo producto Inventory en la lista, badge "✅ Recién provisionado")
- El template selector puede ser un radio group simple en el formulario.

## Referencias

- Journey: ROBOK_v5.md §1.5 — Etapa 1 menciona "Encuentra su producto ya creado por el admin de ROBOK · solo tiene que configurarlo" (Marisol provisiona, Vera/Andrés configuran).
- Personas: 01-personas-y-arquetipos.md §Persona 3 (Marisol — su frase: "Yo no uso ROBOK · yo lo gobierno").
- Inventario: 03-inventario-pantallas.md §H1.
- Decisiones cerradas: ROBOK_v5.md §Decisiones · "Productos los provisiona el admin de ROBOK, no los autocrea el dev lead".
- Principios involucrados: P2 (el producto es la unidad de todo · el provisioning es la creación de esa unidad), P4 (gobernanza configurable en capas · Marisol opera en el nivel ROBOK, asigna admins de productos).
