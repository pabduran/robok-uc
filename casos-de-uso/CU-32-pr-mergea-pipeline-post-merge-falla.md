# CU-32. El PR se mergea y Rob escucha el pipeline post-merge, notificando un fallo

## Identidad

- **Actor**: Vera (Dev Lead)
- **Pantalla principal**: G2. Pre-merge Gate dashboard (sección "External signal listening")
- **Pantallas secundarias**: PR de GitHub (externo, accesible vía link).
- **Pre-condición**: Las pruebas manuales pasaron (CU-29 happy path o CU-31 con fix). Vera hizo code review humano del PR y aprobó. El PR se mergeó a main. El squad NO se desactivó — quedó en estado in-flight escuchando señales del pipeline post-merge (workflows de GitHub Actions, SAST, dependency scan).
- **Post-condición**: El pipeline post-merge corrió. Rob escuchó las señales. Detectó un fallo (ej. tests de regresión rojos). Diagnostica inteligentemente (infra / flaky / código real). Notifica a Vera con contexto. **En V1 Rob NO actúa solo — solo informa**.
- **Etapa del journey**: Etapa 5 — Pre-merge Gate
- **Frecuencia esperada**: 1 de cada 3–5 historias post-merge tiene alguna señal (la mayoría no, pero cuando aparece debe estar claro).

## Disparador

El PR se mergeó. GitHub Actions arranca el pipeline post-merge (suite de regresión, deploy a staging, smoke tests). Algunos workflows fallan o pasan con problemas. Rob escucha los eventos.

## Flujo principal (happy path) — pipeline pasa todo OK

1. Vera hizo code review y aprobó el PR. Click en "Merge to main" en GitHub.
2. ROBOK detecta el merge vía webhook. La sección "PR" en G2 cambia a "🚀 Mergeado · ver PR ↗".
3. ROBOK marca el cycle time como cerrado: "Cycle time total: 2 días 6 horas desde 'Rob, dale' (E3) hasta merge."
4. La sección "External signal listening" en G2 pasa a "🟢 Activo · escuchando workflows post-merge". El squad sigue vivo escuchando el pipeline después del PR.
5. Workflows de GitHub Actions arrancan. ROBOK los registra en la sección como un feed:
   - "⏳ Test suite de regresión · en curso"
   - "⏳ Deploy a staging · en curso"
   - "⏳ Smoke tests · en cola"
6. Pasan los minutos. Workflows van terminando:
   - "✅ Test suite de regresión · 1.247 pasaron, 0 fallos"
   - "✅ Deploy a staging · OK"
   - "✅ Smoke tests · OK"
7. ROBOK actualiza la sección: "🟢 Pipeline post-merge OK · historia cerrada · cycle time registrado". El squad se desactiva. El badge ambient del producto refleja "Sin historias en curso".

## Variantes

### V1. Pipeline post-merge falla (test de regresión)

Después del merge, el workflow "Test suite de regresión" falla en 2 tests no relacionados con la historia de PROM-1234.

1. Rob escucha la falla. La sección "External signal listening" en G2 cambia a "🔴 Fallo en pipeline post-merge".
2. Rob diagnostica inteligentemente y categoriza:
   - **Infra**: ¿el runner se cayó? ¿hubo timeout?
   - **Flaky**: ¿estos tests fallan intermitentemente en historial reciente? Rob revisa los últimos 30 días — el primer test fallaba ya en 2 builds anteriores (flaky), el segundo es nuevo.
   - **Código real**: ¿el cambio de la historia introdujo regresión?
3. Rob redacta el diagnóstico:
   - "Test 1 (`test_promo_old_format`): **FLAKY** — falló 2 veces en últimos 30 días sin cambios de código. No relacionado con PROM-1234."
   - "Test 2 (`test_quote_with_legacy_currency`): **CÓDIGO REAL** — el refactor de pricing afectó el manejo del campo `currency` en el flujo legacy. Regresión real."
4. ROBOK notifica a Vera (capa Interrupting porque el diagnóstico marcó "código real"):
   - Notificación + Slack/Teams: "🔴 Pipeline post-merge falló · PROM-1234 · 1 regresión real detectada · [Ver en ROBOK]".
