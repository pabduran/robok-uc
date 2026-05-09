# ROBOK — Guía para construir casos de uso desde los journeys

> **Propósito de este doc:** ser el instructivo que el próximo agente (o el dueño del producto en otra sesión) va a usar para escribir casos de uso bien formados a partir de los journeys de ROBOK v5. Sin esta guía, los casos de uso van a salir inconsistentes, mezclando granularidades, inventando pantallas que no existen, o repitiendo lo que ya está en los journeys.
>
> **Quién lo lee:** Claude (u otro agente de IA), o un humano colaborador, en una próxima sesión.
>
> **Lo que NO es:** los casos de uso en sí. Es la guía para construirlos.

---

## Contexto que YA existe (no inventar nada)

Antes de escribir casos de uso, leer en este orden:

1. **`ROBOK_v5.md`** — los journeys completos de las 5 etapas + principios. Es la fuente de verdad funcional.
2. **`01-personas-y-arquetipos.md`** — quiénes son los actores humanos de cada caso de uso.
3. **`02-glosario-y-modelo-conceptual.md`** — términos canónicos. **Cualquier término que aparezca en un caso de uso debe salir de acá** o agregarse al glosario explícitamente.
4. **`03-inventario-pantallas.md`** — las 18 pantallas que existen. Los casos de uso solo pueden referenciar pantallas que están en el inventario.
5. **`04-componentes-ui.md`** — los componentes UI, para saber qué se ve en cada pantalla.

**Si un caso de uso necesita una pantalla nueva o un término nuevo, primero se agrega al inventario o glosario, y después se escribe el caso de uso.** Esa es la regla anti-sobreingeniería: nada existe en el caso de uso si no está en los docs base.

---

## Qué es un caso de uso en ROBOK

**Un caso de uso describe UN flujo concreto que UN actor recorre por UNA pantalla principal con UN objetivo claro.**

No es:
- Un journey completo (eso ya está en ROBOK v5).
- Una user story de Jira (es más detallado).
- Una spec funcional con FRs (es más cercano al usuario, menos al sistema).
- Un test case (no testea, describe el flujo).

Es:
- El "guion" de lo que pasa en pantalla cuando alguien hace algo.
- La granularidad correcta para que un developer pueda construir ese flujo y para que un diseñador pueda dibujarlo.

---

## Granularidad — cuándo dividir, cuándo unir

**Regla: un caso de uso = un actor + una pantalla principal + un resultado observable.**

**✅ Caso de uso bien dimensionado:**
- "Vera marca un ticket para Rob desde el sprint actual" → un actor (Vera), una pantalla (D1), un resultado (ticket pasa a estado "Para Rob").
- "Vera aprueba un plan en el muro de discusión" → un actor (Vera), una pantalla (E3 + modal E4), un resultado (squad arranca).
- "Diego comenta en el muro y Rob ajusta el plan" → un actor (Diego), una pantalla (E3), un resultado (mensaje publicado + plan actualizado).

**❌ Caso de uso demasiado grande:**
- "Vera onboardea un producto desde cero hasta que arranca el squad" → eso es 4 etapas mezcladas. Romper en 4-6 casos de uso.

**❌ Caso de uso demasiado chico:**
- "Vera apreta el botón Aprobar" → eso es un click, no un caso de uso. Combinar con el contexto.

**Regla práctica:**
- Si tu caso de uso necesita más de 3 pantallas distintas como "principales", romperlo.
- Si tu caso de uso se cuenta en menos de 5 pasos, está bien — mantenerlo corto.

---

## Estructura recomendada de un caso de uso

Cada caso de uso tiene este esqueleto. Adaptable, pero conviene que todos tengan la misma forma para que sean comparables.

