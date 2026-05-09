# CU-09. Vera marca un ticket para Rob desde el backlog enriquecido (camino "delegar de una")

## Identidad

- **Actor**: Vera (Dev Lead)
- **Pantalla principal**: D1. Sprint actual — backlog enriquecido
- **Pantallas secundarias**: E1. Confirmación de scope (salida).
- **Pre-condición**: Vera está en D1 y ya escaneó el sprint (CU-08). Identificó un ticket que quiere delegar (ej. PROM-1234) con spec OK y entiende los módulos y dependencias.
- **Post-condición**: El ticket queda marcado para Rob (estado "Para Rob — En análisis"). ROBOK transiciona a E1 — Confirmación de scope. El ticket arranca el ciclo de Etapa 3.
- **Etapa del journey**: Etapa 2 — Selección y priorización
- **Frecuencia esperada**: varias veces por sprint (típicamente 2–5 historias por sprint).

## Disparador

Vera ya decidió delegar PROM-1234. Tiene la card abierta o destacada en D1.

## Flujo principal (happy path)

1. Vera está en D1 sobre la card de PROM-1234 ("Refactor pricing calculator", spec ✅, depende de PROM-1233 ya cerrado, riesgo histórico 📈 en pricing).
2. Vera revisa la matriz costo × tiempo en la card: Express USD 80 · 4h, Estándar USD 50 · 1d, Económico USD 25 · 3d, Sprint-pace USD 18 · 2 sem.
3. Vera elige el modo (Express / Estándar / Económico / Sprint-pace) — opcional acá, también se puede dejar para E4. Click en el pill "Estándar" lo destaca con borde de selección.
4. Vera click en "Delegar de una".
5. ROBOK marca el ticket en estado "⭐ Para Rob — En análisis" en la card del backlog (cambio visible inmediatamente: la card cambia de pill de estado y agrega el indicador).
6. ROBOK transiciona a E1 — Confirmación de scope (cubierto en CU-12). Rob ya empezó a inferir el scope sobre el mapa de componentes mientras carga.

## Variantes

### V1. Vera no preselecciona modo

La preselección de modo en D1 es opcional. Vera puede saltearla y elegir el modo más tarde en E4 (modal de modo + quórum) cuando el plan esté discutido y los costos refinados. Es más común dejarlo para E4 porque el costo final puede diferir de la pre-estimación.

### V2. Spec ambigua (⚠️) pero Vera decide delegar igual

Vera ve el chip ⚠️ "spec ambigua" en la card. Decide delegar de todas formas — Rob va a marcar las ambigüedades en el muro de discusión (E3) durante la fase de análisis. Vera sabe que va a tener que aclararlas con el equipo durante la conversación.

### V3. Ticket pre-marcado desde Jira

El ticket vino con ⭐ desde el planning (etiqueta o campo custom de Jira). Vera lo confirma con "Delegar de una" sin cambios. El flujo es idéntico — el pre-marcado es input, no decisión.

## Caminos alternativos / errores

- **Si la spec falta info (❌)**: el botón "Delegar de una" abre un confirm "La spec está incompleta. Rob no va a poder estimar bien. ¿Delegar igual?". Vera puede cancelar y completar la spec en Jira primero, o seguir aceptando el riesgo.
- **Si el ticket depende de otro ticket abierto** (ej. PROM-5678 depende de PROM-1233 sin resolver): el confirm avisa "Este ticket depende de PROM-1233 que sigue abierto. ¿Delegar igual?". Vera decide.
- **Si Rob no tiene contexto suficiente para inferir scope** (ej. un módulo que no quedó indexado bien en el onboarding): E1 carga con scope vacío y un mensaje "No pude inferir scope inicial. Marcalo manualmente en el mapa." (cubierto en CU-12 / CU-13).

## Decisiones del usuario en este flujo

- Qué ticket delegar.
- Si preseleccionar modo en D1 o dejarlo para E4.
- Qué hacer si la spec está incompleta o las dependencias no están claras (delegar igual, pausar, o completar la spec primero).

## Componentes UI involucrados

- 2. Card de ticket en el backlog enriquecido
- 3. Pill de modo costo × tiempo
- 14. Pill de estado (la card pasa a "Para Rob — En análisis")

## Notas para el prototipo HTML

- Estado de la card antes y después de delegar:
  - `d1-card-pre-delegar.html` (card normal con botones visibles)
  - `d1-card-post-delegar.html` (card con pill "Para Rob — En análisis" y botones reemplazados por "Ver análisis →")
- El click en "Delegar de una" linkea a `e1-scope-inferido.html` (estado inicial de E1).

## Referencias

- Journey: ROBOK_v5.md §1.6 — Etapa 2, "Dos decisiones distintas en esta etapa: Delegar".
- Inventario: 03-inventario-pantallas.md §D1.
- Componentes UI: 04-componentes-ui.md §2, §3, §14.
- Principios involucrados: P3 (el humano decide qué delegar — el ticket no se delega solo), P8 (decisión bidimensional — el modo se elige acá o en E4).
