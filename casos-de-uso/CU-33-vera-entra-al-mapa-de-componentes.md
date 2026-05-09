# CU-33. Vera entra al mapa de componentes para entender el sistema antes de planning

## Identidad

- **Actor**: Vera (Dev Lead)
- **Pantalla principal**: C3. Mapa de componentes (modo lectura)
- **Pantallas secundarias**: C1. Vista del producto (entrada típica), eventualmente E1. Confirmación de scope (uso del mismo mapa en modo selección).
- **Pre-condición**: El producto Promise Engine está onboardeado y el mapa fue generado durante el indexado. Vera necesita orientarse en el sistema antes de un planning, antes de delegar una historia compleja, o para chequear cómo se conectan dos servicios.
- **Post-condición**: Vera entiende mejor el sistema o respondió la pregunta puntual que tenía. Cierra el mapa o sigue navegando.
- **Etapa del journey**: Vista auxiliar (transversal — no es parte de un journey específico, se usa antes/durante varias etapas)
- **Frecuencia esperada**: 1–3 veces por semana, según necesidad.

## Disparador

Vera tiene una pregunta sobre el sistema: ¿qué servicios consumen Postgres? ¿el módulo de pricing depende del worker? ¿qué cambió en la infra desde que entré la última vez? Click en "Mapa" desde el header con badge ambient o desde la sección "Mapa del producto" en C1.

## Flujo principal (happy path)

1. Vera click en "Mapa" desde el header (componente 1) o desde C1.
2. ROBOK lleva a C3 — Mapa de componentes en modo lectura. La pantalla muestra:
   - **Diagrama interactivo** del producto: nodos que representan repos, servicios, storage, cloud components, dependencias externas. Con tipos visuales distintos (📦 Repo cuadrado, ⚙️ Servicio rectángulo verde, 🗄️ Storage cilindro, ☁️ Cloud component nube, 🔗 External dependency línea punteada).
   - Para Promise Engine: `promise-ui` → `promise-api` → (`billing-api` + `promise-worker`) → (`postgres` + `redis`).
   - **Banner superior**: "Mapa · Promise Engine · v actual" con dropdown para versiones antiguas (cubierto en CU-34).
   - **Filtros**: por tipo de nodo, por módulo, por antigüedad de cambios.
3. Vera ve el mapa completo. Quiere saber qué servicios usan Postgres. Click en el nodo `postgres`.
4. ROBOK abre un panel lateral con detalle del nodo:
   - Nombre, tipo (🗄️ Storage), descripción.
   - Quiénes lo consumen: `promise-api`, `billing-api`.
   - Quiénes definen su esquema: `promise-api` (owner del schema migrations).
   - ADRs relevantes: ADR-2025-088 (estrategia de migrations), ADR-2026-014 (uso de Decimal para precisión monetaria).
   - Última actividad: "schema migration hace 3 semanas".
5. Vera lee. Tiene la respuesta. Cierra el panel y vuelve al mapa completo.
6. Vera cierra C3 y vuelve a C1 (o se va a otra pantalla).

## Variantes

### V1. Vera filtra el mapa para enfocarse

Vera quiere ver solo los servicios (no la infra). Click en el filtro "Tipo de nodo · ⚙️ Servicios". El mapa oculta los demás. Cuando termina, click en "Reset filtros" o cerrar el panel del filtro.

### V2. Vera explora un módulo específico

Vera está pensando si delegar a Rob una historia que toca `promise-worker`. Click en `promise-worker`. Panel lateral muestra: descripción, tareas asíncronas que ejecuta, dependencias (consume `redis`), historial de cambios reciente, ADRs propios. Vera entiende mejor antes de decidir delegar.

### V3. El mapa cambió y Vera lo nota

Vera ve un nodo nuevo (`promise-cache`) que no recordaba. Click → panel lateral con descripción "agregado hace 2 semanas, owner: @sofia". Vera puede pedir a Rob un refresh sobre ese nodo si quiere más contexto, o ir al ADR que justificó el cambio.

## Caminos alternativos / errores

- **Si el mapa no se renderiza** (caso raro de error de datos): C3 muestra "No pude renderizar el mapa · [Reintentar]". El resto del producto sigue funcional.
- **Si un nodo no tiene datos suficientes** (ej. un repo que el indexado no logró parsear bien): el panel lateral muestra "Información parcial — Rob no entendió bien este componente. ¿Querés pedir un re-indexado?". Coherente con "Cosas que no entendí bien" del insight inicial (B3).

## Decisiones del usuario en este flujo

- Filtrar o ver el mapa completo.
- Explorar un nodo en profundidad o solo escanear el grafo.
- Si el mapa parece desactualizado: pedir refresh on-demand.

## Componentes UI involucrados

- 1. Header con badge ambient
- 8. Mapa de componentes (modo lectura — la pieza central)

## Notas para el prototipo HTML

- C3 es el mapa standalone. Para el prototipo, no hace falta render dinámico real — un SVG estático con nodos clickeables es suficiente, o una imagen anotada.
- Estados sugeridos como archivos separados:
  - `c3-mapa-completo.html` (todos los nodos visibles)
  - `c3-mapa-filtrado-servicios.html` (V1: solo servicios)
  - `c3-mapa-nodo-seleccionado.html` (panel lateral abierto sobre `postgres`)
  - `c3-mapa-con-nodo-parcial.html` (un nodo con info parcial, banner amarillo)

## Referencias

- Journey: ROBOK_v5.md §1.7 — Etapa 3 menciona el mapa como vista permanente del producto, doble uso (entender el sistema + acotar scope).
- Inventario: 03-inventario-pantallas.md §C3.
- Componentes UI: 04-componentes-ui.md §8 (mapa de componentes).
- Principios involucrados: P5 (mostrar trabajo · el mapa es la representación visual del entendimiento de Rob sobre el producto), P7 (trabajo informado por contexto fresco · el mapa se actualiza con el indexado).
