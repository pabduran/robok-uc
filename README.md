# ROBOK — Repo de visión y diseño

> Laboratorio de diseño para **ROBOK v5** — la plataforma de desarrollo asistida por IA que se describe en `ROBOK_v5.md`. Acá viven los docs base, las skills para Claude Code, y (próximamente) los casos de uso y el prototipo HTML.

---

## Qué es este repo

Este repo NO es código. Es un repo de **diseño y especificación** que sirve para:

1. Madurar la visión de ROBOK antes de implementarla.
2. Producir casos de uso bien formados desde los journeys.
3. Construir un prototipo HTML estático para validar el flujo visualmente (estilo Figma pero en HTML, listo para reutilizar después).
4. Mantener consistencia entre sesiones de Claude Code mediante skills personalizadas.

**Repo separado del código.** El repo de código del MVP actual (agente de Slack para registro arquitectónico, hexagonal pragmático) vive aparte. Esa decisión es deliberada — protege contra contaminar el código existente con ideas todavía en validación, y protege al diseño de las restricciones del código existente. Cuando converjan, será con evidencia.

---

## Estructura del repo

```
.
├── README.md                          ← este archivo
├── CLAUDE.md                          ← instrucciones para Claude Code en cada sesión
├── ROBOK_v5.md                        ← fuente de verdad funcional (los journeys)
│
├── 01-personas-y-arquetipos.md        ← Vera, Diego, Marisol, Pablo
├── 02-glosario-y-modelo-conceptual.md ← términos canónicos + anti-glosario
├── 03-inventario-pantallas.md         ← las 18 pantallas de V1
├── 04-componentes-ui.md               ← los 15 componentes UI
├── 05-guia-casos-de-uso.md            ← plantilla y guion para los 35 CU
│
├── .claude/
│   └── skills/                        ← skills personalizadas para Claude Code
│       ├── robok-casos-de-uso/SKILL.md
│       ├── robok-prototipo-html/SKILL.md
│       └── robok-glosario-guard/SKILL.md
│
├── casos-de-uso/                      ← (vacío por ahora) los 35 CU van acá
│
└── prototipo/                         ← (vacío por ahora) las pantallas HTML van acá
```

---

## Estado actual

| Bloque | Estado |
|---|---|
| `ROBOK_v5.md` (visión y journeys) | ✅ Estable |
| Docs base 01–05 | ✅ Drafts iniciales, listos para iterar |
| Skills para Claude Code | ✅ Instaladas en `.claude/skills/` |
| Casos de uso (35 esperados) | ⏳ Pendientes |
| Prototipo HTML (18 pantallas) | ⏳ Pendiente |
| Validación con usuarios reales | ⏳ Pendiente |

---

## Los cinco documentos base

| Doc | Para qué sirve |
|---|---|
| **[01 — Personas y arquetipos](./01-personas-y-arquetipos.md)** | Define a Vera (Dev Lead), Diego (Developer), Marisol (Admin de ROBOK) y Pablo (Stakeholder externo). Sin esto, los casos de uso quedan en "el usuario hace X". |
| **[02 — Glosario y modelo conceptual](./02-glosario-y-modelo-conceptual.md)** | Términos canónicos. Anti-glosario. Reglas de naming en pantalla. Diagrama ER. |
| **[03 — Inventario de pantallas](./03-inventario-pantallas.md)** | Las 18 pantallas que existen en V1. Mapa de navegación. Priorización para el prototipo en 4 pasadas. |
| **[04 — Componentes UI](./04-componentes-ui.md)** | Los 15 componentes con anatomía visual, estados, comportamiento. Stack del prototipo (HTML + Tailwind CDN). |
| **[05 — Guía para construir casos de uso](./05-guia-casos-de-uso.md)** | Plantilla de CU + lista de los 35 esperados + anti-patrones. Es el guion de trabajo. |

---

## Las tres skills de Claude Code

Las skills viven en `.claude/skills/` y se commitean junto al repo. Claude Code las detecta automáticamente cuando el pedido coincide con sus triggers.

| Skill | Trigger | Qué hace |
|---|---|---|
| **robok-casos-de-uso** | Pedir escribir/revisar/modificar casos de uso | Aplica la plantilla del doc 05, valida contra glosario y inventario, evita anti-patrones |
| **robok-prototipo-html** | Pedir construir/modificar pantallas HTML | Fija stack (HTML + Tailwind CDN), una pantalla = un archivo, datos mock consistentes |
| **robok-glosario-guard** | Cualquier escritura de docs ROBOK | Guardián transversal contra deriva semántica entre sesiones |

