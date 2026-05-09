# CU-20. Vera ve el squad arrancar y se familiariza con la pantalla F1

## Identidad

- **Actor**: Vera (Dev Lead)
- **Pantalla principal**: F1. Vista del squad activo
- **Pantallas secundarias**: E4 (entrada — el squad arranca tras la aprobación), C2. Mis historias en curso (alternativa de entrada).
- **Pre-condición**: Plan aprobado en E4 (CU-17). Squad recién spawneado: cada agente tiene su workspace aislado (git worktree o equivalente). Las cards de los agentes están apareciendo.
- **Post-condición**: Vera entiende el estado del squad: ve los agentes activos, lee la timeline editorial, entiende el status bar. Cierra F1 confiada o se queda mirando.
- **Etapa del journey**: Etapa 4 — Implementación con squad
- **Frecuencia esperada**: una vez por historia delegada que se aprobó (la primera vez que entra al squad después de aprobar; las visitas siguientes son más rápidas).

## Disparador

Vera apretó "Aprobar plan" en E4 y el quórum se cumplió. ROBOK la lleva automáticamente a F1 — Vista del squad activo. Alternativamente, Vera entra desde C2 días después, cuando vuelve a la sesión y el squad ya estuvo trabajando en background.

## Flujo principal (happy path)

1. Vera ve F1 con sus tres zonas:
   - **Zona 1 — Status Bar (top, siempre visible)**: 🟢 Trabajando · 🚀 Estándar · 📊 5% · 💵 USD 3 / USD 55 · ⏱️ ~6h restantes · controles globales [Pausar] [Detener] [Pedir actualización].
   - **Zona 2 — Equipo en acción (centro)**: cards de cada agente activo. En este momento ve 3 cards:
     - 🎨 **Architect** — 🟢 Trabajando · "Refinando descomposición de T-1" · 0:30s · "borroneó los signatures de las funciones".
     - 🔨 **Implementer #1** — ⚪ Idle · "Esperando handoff del Architect en T-1".
     - 🛡️ **Security Reviewer** — ⚪ Idle · "Observando · entrará en checkpoint de T-2".
   - **Zona 3 — Timeline editorial (derecha, scrollable)**: feed con los primeros eventos:
     - "🆕 Squad arrancado · 5 agentes asignados · modo Estándar"
     - "🎨 Architect tomó T-1 (modelo nuevo en `pricing/domain/`)"
     - "📄 Plan congelado en snapshot · ver ADRs"
   - Filtro "Novedades desde tu última visita" activo por defecto.
2. Vera ve la **caja de double-texting** en el lateral o abajo: "Mensaje al squad o a un agente específico" (cubierto en CU-24).
3. Vera entiende el estado: el Architect está arrancando, el Implementer espera, el Security Reviewer está observando. Sin sorpresas.
4. Vera no necesita actuar. Cierra F1 y se va a su 1:1, confiada en que ROBOK le va a avisar (notificación interrupting) cuando llegue un checkpoint que requiera su atención.

## Variantes

### V1. Vera vuelve después de horas y usa "Novedades desde tu última visita"

Vera vuelve 4 horas después. Entra a C2 → Promise Engine · PROM-1234 → F1. La timeline tiene el filtro "Novedades desde tu última visita" activo y muestra solo lo que pasó después de su última sesión (ej. "✅ T-1 cerrada", "🆕 T-2 iniciada por 🔨 Implementer #1", "📄 ADR-2026-014 publicado"). Vera se reorienta en segundos. Re-entry cost minimizado.

### V2. Vera ve el squad en pleno trabajo paralelo

Más adelante, Vera entra a F1 y ve 5 cards activas: 🎨 Architect terminó (✅), 2 🔨 Implementers trabajando en paralelo en T-2 y T-3, 🧪 Tester validando T-1 ya cerrada, 👀 Reviewer revisando código. Flechas entre cards muestran handoffs en curso. Sin orchestrator visible — la coordinación es directa.

### V3. Squad con Orchestrator (jerarquía)

Cuando ROBOK detecta que el squad necesita coordinación entre varios agentes, activa un Orchestrator automáticamente. F1 lo muestra como una card jerárquicamente superior con flechas a los agentes que coordina. Vera puede hacer drill-down (CU-21) para ver cómo el Orchestrator decide.

## Caminos alternativos / errores

- **Si el squad no arranca por error de provisioning** (ej. workspace no se pudo crear): F1 muestra estado 🔴 Bloqueado en el status bar y un mensaje "No pude crear workspace para Implementer #2 — ver detalle". Vera puede reintentar desde el status bar.
- **Si no hay actividad nueva desde la última visita**: la timeline muestra "Sin novedades desde tu última visita · ver actividad anterior" + las novedades anteriores quedan accesibles con un click.

## Decisiones del usuario en este flujo

- Quedarse mirando o cerrar y volver más tarde.
- Si quiere profundizar: hacer drill-down (CU-21) en algún agente o leer la timeline.
- Si algo no calza: usar double-texting (CU-24), pausar el squad, o detener.

## Componentes UI involucrados

- 1. Header con badge ambient (en estado 🟢 "Squad activo · PROM-1234" mientras el squad trabaja)
- 5. Status Bar del squad (top de F1)
- 4. Card de agente del squad (zona central, una por agente)
- 6. Timeline editorial (zona derecha)
- 14. Pill de estado (en el status bar y en cada card)

## Notas para el prototipo HTML

- F1 es la pantalla más importante de TODO ROBOK (priorización Pasada 1 según doc 03). Vale invertir todo lo posible.
- Layout: status bar fija arriba, columna central con cards de agentes (grid 1–3 columnas según ancho), columna derecha con timeline.
- Estados sugeridos como archivos separados:
  - `f1-squad-arrancando.html` (estado inicial: 3 agentes, Architect trabajando)
  - `f1-squad-pleno-trabajo.html` (V2: 5 agentes en paralelo)
  - `f1-squad-con-orchestrator.html` (V3: jerarquía visible)
  - `f1-squad-bloqueado.html` (error de provisioning, status bar 🔴)
  - `f1-novedades-desde-ultima-visita.html` (V1: timeline filtrada)

## Referencias

- Journey: ROBOK_v5.md §1.8 — Etapa 4, "Anatomía de la pantalla del Squad" (Zonas 1, 2, 3) y "Patrones clave que diferencian a ROBOK".
- Inventario: 03-inventario-pantallas.md §F1.
- Componentes UI: 04-componentes-ui.md §4 (card de agente), §5 (status bar), §6 (timeline editorial).
- Principios involucrados: P5 (mostrar trabajo, no logs — la timeline es editorial, no `tail -f`), P6 (presencia adaptativa — Vera elige cuánto involucrarse), P3 (squad espera al humano — el badge ambient cambia cuando hay un checkpoint).
