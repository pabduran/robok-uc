# CLAUDE.md — Instrucciones para Claude Code en este repo

Este archivo se carga automáticamente por Claude Code en cada sesión que trabajés en este repo. Su propósito es dar el contexto mínimo necesario para que las sesiones sean productivas y consistentes sin tener que repetir información cada vez.

---

## Qué es este repo

Repo de **diseño y especificación** de ROBOK — una plataforma de desarrollo asistida por IA con squads de agentes. NO es un repo de código de producto. Acá viven los docs de visión, las skills personalizadas, y (próximamente) los casos de uso y el prototipo HTML.

ROBOK no es un chatbot ni un wrapper sobre LLMs. Es un sistema con squads de agentes que trabajan en historias completas, con gates humanos no-negociables y trazabilidad fuerte. El detalle conceptual completo está en `ROBOK_v5.md`.

---

## Fuentes de verdad — leelas en este orden cuando sea relevante

| Archivo | Cuándo leerlo |
|---|---|
| `ROBOK_v5.md` | Siempre que se trabaje sobre la visión funcional, journeys o principios. |
| `01-personas-y-arquetipos.md` | Cuando se mencionen actores. NUNCA usar "el usuario" — siempre Vera, Diego, Marisol o Pablo. |
| `02-glosario-y-modelo-conceptual.md` | Antes de escribir cualquier doc — los términos salen de acá. |
| `03-inventario-pantallas.md` | Antes de referenciar o construir pantallas. Solo existen 18 pantallas (A1–H1). |
| `04-componentes-ui.md` | Antes de construir HTML. Solo existen 15 componentes documentados. |
| `05-guia-casos-de-uso.md` | Antes de escribir casos de uso — la plantilla y los anti-patrones viven acá. |

**Regla operativa:** si necesitás un término, una pantalla o un componente que no está en estos docs, **NO lo inventes**. Detenete, proponé el agregado al usuario, esperá confirmación, agregalo al doc base, y recién después usalo.

---

## Skills disponibles

Tres skills viven en `.claude/skills/`. Activate la que aplique antes de generar contenido:

- **`robok-casos-de-uso`** — para escribir/editar casos de uso. Aplica la plantilla del doc 05.
- **`robok-prototipo-html`** — para construir HTML del prototipo. Fija stack y datos mock.
- **`robok-glosario-guard`** — guardián transversal. Activala proactivamente cuando estés escribiendo cualquier cosa que vaya a quedar en el repo.

---

## Reglas duras (no negociables)

### Sobre vocabulario

- **Términos del glosario (doc 02) o nada.** Anti-glosario aplica: no usar "bot", "workflow" (interno), "run", "log" (cuando sea timeline editorial), "configurar el agente" (cuando sea rol del squad), "aprobar el documento" (cuando sea aprobar el plan), "confirmar" (cuando sea validar conversando).
- **Roles del squad en inglés** con emoji canónico: 🎨 Architect, 🔨 Implementer, 🧪 Tester, 👀 Reviewer, 📝 Doc-writer, 🛡️ Security Reviewer.
- **Roles humanos por nombre propio**: Vera, Diego, Marisol, Pablo. Nunca "el usuario".
- **Modos de costo**: Express, Estándar, Económico, Sprint-pace.

### Sobre alcance V1

- **No mezclar V1 con V2/V3.** Si algo requiere features de V2 (auto-fix automático, embed Slack/Teams, real-time collaboration, mobile, multi-tenant admin), ese contenido se posterga.
- **Desktop-first.** Sin responsive perfecto. Mobile es V2.
- **Sin backend.** El prototipo es 100% estático.

### Sobre los principios del producto

Si describís un comportamiento de ROBOK, respetá estos principios siempre:

- **El humano lidera, los agentes asisten.** Los gates humanos son no-negociables. Rob propone, el humano decide.
- **Inferencia con confirmación humana.** Rob nunca actúa sobre una inferencia propia sin que un humano la valide.
- **El squad espera al humano en checkpoints.** Aunque sea overnight, aunque sea Sprint-pace. Sin excepciones.
- **Demostrar entendimiento, no declararlo.** ROBOK nunca dice "✓ listo" sin mostrar evidencia.
- **Propose-diff-then-approve para tests.** Los tests rotos NO se auto-arreglan — Rob propone diff, humano aprueba explícitamente.
- **Mostrar trabajo, no logs.** El squad se ve como gente trabajando, no como `tail -f`.

