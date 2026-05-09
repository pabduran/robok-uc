---
name: robok-prototipo-html
description: Use this skill when the user asks to build, modify, refine, or review HTML prototype screens for ROBOK. Triggers include phrases like "construime las pantallas", "armá el prototipo", "agregá el estado X a la pantalla F1", "mejorá la pantalla del muro", or any request to produce or edit `.html` files for ROBOK screens. Do NOT use for use case writing or for general ROBOK design questions.
---

# Skill — Construir el prototipo HTML de ROBOK

Esta skill te guía para producir el prototipo visual de ROBOK como pantallas HTML estáticas, navegables entre sí, sin caer en sobreingeniería.

## Pasos obligatorios antes de empezar

1. **Leé estos archivos en este orden, en cada sesión:**
   - `03-inventario-pantallas.md` (las 18 pantallas y sus estados)
   - `04-componentes-ui.md` (los 15 componentes con anatomía y paleta)
   - `02-glosario-y-modelo-conceptual.md` (naming en pantalla y anti-glosario)
   - El o los casos de uso que la pantalla resuelve (en `casos-de-uso/CU-XX-*.md`)
   - `01-personas-y-arquetipos.md` solo si vas a poblar datos mock con personas

2. **Verificá qué pantallas ya existen** en el directorio del prototipo (típicamente `prototipo/`). No reescribas, extendé.

## Stack obligatorio (no negociable)

- **HTML5 estático**, un archivo `.html` por pantalla.
- **Tailwind CSS via CDN** (`<script src="https://cdn.tailwindcss.com"></script>`). NO build step. NO PostCSS. NO npm.
- **Iconografía: emojis nativos** (🟢 🟡 🔴 🎨 🔨 🧪 👀 📝 🛡️ ⭐ 💡 etc — los del glosario y componentes UI). Opcional: Lucide/Heroicons via CDN si hace falta algún icono específico.
- **Vanilla JS si necesitás interactividad simple** (modal abrir/cerrar, tabs). NO React, NO Vue, NO frameworks.
- **Datos hardcodeados en el HTML.** NADA de fetch, NADA de backend, NADA de localStorage.

## Reglas duras (no negociables)

- **Una pantalla = un archivo.** Nombrarlo con el código de pantalla del inventario: `f1-squad-activo.html`, `e3-muro-discusion.html`, `b3-insight-inicial.html`.
- **Estados = archivos separados.** No JS condicionando el render. Si la pantalla F1 tiene 4 estados, hay 4 archivos: `f1-squad-trabajando.html`, `f1-squad-esperando.html`, `f1-squad-bloqueado.html`, `f1-squad-terminado.html`. Eso facilita revisar y compartir.
- **Navegación entre pantallas con `<a href="otra-pantalla.html">`.** Sin SPA, sin routing JS. El usuario puede abrir cualquier archivo directamente.
- **Componentes según el doc 04.** Anatomía, estados, comportamiento. No inventar variantes que no estén descritas.
- **Términos según el doc 02.** Anti-glosario aplica también en el prototipo: nunca "bot", "workflow", "run", "log".
- **Desktop-first.** No responsive perfecto. ROBOK V1 es desktop. No perdás tiempo en mobile.

## Paleta sugerida (del doc 04)

```css
/* Fondos: blanco o gris muy claro */
bg-white, bg-gray-50

/* Acción primaria */
bg-blue-600  /* #2563eb — links y botones primarios */

/* Estados */
bg-green-500  /* squad trabajando, validación OK */
bg-amber-500  /* esperando humano, atención voluntaria */
bg-red-500    /* bloqueado, fallo */
bg-purple-600 /* insights, narración de Rob */
bg-orange-500 /* delegación, marcas "para Rob" */
```

Usá Tailwind utility classes correspondientes. Para variantes claras (fondos de pills): `bg-green-100 text-green-700`, etc.

## Estructura sugerida de cada archivo HTML

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>ROBOK · <Nombre de la pantalla></title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    /* Solo si necesitás estilos custom que Tailwind no cubre */
  </style>
</head>
<body class="bg-gray-50 min-h-screen">

  <!-- Header con badge ambient (componente 1) -->
  <header class="bg-white border-b border-gray-200 px-6 py-3 flex items-center justify-between">
    <div class="flex items-center gap-2">
      <span class="font-semibold">ROBOK</span>
      <span class="text-gray-400">·</span>
      <button class="font-medium hover:bg-gray-100 px-2 py-1 rounded">Promise Engine ▼</button>
    </div>
    <div class="flex items-center gap-2">
      <span class="text-sm">🟢 Todo bien · 3 historias en curso</span>
    </div>
  </header>

  <!-- Breadcrumbs si aplica -->
  <nav class="px-6 py-3 text-sm text-gray-600">
    <a href="c1-vista-producto.html" class="hover:underline">Promise Engine</a>
    <span class="mx-2">›</span>
    <span class="text-gray-900">Squad activo · PROM-1234</span>
  </nav>

  <!-- Contenido principal de la pantalla -->
  <main class="px-6 py-6">
    <!-- ... -->
  </main>

  <!-- Modales o paneles laterales si aplican (Tailwind + minimal JS) -->

  <script>
    // Solo si hay interactividad simple (abrir modal, toggle tab, etc).
  </script>