5. Vera click. Entra a G2, sección "External signal listening" expandida con el diagnóstico completo.
6. **En V1 Rob NO actúa solo — solo informa**. Vera decide qué hacer:
   - Abrir nueva historia en Jira para fixear la regresión (separada, no como ajuste de PROM-1234 — el merge ya pasó).
   - Si la regresión es crítica: rollback manual del merge (acción humana, no automatizada en V1).
   - Si es menor y se puede arreglar después: crear ticket y planificar.
7. Vera decide abrir un ticket nuevo PROM-1235 para la regresión y dejar PROM-1234 cerrado.

### V2. Pipeline post-merge falla (SAST nueva)

Después del merge, SAST detecta una vulnerabilidad nueva (no detectada en pre-merge porque era contra dependencias actualizadas durante el deploy). Rob diagnostica:
- Severidad: HIGH.
- Componente afectado: módulo de pricing.
- Origen: dependencia transitiva actualizada en el deploy.
Notifica a Vera + 🛡️ Pablo (Security Officer está suscripto a alertas de seguridad post-merge). En V1 Rob informa, propone uno de los 4 caminos del external signal listening (típicamente "vuelta a Etapa 3" o "auto-fix" según severidad), pero **no lo aplica** — Vera tiene que decidir.

### V3. Pipeline pasa pero con advertencia menor

Code coverage bajó del 91% al 88%. Rob lo registra como "⚠️ Coverage bajó 3% post-merge — no bloqueante". Notifica a Vera en capa Summary (no urgente). Vera puede decidir si pedir tests adicionales en futuras iteraciones o aceptar el trade-off.

## Caminos alternativos / errores

- **Si el pipeline post-merge tarda mucho** (ej. >2h): ROBOK no lo oculta. La sección muestra "⏳ Pipeline en curso · más lento de lo esperado, escaneando dependencias".
- **Si Rob no puede diagnosticar la falla** (caso raro): notificación dice "Fallo detectado, no pude clasificar. Ver workflow ↗". Vera tiene que investigar manualmente.
- **Si Vera quiere un auto-fix sobre el fallo**: en V1 NO está disponible. ROBOK informa que esa capability viene en V2.

## Decisiones del usuario en este flujo

- Si abrir ticket nuevo, hacer rollback, o ignorar el fallo del pipeline.
- Si el diagnóstico de Rob (flaky vs código real) es correcto o requiere segunda mirada.
- Cómo priorizar el fix de regresión vs otras historias del sprint.

## Componentes UI involucrados

- 1. Header con badge ambient (puede pasar a 🟡 si la regresión es importante)
- 6. Timeline editorial (variante condensada — registra eventos del pipeline post-merge)
- 10. Notificación interrupting (cuando Rob detecta código real)
- 14. Pill de estado (en cada workflow del pipeline)

## Notas para el prototipo HTML

- La sección "External signal listening" en G2 debe ser un feed scrollable de workflows con su estado.
- Estados sugeridos como archivos separados:
  - `g2-pipeline-post-merge-en-curso.html` (workflows ⏳)
  - `g2-pipeline-post-merge-ok.html` (todos ✅, historia cerrada)
  - `g2-pipeline-post-merge-falla-flaky.html` (V1: una falla flaky, no escalada)
  - `g2-pipeline-post-merge-falla-codigo-real.html` (V1: regresión real con diagnóstico de Rob y notificación interrupting)
  - `g2-pipeline-post-merge-vulnerabilidad.html` (V2: SAST con HIGH severity)

## Referencias

- Journey: ROBOK_v5.md §1.8 (External signal listening — el squad sigue vivo después del PR) + §1.9 (Etapa 5, "Listening del pipeline post-merge sin auto-fix" en V1).
- Inventario: 03-inventario-pantallas.md §G2 (estado "Post-merge con señal externa").
- Decisiones cerradas: ROBOK_v5.md §Decisiones · "Listening de pipeline en V1 es informativo, no actuativo".
- Componentes UI: 04-componentes-ui.md §6 (timeline), §10 (notificación interrupting).
- Principios involucrados: P3 (V1 Rob informa, NO actúa solo · el humano decide), P5 (diagnóstico inteligente con clasificación, no solo "falló"), P7 (cada cambio en el pipeline post-merge queda anclado al snapshot del producto).
