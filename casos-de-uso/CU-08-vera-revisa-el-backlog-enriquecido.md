# CU-08. Vera entra al sprint actual y revisa el backlog enriquecido

## Identidad

- **Actor**: Vera (Dev Lead)
- **Pantalla principal**: D1. Sprint actual — backlog enriquecido
- **Pantallas secundarias**: C1. Vista del producto (entrada).
- **Pre-condición**: El producto Promise Engine está onboardeado. El sprint actual está cargado en Jira (14 tickets). Rob ya enriqueció cada ticket con sus anotaciones (preestimación, dependencias, módulos que toca, riesgos, similitud histórica).
- **Post-condición**: Vera leyó el sprint y entendió la sugerencia de orden de Rob. Todavía no decidió qué delegar — eso es CU-09 / CU-10.
- **Etapa del journey**: Etapa 2 — Selección y priorización
- **Frecuencia esperada**: por sprint (al menos una vez al inicio del sprint, y revisitas durante el sprint).

## Disparador

Vera arranca el día (o vuelve después del planning) y necesita decidir qué tickets del sprint delega a Rob. El frame es el sprint actual, no el backlog entero.

## Flujo principal (happy path)

1. Vera está en C1 — Vista del producto Promise Engine. Click en la sección "Sprint actual".
2. ROBOK lleva a D1 — Sprint actual con backlog enriquecido.
3. Vera ve los 14 tickets del sprint en cards. Cada card muestra:
   - Ticket ID + título (ej. "PROM-1234 · Refactor pricing calculator").
   - Calidad de spec: ✅ OK / ⚠️ ambigua / ❌ falta info.
   - Módulos que toca (chips): pricing, billing-api, etc.
   - Dependencias con otros tickets (ej. "depende de PROM-1233").
   - Riesgo histórico (ej. "📈 pricing tuvo 3 retrabajos en últimos 6 meses").
   - Estrella ⭐ si vino pre-marcado para Rob desde Jira.
   - Matriz costo × tiempo en miniatura: 4 pills (Express, Estándar, Económico, Sprint-pace) con costo y tiempo.
   - Similitud histórica si aplica (💡 "similar a PROM-892 resuelto en mar-2026 por USD 42 estándar").
   - Botones: **Delegar de una** · **Pausar, profundizar** · **No es para Rob**.
4. Banner superior: sugerencia de orden de Rob. "Yo arrancaría por estos en este orden, porque..." con justificación expandible. Rob opina sobre el cómo, no sobre el qué — el orden es input para Vera, no decisión.
5. Vera escanea las cards. Identifica:
   - Tres tickets con ⭐ pre-marcados desde el planning (PROM-1234, PROM-1235, PROM-892).
   - Dos tickets con spec ❌ (falta info) — Rob no los recomienda hasta que se complete.
   - Un ticket bloqueado (PROM-5678) por dependencia con PROM-1233 abierto.
6. Vera click en "Ver justificación" del banner para entender por qué Rob propone ese orden ("PROM-1235 primero porque desbloquea PROM-1234, después PROM-892 porque es ágil y libera horas para los más complejos").
7. Vera lee. No decide nada todavía — sigue navegando para abrir D2 (CU-11), delegar (CU-09) o pausar (CU-10).

## Variantes

### V1. Tickets con info insuficiente

Algunos tickets aparecen con chip ❌ "spec falta info". Rob los marca como "necesita info" y NO los recomienda hasta que el equipo complete la spec en Jira. Vera puede filtrar por "spec ❌" para ver cuáles están en esa situación.

### V2. Vera filtra por tipo o estado

Vera filtra el backlog por tipo (bug vs feature) o por estado de spec (solo ✅ OK). Las cards no calzantes se ocultan. La sugerencia de orden de Rob se mantiene sobre el set filtrado.

### V3. Equipo sin sprints (kanban)

Si el producto no usa sprints sino kanban u otra unidad, D1 muestra el equivalente: "tickets en juego ahora". Mismas anotaciones de Rob, sin el frame temporal de 2 semanas.

## Caminos alternativos / errores

- **Si Jira no devolvió tickets**: D1 muestra "Sin tickets en el sprint actual" + un link "¿Conectaste el proyecto correcto en C4 — Configuración del producto?".
- **Si un ticket pre-marcado en Jira tiene spec ❌**: aparece igual con su estrella ⭐ pero el botón "Delegar de una" muestra un confirm antes de avanzar ("La spec está incompleta, ¿delegar igual?").

## Decisiones del usuario en este flujo

- Aceptar el orden sugerido por Rob, modificarlo o ignorarlo (Rob propone, Vera decide).
- Filtrar o no filtrar.
- Qué ticket abrir / delegar / pausar (esas decisiones se cubren en CU-09, CU-10, CU-11).

## Componentes UI involucrados

- 1. Header con badge ambient
- 2. Card de ticket en el backlog enriquecido (la pieza central)
- 3. Pill de modo costo × tiempo (dentro de la card)
- 14. Pill de estado (en chips de spec, módulos, marcas)

## Notas para el prototipo HTML

- D1 es una de las pantallas más densas y visualmente importantes — vale dedicarle tiempo (priorización Pasada 2 según doc 03).
- Estados sugeridos como archivos separados:
  - `d1-backlog-completo.html` (14 tickets variados, tres con ⭐, dos con ❌, uno bloqueado)
  - `d1-backlog-filtrado.html` (V2: solo spec ✅)
  - `d1-backlog-vacio.html` (sin tickets en el sprint)
- Datos mock recomendados (alineados con CLAUDE.md): PROM-1234 (principal), PROM-1235, PROM-1233, PROM-892 (histórico), PROM-5678 (bloqueado).

## Referencias

- Journey: ROBOK_v5.md §1.6 — Etapa 2, especialmente "El frame es el sprint actual, no el backlog entero" y "Anatomía del ticket anotado".
- Inventario: 03-inventario-pantallas.md §D1.
- Componentes UI: 04-componentes-ui.md §2 (card de ticket).
- Principios involucrados: P5 (demostrar entendimiento — cada ticket viene anotado, no es dump de Jira), P8 (decisión bidimensional — la matriz costo × tiempo se ve desde el escaneo del backlog), P3 (Rob opina sobre el cómo, el humano decide qué).