---

## Stack del prototipo HTML

Cuando construyas el prototipo:

- **HTML5 estático** — un archivo `.html` por pantalla.
- **Tailwind CSS via CDN** (`<script src="https://cdn.tailwindcss.com"></script>`). Sin build step.
- **Emojis nativos** para iconografía (los del glosario y componentes UI).
- **Vanilla JS si hace falta** para modal/tabs simples. NO React, NO Vue, NO frameworks.
- **Datos hardcodeados.** NADA de fetch, backend, localStorage.
- **Estados como archivos separados:** `f1-squad-trabajando.html`, `f1-squad-esperando.html`, etc. No JS condicionando el render.

Datos mock consistentes en todas las pantallas:

- Producto principal: **Promise Engine** (Python 3.12, FastAPI, Postgres, Redis, K8s)
- Producto secundario: **Ratings & Reviews**
- Equipo: Vera (Dev Lead), Diego, Sofía, Andrés, Mariana, Joaquín, Luna (Developers)
- Stakeholder externo: Pablo (Security Officer)
- Admin de tenant: Marisol
- Tickets: PROM-1234 (principal), PROM-1235, PROM-1233, PROM-892 (histórico), PROM-5678 (bloqueado)

---

## Orden de trabajo recomendado

### Para casos de uso

**Cronológico** (Etapa 1 → 2 → 3 → 4 → 5 → vistas auxiliares). Cada CU se apoya en el contexto del anterior. Total esperado: 35 CU.

### Para prototipo HTML

**Por impacto demostrativo descendente** (no cronológico):

- **Pasada 1**: F1 Squad activo, E3 Muro de discusión, B3 Insight inicial.
- **Pasada 2**: C1 Hub del producto, D1 Backlog enriquecido, F3 Checkpoint modal.
- **Pasada 3**: E1 Confirmación de scope, C3 Mapa, G2 Pre-merge dashboard, F2 Drill-down.
- **Pasada 4**: el resto.

---

## Convenciones del repo

- **Casos de uso**: un archivo por CU en `casos-de-uso/CU-XX-titulo-corto.md`. Mantener `casos-de-uso/00-indice.md` actualizado.
- **Pantallas HTML**: en `prototipo/`, nombre con código de pantalla (`f1-squad-trabajando.html`). Mantener un `prototipo/index.html` como menú navegable.
- **Comentarios al inicio de cada HTML** indicando qué CU resuelve, qué estado representa, qué componentes UI usa.
- **Commits**: en español, claros, una unidad de trabajo por commit (ej. "Agrega CU-04 a CU-07 de Etapa 1").

---

## Si algo no calza

- **¿La instrucción de este CLAUDE.md contradice un doc base?** Los docs base ganan. Avisá al usuario.
- **¿Necesitás escribir algo que no encaja en ningún doc?** Avisá al usuario antes de hacerlo.
- **¿El usuario te pide flexibilizar las reglas?** Respetalo, pero marcá el output como `[draft sin auditar]`.

---

## Lo que NO hay que hacer

- ❌ Escribir código de producto en este repo (eso vive en el repo del MVP, separado).
- ❌ Inventar pantallas, componentes o términos sin agregarlos antes a los docs base.
- ❌ Mezclar el alcance V1 con features V2/V3.
- ❌ Usar "el usuario" en vez de nombres propios.
- ❌ Caer en lenguaje de implementación ("estado se persiste en transacción ACID") cuando se está describiendo UI o flujo de usuario.
- ❌ Construir el prototipo con frameworks pesados o build steps.

---

## Próxima sesión

Si esta es la primera sesión productiva del repo, el siguiente paso lógico es **escribir los casos de uso de Etapa 1 (CU-01 a CU-07)**, en orden cronológico, siguiendo la skill `robok-casos-de-uso`.