```markdown
# CU-XX. <Título descriptivo en imperativo>

## Identidad
- **Actor**: <Vera / Diego / Marisol / Pablo>
- **Pantalla principal**: <CódigoPantalla. Nombre>  ← debe estar en el inventario
- **Pantallas secundarias**: <opcional>
- **Pre-condición**: <qué tiene que estar pasando antes>
- **Post-condición**: <qué cambió después>
- **Etapa del journey**: <Etapa 1 / 2 / 3 / 4 / 5 / Admin>
- **Frecuencia esperada**: <varias veces al día / por sprint / por onboarding / esporádico>

## Disparador
<Qué hace que el actor llegue a esta pantalla. Puede ser una notificación, una decisión propia, un click desde otra pantalla.>

## Flujo principal (happy path)
1. <Paso 1 — qué ve, qué hace>
2. <Paso 2 — qué responde el sistema>
3. <Paso 3 — siguiente acción del actor>
...
N. <Resultado final>

## Variantes
### V1. <Nombre de la variante>
<En qué se diferencia del happy path. Solo si la variante es significativa.>

### V2. <Otra variante>
...

## Caminos alternativos / errores
- **Si <condición>**: <qué pasa>
- **Si <otra condición>**: <qué pasa>

## Decisiones del usuario en este flujo
<Lista de momentos donde el actor decide algo. Útil para el prototipo: cada decisión = una variante del HTML.>

## Componentes UI involucrados
<Lista de componentes del doc 04. Ej: 1. Header, 5. Status Bar, 4. Card de agente.>

## Notas para el prototipo HTML
<Instrucciones específicas para quien implemente esto en HTML. Estados a mostrar, transiciones, datos mock recomendados.>

## Referencias
- Journey: <enlace a sección de ROBOK_v5.md>
- Principios involucrados: <P1, P5, etc>
```

---

## Casos de uso esperados — checklist por etapa

Esto es la **lista de casos de uso que deberían existir** para cubrir los journeys. Si el agente que escribe los casos de uso hace todos estos, el prototipo HTML va a poder mostrar el producto entero. Si hace menos, hay que justificar qué quedó fuera.

### Etapa 0 — Acceso (3 casos de uso)

- [ ] **CU-01.** Vera entra a ROBOK por primera vez y elige un producto.
- [ ] **CU-02.** Marisol entra como Admin y accede al panel del tenant.
- [ ] **CU-03.** Vera cambia entre dos productos sin volver al login.

### Etapa 1 — Onboarding del producto (4 casos de uso)

- [ ] **CU-04.** Vera onboardea un producto recién provisionado: carga contexto y mapea equipo.
- [ ] **CU-05.** Vera mira el indexado en curso con narración explícita y entiende qué está pasando.
- [ ] **CU-06.** Vera lee el insight inicial proactivo y lo confirma.
- [ ] **CU-07.** Vera lee el insight inicial, encuentra errores, y los corrige conversando con Rob.

### Etapa 2 — Selección y priorización (4 casos de uso)

- [ ] **CU-08.** Vera entra al sprint actual y revisa el backlog enriquecido.
- [ ] **CU-09.** Vera marca un ticket para Rob desde el backlog enriquecido (camino "delegar de una").
- [ ] **CU-10.** Vera marca un ticket en pausa para profundizar después.
- [ ] **CU-11.** Vera entra al detalle de un ticket anotado para ver más contexto.

### Etapa 3 — Análisis profundo y plan (6 casos de uso)

- [ ] **CU-12.** Vera confirma el scope que Rob propone para una historia.
- [ ] **CU-13.** Vera ajusta el scope agregando un componente que Rob no consideró.
- [ ] **CU-14.** Vera espera el análisis profundo y observa la narración.
- [ ] **CU-15.** Vera lee el plan en el muro y discute con Rob hasta cerrarlo.
- [ ] **CU-16.** Diego comenta en el muro y Rob ajusta el plan en respuesta.
- [ ] **CU-17.** Vera aprueba el plan: elige modo y se cumple el quórum.
- [ ] **CU-18.** Vera cancela un plan; queda guardado para retomar después.
- [ ] **CU-19.** Pablo es invitado al muro por requisito de quórum (módulo de auth).

### Etapa 4 — Implementación con squad (7 casos de uso)

- [ ] **CU-20.** Vera ve el squad arrancar y se familiariza con la pantalla F1.
- [ ] **CU-21.** Vera hace drill-down en un agente para ver qué está haciendo.
- [ ] **CU-22.** Vera recibe una notificación interrupting de un checkpoint y lo resuelve.
- [ ] **CU-23.** Vera aprueba un fix de test propuesto en checkpoint (propose-diff-then-approve).
- [ ] **CU-24.** Vera usa double-texting para dar instrucciones al squad sin reiniciarlo.
- [ ] **CU-25.** Vera ve el costo desbordarse y el squad pausa automáticamente.
- [ ] **CU-26.** El squad termina, Vera recibe el reporte de cierre.
- [ ] **CU-27.** Diego mira el squad activo (sin permisos de aprobar) y aprende cómo Rob descompone.

