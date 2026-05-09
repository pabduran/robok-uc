# ROBOK — Anatomía de componentes UI

> **Propósito de este doc:** describir los componentes de UI clave que aparecen una y otra vez en las pantallas, con suficiente detalle para que el constructor del prototipo HTML los implemente como piezas reutilizables. NO es una librería de componentes con código — es la descripción funcional y visual.
>
> **Regla:** un componente entra acá si aparece en ≥2 pantallas. Si aparece en una sola, vive en la descripción de esa pantalla.

---

## Filosofía visual de ROBOK

Antes de los componentes, una página de filosofía para que el prototipo tenga consistencia.

**ROBOK NO se ve como:**
- Un IDE (VS Code, Cursor) → demasiado código, demasiados paneles, demasiada densidad técnica.
- Un terminal o CI/CD viewer (GitHub Actions, Jenkins) → demasiado log-céntrico.
- Un chatbot (ChatGPT, Claude.ai) → demasiado conversacional, sin estructura.
- Un dashboard tipo Datadog → demasiado métrico, poco humano.

**ROBOK SÍ se ve como:**
- **Linear + Jira + Figma comentarios**: cards, tags, estados visuales, comentarios contextuales.
- **Pantallas con respiración**: espacios en blanco generosos, no densidad de información.
- **Color con propósito**: el color comunica estado (verde / amarillo / rojo / azul para acciones), no decoración.
- **Tipografía clara**: jerarquía fuerte, no flat. El usuario debe saber a qué prestarle atención sin leer todo.

**Paleta sugerida (orientativa):**
- **Fondos**: blancos limpios o gris muy claro `#fafafa`. Modo oscuro opcional.
- **Acción primaria**: azul `#0066ff` o similar (botones, links).
- **Verde activo**: `#22c55e` (squad trabajando, validación OK).
- **Amarillo atención**: `#f59e0b` (esperando humano, atención voluntaria).
- **Rojo bloqueante**: `#ef4444` (bloqueado, fallo, denegado).
- **Morado contexto**: `#8b5cf6` (insights, narración de Rob).
- **Naranja delegación**: `#f97316` (acciones de "delegar", marcas de "para Rob").
- **Grises**: para meta-información, timestamps, secundarios.

---

## Componente 1 — Header con badge ambient

**Propósito:** que el Dev Lead siempre sepa el estado global de su producto sin tener que ir a buscarlo.

**Aparece en:** todas las pantallas internas de un producto.

**Anatomía:**
```
┌──────────────────────────────────────────────────────────────────────────┐
│ ROBOK · Promise Engine ▼          🟢 Todo bien · 3 historias en curso    │
└──────────────────────────────────────────────────────────────────────────┘
```

**Estados del badge ambient:**

| Estado            | Color visual | Texto ejemplo                                 |
| ----------------- | -----------: | --------------------------------------------- |
| Todo bien         | 🟢 verde    | "3 historias en curso"                        |
| Atención voluntaria | 🟡 amarillo | "Squad esperando tu input · PROM-1234"        |
| Algo bloqueado    | 🔴 rojo     | "Squad bloqueado · PROM-5678"                 |
| Sin actividad     | ⚪ gris     | "Sin historias en curso"                      |

**Comportamiento:**
- Hover sobre el badge → tooltip glanceable con resumen de las historias en curso.
- Click sobre el badge → navega a "Mis historias en curso".
- El selector "Promise Engine ▼" permite cambiar de producto sin volver al selector.

---

## Componente 2 — Card de ticket en el backlog enriquecido

**Propósito:** mostrar un ticket anotado por Rob de forma escaneable para que el Dev Lead decida en segundos.

**Aparece en:** pantalla D1 (backlog enriquecido), variante reducida en C1 (landing del producto).

**Anatomía:**
```
┌────────────────────────────────────────────────────────────────────┐
│ ⭐ PROM-1234 · Refactor pricing calculator      [✅ spec OK]      │
│ ─────────────────────────────────────────────────────────────────  │
│ 🏷️ Módulos: pricing, billing-api  · 🔗 depende de PROM-1233       │
│ 📈 Riesgo: pricing tuvo 3 retrabajos en últimos 6 meses           │
│                                                                   │
│ [Express USD 80 · 4h] [Estándar USD 50 · 1d] [Eco USD 25 · 3d]    │
│ [Sprint-pace USD 18 · 2 sem]                                       │
│                                                                   │
│ 💡 Similar a PROM-892 resuelto en mar-2026 ($42 estándar)         │
│                                                                   │
│            [Delegar de una]  [Pausar, profundizar]  [No es Rob]   │
└────────────────────────────────────────────────────────────────────┘
```

