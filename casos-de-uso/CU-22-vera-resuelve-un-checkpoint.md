# CU-22. Vera recibe una notificación interrupting de un checkpoint y lo resuelve

## Identidad

- **Actor**: Vera (Dev Lead)
- **Pantalla principal**: F3. Pantalla de checkpoint (modal bloqueante)
- **Pantallas secundarias**: cualquier pantalla de ROBOK donde Vera estaba (la notificación llega como overlay) o Slack/Teams (link entrante).
- **Pre-condición**: El squad de PROM-1234 está activo. Llegó a un checkpoint duro definido en el plan (ej. "validar el contrato del endpoint antes de codear T-3 que depende de T-2"). El squad pausó. NO avanza sin la respuesta de Vera.
- **Post-condición**: Vera resolvió el checkpoint con una de tres acciones: aprobar, pedir cambios o detener squad. El squad reanuda según la decisión.
- **Etapa del journey**: Etapa 4 — Implementación con squad
- **Frecuencia esperada**: 1–4 veces por historia (depende del modo y del número de checkpoints en el plan).

## Disparador

ROBOK detecta que el squad llegó a un checkpoint definido en el plan. Lanza la **notificación interrupting** (componente 10) por dos canales: overlay en cualquier pantalla de ROBOK donde Vera esté + notificación a Slack/Teams con link.

## Flujo principal (happy path)

1. Vera está revisando el sprint en D1 de otro producto. ROBOK lanza la notificación interrupting como toast persistente en la esquina superior derecha:
   > 🟡 Squad esperándote · PROM-1234
   > Implementer pidió tu input sobre el contrato del endpoint
   > [Ver checkpoint] [Más tarde]
2. En paralelo, Vera recibe la misma alerta en Slack: "🟡 Squad esperándote · PROM-1234 — checkpoint requiere acción · [Abrir en ROBOK]". V1 = link, no aprobación desde Slack.
3. Vera click en "Ver checkpoint" desde el toast (o desde el link de Slack). ROBOK la lleva a F3.
4. F3 abre como modal bloqueante con el contenido del checkpoint:
   - **Por qué el squad pausó**: "Checkpoint del plan — validar contrato del endpoint POST /v1/quote antes de implementar T-3 que depende de la firma."
   - **Lo que el squad hizo hasta ahora**: resumen breve (T-1 cerrada, T-2 borradoreada con la firma propuesta).
   - **Lo que propone hacer a continuación**: "Implementer #1 propone esta firma para el endpoint: `POST /v1/quote { items: [...], promo_code?: str } → { quote_id: str, total: Decimal, expires_at: datetime }`. Si aprobás, T-3 arranca con esa firma."
   - Botones: **[Aprobar]** · **[Pedir cambios]** (con caja de texto) · **[Detener squad]**.
5. Vera lee. La firma le parece bien. Click en "Aprobar".
6. ROBOK marca el checkpoint como ✅ resuelto, cierra F3 y reanuda al squad. La timeline editorial registra el evento "✅ Checkpoint del contrato resuelto por Vera · T-3 arrancado".
7. La notificación interrupting desaparece.

## Variantes

### V1. Vera pide cambios

Vera ve la firma propuesta y considera que falta el campo `currency` para soportar productos multi-moneda. Click en "Pedir cambios". Caja de texto: escribe "agregá `currency: str` (ISO 4217) al body, default 'USD'. Es para soportar el rollout multi-moneda del próximo trimestre". Envía. ROBOK reanuda al squad con el feedback. El Implementer ajusta y vuelve a checkpoint con la nueva firma para confirmar (segunda pasada por F3).

### V2. Vera detiene el squad

Vera lee el checkpoint y se da cuenta de que la historia debe replantearse (ej. la firma propuesta evidencia que el plan asumió un modelo de datos incorrecto). Click en "Detener squad". ROBOK pide confirmación obligatoria ("Detener significa que el squad termina; el workspace queda preservado pero el squad no continúa. ¿Seguro?"). Vera confirma. El squad termina, F1 muestra "🛑 Detenido por decisión humana". Vera puede volver a E3 a replanificar (genera un nuevo plan o ajusta el existente).

### V3. Vera no responde a tiempo

Vera está en una reunión larga. La notificación interrupting queda visible. Después de N horas configurables (ej. 4h), ROBOK manda un recordatorio (otro toast + Slack). Si pasa más tiempo (ej. 24h), el badge ambient del producto cambia a 🟡 "Squad esperando hace mucho — PROM-1234". El squad NO avanza solo. Eventualmente Vera vuelve y resuelve.

### V4. Vera posterga con "Más tarde"

Vera ve el toast pero está en algo urgente. Click en "Más tarde". El toast minimiza a un badge en el header. Vera puede volver al checkpoint cuando quiera desde el badge o desde C2.

## Caminos alternativos / errores

- **Si Vera entra a F3 desde Slack en mobile**: V1 redirige al desktop (no hay vista mobile de F3 en V1). El link funciona, la pantalla no se renderiza bien — esperado.
- **Si el checkpoint requiere quórum extra** (raro pero posible): F3 muestra una variante con la lista de aprobadores adicionales y queda en espera del segundo (similar a CU-17 V1).

## Decisiones del usuario en este flujo

- Aprobar / Pedir cambios / Detener squad.
- Si pedir cambios: ser específica con el feedback.
- Si detener: aceptar que la historia vuelve a Etapa 3.
- Si "Más tarde": posponer pero saber que el squad no avanza.

## Componentes UI involucrados

- 10. Notificación interrupting (toast en ROBOK + Slack/Teams)
- 9. Pantalla de checkpoint (modal bloqueante — la pieza central de F3)
- 1. Header con badge ambient (cambia a 🟡 mientras el checkpoint está pendiente)

## Notas para el prototipo HTML

- F3 es modal bloqueante con backdrop. Usar `<dialog>` HTML5 o div con backdrop CSS.
- Estados sugeridos como archivos separados:
  - `f3-checkpoint-estandar.html` (caso normal: aprobar firma de endpoint)
  - `f3-checkpoint-pedir-cambios.html` (V1: caja de texto desplegada)
  - `f3-checkpoint-detener-confirm.html` (V2: confirmación obligatoria de detener)
  - `f1-con-toast-checkpoint.html` (vista de F1 con la notificación interrupting visible arriba a la derecha)

## Referencias

- Journey: ROBOK_v5.md §1.8 — Etapa 4, sección "Squad espera al humano en checkpoints — siempre" y "Status en 4 capas — Interrupting".
- Inventario: 03-inventario-pantallas.md §F3.
- Componentes UI: 04-componentes-ui.md §9 (pantalla de checkpoint), §10 (notificación interrupting).
- Principios involucrados: P3 (el squad espera al humano · gates no-negociables), P6 (notificación interrupting es la única capa que demanda atención obligatoria).