### Etapa 5 — Pre-merge Gate (5 casos de uso)

- [ ] **CU-28.** Vera ve las validaciones automatizadas pasar y el PR draft abrirse.
- [ ] **CU-29.** Vera ve el plan de pruebas manuales en Jira (espejado en ROBOK).
- [ ] **CU-30.** Diego marca una prueba manual como fallida y se abre mini-muro.
- [ ] **CU-31.** Vera resuelve el mini-muro: clasifica como "fix directo" y vuelve al squad.
- [ ] **CU-32.** El PR se mergea y Rob escucha el pipeline post-merge, notificando un fallo.

### Vistas auxiliares (3 casos de uso)

- [ ] **CU-33.** Vera entra al mapa de componentes para entender el sistema antes de planning.
- [ ] **CU-34.** Vera revisa una versión antigua del mapa para auditar una decisión de hace 6 meses.
- [ ] **CU-35.** Marisol provisiona un nuevo producto en el panel del tenant y asigna un Dev Lead.

**Total: 35 casos de uso en V1.**

---

## Patrones recurrentes — frases que deben aparecer

Hay frases del producto que tienen que estar literalmente en los casos de uso porque son no-negociables. Si un caso de uso describe el comportamiento contrario, está mal.

| Patrón                            | Cómo debe aparecer en casos de uso                                     |
| --------------------------------- | ---------------------------------------------------------------------- |
| **Inferencia con confirmación**   | "Rob propone X. Vera confirma o corrige antes de que Rob actúe."        |
| **El squad espera al humano**     | "El squad pausa. NO avanza sin la respuesta de Vera."                   |
| **Demostrar entendimiento**       | "Rob muestra qué entendió: <bloques concretos>. NO solo dice 'listo'." |
| **Validación conversacional**     | "Vera valida preguntando, no haciendo clic en 'confirmar'."             |
| **Decisión bidimensional**        | "Vera elige el modo (Express / Estándar / Económico / Sprint-pace)."    |
| **Propose-diff-then-approve**     | "Rob propone el diff. Vera aprueba el diff explícitamente."             |
| **Status en 4 capas**             | "Vera ve la novedad en <capa: ambient/glanceable/interrupting/summary>" |
| **External signal listening**     | "El squad sigue vivo escuchando el pipeline después del PR."            |
| **Mostrar trabajo, no logs**      | "La timeline editorial muestra eventos importantes, no tool calls."     |

---

## Cómo evitar sobreingeniería al escribir casos de uso

**Síntomas de sobreingeniería:**

- 🚩 Estás describiendo un caso de uso que cubre "varias historias en paralelo" → hacelos por separado, uno por historia.
- 🚩 El caso de uso se ramifica en >5 caminos alternativos → algo está demasiado cargado, partilo.
- 🚩 El caso de uso requiere features que NO están en V1 (mobile, embed Slack, real-time collaboration) → recordar la lista de "lo que NO está en V1".
- 🚩 Estás inventando una pantalla nueva para resolver un caso de uso → revisá si una pantalla existente del inventario lo cubre.
- 🚩 El caso de uso describe "el sistema decide X" como si fuera autónomo → revisar el principio "el humano lidera, los agentes asisten".

**Síntomas de buen caso de uso:**

- ✅ Una persona (Vera, Diego, Marisol, Pablo) en una pantalla del inventario.
- ✅ El happy path se cuenta en 5-10 pasos.
- ✅ Las variantes son 1-3, no 8.
- ✅ Cada paso menciona qué se ve (componente UI) y qué pasa (acción).
- ✅ Los nombres salen del glosario sin inventos.
- ✅ El resultado final es un cambio observable.

---

## Consejo metodológico para el agente que va a hacer esto

**Orden recomendado de trabajo:**

1. **Empezar por las Etapas 4 y 3** (los casos de uso del Squad y el Muro). Son los más importantes para el prototipo.
2. **Después la Etapa 2** (selección de sprint) — es la "puerta de entrada" al ciclo.
3. **Después la Etapa 1** (onboarding) — es one-time pero importante para la primera impresión del prototipo.
4. **Después la Etapa 5** (Pre-merge Gate) — más complejo, conviene tenerlo claro al final.
5. **Por último las vistas auxiliares y el admin.**

