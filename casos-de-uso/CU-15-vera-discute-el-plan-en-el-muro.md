# CU-15. Vera lee el plan en el muro y discute con Rob hasta cerrarlo

## Identidad

- **Actor**: Vera (Dev Lead)
- **Pantalla principal**: E3. Muro de discusión del plan
- **Pantallas secundarias**: E2 (entrada), E4. Modo + quórum (salida).
- **Pre-condición**: Rob terminó el análisis profundo en E2 y publicó el plan completo en el muro.
- **Post-condición**: El plan llega a un estado "listo para aprobar" después de la discusión. Vera está por apretar "Dale" (que abre E4 — cubierto en CU-17).
- **Etapa del journey**: Etapa 3 — Análisis profundo y plan
- **Frecuencia esperada**: una vez por historia delegada — pero la discusión puede durar varios mensajes y minutos a horas.

## Disparador

ROBOK transiciona desde E2 cuando el análisis termina, o Vera entra a E3 desde C2 (Mis historias en curso) si volvió a la sesión más tarde.

## Flujo principal (happy path)

1. Vera ve E3 — Muro de discusión del plan, dividido en dos zonas:
   - **Zona del plan (izquierda)**: el plan propuesto por Rob, navegable por secciones colapsables.
   - **Zona del muro (derecha)**: hilo de discusión con avatares de cada participante.
2. Vera revisa la zona del plan — secciones expandibles:
   - **Descomposición en tareas**: T-1 (modelo nuevo en `pricing/domain/`), T-2 (endpoint POST /v1/quote en `billing-api`), T-3 (tests de integración), con dependencias y nivel de paralelización.
   - **ADRs propuestos**: ADR-2026-014 (uso de Decimal para precisión monetaria), ADR-2026-015 (caching de quotes en Memcached).
   - **Casos de prueba propuestos**: lista con casos de borde detectados.
   - **Estrategia de ramas**: una rama única `rob/PROM-1234` con squash-merge al cierre.
   - **Agentes y configuración**: 1 🎨 Architect, 2 🔨 Implementer, 1 🧪 Tester, 1 👀 Reviewer, 1 🛡️ Security Reviewer (transversal).
   - **Checkpoints**: 3 puntos de control humano marcados.
   - **Riesgos detectados**: pricing tuvo 3 retrabajos históricos — Rob propone test extra para regresión.
   - **Matriz costo × tiempo refinada**: Express USD 95 · 4h, Estándar USD 60 · 1d, Económico USD 28 · 3d, Sprint-pace USD 20 · 2 sem. Justificación del delta vs pre-estimación: "+USD 10 sobre la pre-estimación de Estándar porque hay que crear tests adicionales para el módulo histórico de pricing".
3. Vera ve que el costo creció vs CU-09. Quiere discutir un punto: la decisión de usar Memcached para caching cuando el equipo migró pero todavía no está estable.
4. Vera click en "Citar parte del plan" sobre ADR-2026-015. La cita aparece en su mensaje en el muro.
5. Vera escribe en el muro: "@Rob — Memcached todavía no es prod-ready en este equipo. ¿Podemos diferir el caching o caer a Redis aún?".
6. Rob responde en el muro (👤 Vera y 🤖 Rob — Architect aparecen como participantes con sus avatares). Rob propone: "Diferir el caching. Saco el ADR-2026-015 del plan. La performance va a estar en el límite pero pasa los benchmarks. ¿Te parece?". Rob propone X. Vera confirma o corrige antes de que Rob actúe.
7. Vera responde "dale, sin caching para esta historia". Rob actualiza el plan: el ADR-2026-015 desaparece y aparece un banner "Plan actualizado" con [Ver diff del plan].
8. Vera revisa el diff. La matriz costo × tiempo se recalcula automáticamente: Estándar bajó a USD 55 · 1d.
9. Vera no tiene más comentarios. El plan está listo para aprobar.
10. Vera click en "Dale" (botón top-right). ROBOK abre E4 — Selector de modo + quórum (cubierto en CU-17).

