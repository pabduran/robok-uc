# CU-14. Vera espera el análisis profundo y observa la narración

## Identidad

- **Actor**: Vera (Dev Lead)
- **Pantalla principal**: E2. Análisis profundo en curso
- **Pantallas secundarias**: E1 (entrada), E3. Muro de discusión del plan (salida).
- **Pre-condición**: Vera confirmó el scope en E1 (CU-12 o CU-13). Rob arrancó el análisis profundo sobre los componentes confirmados.
- **Post-condición**: Análisis terminado. Plan publicado. ROBOK transiciona a E3 con el plan ya en el muro listo para discutir.
- **Etapa del journey**: Etapa 3 — Análisis profundo y plan
- **Frecuencia esperada**: una vez por historia delegada. La duración varía por modo (Express en minutos, Sprint-pace en horas).

## Disparador

Vera apretó "Confirmar scope" en E1.

## Flujo principal (happy path)

1. ROBOK muestra E2 — Análisis profundo en curso. La pantalla es una narración explícita por fase, similar a la del indexado (B2) pero más rica.
2. Vera ve líneas que avanzan visiblemente con emoji y descripción humana:
   - "🔍 Leyendo módulos del scope confirmado: pricing, billing-api"
   - "📐 Detectando patrones arquitectónicos relevantes — patrón hexagonal confirmado"
   - "📊 Buscando históricos similares — encontré 3 candidatos (PROM-892, PROM-743, PROM-401)"
   - "🎨 Architect borroneando descomposición — 3 tareas tentativas"
   - "🛡️ Security Reviewer revisando dependencias y modelo de auth — sin alertas hasta ahora"
   - "💵 Refinando matriz costo × tiempo con datos del análisis"
3. Vera entiende qué está pasando sin tener que adivinar. El silencio es la peor fricción posible — esta narración es lo que materializa el principio de transparencia operacional.
4. Vera puede cerrar la pestaña; el análisis sigue en background.
5. Cuando el análisis termina, ROBOK transiciona automáticamente a E3 — Muro de discusión del plan, con el plan ya publicado por Rob (cubierto en CU-15).

## Variantes

### V1. Vera vuelve a media análisis

Si Vera cerró E2 y vuelve unos minutos después (entrando al producto desde A2 → C1 → C2 "Mis historias en curso"), ROBOK la lleva directo a E2 con la narración actualizada. Las fases ya completas aparecen con ✅, las en curso con su estado.

### V2. Una fase del análisis tarda más de lo esperado

Si "🔍 Leyendo módulos" tarda más de lo proyectado, Rob no lo oculta. La línea muestra "🔍 Leyendo módulos del scope — más lento de lo esperado, hay mucha doc en pricing". Sin spin engaño.

### V3. Modo Sprint-pace o Económico

Si Vera eligió un modo lento, E2 muestra explícitamente "Modo Económico — el análisis se procesa en batch, terminación estimada en 2–3 horas". Vera puede cerrar tranquila; ROBOK le va a notificar por la notificación interrupting cuando el plan esté publicado.

## Caminos alternativos / errores

- **Si una fase falla** (ej. Rob no puede leer el repo porque el token expiró): la línea pasa a "❌ Error — token expirado · [Reintentar / Ver detalle]". Las otras fases siguen si pueden, o el análisis se pausa pidiendo intervención.
- **Si Rob detecta a media análisis que el scope es insuficiente**: pausa el análisis y vuelve a E1 con un mensaje "Necesito incluir `promise-worker` para analizar bien — encontré una dependencia que no estaba en el scope. ¿Confirmás?". Aplica el principio de inferencia con confirmación humana — Rob propone, Vera confirma.

## Decisiones del usuario en este flujo

- Esperar mirando o cerrar y volver más tarde (no bloquea).
- Si una fase falla: reintentar o cancelar el análisis.
- Si Rob propone ampliar scope: confirmar o rechazar.

## Componentes UI involucrados

- 1. Header con badge ambient (puede pasar a 🟡 "Squad esperándote" si Rob propone ampliar scope)
- 11. Insight inicial / proactivo — variante acotada (la narración de E2 reusa la estética del insight de B3, en versión textual y temporal)

## Notas para el prototipo HTML

- E2 es una versión textual del Squad — una timeline animada de la narración.
- Estados sugeridos como archivos separados:
  - `e2-analisis-arrancando.html` (primeras 2 fases en curso)
  - `e2-analisis-mid.html` (5 fases con varias completas, una en curso)
  - `e2-analisis-completo.html` (todas ✅, transición a E3)
  - `e2-analisis-error.html` (V con una fase en ❌)
- Para simular avance, CSS animations o tres archivos sucesivos con setTimeout.

## Referencias

- Journey: ROBOK_v5.md §1.7 — Etapa 3, paso E del flujo detallado ("Rob analiza en profundidad — loader con narración visible").
- Inventario: 03-inventario-pantallas.md §E2.
- Principios involucrados: P5 (demostrar entendimiento — la narración muestra el trabajo), P6 (presencia adaptativa — Vera puede mirar o no, nunca está a oscuras).