**Estados:**

| Estado                             | Visual                                                    |
| ---------------------------------- | --------------------------------------------------------- |
| Pre-marcado para Rob               | ⭐ visible al lado del ticket ID                          |
| Spec OK                            | ✅ chip verde                                              |
| Spec ambigua                       | ⚠️ chip amarillo                                           |
| Spec falta info                    | ❌ chip rojo + Rob no recomienda hasta completar          |
| Riesgo histórico                   | 📈 banner amarillo                                         |
| Tiene similitud histórica          | 💡 sección extra                                           |
| Modo recomendado por Rob           | El pill correspondiente está destacado con borde          |

**Comportamiento:**
- Click en cualquier parte de la card (que no sea botón) → expande a D2 (detalle del ticket).
- Click en pill de modo → preselecciona ese modo si después se delega.
- Click en "Delegar de una" → flujo a E1 (confirmación de scope).

---

## Componente 3 — Pill de modo costo × tiempo

**Propósito:** comunicar de un vistazo el trade-off costo × tiempo de un modo.

**Aparece en:** card de ticket, plan en muro de discusión, modal de modo + quórum, dashboard.

**Anatomía:**
```
┌─────────────────────────┐
│ ⚡ Express              │
│ USD 80 · 4 horas        │
└─────────────────────────┘
```

**Variantes:**
- **Express** ⚡: `#ef4444` borde, "horas"
- **Estándar** 🚀: `#0066ff` borde, "1 día"
- **Económico** 🌱: `#22c55e` borde, "2-3 días"
- **Sprint-pace** ⏳: `#8b5cf6` borde, "1-2 semanas"

**Estados:**
- Default
- Seleccionado (fondo del color)
- Recomendado por Rob (badge "Sugerido por Rob")
- No disponible (gris, ej. si la historia es bloqueante para otra)

---

## Componente 4 — Card de agente del squad

**Propósito:** representar a un agente como "miembro del equipo trabajando", no como proceso.

**Aparece en:** F1 (vista del squad), variante reducida en C2 (mis historias en curso).

**Anatomía:**
```
┌────────────────────────────────────────┐
│ 🔨 Implementer #1                      │
│ ────────────────────────────────────── │
│ 🟢 Trabajando                          │
│                                        │
│ 📁 pricing/calculator.py               │
│ ▓▓▓▓▓▓▓░░░ 70%                        │
│                                        │
│ Hace 30s · escribió 12 líneas          │
└────────────────────────────────────────┘
```

**Roles y emojis (canónicos):**
- 🎨 Architect
- 🔨 Implementer
- 🧪 Tester
- 👀 Reviewer
- 📝 Doc-writer
- 🛡️ Security Reviewer (transversal — siempre visible cuando aparece)

**Estados visuales:**
- 🟢 Trabajando — fondo blanco, borde verde
- 🟡 Esperándote — fondo amarillo claro, borde amarillo, **CTA visible**
- ⚪ Idle — fondo gris claro, borde gris
- 🔴 Error / Bloqueado — fondo rojo claro, borde rojo

**Comportamiento:**
- Click en la card → abre F2 (drill-down lateral).
- Si está esperando al humano → CTA "Resolver" lleva directo al checkpoint (F3).
- Las flechas de handoff entre cards son visibles cuando un agente le pasa algo a otro.

---

## Componente 5 — Status Bar del squad

**Propósito:** estado global del squad siempre visible.

**Aparece en:** F1 (vista del squad).

**Anatomía:**
```
┌─────────────────────────────────────────────────────────────────────────────────┐
│ 🟢 Trabajando · Estándar · 📊 65% · 💵 USD 32 / USD 50 · ⏱️ 2h restantes       │
│                              [Pausar]  [Detener]  [Pedir actualización]         │
└─────────────────────────────────────────────────────────────────────────────────┘
```

**Sub-componentes:**
- **Estado global** (1 chip con emoji + texto)
- **Modo** (pill — reuso del componente 3)
- **Progreso** (barra + %)
- **Costo actual vs presupuesto** (texto con color: verde si <80% del budget, amarillo 80-100%, rojo si >100%)
- **Tiempo estimado restante** (texto)
- **Controles globales** (3 botones)