</body>
</html>
```

## Datos mock — usá estos consistentemente

Para que las pantallas se sientan parte del mismo producto, usá estos datos a lo largo de todo el prototipo:

**Producto principal:**
- Nombre: "Promise Engine"
- Stack: Python 3.12, FastAPI, SQLAlchemy, Postgres, Redis, K8s
- Repos: `promise-api`, `promise-worker`, `promise-ui`

**Producto secundario (para selector):**
- "Ratings & Reviews"

**Equipo:**
- Vera (Dev Lead — vos en el prototipo)
- Diego, Sofía, Andrés, Mariana, Joaquín, Luna (Developers)

**Stakeholder externo:**
- Pablo (Security Officer)

**Admin de tenant:**
- Marisol (head of platform engineering)

**Tickets de ejemplo:**
- PROM-1234 — "Refactor pricing calculator" (historia activa principal)
- PROM-1235 — "Endpoint cancelación con razón estructurada"
- PROM-1233 — dependencia de la 1234
- PROM-892 — historia histórica similar (resuelta marzo 2026)
- PROM-5678 — historia bloqueada para mostrar estado rojo

**Costos de ejemplo:**
- Express USD 80 · 4 horas
- Estándar USD 50 · 1 día
- Económico USD 25 · 2-3 días
- Sprint-pace USD 18 · 1-2 semanas

## Priorización para construir (del doc 03)

**Pasada 1 — el corazón del producto:**
1. F1. Vista del squad activo (con sus 4 estados visuales)
2. E3. Muro de discusión del plan
3. B3. Insight inicial proactivo

**Pasada 2 — el hub y la decisión:**
4. C1. Vista del producto (landing)
5. D1. Sprint actual — backlog enriquecido
6. F3. Pantalla de checkpoint (modal)

**Pasada 3 — flujos de soporte:**
7. E1. Confirmación de scope
8. C3. Mapa de componentes
9. G2. Pre-merge Gate dashboard
10. F2. Drill-down de agente (panel lateral)

**Pasada 4 — completar el ciclo:**
11. A1, A2 (login y selector)
12. B1, B2 (carga contexto e indexado)
13. C2, C4 (historias en curso, configuración)
14. D2 (detalle de ticket)
15. E2, E4 (análisis en curso, modal modo+quórum)
16. G1, G3 (reporte cierre, mini-muro)
17. H1 (panel admin)

## Anti-patrones a evitar

- 🚩 **Stack inflado.** Si te tienta meter React/Vue/build step → parate. HTML estático + Tailwind CDN alcanza para todo el prototipo.
- 🚩 **Vista mobile.** No es V1. No la construyas.
- 🚩 **Real-time collaboration en el muro (cursores tipo Figma).** No es V1.
- 🚩 **Embed de aprobaciones en Slack.** No es V1. V1 = link.
- 🚩 **Backend o API real.** Datos hardcodeados.
- 🚩 **Animaciones complejas.** Transiciones CSS estándar (`transition-all duration-200`) alcanzan.
- 🚩 **Iconos custom.** Emojis nativos cubren todo.
- 🚩 **Inventar pantallas.** Solo las 18 del inventario.
- 🚩 **Renombrar componentes.** Los del doc 04 tienen nombres canónicos.
- 🚩 **Mezclar idiomas en UI.** El producto es en español. Los roles del squad SÍ van en inglés (Architect, Implementer, Tester, Reviewer, Doc-writer, Security Reviewer) por consistencia técnica — eso está fijado en el doc 02.

## Checklist antes de cerrar una pantalla

- [ ] El nombre del archivo coincide con el código del inventario.
- [ ] Header con badge ambient presente (si aplica).
- [ ] Breadcrumbs si la pantalla está dentro de un producto.
- [ ] Todos los textos visibles usan términos del glosario.
- [ ] Todos los pills de estado siguen los emojis canónicos del doc 02.
- [ ] Los componentes que aparecen están en el doc 04 (no inventé).
- [ ] Los datos mock son consistentes con el resto del prototipo.
- [ ] Si la pantalla tiene varios estados, hay un archivo por estado.
- [ ] Los `<a href>` apuntan a archivos que existen o que tengo que crear próximamente.
- [ ] Funciona abriendo el archivo directamente con doble click (sin servidor).

## Output esperado

Cuando termines una pasada, dejá:

1. **Archivos `.html`** en `prototipo/` con nombres del inventario.
2. **Un `prototipo/index.html`** que liste todas las pantallas creadas con links — sirve de "menú" para revisar el prototipo entero.
3. **Comentarios HTML al inicio de cada archivo** indicando: qué CU resuelve, qué estado representa, qué componentes UI usa.

## Si encontrás un componente que no está documentado

Si necesitás un componente que no está en el doc 04:

1. **NO lo improvises.** Detenete.
2. **Proponé al usuario agregarlo al doc 04** con su anatomía y estados.
3. **Una vez aprobado y agregado al doc**, recién implementarlo en HTML.

La fricción es deliberada — protege contra inconsistencia visual.