---

## Cómo trabajar acá

### Si vas a escribir casos de uso

1. Leé `ROBOK_v5.md` completo.
2. Leé los docs 01 → 02 → 03 → 04 en ese orden.
3. Usá el doc 05 como guion.
4. **Escribí los CU en orden cronológico** (Etapa 1 → 2 → 3 → 4 → 5 → vistas auxiliares). Esto evita inconsistencias porque cada CU se apoya en el contexto del anterior.
5. Un archivo por CU en `casos-de-uso/CU-XX-titulo-corto.md`. Mantené un `casos-de-uso/00-indice.md` como tabla resumen.

### Si vas a construir el prototipo HTML

1. Asegurate de que ya existan los casos de uso de las pantallas que vas a hacer.
2. Leé el doc 03 (inventario) y el doc 04 (componentes).
3. **Construí en orden de impacto demostrativo** (Pasada 1: F1 Squad activo, E3 Muro, B3 Insight). Esto SÍ aplica acá — el prototipo se muestra, los CU se leen secuencialmente.
4. Stack: HTML + Tailwind CDN, una pantalla por archivo, datos hardcodeados, desktop-first. Sin React, sin build step, sin backend.

### Si encontrás un hueco en los docs base

- **Pantalla nueva que necesitás** → primero agregala al doc 03, después la usás.
- **Término nuevo** → primero al doc 02 (glosario), después lo usás.
- **Componente UI nuevo** → primero al doc 04, después lo construís.

La fricción es deliberada — protege contra deriva e invenciones inconsistentes entre sesiones.

---

## Próximas sesiones recomendadas

| Sesión | Qué hacer | Salida esperada |
|---|---|---|
| **2 — CU Etapa 1+2** | Escribir CU-01 a CU-11 (acceso, onboarding, selección de sprint) | 11 archivos + índice |
| **3 — CU Etapa 3** | Escribir CU-12 a CU-19 (análisis profundo, muro, plan) | 8 archivos |
| **4 — CU Etapa 4** | Escribir CU-20 a CU-27 (squad activo, drill-down, checkpoints) | 8 archivos |
| **5 — CU Etapa 5 + auxiliares** | Escribir CU-28 a CU-35 | 8 archivos |
| **6 — Validación de CU** | Auditar los 35 CU contra principios y anti-patrones | Lista de correcciones |
| **7 — Prototipo Pasada 1** | F1 Squad + E3 Muro + B3 Insight con sus estados | ~10 archivos HTML |
| **8 — Prototipo Pasada 2** | C1 Hub + D1 Backlog + F3 Checkpoint | ~6 archivos HTML |
| **9–10 — Prototipo Pasadas 3-4** | Resto de las 18 pantallas | resto de HTML + index |

---

## Principios anti-sobreingeniería

- **Solo lo que está en ROBOK v5 + lo necesario para los CU y el prototipo.** No se inventan features.
- **Números acotados:** 18 pantallas, 15 componentes, 35 casos de uso. Cubre V1 sin pretender resolver el universo.
- **No hay vistas mobile, ni real-time collab, ni embed Slack en V1.** Esas son features V2.
- **Los gates humanos son no-negociables** (P3 de ROBOK v5). Los CU van a reflejarlo, los componentes UI ya lo materializan.
- **Diseño para 12 meses, no para 5 años.**

---

## Lo que NO está acá (vive en otro lado)

- **Código del MVP actual** (agente de Slack para registro arquitectónico) → repo separado.
- **Arquitectura técnica del backend** (capacidades fundacionales, hexagonal, adapters) → docs en el otro repo.
- **Pricing, monetización, GTM** → fuera del alcance de este repo.

---

## Notas de criterio

Los docs base se generaron en una pasada inicial intencional para tener una base sólida sobre la cual iterar. Eso significa:

- **Hay decisiones tomadas que conviene revisar al usar:** nombres exactos del glosario, paleta de colores sugerida, granularidad de algunos CU.
- **Placeholders sensatos:** "Promise Engine" como producto ejemplo, "Vera" como Dev Lead canónica.
- **Omisiones intencionales** donde más detalle era sobreingeniería temprana: pantallas H1 (admin) y C4 (configuración) descritas a nivel funcional sin el detalle de las pantallas centrales.

Todo es editable. Los docs están pensados para iterar.