**Comportamiento:**
- Si el costo se desboca >100%, la barra se pone roja **y aparece un dialog modal** preguntando "¿continuar igual o pausar?". Coherente con la fricción "costo se desboca" del journey.
- "Pausar" → squad pausa pero no termina. Cards de agentes pasan a ⚪.
- "Detener" → confirmación obligatoria. Squad termina, workspace se preserva.

---

## Componente 6 — Timeline editorial

**Propósito:** feed de eventos importantes del squad, NO log de tool calls.

**Aparece en:** F1 (vista del squad), variante condensada en G2 (Pre-merge Gate dashboard).

**Anatomía:**
```
┌──────────────────────────────────────┐
│ Timeline                  [Filtros ▼] │
├──────────────────────────────────────┤
│ 🆕 Novedades desde tu última visita  │
│                                      │
│ 14:32 · 🎨 Architect                 │
│   Terminó el design doc para T-3     │
│   [Ver →]                            │
│                                      │
│ 14:18 · 🔨 Implementer #1            │
│   Abrió PR draft #1234               │
│   [Ver PR →]                         │
│                                      │
│ 13:55 · ⚠️ Implementer #2            │
│   Pidió tu input sobre naming        │
│   [Resolver →]                       │
│                                      │
│ ──────────── Anteriores ────────────│
│                                      │
│ 12:14 · ✅ Tarea T-2 cerrada         │
│ 11:48 · 🧪 Tester en T-1             │
│   23 pasaron, 1 falló — investigando │
└──────────────────────────────────────┘
```

**Tipos de evento (con emoji):**
- 🆕 Inicio de tarea
- ✅ Cierre de tarea
- 📄 Artefacto producido (ADR, PR, doc)
- ⚠️ Atención requerida
- 💬 Mensaje de un agente que el Dev Lead debería leer
- 🔁 Handoff entre agentes
- ❌ Fallo / error
- 🛡️ Alerta de seguridad

**Filtros disponibles:**
- Por agente
- Por tipo de evento
- Por severidad
- "Novedades desde tu última visita" (default)

**Comportamiento:**
- Click en evento → según tipo: abre PR, ADR, archivo, drill-down del agente, etc.
- Eventos antiguos colapsan después de N horas para no saturar.

---

## Componente 7 — Avatar y mensaje del muro de discusión

**Propósito:** que humanos y agentes se vean como participantes igualmente "presentes" en el muro.

**Aparece en:** E3 (muro de discusión), G3 (mini-muro), B3 (validación conversacional del onboarding).

**Anatomía:**
```
─────────────────────────────────────────────────
🤖 Rob · Architect · hace 2 min

Propongo descomponer en 3 tareas:
  1. Crear modelo nuevo en pricing.py
  2. Endpoint POST /v1/quote
  3. Tests de integración

📎 Plan completo · 📎 ADR-2026-014

[Comentar] [Citar parte del plan]
─────────────────────────────────────────────────

👤 Vera · Dev Lead · hace 1 min

Ojo con T-1, en este producto los modelos viven
en `pricing/domain/`, no en `pricing.py`.

[↩️ Citado por Rob]
─────────────────────────────────────────────────

🤖 Rob · Architect · hace 30s

Tenés razón. Ajusto el plan.
✏️ Plan actualizado: T-1 ahora apunta a `pricing/domain/`.

[Ver diff del plan]
─────────────────────────────────────────────────
```

**Tipos de avatar:**
- 👤 humano (con foto si está disponible) + nombre + rol
- 🤖 agente con emoji canónico de su rol (🎨 🔨 🧪 👀 📝 🛡️)
- 🤖 Rob "general" (cuando no es ningún rol específico, ej. el Architect del onboarding)

**Acciones del mensaje:**
- Comentar (responde en hilo)
- Citar parte del plan (linkea una sección específica)
- Reaccionar (👍 ❌ ❓) — opcional para v1

**Tipos de mensaje:**
- Mensaje normal (texto + opcional adjuntos)
- Mensaje con cambio de plan (banner "Plan actualizado" + diff)
- Mensaje con propuesta de decisión (banner + botones de acción)
- Mensaje de Security Reviewer (borde rojo o badge "🛡️ Seguridad")
- Mensaje del sistema ("Vera invitó a Pablo al muro", "Plan congelado para aprobación")

