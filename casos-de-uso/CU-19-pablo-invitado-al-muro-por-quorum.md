# CU-19. Pablo es invitado al muro por requisito de quórum (módulo de auth)

## Identidad

- **Actor**: Pablo (Stakeholder externo — Security Officer)
- **Pantalla principal**: E3. Muro de discusión del plan (vista con permisos limitados)
- **Pantallas secundarias**: A1. Login / SSO (entrada).
- **Pre-condición**: La historia PROM-1234 toca el módulo de auth. La regla de quórum del producto Promise Engine requiere aprobación de un Security Officer cuando se toca auth. Vera ya aprobó en E4 (CU-17 V1) y el plan quedó en "🟡 En espera de quórum · esperando a Pablo". ROBOK invitó a Pablo al muro. Pablo recibió notificación en Slack/Teams.
- **Post-condición**: Pablo leyó el plan, dejó comentarios y aprobó (o pidió cambios). Si aprobó, el quórum se completa, el squad arranca, y Vera recibe notificación "Quórum cumplido".
- **Etapa del journey**: Etapa 3 — Análisis profundo y plan
- **Frecuencia esperada**: 2–3 veces por semana para Pablo (solo cuando lo invitan por quórum, no entra al día a día).

## Disparador

Pablo recibe notificación: "Vera te invitó al muro de PROM-1234 (toca módulo de auth) · [Ver plan]". Click en el link.

## Flujo principal (happy path)

1. Pablo abre el link. ROBOK lo lleva a A1 — Login / SSO si no estaba autenticado.
2. Pablo entra a E3 — Muro de discusión del plan, con permisos acotados:
   - Lee el plan completo (zona izquierda).
   - Lee la discusión previa (zona derecha) — incluyendo los comentarios de Vera, Rob y Diego (si CU-16 ocurrió).
   - Comenta en el muro.
   - Aprueba o rechaza (botones específicos para aprobadores de quórum).
   - **NO** puede cancelar el plan ni cambiar el modo (no es Dev Lead del producto).
3. Pablo ve un banner especial en el tope: "Te invitaron por requisito de quórum: módulo de auth tocado. Tu aprobación es necesaria para que el squad arranque."
4. Pablo lee el plan. Foco en la sección de seguridad: 🛡️ Security Reviewer ya marcó dos riesgos en la discusión: nuevo endpoint público y dependencia con CVE menor.
5. Pablo lee los comentarios del 🛡️ Security Reviewer (mensaje en el muro con borde rojo o badge "🛡️ Seguridad" según el componente 7). Confirma que el análisis cubre los puntos importantes.
6. Pablo tiene una duda: el endpoint nuevo, ¿queda detrás del WAF de la organización?
7. Pablo escribe en el muro: "@Vera — el endpoint POST /v1/quote ¿queda detrás del WAF de la org? Necesito confirmación antes de aprobar."
8. Vera (que está conectada o entra después por la notificación de mención) responde: "Sí, todos los endpoints públicos van detrás del WAF. Ver ADR-2025-088 que cubre la regla a nivel producto." Vera linkea el ADR existente.
9. Pablo lee el ADR. Conforme. Click en "Aprobar" (botón visible para aprobadores de quórum).
10. ROBOK marca a Pablo como aprobador cumplido. El estado del plan pasa de "🟡 En espera de quórum" a "✅ Quórum cumplido". El squad arranca y Vera recibe notificación.

## Variantes

### V1. Pablo pide cambios (no aprueba directo)

Pablo lee el plan y ve un riesgo no atendido (ej. la dependencia nueva tiene una CVE crítica reciente que el Security Reviewer no captó porque su base de datos de CVEs no estaba actualizada). Pablo escribe en el muro un comentario detallado y aprieta "Pedir cambios" en lugar de "Aprobar". El plan queda en estado "🟡 Cambios pedidos por aprobador" y Vera tiene que volver al muro para resolverlo (típicamente: ajustar plan o cancelar).

### V2. Pablo rechaza con justificación

Pablo decide que la historia no debería avanzar (ej. el módulo de auth está en pleno proceso de migración y este cambio chocaría). Click en "Rechazar". ROBOK pide justificación obligatoria. El plan queda en "🛑 Rechazado por aprobador · ver muro". Vera puede ajustar y volver a pedir aprobación, o cancelar el plan (CU-18).

### V3. Pablo entra solo a leer (sin compromiso)

Pablo recibe la notificación pero entra solo a leer el contexto sin actuar. El plan sigue en "🟡 En espera de quórum". Vera puede mandarle recordatorio si pasa demasiado tiempo (ROBOK lo hace automáticamente después de N horas — coherente con la fricción "checkpoint sin respuesta" del journey).

## Caminos alternativos / errores

- **Si Pablo no entiende parte del plan**: puede pedir aclaración en el muro. Rob (o Vera) responden. La aprobación queda en pausa hasta que Pablo se sienta listo.
- **Si Pablo aprueba pero el plan se cancela después**: ROBOK le notifica "El plan que aprobaste fue cancelado · ver muro". Su aprobación queda registrada en el historial para auditoría.

## Decisiones del usuario en este flujo

- Aprobar, pedir cambios o rechazar.
- Comentar antes de aprobar o aprobar directo.
- Pedir contexto adicional o trabajar con lo que está.

## Componentes UI involucrados

- 1. Header con badge ambient (variante para Pablo: muestra solo el contexto del muro al que fue invitado, sin acceso a otras pantallas del producto)
- 7. Avatar y mensaje del muro de discusión (con badge especial para 🛡️ Seguridad y para humanos invitados)
- 14. Pill de estado (en el banner "esperando a Pablo" y en el plan)

## Notas para el prototipo HTML

- Para Pablo, el muro se ve igual que para Vera pero con:
  - Banner de invitación arriba.
  - Botones específicos: "Aprobar", "Pedir cambios", "Rechazar". Sin "Cancelar plan" ni "Dale".
  - El header ambient se reduce: solo nombre del producto y del ticket, sin acceso a sprint/mapa/squad de otros productos.
- Estados sugeridos como archivos separados:
  - `e3-muro-pablo-invitado.html` (vista de Pablo recién entrado, banner amarillo de invitación)
  - `e3-muro-pablo-aprobando.html` (Pablo escribió comentario y va a aprobar)
  - `e3-muro-quorum-cumplido.html` (estado tras la aprobación de Pablo, banner verde "Quórum cumplido")

## Referencias

- Journey: ROBOK_v5.md §1.7 — Etapa 3, "Quórum configurable" + Security Reviewer transversal en Etapa 3.
- Personas: 01-personas-y-arquetipos.md §Persona 4 (Pablo — su frase: "Tráiganme el plan, no el problema").
- Inventario: 03-inventario-pantallas.md §E3 (estado "Plan en espera de quórum").
- Componentes UI: 04-componentes-ui.md §7 (mensaje de Security Reviewer con borde/badge especial).
- Principios involucrados: P3 (gates humanos no-negociables — el quórum es uno), P4 (gobernanza configurable — la regla la definió Vera y ROBOK la respeta), P5 (demostrar entendimiento — Pablo recibe el plan digerido, no el problema).
