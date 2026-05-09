# CU-05. Vera mira el indexado en curso con narración explícita y entiende qué está pasando

## Identidad

- **Actor**: Vera (Dev Lead)
- **Pantalla principal**: B2. Onboarding — indexado en curso
- **Pantallas secundarias**: B1 (entrada), B3. Insight inicial proactivo (salida).
- **Pre-condición**: Vera completó B1 — fuentes conectadas y equipo mapeado — y disparó "Iniciar indexado".
- **Post-condición**: Las fuentes quedaron indexadas (o algunas marcadas como pendientes/erradas). ROBOK transiciona automáticamente a B3 con el insight inicial.
- **Etapa del journey**: Etapa 1 — Onboarding del producto
- **Frecuencia esperada**: una vez por producto (one-time). Puede repetirse parcialmente si Vera pide un re-indexado on-demand más adelante.

## Disparador

Vera apretó "Iniciar indexado" en B1.

## Flujo principal (happy path)

1. ROBOK muestra B2 con narración explícita por fuente. NO es un spinner mudo.
2. Vera ve líneas que avanzan visiblemente:
   - "📚 Indexando 142 páginas de Confluence — 67 procesadas"
   - "💻 Leyendo 4 repos — promise-api ✅ promise-worker ✅ promise-ui (en curso) · promise-tools (en cola)"
   - "🏗️ Mapeando ambientes desde IaC — staging ✅ prod (en curso)"
   - "📋 Cargando últimos 30 días de Jira — 87 tickets recorridos"
3. Vera entiende qué está pasando sin tener que adivinar. La narración es lo que materializa "transparencia operacional como requisito no-funcional fuerte" — el silencio es la peor fricción posible.
4. Vera puede cerrar la pestaña; el indexado sigue en background.
5. Cuando todas las fuentes terminan, ROBOK transiciona automáticamente a B3 — Insight inicial proactivo (cubierto en CU-06 y CU-07).

## Variantes

### V1. Vera vuelve a entrar a media indexación

Si Vera cerró la pestaña y vuelve unos minutos después (entrando al producto desde A2), ROBOK la lleva directo a B2 con el progreso actualizado, no reiniciado. Las fuentes ya completas aparecen con ✅, las en curso con su progreso real.

### V2. Una fuente falla mid-indexado

Si el token de Confluence expiró durante el indexado, el bloque correspondiente pasa a "❌ Error — token expirado · [Reintentar]". Las otras fuentes siguen su curso. Vera puede arreglar el token y reintentar sin reiniciar el indexado completo.

## Caminos alternativos / errores

- **Si una fuente queda en error y Vera la salta**: el indexado termina con esa fuente marcada como pendiente. B3 muestra esa información dentro del bloque "🤔 Cosas que no entendí bien" (ej. "no pude leer Confluence — el insight queda con la doc faltante").
- **Si el indexado tarda mucho más de lo esperado**: ROBOK NO oculta el problema. El bloque correspondiente muestra "📚 Confluence — 142/450 páginas (más lento de lo esperado)".

## Decisiones del usuario en este flujo

- Esperar mirando o cerrar y volver más tarde (no bloquea el flujo).
- Reintentar una fuente fallida o seguir sin ella.

## Componentes UI involucrados

- 1. Header con badge ambient (sigue en ⚪ "Sin actividad" hasta que el producto está operativo).

## Notas para el prototipo HTML

- B2 es una pantalla orientada a estado animado. No requiere interactividad real.
- Estados sugeridos como archivos separados:
  - `b2-indexando-inicio.html` (todas las fuentes en "iniciando")
  - `b2-indexando-progreso.html` (algunas ✅, otras "en curso")
  - `b2-indexando-error-fuente.html` (V2: una fuente en ❌)
  - `b2-indexando-completo.html` (todas en ✅, transición a B3)
- Para simular avance, basta con CSS animations o tres archivos con estados sucesivos.

## Referencias

- Journey: ROBOK_v5.md §1.5 — Etapa 1, paso F del flujo detallado, y nota sobre "transparencia operacional como requisito no-funcional fuerte".
- Inventario: 03-inventario-pantallas.md §B2.
- Principios involucrados: P5 (demostrar entendimiento, no declararlo — la narración es la primera forma de mostrar trabajo), P6 (presencia adaptativa — Vera puede mirar o no, pero nunca está a oscuras).
