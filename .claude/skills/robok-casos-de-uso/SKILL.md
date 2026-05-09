---
name: robok-casos-de-uso
description: Use this skill when the user asks to write, review, refine, modify, or audit ROBOK use cases (casos de uso). Triggers include phrases like "escribime los casos de uso", "agregá el CU-XX", "revisá los casos de uso de la Etapa N", "este caso de uso está mal" or any request that would create or edit a use case file. Do NOT use for general ROBOK questions or for prototype HTML work.
---

# Skill — Escribir y mantener casos de uso de ROBOK

Esta skill te guía para producir casos de uso de ROBOK con formato consistente, sin inventar términos ni pantallas, y respetando los principios del producto.

## Pasos obligatorios antes de empezar

1. **Leé estos archivos en este orden, en cada sesión, sin saltearte ninguno:**
   - `ROBOK_v5.md` (la fuente de verdad funcional — los journeys de las 5 Etapas y los principios)
   - `00-README.md` (índice general)
   - `01-personas-y-arquetipos.md` (Vera, Diego, Marisol, Pablo)
   - `02-glosario-y-modelo-conceptual.md` (términos canónicos y anti-glosario)
   - `03-inventario-pantallas.md` (las 18 pantallas existentes)
   - `04-componentes-ui.md` (los 15 componentes)
   - `05-guia-casos-de-uso.md` (la plantilla y la lista de los 35 CU)

2. **Verificá qué casos de uso ya existen** en la carpeta donde se guarden (típicamente `casos-de-uso/` en el repo). No reescribas lo que ya está.

## Reglas duras (no negociables)

- **Solo usar términos del glosario (doc 02).** Si necesitás un término nuevo, primero agregalo al glosario, después usalo. Mismo principio para anti-glosario: nunca uses "bot", "workflow", "run", "log", "documento de plan", "confirmar".
- **Solo referenciar pantallas del inventario (doc 03).** Las 18 pantallas tienen códigos: A1, A2, B1, B2, B3, C1, C2, C3, C4, D1, D2, E1, E2, E3, E4, F1, F2, F3, G1, G2, G3, H1. Si un caso de uso "necesita" una pantalla nueva, primero agregala al inventario, después escribí el caso de uso.
- **Actores SIEMPRE con nombre propio.** Vera (Dev Lead), Diego (Developer), Marisol (Admin de ROBOK), Pablo (Stakeholder externo). Nunca "el usuario".
- **Granularidad correcta.** Un caso de uso = un actor + una pantalla principal + un resultado observable. Si tu CU necesita >3 pantallas principales, partilo. Si dura <5 pasos, está bien.
- **Respetá los principios de ROBOK v5.** Los más críticos: el squad espera al humano en checkpoints, inferencia con confirmación humana, demostrar entendimiento (no declararlo), propose-diff-then-approve para tests.
- **No mezcles V1 con V2/V3.** Si un CU requiere features V2 (auto-fix automático, embed Slack, real-time collab, mobile), ese CU se posterga, NO se escribe a medias.

## Plantilla obligatoria

Cada caso de uso usa esta estructura (definida en el doc 05):

```markdown
# CU-XX. <Título descriptivo en imperativo>

## Identidad
- **Actor**: <Vera / Diego / Marisol / Pablo>
- **Pantalla principal**: <CódigoPantalla. Nombre>
- **Pantallas secundarias**: <opcional>
- **Pre-condición**: <qué tiene que estar pasando antes>
- **Post-condición**: <qué cambió después>
- **Etapa del journey**: <Etapa 1 / 2 / 3 / 4 / 5 / Admin>
- **Frecuencia esperada**: <varias veces al día / por sprint / por onboarding / esporádico>

## Disparador
<Qué hace que el actor llegue a esta pantalla>

## Flujo principal (happy path)
1. <Paso 1>
2. <Paso 2>
...
N. <Resultado final>

## Variantes
### V1. <Nombre>
<Diferencia con happy path. Solo si es significativa.>

## Caminos alternativos / errores
- **Si <condición>**: <qué pasa>

## Decisiones del usuario en este flujo
<Lista de momentos de decisión. Cada uno = una variante del HTML.>

## Componentes UI involucrados
<Lista del doc 04. Ej: 1. Header, 5. Status Bar, 4. Card de agente.>

## Notas para el prototipo HTML
<Estados a mostrar, transiciones, datos mock recomendados.>

## Referencias
- Journey: <enlace a sección de ROBOK_v5.md>
- Principios involucrados: <P1, P5, etc>
```

