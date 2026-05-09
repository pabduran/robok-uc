# ROBOK — Documentación extra (preparación para casos de uso y prototipo HTML)

> **Para qué sirve esto:** ROBOK v5 ya tiene los journeys del Dev Lead bien armados (Etapas 1-5 + principios). Faltan piezas intermedias entre "el journey" y "el prototipo HTML": personas, glosario, inventario de pantallas, componentes UI, y la guía para extraer casos de uso. Estos cinco docs cubren ese hueco.
>
> **Estado:** drafts iniciales pensados para iterar. No reemplazan ROBOK v5 — lo complementan.
>
> **Próximo paso:** otro agente (o vos en otra sesión) usa el 05-guia-casos-de-uso.md como guion para escribir los casos de uso. Después de los casos de uso, viene el prototipo HTML.

---

## Los cinco documentos

### 📄 [01 — Personas y arquetipos](./01-personas-y-arquetipos.md)
Define a Vera (Dev Lead), Diego (Developer), Marisol (Admin de ROBOK) y Pablo (Stakeholder externo). Cada uno con motivaciones, frustraciones, comportamiento esperado y frase canónica. Sin esto, los casos de uso quedan en "el usuario hace X".

### 📄 [02 — Glosario y modelo conceptual](./02-glosario-y-modelo-conceptual.md)
Términos canónicos del producto. Reglas de naming en pantalla. Anti-glosario (qué NO decir). Diagrama ER del modelo conceptual. Sin esto, los casos de uso van a inventar 5 nombres distintos para "el plan".

### 📄 [03 — Inventario de pantallas](./03-inventario-pantallas.md)
Las 18 pantallas que existen en V1, agrupadas en 8 bloques (Acceso, Onboarding, Hub, Selección, Análisis, Squad, Pre-merge Gate, Admin). Cada pantalla con: quién entra, qué muestra, estados, salida. Mapa global de navegación. Priorización para el prototipo en 4 pasadas.

### 📄 [04 — Componentes UI](./04-componentes-ui.md)
Los 15 componentes que aparecen en múltiples pantallas: Header con badge ambient, Card de ticket, Pill de modo, Card de agente, Status Bar, Timeline editorial, Avatar del muro, Mapa de componentes, etc. Cada uno con anatomía visual ASCII, estados, comportamiento. Stack sugerido para el prototipo (HTML + Tailwind, sin build step).

### 📄 [05 — Guía para construir casos de uso](./05-guia-casos-de-uso.md)
**Es el "puente" hacia el siguiente paso.** Define qué es un caso de uso en ROBOK, granularidad correcta, plantilla recomendada, lista de los 35 casos de uso esperados, anti-patrones a evitar. El próximo agente lee este doc y escribe los casos de uso siguiendo el patrón.

---

## Orden de lectura

**Si sos el agente que va a construir los casos de uso:**

1. Leé `ROBOK_v5.md` completo (la fuente de verdad funcional).
2. Leé los docs 01, 02, 03, 04 en ese orden.
3. Usá el doc 05 como guion de trabajo.
4. Empezá por los casos de uso de Etapas 4 y 3 (los más impactantes para el prototipo).

**Si sos el agente que va a construir el prototipo HTML:**

1. Asegurate de que ya existan los casos de uso (paso anterior).
2. Leé el doc 03 (inventario de pantallas) y el doc 04 (componentes).
3. Empezá por la Pasada 1 del prototipo: pantalla F1 (Squad activo), E3 (Muro de discusión), B3 (Insight inicial).
4. Cada pantalla = un archivo `.html`. Estados como variantes (ej. `f1-squad-trabajando.html`, `f1-squad-esperando.html`).

---

## Principios que estos docs respetan

Los cinco documentos son intencionalmente **anti-sobreingeniería**:

- **Solo lo que está en ROBOK v5 + lo que necesitan los casos de uso.** No se inventaron features nuevas.
- **18 pantallas, 15 componentes, 35 casos de uso.** Números acotados que cubren el producto V1 sin pretender resolver el universo.
- **Stack del prototipo: HTML + Tailwind + emojis.** Sin frameworks pesados, sin backend.
- **No hay vistas mobile, no hay real-time collaboration, no hay embed Slack.** Esas son features V2.
- **Los gates humanos son no-negociables** (P3 de ROBOK v5) — los casos de uso van a reflejarlo, los componentes UI ya lo materializan (modal de checkpoint bloqueante, etc).

---

## Lo que estos docs intencionalmente NO cubren

Cosas que SÍ son importantes pero que viven en otra parte del proyecto:

- **Arquitectura técnica del backend** → vive en `Arquitectura_desacoplada.md`, `c4-containers.md`, `c4-components.md`, `2026-05-06-clean-architecture-design.md`.
- **MVP actual de Slack (registro arquitectónico)** → es un alcance distinto, vive en `spec.md`, `README.md` y los archivos de design 2026-05-07.
- **Implementación de squads, agentes, runtime de IA** → eso lo cubren las capacidades fundacionales y la arquitectura desacoplada.
- **Pricing y monetización** → fuera de scope de estos docs.
- **Estrategia de go-to-market** → fuera de scope.

---

## Si volviste y querés seguir

Próximas sesiones recomendadas, en orden:

**Sesión 2 — Casos de uso (próximo paso obvio)**
- Pedir: "Tengo los docs 01-05. Ahora escribime los 35 casos de uso siguiendo la guía del doc 05. Empezá por las Etapas 4 y 3."
- Salida esperada: 35 casos de uso en el formato de la plantilla.

**Sesión 3 — Validación de casos de uso**
- Pedir: "Revisá los casos de uso contra los principios de ROBOK v5 y los anti-patrones del doc 05. Marcá lo que esté mal."
- Salida esperada: lista de correcciones.

**Sesión 4 — Prototipo HTML, Pasada 1**
- Pedir: "Construime las pantallas F1, E3, B3 en HTML + Tailwind, con sus estados. Datos hardcodeados. Una pantalla por archivo."
- Salida esperada: 3 archivos `.html` (más sus variantes de estado) navegables entre sí.

**Sesión 5 — Prototipo HTML, Pasadas 2-4**
- Iterativas, una pasada por sesión si querés ir despacio o todo de una si querés acelerar.

---

## Notas de criterio

Estos docs fueron generados de un tirón intencionalmente, para que cuando volvieras del paseo del perro tuvieras una base sólida sin esperar nada. Eso significa:

- **Hay decisiones tomadas por mí** que conviene revisar: nombres exactos en el glosario, paleta de colores sugerida, granularidad de algunos casos de uso de la lista.
- **Hay placeholders sensatos** donde había ambigüedad: el "Promise Engine" como producto ejemplo, "Vera" como Dev Lead canónica, etc. Cambiables si querés otra convención.
- **Hay omisiones intencionales** donde sentí que entrar más era sobreingeniería: las pantallas H1 (admin) y C4 (configuración) están descritas a nivel funcional pero sin el detalle de las pantallas centrales — coherente con que no son el corazón del producto.

Si algo no te calza al volver, todo es editable y los docs están pensados para iterar.