**Para cada caso de uso, hacer en este orden:**

1. Leer la sección correspondiente del journey en ROBOK v5.
2. Identificar el actor (mirar persona en doc 01).
3. Identificar la pantalla principal (mirar inventario en doc 03).
4. Listar los componentes que aparecen (mirar doc 04).
5. **Recién entonces escribir el caso de uso** usando la plantilla.

**Si en algún momento sentís que un caso de uso "no cuaja":**

- Revisá si estás mezclando dos casos de uso en uno → partir.
- Revisá si la pantalla principal está bien elegida → hay 18 pantallas, hay UNA correcta.
- Revisá si estás inventando un término o una pantalla → no inventar, agregar al doc base primero.

---

## Lista corta — qué entregar al final

Cuando termines de escribir los 35 casos de uso, vas a tener:

- **35 archivos** de casos de uso (uno por caso de uso) o **un solo archivo grande** con secciones (a elección — pero si son 35, valga la pena partirlos).
- **Una tabla resumen** con: ID, título, actor, pantalla principal, etapa.
- **Un mapa de "casos de uso por pantalla"**: para cada una de las 18 pantallas, qué casos de uso la usan.
- **Un mapa de "casos de uso por persona"**: para Vera, Diego, Marisol, Pablo, qué casos de uso protagonizan.

Esto último es lo que el agente que arme el prototipo HTML va a usar para saber **qué pantallas dibujar primero**.

---

## Cierre — el siguiente paso después de los casos de uso

Después de tener los casos de uso, el flujo de trabajo continúa así:

1. **Casos de uso completos** ← este doc te lleva acá
2. **Priorización para el prototipo** → revisar la pasada 1, 2, 3, 4 del doc 03 (inventario)
3. **Prototipo HTML pasada 1**: 3 pantallas más distintivas (F1 Squad, E3 Muro, B3 Insight)
4. **Prototipo HTML pasada 2**: 3 pantallas core (C1 Hub, D1 Backlog, F3 Checkpoint)
5. **Prototipo HTML pasada 3**: 4 pantallas de soporte
6. **Prototipo HTML pasada 4**: el resto

Y eso es lo que vas a poder llevar a una sesión de Figma-equivalente o a una demo.

---

## Anti-patrones específicos a evitar

Cosas que el agente debería NO hacer al escribir los casos de uso, listadas explícitamente:

1. **No describir "lo que pasa por dentro" del Context Manager o del Planner.** Los casos de uso son de UI, no de arquitectura.
2. **No incluir SLAs o métricas técnicas.** "Rob responde en <2s" no va en un caso de uso, va en un NFR.
3. **No usar lenguaje técnico de implementación.** "El estado se persiste en una transacción ACID" no aparece — sí aparece "el estado queda guardado".
4. **No describir el comportamiento del LLM en sí.** "El modelo elige el mejor approach" no va — sí va "Rob propone un approach con justificación".
5. **No usar nombres genéricos como "el usuario".** Siempre Vera, Diego, Marisol, Pablo.
6. **No reutilizar nombres de productos reales del cliente.** Promise Engine y Ratings & Reviews son ejemplos genéricos para los docs.
7. **No inventar checkpoints.** Los checkpoints existen donde el plan los pone — no son sorpresa para el caso de uso.
8. **No mezclar V1 con V2.** Si un caso de uso requiere features de V2 (auto-fix automático, embed Slack), ese caso de uso se posterga, NO se escribe a medias.

---

## Cierre del cierre

Si seguiste esta guía, los casos de uso van a ser:

- **Consistentes** entre sí (mismo formato, mismos términos).
- **Mapeables a pantallas concretas** del prototipo HTML.
- **Honestos sobre el alcance V1** (sin features fantasma).
- **Útiles para el constructor del prototipo** (componentes UI explicitados, estados claros).
- **Verificables** contra los principios de ROBOK v5 (los gates humanos son no-negociables, etc).

Y el constructor del prototipo va a poder decir: "ok, esta pantalla resuelve los CU-15, CU-16, CU-19. Estos son los estados que tengo que mostrar."

Eso es lo que querés tener antes de empezar a hacer HTML.
