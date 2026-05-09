# CU-04. Vera onboardea un producto recién provisionado: carga contexto y mapea equipo

## Identidad

- **Actor**: Vera (Dev Lead)
- **Pantalla principal**: B1. Onboarding — paso de carga de contexto
- **Pantallas secundarias**: A2. Selector de productos (entrada), B2. Onboarding — indexado en curso (salida).
- **Pre-condición**: Marisol provisionó el producto Promise Engine y le asignó a Vera el rol de Dev Lead. El producto todavía no fue onboardeado — sin fuentes conectadas, sin equipo mapeado.
- **Post-condición**: Las fuentes de contexto del producto (Confluence, repos, IaC, Jira) quedan conectadas. El equipo queda mapeado con sus roles (Vera como Dev Lead, los Developers identificados). ROBOK arranca el indexado y lleva a Vera a B2.
- **Etapa del journey**: Etapa 1 — Onboarding del producto
- **Frecuencia esperada**: una vez por producto (one-time).

## Disparador

Vera entra al producto Promise Engine desde A2 por primera vez. ROBOK detecta que es la primera sesión del producto y la lleva directo al onboarding.

## Flujo principal (happy path)

1. Vera click en la card de Promise Engine en A2. ROBOK detecta que el producto no está onboardeado y muestra B1 — paso de carga de contexto.
2. Vera ve un formulario con cards por **fuente de contexto**: 📚 Confluence, 💻 Repos de código, 🏗️ IaC, 📋 Jira. Algunas vienen preconfiguradas por Marisol (ej. el espacio de Confluence del equipo ya quedó enlazado al provisionar).
3. Para cada fuente todavía sin conectar, Vera click en "Conectar". Se abre la pantalla OAuth del proveedor (Atlassian para Jira/Confluence, GitHub para los repos). Vera completa la autorización.
4. Cada card pasa a estado "✅ Conectada" con un resumen breve ("4 repos detectados", "espacio PROM en Confluence", "proyecto PROM en Jira").
5. En la sección inferior de B1, Vera mapea el **equipo del producto**. ROBOK propone una lista preautocompletada desde el directorio SSO con las personas del grupo de Promise Engine. Vera marca a Diego, Sofía, Andrés, Mariana, Joaquín y Luna como Developers; ella queda con el rol de Dev Lead.
6. Vera click en "Iniciar indexado".
7. ROBOK arranca el indexado, transiciona a B2 — Onboarding indexado en curso (cubierto en CU-05).

## Variantes

### V1. Una fuente no aplica al equipo

Si Promise Engine no usa Confluence (la doc vive en otro lado), Vera marca la card de Confluence como "No usamos esta fuente". ROBOK acepta sin esa fuente y registra el gap, que más tarde aparecerá en el bloque "Cosas que no entendí bien" del insight inicial (CU-06 / CU-07).

### V2. Vera comparte el rol de Dev Lead con otra persona

Vera marca a otro Dev Lead (otro humano del equipo) como co-Dev Lead. La gobernanza por quórum (configurable después) puede requerir aprobaciones cruzadas más adelante.

## Caminos alternativos / errores

- **Si OAuth con un proveedor falla**: la card pasa a "❌ Error de conexión" con un mensaje devuelto por el proveedor + botón "Reintentar". Vera puede seguir con el resto y volver después.
- **Si Vera deja la pantalla a medias**: ROBOK guarda el estado parcial. Cuando Vera vuelva a entrar al producto, B1 retoma desde donde quedó (sin reiniciar conexiones ya hechas).
- **Si Vera no puede mapear a alguien que no aparece en el directorio SSO**: ROBOK ofrece una entrada manual ("Invitar por email") que dispara una invitación al proveedor de identidad.

## Decisiones del usuario en este flujo

- Qué fuentes conectar y cuáles marcar como "no usamos".
- A quién incluir en el equipo del producto y con qué rol.
- Si arrancar el indexado ahora o postergar (B1 acepta cerrar y volver).

## Componentes UI involucrados

- 1. Header con badge ambient (en estado ⚪ "Sin actividad" porque el producto todavía no está operativo)
- 14. Pill de estado (en cards de fuentes y miembros del equipo)

## Notas para el prototipo HTML

- B1 es un formulario en dos secciones: "Fuentes de contexto" arriba y "Equipo del producto" abajo, con un CTA grande "Iniciar indexado" al pie.
- Estados sugeridos como archivos separados:
  - `b1-onboarding-vacio.html` (recién entró, fuentes sin conectar, equipo vacío)
  - `b1-onboarding-fuentes-conectadas.html` (todas las fuentes en ✅)
  - `b1-onboarding-listo.html` (fuentes + equipo, botón "Iniciar indexado" habilitado)
  - `b1-onboarding-error-fuente.html` (V1: una fuente en ❌)
- Los OAuth se pueden simular con un click que cambia el estado de la card.

## Referencias

- Journey: ROBOK_v5.md §1.5 (Etapa 1 — Onboarding del producto, especialmente los pasos B–D del flujo detallado).
- Inventario: 03-inventario-pantallas.md §B1.
- Principios involucrados: P2 (el producto es la unidad de todo — el equipo y las fuentes se mapean dentro del producto), P3 (el humano lidera — Vera decide quién entra y qué fuentes se usan).