## Orden recomendado para escribir los 35 CU

**Cronológico, NO por impacto demostrativo.** Cada CU se apoya en el contexto del anterior, así evitás inconsistencias.

1. **Etapa 0 — Acceso**: CU-01 a CU-03
2. **Etapa 1 — Onboarding**: CU-04 a CU-07
3. **Etapa 2 — Selección**: CU-08 a CU-11
4. **Etapa 3 — Análisis y plan**: CU-12 a CU-19
5. **Etapa 4 — Implementación**: CU-20 a CU-27
6. **Etapa 5 — Pre-merge Gate**: CU-28 a CU-32
7. **Vistas auxiliares y admin**: CU-33 a CU-35

(El orden por impacto demostrativo aplica al PROTOTIPO HTML, no a los casos de uso — eso vive en otra skill.)

## Anti-patrones a evitar

Antes de finalizar un caso de uso, revisá esta checklist mental:

- 🚩 ¿Estoy describiendo "lo que pasa por dentro" del Context Manager o del Planner? → eliminá. Los CU son de UI, no de arquitectura.
- 🚩 ¿Incluyo SLAs o métricas técnicas (ej. "Rob responde en <2s")? → eliminá. Eso es un NFR.
- 🚩 ¿Uso lenguaje de implementación ("estado se persiste en transacción ACID")? → reemplazá por "el estado queda guardado".
- 🚩 ¿Describo el comportamiento del LLM ("el modelo elige el mejor approach")? → reemplazá por "Rob propone un approach con justificación".
- 🚩 ¿Uso "el usuario" en algún lugar? → reemplazá por Vera/Diego/Marisol/Pablo.
- 🚩 ¿Inventé un checkpoint que no estaba en el plan? → eliminá. Los checkpoints viven en el plan, no son sorpresas.
- 🚩 ¿Mi CU tiene >5 caminos alternativos? → partilo en CU separados.
- 🚩 ¿Mi CU describe "varias historias en paralelo"? → uno por historia.

## Frases canónicas que DEBEN aparecer cuando aplique

Si tu CU describe alguno de estos comportamientos, usá literalmente estas frases (o muy parecidas) — son no-negociables del producto:

| Comportamiento | Frase canónica |
|---|---|
| Inferencia con confirmación | "Rob propone X. Vera confirma o corrige antes de que Rob actúe." |
| Squad espera al humano | "El squad pausa. NO avanza sin la respuesta de Vera." |
| Demostrar entendimiento | "Rob muestra qué entendió: <bloques>. NO solo dice 'listo'." |
| Validación conversacional | "Vera valida preguntando, no haciendo clic en 'confirmar'." |
| Decisión bidimensional | "Vera elige el modo (Express / Estándar / Económico / Sprint-pace)." |
| Propose-diff-then-approve | "Rob propone el diff. Vera aprueba el diff explícitamente." |
| External signal listening | "El squad sigue vivo escuchando el pipeline después del PR." |
| Mostrar trabajo, no logs | "La timeline editorial muestra eventos importantes, no tool calls." |

## Output esperado

Cuando termines un set de casos de uso (ej. los de una Etapa completa), dejá:

1. **Un archivo por CU** en `casos-de-uso/CU-XX-titulo-corto.md` (preferido sobre un solo archivo gigante).
2. **Tabla resumen actualizada** en `casos-de-uso/00-indice.md` con: ID, título, actor, pantalla principal, etapa, estado (draft / revisado / final).
3. **Verificación cruzada**: ningún término fuera del glosario, ninguna pantalla fuera del inventario.

## Si encontrás algo que no calza

Si mientras escribís un caso de uso descubrís que falta una pantalla, un componente, o un término en los docs base:

1. **NO inventes.** Detenete.
2. **Proponé el agregado al doc base** (glosario, inventario o componentes) y pedí confirmación al usuario antes de continuar.
3. **Una vez aprobado**, agregalo al doc base correspondiente, después seguí con el caso de uso.

Esa fricción es deliberada — protege contra deriva.
