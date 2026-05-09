# CU-06. Vera lee el insight inicial proactivo y lo confirma

## Identidad

- **Actor**: Vera (Dev Lead)
- **Pantalla principal**: B3. Onboarding — Insight inicial proactivo
- **Pantallas secundarias**: B2 (entrada), C1. Vista del producto (salida).
- **Pre-condición**: Indexado terminó en B2. Rob produjo el insight inicial.
- **Post-condición**: Vera valida el insight sin correcciones grandes. El producto queda marcado como listo para operar; ROBOK la lleva a C1.
- **Etapa del journey**: Etapa 1 — Onboarding del producto
- **Frecuencia esperada**: una vez por producto (one-time, en el happy path).

## Disparador

ROBOK transiciona automáticamente desde B2 cuando termina el indexado y muestra B3 con el insight ya generado.

## Flujo principal (happy path)

1. Vera ve B3 — el canvas "Lo que entendí del producto". Rob muestra qué entendió: bloques digeridos con resumen, stack, estructura, convenciones, actividad reciente, "🤔 Cosas que no entendí bien" y equipo mapeado. NO solo dice "listo".
2. Vera lee el resumen en una frase: "Promise Engine es un servicio que calcula y honra promesas de entrega para los clientes de e-commerce."
3. Vera revisa el bloque de stack: Python 3.12, FastAPI, SQLAlchemy, Postgres, Redis, K8s. Coincide con la realidad.
4. Vera revisa la estructura: promise-api, promise-worker, promise-ui, con patrón hexagonal `domain/` + `infra/`. Correcto.
5. Vera revisa convenciones: tests en `tests/` espejo, CI con GitHub Actions <2min, naming snake_case archivos / PascalCase clases. Correcto.
6. Vera revisa "🤔 Cosas que no entendí bien": Rob fue honesto sobre dos dudas: el módulo `legacy/` sin referencias y el repo `promise-tools` que no aparece en CI. Las dudas son razonables y no bloquean arrancar.
7. Vera revisa el equipo: Vera (Dev Lead), Diego, Sofía, Andrés, Mariana, Joaquín, Luna (Developers). Correcto.
8. Vera no necesita corregir nada. Click en "Producto listo".
9. ROBOK lleva a Vera a C1 — Vista del producto Promise Engine. El badge ambient cambia a 🟢 "Sin historias en curso".

## Variantes

### V1. Vera tiene una pregunta menor pero no necesita corregir

Vera quiere chequear un detalle sin tocar el insight (ej. "¿qué versión exacta de Python detectaste?"). Click en "Preguntarle a Rob" → se abre un panel lateral con la conversación (componente 7. Avatar y mensaje del muro). Vera valida preguntando, no haciendo clic en "confirmar". Rob responde inline ("Python 3.12.4 detectado en `pyproject.toml` y reflejado en el Dockerfile"). Vera cierra el panel y aprueba con "Producto listo".

## Caminos alternativos / errores

- **Si Vera intenta aprobar antes de leer "Cosas que no entendí bien"**: ROBOK no la bloquea (el insight es informativo, no un wizard), pero ese bloque queda visible y puede revisarse después desde C1 → Configuración del producto (C4) → Fuentes de contexto.

## Decisiones del usuario en este flujo

- Aprobar de una o preguntar primero a Rob antes de aprobar.
- Aceptar las dudas de "Cosas que no entendí bien" como están o resolverlas (CU-07 cubre el caso de resolverlas).

## Componentes UI involucrados

- 1. Header con badge ambient
- 11. Insight inicial / proactivo (la pantalla)
- 7. Avatar y mensaje del muro de discusión (en variante V1, panel lateral con la pregunta a Rob)

## Notas para el prototipo HTML

- B3 es una de las pantallas más visualmente ricas del onboarding y la que mejor representa el principio "demostrar entendimiento".
- Estados sugeridos como archivos separados:
  - `b3-insight-inicial.html` (recién generado, nada tocado)
  - `b3-insight-conversando.html` (V1: panel lateral abierto con conversación con Rob)
  - `b3-insight-listo.html` (transición a C1)
- Datos mock recomendados: usar Promise Engine (Python 3.12, FastAPI, Postgres, Redis, K8s), repos `promise-api`, `promise-worker`, `promise-ui`, equipo Vera + 6 Developers (los del CLAUDE.md).

## Referencias

- Journey: ROBOK_v5.md §1.5 — Etapa 1, "Anatomía del insight inicial" y "Validación activa, no pasiva".
- Inventario: 03-inventario-pantallas.md §B3.
- Componentes UI: 04-componentes-ui.md §11 (Insight inicial / proactivo).
- Principios involucrados: P5 (demostrar entendimiento, no declararlo — el insight ES el principio materializado), P3 (el humano lidera — Vera valida, Rob no avanza solo).