---

## Componente 8 — Diagrama del mapa de componentes

**Propósito:** vista permanente del producto y herramienta para acotar scope.

**Aparece en:** C3 (mapa standalone), E1 (confirmación de scope), preview en C1.

**Anatomía (representación textual):**
```
┌─────────────────────────────────────────────────────────────┐
│ Mapa · Promise Engine               [v actual ▼] [Filtros ▼]│
├─────────────────────────────────────────────────────────────┤
│                                                             │
│      [promise-ui]                                           │
│         │                                                   │
│         ▼                                                   │
│      [promise-api] ──────► [billing-api]                    │
│         │                       │                           │
│         ▼                       ▼                           │
│      [promise-worker]      [postgres]                       │
│         │                                                   │
│         ▼                                                   │
│      [redis]                                                │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

**Tipos de nodo:**
- 📦 Repo (cuadrado azul)
- ⚙️ Servicio (rectángulo verde)
- 🗄️ Storage (cilindro)
- ☁️ Cloud component (forma de nube)
- 🔗 External dependency (línea punteada)

**Modos:**
- **Modo lectura** (default): hover muestra detalle, click abre panel lateral.
- **Modo selección** (cuando E1 lo invoca): nodos clickeables, los seleccionados se highlightean, contador "3 componentes en scope".
- **Modo versión antigua**: banner amarillo "Viendo snapshot del 14 abr 2026 · [Volver al actual]".

**Comportamiento para el prototipo:**
- No hace falta render dinámico — un SVG estático con nodos clickeables alcanza.
- Para versiones, un dropdown con 3 fechas mock.

---

## Componente 9 — Pantalla de checkpoint (modal bloqueante)

**Propósito:** materializar el principio "el squad espera al humano".

**Aparece en:** F3 (pantalla de checkpoint estándar), variante en G3 mini-muro.

**Anatomía:**
```
┌─────────────────────────────────────────────────────────────┐
│ ⏸️ Squad esperándote                              [Cerrar ✕] │
├─────────────────────────────────────────────────────────────┤
│ Checkpoint: Test arreglado, requiere tu aprobación          │
│                                                             │
│ Lo que pasó:                                                │
│ El Tester detectó que test_pricing_with_promo fallaba       │
│ después del cambio en T-2. El Implementer #2 propuso un     │
│ ajuste al test.                                             │
│                                                             │
│ Diff propuesto:                                             │
│ ┌─────────────────────────────────────────────────────────┐ │
│ │ - assert calculate(100, "PROMO20") == 80               │ │
│ │ + assert calculate(100, "PROMO20") == 75               │ │
│ │   # ahora aplica también fee de 5% post-descuento     │ │
│ └─────────────────────────────────────────────────────────┘ │
│                                                             │
│ ⚠️ Importante: aprobá solo si el cambio refleja la lógica    │
│    correcta, no si "solo querés que pase verde".            │
│                                                             │
│         [Aprobar]   [Pedir cambios]   [Detener squad]       │
└─────────────────────────────────────────────────────────────┘
```

**Variantes:**
- **Estándar**: descripción + propuesta + botones.
- **Propose-diff-then-approve** (test, fix de seguridad): incluye diff destacado con copy preventivo "no aprobar solo para que pase verde".
- **Quórum extra**: incluye lista de aprobadores requeridos y a quién falta.

**Comportamiento:**
- Es modal bloqueante — el Dev Lead no puede esquivarlo desde el cierre. Sí puede cerrar (✕) y volver más tarde, pero el squad sigue parado.
- Si el Dev Lead no responde en N horas, ROBOK manda recordatorio (otro componente).

---

## Componente 10 — Notificación interrupting

**Propósito:** llamar la atención del Dev Lead cuando un checkpoint requiere acción.

**Aparece como:** overlay en cualquier pantalla del producto + notificación en Slack/Teams (con link).

**Anatomía visual (ROBOK):**
```
┌─────────────────────────────────────────────────────────────┐
│ 🟡 Squad esperándote · PROM-1234                            │
│ Implementer pidió tu input sobre el naming del endpoint      │
│                              [Ver checkpoint]   [Más tarde] │
└─────────────────────────────────────────────────────────────┘
```

**Comportamiento:**
- Aparece en la esquina superior derecha (toast persistente, no auto-dismiss).
- Click en "Ver checkpoint" → abre F3.
- Click en "Más tarde" → minimiza a badge en el header.
- Si el checkpoint queda sin respuesta más de N horas, escala (otra notificación, eventualmente cambio de estado del producto a 🟡 en el badge ambient).

**Notificación Slack/Teams (V1):**
- Texto descriptivo + link a F3.
- **NO permite aprobar desde Slack** (eso es V2).

---

## Componente 11 — Insight inicial / proactivo

**Propósito:** visualizar "lo que ROBOK entendió" — el principio "demostrar entendimiento, no declararlo".

**Aparece en:** B3 (insight del onboarding), variantes acotadas en E2 (análisis profundo) y E3 (muro).

**Anatomía:**
```
┌─────────────────────────────────────────────────────────────────┐
│ 💡 Lo que entendí del producto                                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│ Promise Engine es un servicio que calcula y honra promesas      │
│ de entrega para los clientes de e-commerce.                     │
│                                                                 │
│ ── Stack ─────────────────────────────────────────────────────  │
│ Python 3.12 · FastAPI · SQLAlchemy · Postgres · Redis · K8s     │
│                                                                 │
│ ── Estructura ─────────────────────────────────────────────────  │
│ 📦 promise-api (servicio principal)                             │
│ 📦 promise-worker (cola de cálculo async)                       │
│ 📦 promise-ui (frontend interno de monitoreo)                   │
│ Patrón observado: hexagonal con domain/ y infra/                │
│                                                                 │
│ ── Convenciones detectadas ────────────────────────────────────  │
│ • Tests viven en `tests/` espejo del código                     │
│ • CI corre con GitHub Actions, suite completa <2min             │
│ • Naming: snake_case archivos, PascalCase clases                │
│                                                                 │
│ ── Actividad reciente ─────────────────────────────────────────  │
│ Últimos 14 días: 47 commits · 12 PRs mergeados · 3 hotfixes     │
│                                                                 │
│ ── 🤔 Cosas que NO entendí bien ───────────────────────────────  │
│ • Hay un módulo `legacy/` que no se referencia desde nadie.     │
│   ¿Está deprecado? ¿Hay tickets para borrarlo?                  │
│ • El repo `promise-tools` parece interno pero no aparece en     │
│   los CI workflows. ¿Es de uso manual?                          │
│                                                                 │
│ ── Equipo mapeado ─────────────────────────────────────────────  │
│ 👤 Vera (Dev Lead, vos)                                         │
│ 👤 Diego, Sofía, Andrés, Mariana, Joaquín, Luna (Developers)    │
│                                                                 │
│              ¿Algo de esto está mal o falta?                    │
│                                                                 │
│   [Corregir inline]   [Preguntarle a Rob]   [Producto listo]    │
└─────────────────────────────────────────────────────────────────┘
```

**Características clave:**
- **Honesto sobre lo que NO entendió** — el bloque "🤔 Cosas que NO entendí bien" es no negociable.
- **CTA invitacional** — la pregunta "¿algo de esto está mal o falta?" abre el espacio a corrección.
- **Tres acciones**: corregir inline (edita campos), preguntarle a Rob (abre conversación), producto listo (avanza).

---

## Componente 12 — Validaciones automatizadas (lista de checks)

**Propósito:** mostrar el estado de los checks pre-PR.

**Aparece en:** G2 (Pre-merge Gate dashboard).

**Anatomía:**
```
┌─────────────────────────────────────────────────┐
│ Validaciones automatizadas                       │
├─────────────────────────────────────────────────┤
│ ✅ Unit tests              142 pasaron, 0 fallos │
│ ✅ Lint                    sin warnings          │
│ ✅ Type check              sin errores           │
│ ✅ SAST (Semgrep)          0 hallazgos           │
│ ✅ Gitleaks                0 secretos            │
│ ⚠️ Dependency scan         1 CVE menor [Ver]     │
│                                                 │
│ Total: 5/6 OK · 1 advertencia (no bloqueante)    │
└─────────────────────────────────────────────────┘
```

**Estados por check:**
- ⏳ En curso
- ✅ Pasó
- ⚠️ Pasó con advertencia
- ❌ Falló (bloqueante)

**Comportamiento:**
- Click en un check → muestra detalle en panel lateral.
- Si hay falla bloqueante, banner rojo "PR draft no se puede abrir hasta resolver" + acción.

---

## Componente 13 — Plan de pruebas manuales (espejo de Jira)

**Propósito:** ver dentro de ROBOK lo que está en Jira sin saltar a Jira.

**Aparece en:** G2 (Pre-merge Gate dashboard).

**Anatomía:**
```
┌─────────────────────────────────────────────────────────────┐
│ Plan de pruebas manuales · PROM-1234   [Ver en Jira ↗]      │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│ ✅ 1. Calcular promesa con cliente nuevo (Sofía · ayer)     │
│ ✅ 2. Calcular promesa con código de descuento (Diego · 2h) │
│ ❌ 3. Cancelar promesa pendiente (Diego · hace 30 min)      │
│      Falla observada — [Ver mini-muro →]                    │
│ ⏸️ 4. Validar timeout en cola Redis (pendiente)             │
│ N/A 5. Compatibilidad navegador antiguo (no aplica)         │
│                                                             │
│ Estado: 2/5 pasados · 1 falló · 1 pendiente · 1 N/A          │
└─────────────────────────────────────────────────────────────┘
```

**Estados por item:**
- ⏸️ Pendiente
- ✅ Pasó
- ❌ Falló (abre mini-muro)
- N/A No aplica

**Comportamiento:**
- ROBOK lee y refleja el estado desde Jira (espejo).
- Click en un item → ve el reporte estructurado del humano (pasos, screenshot, expected vs actual).
- Si falló → CTA al mini-muro de discusión (G3).

---

## Componente 14 — Pill de estado

**Propósito:** comunicar estado de cualquier entidad (historia, tarea, validación) de forma escaneable.

**Aparece en:** todas las pantallas con listas.

**Anatomía:**
```
[🟢 Trabajando]  [🟡 Esperándote]  [🔴 Bloqueado]  [✅ Completo]
[⏸️ En pausa]    [📝 En análisis]   [🎨 En diseño]   [🛑 Cancelado]
[📋 En sprint]   [⭐ Para Rob]      [🔁 En review]   [🚀 Mergeado]
```

**Reglas:**
- Forma siempre pill (rounded), padding consistente.
- Color del fondo: claro del color del estado (no saturado).
- Color del texto: oscuro del mismo color.
- Emoji a la izquierda, texto a la derecha.
- **Un solo emoji por estado** — no cambiar entre pantallas.

---

## Componente 15 — Selector de modo + quórum (modal)

**Propósito:** materializar la decisión final de delegación.

**Aparece en:** E4 (al apretar "Dale" en E3).

**Anatomía:**
```
┌─────────────────────────────────────────────────────────────┐
│ Aprobar plan · PROM-1234                       [Cerrar ✕]   │
├─────────────────────────────────────────────────────────────┤
│ Elegí el modo de ejecución                                  │
│                                                             │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌────┐  │
│  │ ⚡ Express  │  │ 🚀 Estándar │  │ 🌱 Económico│  │⏳..│  │
│  │ USD 80      │  │ USD 50  ✓   │  │ USD 25      │  │USD │  │
│  │ 4 horas     │  │ 1 día       │  │ 2-3 días    │  │18  │  │
│  └─────────────┘  └─────────────┘  └─────────────┘  └────┘  │
│                                                             │
│ Quórum requerido para esta historia                         │
│ ─────────────────────────────────────────────────────────── │
│ 💡 Esta historia toca el módulo de auth — requiere también │
│    aprobación de un Security Officer.                       │
│                                                             │
│ Aprobadores actuales:                                       │
│  ✅ Vera (vos · Dev Lead)                                    │
│  ⏳ Pablo (Security Officer · invitado al muro)             │
│                                                             │
│                              [Cancelar]   [Aprobar plan]    │
└─────────────────────────────────────────────────────────────┘
```

**Comportamiento:**
- Selección de modo es obligatoria.
- Si quórum requiere otro humano → aprobación queda "en espera" hasta que ese humano valide.
- Botón "Aprobar plan" cambia de texto a "Esperar quórum" si aplica.

---

## Componentes opcionales (V2+)

Estos los menciono solo para que el constructor del prototipo no los inventee. **NO van en el prototipo V1.**

- Botones de aprobación dentro de Slack/Teams (V2 — solo lo es notificación + link en V1)
- Cursor en vivo en el muro a la Figma (V2)
- Vista mobile responsive (no V1)
- Embed del muro en otras herramientas (V2)
- Vista de "Skills marketplace" para configurar agentes con skills custom (V2+)

---

## Tabla resumen — qué componente aparece en qué pantalla

| Componente                                  | A2 | B3 | C1 | C2 | C3 | D1 | D2 | E1 | E2 | E3 | E4 | F1 | F2 | F3 | G1 | G2 | G3 |
| ------------------------------------------- | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| 1. Header con badge ambient                 |    |    | ●  | ●  | ●  | ●  | ●  | ●  | ●  | ●  |    | ●  | ●  |    | ●  | ●  |    |
| 2. Card de ticket en el backlog enriquecido |    |    | ◐  |    |    | ●  |    |    |    |    |    |    |    |    |    |    |    |
| 3. Pill de modo costo × tiempo              |    |    |    | ●  |    | ●  | ●  |    |    | ●  | ●  |    |    |    |    |    |    |
| 4. Card de agente del squad                 |    |    | ◐  | ◐  |    |    |    |    |    |    |    | ●  | ●  |    |    |    |    |
| 5. Status Bar del squad                     |    |    |    |    |    |    |    |    |    |    |    | ●  |    |    |    |    |    |
| 6. Timeline editorial                       |    |    |    |    |    |    |    |    |    |    |    | ●  |    |    |    | ◐  |    |
| 7. Avatar y mensaje del muro                |    | ●  |    |    |    |    |    |    |    | ●  |    |    |    |    |    |    | ●  |
| 8. Mapa de componentes (diagrama)           |    |    | ◐  |    | ●  |    |    | ●  |    | ◐  |    |    |    |    |    |    |    |
| 9. Pantalla de checkpoint (modal)           |    |    |    |    |    |    |    |    |    |    |    |    |    | ●  |    |    |    |
| 10. Notificación interrupting               |    |    | ◐  | ◐  | ◐  | ◐  | ◐  | ◐  | ◐  | ◐  |    | ◐  | ◐  |    | ◐  | ◐  |    |
| 11. Insight inicial / proactivo             |    | ●  |    |    |    |    |    |    | ◐  |    |    |    |    |    |    |    |    |
| 12. Validaciones automatizadas              |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    | ●  |    |
| 13. Plan de pruebas manuales (espejo Jira)  |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    | ●  |    |
| 14. Pill de estado                          | ●  |    | ●  | ●  |    | ●  | ●  |    |    | ●  | ●  | ●  |    |    | ●  | ●  | ●  |
| 15. Selector de modo + quórum (modal)       |    |    |    |    |    |    |    |    |    |    | ●  |    |    |    |    |    |    |

Leyenda: ● principal · ◐ variante reducida o preview

---

## Recomendaciones para implementar el prototipo HTML

**Stack sugerido:**
- HTML5 + Tailwind CSS (CDN, sin build step)
- Iconografía: emojis nativos + Lucide o Heroicons opcionalmente
- Sin frameworks pesados — vanilla JS si se necesita interactividad simple
- Mockear navegación con archivos `.html` independientes y links entre sí
- Datos hardcodeados en el HTML, sin fetch real

**Patrones de implementación:**
- Cada pantalla = un archivo `.html`.
- Componentes que se repiten = bloques de HTML copiados (en el prototipo el DRY no es prioridad — la legibilidad sí).
- Estados se simulan con archivos separados (ej. `f1-squad-trabajando.html`, `f1-squad-esperando.html`, `f1-squad-bloqueado.html`).
- Interactividad mínima: clicks navegan a otro archivo, hovers son CSS, modales con `<dialog>` o JS simple.

**Lo que NO hace falta en el prototipo:**
- Backend
- Auth real (basta con un botón "Entrar como Vera" en login)
- Datos vivos (todo hardcodeado)
- Responsive perfecto (desktop-first es suficiente)
- Animaciones complejas (transiciones CSS estándar alcanzan)

**Lo que SÍ vale invertir:**
- Que la pantalla F1 (Squad activo) se vea increíble — es la metáfora central
- Que la pantalla E3 (Muro de discusión) se sienta como un espacio compartido, no un chat
- Que la pantalla B3 (Insight inicial) demuestre el principio "mostrar trabajo"
- Que los pills de estado y de modo sean consistentes en todas las pantallas