## Variantes

### V1. Plan sin discusión (caso simple)

Si Rob acertó en todo, Vera lee el plan, no tiene comentarios, y aprieta "Dale" sin escribir nada en el muro. El muro queda vacío salvo por el mensaje inicial de Rob ("Plan publicado · 5 tareas · 2 ADRs · USD 50 estándar"). Es válido — el muro existe para discutir, no para llenar.

### V2. Iteraciones múltiples

Vera y Rob (y eventualmente Diego como CU-16) tienen 4–6 idas y vueltas. Cada cambio del plan dispara un banner "Plan actualizado" con diff. El muro acumula el debate. Cuando se cierra, el ADR-record final refleja la conversación.

### V3. Vera invita a un humano específico

Vera ve que el plan toca un módulo donde otro Dev Lead tiene contexto. Click en "Invitar al muro" → selecciona a otro Dev Lead. Esa persona recibe notificación y entra al muro con permisos completos.

## Caminos alternativos / errores

- **Si la matriz costo × tiempo dio muy distinta a la pre-estimación**: Rob avisa explícitamente con un banner sobre el plan: "⚠️ Costo creció 3x respecto a la pre-estimación de D1, porque pricing no tiene tests del módulo histórico — hay que crearlos primero. ¿Querés revisar?". Vera decide seguir, ajustar el scope, o cancelar (CU-18).
- **Si Vera quiere cancelar a media discusión**: ver CU-18.
- **Si quórum requiere otro humano**: el botón "Dale" se mantiene pero E4 va a mostrar el quórum pendiente (cubierto en CU-17 / CU-19).

## Decisiones del usuario en este flujo

- Discutir o aprobar de una.
- Aceptar las propuestas de Rob a cada iteración o pedir más cambios.
- Invitar a otros humanos al muro (variante V3).
- Cuándo cortar la discusión y aprobar.

## Componentes UI involucrados

- 1. Header con badge ambient
- 7. Avatar y mensaje del muro de discusión (la zona derecha — central)
- 3. Pill de modo costo × tiempo (en la sección de matriz del plan)
- 8. Mapa de componentes (variante reducida, en la sección de scope del plan)
- 14. Pill de estado (en cada tarea, ADR, agente del plan)

## Notas para el prototipo HTML

- E3 es la pantalla MÁS importante de Etapa 3 (priorización Pasada 1 según doc 03). Vale invertir mucho.
- Layout: dos columnas, plan a la izquierda (60% del ancho) y muro a la derecha (40%).
- Estados sugeridos como archivos separados:
  - `e3-plan-recien-publicado.html` (mensaje de Rob inicial, muro casi vacío)
  - `e3-plan-con-discusion.html` (varios mensajes, plan original sin cambios todavía)
  - `e3-plan-actualizado.html` (después de un cambio, banner "Plan actualizado" con diff colapsable)
  - `e3-plan-listo-para-aprobar.html` (discusión cerrada, botón "Dale" destacado)
- Reutilizar componente 7 con dos avatares mínimo (👤 Vera, 🤖 Rob — Architect). Para variantes, agregar 👤 Diego (CU-16) y 👤 Pablo (CU-19).

## Referencias

- Journey: ROBOK_v5.md §1.7 — Etapa 3, "El muro de discusión del plan", "Anatomía del plan completo", "Manejo del gap costo Etapa 2 vs Etapa 3".
- Inventario: 03-inventario-pantallas.md §E3.
- Componentes UI: 04-componentes-ui.md §7 (avatar y mensaje), §8 (mapa).
- Principios involucrados: P3 (inferencia con confirmación — Rob propone cambios, Vera confirma), P5 (demostrar entendimiento — el plan muestra el trabajo, no solo declara), P4 (gobernanza configurable — el muro permite a varios humanos opinar).
