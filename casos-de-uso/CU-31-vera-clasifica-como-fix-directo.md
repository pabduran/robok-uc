# CU-31. Vera resuelve el mini-muro: clasifica como "fix directo" y vuelve al squad

## Identidad

- **Actor**: Vera (Dev Lead)
- **Pantalla principal**: G3. Mini-muro de discusión (fallo en prueba manual)
- **Pantallas secundarias**: F1. Vista del squad activo (cuando el squad reanuda).
- **Pre-condición**: El mini-muro G3 está abierto desde CU-30. Diego reportó la falla de P-3, Rob propuso diagnóstico inicial con clasificación "Fix directo". Vera recibió notificación interrupting.
- **Post-condición**: Vera valida (o ajusta) el diagnóstico, aprueba la clasificación. El squad vuelve a estar activo (sin re-aprobación de Etapa 3 porque el fix es acotado) y toma el fix como nueva tarea. Cuando termina, las pruebas manuales se re-ejecutan.
- **Etapa del journey**: Etapa 5 — Pre-merge Gate
- **Frecuencia esperada**: 0–1 vez por historia (cuando G3 se abrió en CU-30).

## Disparador

Vera click en "Ver mini-muro" desde la notificación interrupting o desde el banner amarillo en G2.

## Flujo principal (happy path)

1. Vera entra a G3 — Mini-muro de discusión.
2. Vera lee el reporte de Diego (estructurado: pasos, screenshot, expected vs observed). El comportamiento es claro: la promesa queda en `cancelling` indefinidamente.
3. Vera lee el diagnóstico inicial de Rob:
   - Causa probable: refactor de T-2 modificó el flujo de cancelación, el job async del worker probablemente no fue redeployado.
   - Scope del fix: pequeño.
   - Clasificación propuesta: 🔧 **Fix directo** (cambio acotado, no afecta el plan).
4. Vera confirma el diagnóstico. El razonamiento le hace sentido — el refactor de T-2 sí cambió el flujo de cancelación. Click en "Aprobar clasificación: Fix directo".
5. ROBOK ejecuta:
   - El squad (que estaba en estado de cierre tras CU-26) reactiva el rol 🔨 Implementer y 🧪 Tester. Workspace preservado.
   - Crea una nueva tarea en el plan: T-5 "Fix flujo de cancelación post-refactor — verificar publicación de evento al worker".
   - El squad arranca a procesar T-5 en la misma rama `rob/PROM-1234`.
   - F1 vuelve a mostrar agentes activos.
   - El estado de P-3 en G2 cambia a "🔄 En reparación · squad procesando".
6. Vera puede quedarse en G3 o saltar a F1 para ver el squad. Cuando T-5 cierra, las validaciones automatizadas se re-corren y P-3 vuelve a estado ⏸️ pendiente para que Diego (u otro humano) la re-ejecute.
7. Diego re-ejecuta P-3, ahora pasa ✅. El estado en G2: "5/5 pasados". Banner verde "Pruebas manuales completas".

## Variantes

### V1. Vera clasifica como "Cambio arquitectónico" (vuelta a Etapa 3)

Vera lee el reporte y entiende que el problema no es un fix puntual sino que el refactor de T-2 reveló que el flujo de cancelación necesita una nueva pieza arquitectónica (ej. una lock distribuida que el plan original no consideró).

1. Vera click en "Reclasificar". Selecciona "Cambio arquitectónico".
2. ROBOK abre un thread en el muro de discusión grande (E3) — vuelta a Etapa 3 — con el contexto del fallo. El plan se reabre formalmente.
3. Rob propone ajustes al plan. Vera y Diego participan en la nueva discusión.
4. Cuando se aprueba el plan ajustado, el squad arranca con el plan nuevo (puede heredar parte del trabajo previo de la rama existente).

### V2. Vera clasifica como "No es bug" (ajuste de spec)

Vera lee el reporte y se da cuenta de que el comportamiento "observado" es en realidad el correcto — la spec original tenía mal el expected. Click en "Reclasificar". Selecciona "No es bug · ajustar criterio de aceptación". ROBOK marca el ítem en G2 como "✅ Pasó · spec ajustada por Vera". Rob propone editar el item del checklist en Jira con el nuevo expected; Vera aprueba el edit. El squad no toma ninguna tarea adicional.

### V3. Vera clasifica como "No es bug" (problema del entorno de Diego)

Continuación de CU-30 V3. Vera y Diego identifican que el worker estaba detenido en local. Click en "Reclasificar". Selecciona "No es bug · problema del entorno". ROBOK marca el item como "✅ Pasó (con nota de entorno)" y le sugiere a Diego correr la prueba contra otro entorno o pedir a otro Developer que la ejecute para confirmar.

## Caminos alternativos / errores

- **Si Vera quiere consultar más antes de clasificar**: comenta en el mini-muro pidiendo más info ("@Diego — ¿podés probar si pasa lo mismo después de reiniciar el worker?"). Diego responde, Vera reclasifica.
- **Si Vera detiene el squad en lugar de aprobar fix**: click en "Detener squad y replantear". Squad termina (similar a CU-22 V2). El plan de Etapa 3 se reabre completamente.
- **Si después del fix la prueba sigue fallando**: G3 vuelve a abrir con el segundo intento. Si el problema persiste 2–3 veces, ROBOK sugiere automáticamente reclasificar a "Cambio arquitectónico" (V1).

## Decisiones del usuario en este flujo

- Aprobar la clasificación inicial de Rob o reclasificar (Fix directo / Cambio arquitectónico / No es bug).
- Si va a vuelta a Etapa 3: aceptar el costo de re-planificación.
- Si el fix se ejecuta y vuelve a fallar: paciencia, escalación, o cancelación de la historia.

## Componentes UI involucrados

- 7. Avatar y mensaje del muro de discusión (componente del mini-muro)
- 10. Notificación interrupting (la que llevó a Vera al mini-muro)
- 14. Pill de estado (en el item de prueba manual y en la clasificación)
- 4. Card de agente del squad (cuando el squad reactiva en F1)

## Notas para el prototipo HTML

- Estados sugeridos como archivos separados:
  - `g3-mini-muro-aprobando-fix-directo.html` (Vera por confirmar la clasificación)
  - `g3-mini-muro-reclasificado-arquitectonico.html` (V1: clasificación cambiada, banner indicando "vuelta a Etapa 3")
  - `g3-mini-muro-no-es-bug.html` (V2/V3: clasificación "No es bug" con el ajuste de spec o nota de entorno)
  - `f1-squad-reactivado-por-fix.html` (squad procesando T-5 en F1, status bar muestra "🔄 Procesando fix de pruebas manuales")

## Referencias

- Journey: ROBOK_v5.md §1.9 — Etapa 5, "Mini-muro de discusión cuando algo falla" (Rob diagnóstica + 3 caminos: fix directo, cambio arquitectónico, no es bug).
- Inventario: 03-inventario-pantallas.md §G3.
- Componentes UI: 04-componentes-ui.md §7.
- Principios involucrados: P3 (Vera valida la clasificación · el humano decide el camino), P5 (Rob propone diagnóstico con justificación, Vera tiene el contexto para decidir), P7 (snapshot del producto al momento del fix queda anclado, igual que cualquier otro cambio).
