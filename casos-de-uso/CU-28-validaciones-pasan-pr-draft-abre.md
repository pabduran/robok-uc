# CU-28. Vera ve las validaciones automatizadas pasar y el PR draft abrirse

## Identidad

- **Actor**: Vera (Dev Lead)
- **Pantalla principal**: G2. Pre-merge Gate dashboard
- **Pantallas secundarias**: G1. Reporte de cierre (entrada).
- **Pre-condición**: El squad cerró Etapa 4 (CU-26). El código vive en la rama `rob/PROM-1234` sin abrir todavía a GitHub. Las validaciones automatizadas se disparan automáticamente: unit tests verificación, lint, type check, SAST, Gitleaks, dependency scan.
- **Post-condición**: Validaciones pasaron (o pasaron con advertencias no-bloqueantes). PR draft abierto en GitHub. El cycle time empieza a "cerrarse" — desde "Rob, dale" en Etapa 3 hasta este punto, ROBOK registra el tiempo.
- **Etapa del journey**: Etapa 5 — Pre-merge Gate
- **Frecuencia esperada**: una vez por historia que llegó a Etapa 5.

## Disparador

Vera transicionó desde G1 a G2 (CU-26 paso 5). ROBOK auto-arrancó las validaciones automatizadas mientras Vera leía el reporte de cierre.

## Flujo principal (happy path)

1. Vera ve G2 — Pre-merge Gate dashboard. La pantalla está orientada a estado, con secciones:
   - **Validaciones automatizadas** (sección superior): lista con checks en curso o completados.
     - ⏳ → ✅ Unit tests verificación · 142 pasaron, 0 fallos
     - ⏳ → ✅ Lint · sin warnings
     - ⏳ → ✅ Type check · sin errores
     - ⏳ → ✅ SAST (Semgrep) · 0 hallazgos
     - ⏳ → ✅ Gitleaks · 0 secretos
     - ⏳ → ⚠️ Dependency scan · 1 CVE menor [Ver]
     - Total: 5/6 OK · 1 advertencia (no bloqueante)
   - **PR draft**: vacío / "Aún no abierto, esperando que las validaciones pasen".
   - **Plan de pruebas manuales**: "Aún no posteado en Jira" (se postea cuando el PR draft abre).
   - **External signal listening**: "Activo después del merge".
   - **Cycle time tracker**: "1 día 4 horas desde 'Rob, dale' (E3 aprobación)".
2. Vera ve los checks pasar uno por uno (animación o transición). El único en amarillo es Dependency scan con una CVE menor.
3. Vera click en "Ver" sobre la CVE menor. Panel lateral muestra: "Dependencia `urllib3` versión 1.26.x tiene CVE-2024-XXXX (severidad LOW). Fix disponible en 1.26.20+. No bloqueante para esta historia, pero conviene actualizar en otro PR."
4. Vera lee. La CVE no afecta esta historia. La marca como "vista, no bloqueante" y deja la actualización para otro ticket.
5. Como las validaciones bloqueantes pasaron (5/6 OK, 1 advertencia no-bloqueante), ROBOK abre automáticamente el **PR draft** en GitHub.
6. La sección "PR draft" se actualiza: link al PR `#1234` con título "PROM-1234 · Refactor pricing calculator [DRAFT]". Estado del PR: "Draft, esperando pruebas manuales".
7. ROBOK postea el plan de pruebas manuales en Jira como comentario estructurado del ticket PROM-1234 (cubierto en CU-29). La sección "Plan de pruebas manuales" en G2 se actualiza con el espejo del checklist.
8. Vera ve el cycle time tracker: "1 día 4 horas desde 'Rob, dale' · PR draft abierto". El tiempo "interno del squad" cerró; ahora arranca el reloj de pruebas manuales y code review.

## Variantes

### V1. Una validación bloqueante falla

Lint reporta error. La sección "Validaciones automatizadas" muestra ❌ Lint con detalle. Banner rojo en el tope: "Validación bloqueante falló — el PR draft no se va a abrir hasta resolver. ¿Mandar al squad para auto-fix?". Vera click en "Mandar al squad" → ROBOK abre tarea nueva en el squad (que volvió temporalmente activo) para arreglar el lint. Cuando el squad arregla, las validaciones se re-corren.

### V2. SAST detecta vulnerabilidad real

SAST encuentra una vulnerabilidad de severidad alta (no LOW como la CVE menor). Banner rojo: "Vulnerabilidad detectada — el squad debe procesar". Va por mecánica de "external signal listening": el 🛡️ Security Reviewer propone uno de los 4 caminos (auto-fix / vuelta a Etapa 3 / excepción aprobada / bloqueante). Vera decide. Si el camino es "vuelta a Etapa 3", el plan se reabre en el muro.

### V3. Gitleaks encuentra secreto leakeado

Gitleaks detecta un token API hardcodeado. Banner rojo "Bloqueante absoluto — secreto detectado". Mecánica de external signal listening con camino "Bloqueante absoluto": el merge queda denegado hasta que se rote el secreto y se limpie del git history. El 🛡️ Security Reviewer redacta los pasos.

## Caminos alternativos / errores

- **Si Vera entra a G2 antes de que las validaciones terminen**: ve los checks en estado ⏳ "En curso" con barra de progreso si aplica. Refresh automático a medida que terminan.
- **Si una validación tarda mucho más de lo esperado**: ROBOK no la oculta. La línea muestra "⏳ SAST · más lento de lo esperado, escaneando dependencias transitivas".
- **Si un check de seguridad encuentra falsos positivos repetidos**: Vera puede invocar la mecánica de "Excepción aprobada" desde el panel del check. Queda como ADR.

## Decisiones del usuario en este flujo

- Cómo resolver advertencias no-bloqueantes (ahora vs después).
- Si una validación bloqueante falla: mandar al squad para auto-fix, replantear, o aprobar excepción.
- Si la CVE / vulnerabilidad / secreto requiere acción inmediata o se posterga.

## Componentes UI involucrados

- 1. Header con badge ambient
- 12. Validaciones automatizadas (lista de checks — la pieza central de esta sección)
- 6. Timeline editorial (variante condensada — registra los eventos del Pre-merge Gate)
- 14. Pill de estado (en cada check)

## Notas para el prototipo HTML

- G2 es dashboard tipo "pipeline visual" con secciones en orden de ejecución. Vale invertir en la legibilidad — no debe parecer un log.
- Estados sugeridos como archivos separados:
  - `g2-validaciones-en-curso.html` (algunos checks ⏳, otros ✅)
  - `g2-validaciones-pasaron.html` (5/6 OK, 1 advertencia, PR draft acaba de abrir)
  - `g2-validacion-bloqueante.html` (V1: ❌ en Lint con banner rojo)
  - `g2-sast-vulnerabilidad.html` (V2: vulnerabilidad detectada con 4 caminos visibles)

## Referencias

- Journey: ROBOK_v5.md §1.9 — Etapa 5, "Validaciones automatizadas pre-PR" y "El PR draft se abre al final, no al comienzo".
- Inventario: 03-inventario-pantallas.md §G2.
- Componentes UI: 04-componentes-ui.md §12 (validaciones automatizadas).
- Principios involucrados: P5 (mostrar trabajo — los checks visibles, no escondidos), P3 (validaciones bloqueantes son gates duros — el PR no abre sin ellas).
