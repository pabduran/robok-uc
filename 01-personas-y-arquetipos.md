# ROBOK — Personas y arquetipos de usuario

> **Propósito de este doc:** dar nombre, motivaciones y comportamientos a las personas que aparecen en los journeys. Sin esto, los casos de uso terminan siendo "el usuario hace X" sin saber quién es ese usuario ni qué espera. Con esto, los casos de uso se escriben en primera persona y el prototipo HTML representa pantallas para alguien específico.
>
> **No es:** un set de personas de marketing. Es un set funcional para diseño.

---

## ¿Por qué estas personas y no otras?

ROBOK v5 menciona explícitamente tres roles humanos: **Admin de ROBOK**, **Dev Lead** y **Developer**. Existe una cuarta figura implícita: el **Stakeholder externo** (un arquitecto del cliente que aprueba el plan, un security officer que valida un trade-off) que entra ocasionalmente al muro de discusión sin ser parte del equipo.

A esos cuatro humanos hay que sumar **un quinto actor** que aparece en pantalla pero no es humano: el **Squad de agentes** (con sus 6 roles canónicos). No es un usuario, pero se "lee" en pantalla y los humanos interactúan con él. Lo dejamos fuera de este doc — vive en el doc de componentes UI.

---

## Persona 1 — Vera, la Dev Lead

**Edad / experiencia:** 32, 8 años como ingeniera, 2 como tech lead. Hace pair programming, escribe código todavía cada semana.

**Contexto:** lidera el equipo de Promise Engine en una empresa SaaS. Reporta a una head de ingeniería. Tiene 6 developers en su squad humano. Hace sprints de 2 semanas, trunk-based.

**Su día con ROBOK:**

- Llega a la oficina, abre ROBOK, ve la vista del producto Promise Engine.
- Tiene 3 historias en curso con squads activos. Una llegó a un checkpoint anoche y le pide aprobación.
- Después de resolver eso, entra al sprint actual. ROBOK le tiene anotados los 14 tickets.
- Marca dos para Rob (uno modo Estándar, uno modo Sprint-pace porque no es urgente).
- Dispara el análisis profundo del primero. Entra al muro de discusión, conversa con Rob sobre el plan, ajusta scope, aprueba.
- Cierra ROBOK y se va a su 1:1.

**Lo que más valora:**

- **No tener que micromanagear.** Confía en que Rob le va a avisar cuando lo necesite.
- **Ver el trabajo en lugar de logs.** El squad como cards de gente trabajando, no como `tail -f`.
- **Decidir trade-offs con números.** "Express USD 80 vs Sprint-pace USD 25" es información, no abstracción.

**Lo que la frustra de las herramientas existentes (Devin, Cursor, Copilot):**

- Que avancen "con su mejor opción" cuando ella no respondió. Eso le rompió un módulo una vez.
- Que el código generado parezca correcto pero no respete las convenciones del producto.
- Que no haya forma de saber qué está pasando sin meterse a leer logs raw.

**Su miedo:**

- Que un test mal arreglado oculte un bug real y llegue a producción. Por eso valora muchísimo el "propose-diff-then-approve" de tests.

**Cómo lee la pantalla:**

- Primero busca el **estado global** (¿hay algo que demande mi atención?).
- Después la **vista del squad activo** si entró por una historia específica.
- El **mapa de componentes** lo abre cuando algo no le calza.
- **NO lee logs** salvo en drill-down profundo.

**Su frase:** "No quiero un agente que adivine — quiero uno que me pregunte."

---

## Persona 2 — Diego, el Developer

**Edad / experiencia:** 27, 4 años de experiencia, 1 año en este equipo.

**Contexto:** developer del squad de Promise Engine. No tiene permisos de aprobación en ROBOK. Lee, comenta, eventualmente challengea decisiones.

**Su día con ROBOK:**

- Recibe notificación en Slack: "Plan publicado para PROM-1234". Abre el link.
- Lee el plan en el muro de discusión. Ve que Rob propuso usar la lib `httpx` pero el equipo prefiere `requests` por consistencia.
- Comenta en el muro: "ojo, en este producto usamos `requests`, no `httpx`".
- Vera ve el comentario, le da razón, ajusta. Rob actualiza el plan.
- Más tarde, durante la implementación, entra a curiosear cómo va el squad. Ve las cards. Hace drill-down al Implementer #1 para ver qué archivo está modificando.

**Lo que más valora:**

- **Visibilidad sin responsabilidad.** Puede estar al tanto sin tener que aprobar.
- **Que su comentario importe.** No es decoración — Rob lo lee.
- **Aprender mirando.** Ve cómo Rob descompone una historia compleja y eso lo hace mejor developer.

**Lo que la frustra:**

- Que las decisiones grandes se tomen en hilos de Slack que él no ve.
- Que el código generado por IA sea "mágico" y nadie lo explique.

**Su frase:** "Quiero ver cómo Rob piensa, no solo qué hace."

---

## Persona 3 — Marisol, la Admin de ROBOK

**Edad / experiencia:** 45, head of platform engineering. No escribe código hace 3 años, pero entiende la arquitectura entera de la empresa.

**Contexto:** dueña del tenant de ROBOK en la empresa. Decide qué productos se onboardean, asigna admins de cada producto, gestiona el budget global.

**Su día con ROBOK:**

- Entra una vez por semana. No es su herramienta del día a día.
- Aprovisiona un nuevo producto cuando un equipo nuevo se suma.
- Revisa el reporte de costos: "Promise Engine gastó USD 1.200 esta semana en Rob, Ratings & Reviews USD 800."
- Ve si hay algún producto donde el costo se está saliendo del presupuesto.

**Lo que más valora:**

- **Control sobre quién puede crear qué.** No quiere que cualquiera abra un producto y queme presupuesto.
- **Visibilidad agregada.** Necesita reportes a nivel tenant, no por historia.
- **Que ROBOK se autoadministre.** No quiere ser cuello de botella.

**Lo que la frustra:**

- Herramientas que mezclan permisos de admin con permisos de usuario.
- No tener una vista de "salud del tenant" en una pantalla.

**Su frase:** "Yo no uso ROBOK — yo lo gobierno."

---

## Persona 4 — Pablo, el Stakeholder externo

**Edad / experiencia:** 40, security officer de la empresa. No es parte del squad pero es invitado a aprobar planes que tocan datos sensibles.

**Contexto:** entra a ROBOK 2-3 veces por semana, solo cuando el quórum lo requiere. No tiene producto propio — entra por invitación a un muro específico.

**Su día con ROBOK:**

- Recibe notificación: "Vera te invitó al muro de PROM-1234 (toca módulo de auth)".
- Entra. Lee el plan que propuso Rob. Ve que el Security Reviewer ya marcó dos riesgos: nuevo endpoint público y dependencia con CVE menor.
- Comenta en el muro: "el endpoint debe estar detrás del WAF de la org. ¿Está confirmado?"
- Vera responde con un link al ADR que cubre eso.
- Pablo aprueba.

**Lo que más valora:**

- **Llegar con contexto digerido.** No quiere reconstruir la historia desde cero — quiere ver el plan, los riesgos detectados, las decisiones tomadas.
- **Comentar sin tener que ser admin.** Su rol es opinar, no configurar.
- **Que su aprobación quede registrada.** Para auditorías posteriores.

**Lo que la frustra:**

- Que lo metan a un Slack thread sin contexto.
- Que la decisión ya esté tomada y le pidan "approval rubber stamp".

**Su frase:** "Tráiganme el plan, no el problema."

---

## Cómo se cruzan en los journeys

| Journey                          | Vera (Dev Lead) | Diego (Developer) | Marisol (Admin) | Pablo (Stakeholder) |
| -------------------------------- | :-------------: | :---------------: | :-------------: | :-----------------: |
| **Etapa 1 — Onboarding**         | 🟢 protagonista |     ⚪ ausente     |  🟡 prepara el producto antes |     ⚪ ausente     |
| **Etapa 2 — Selección sprint**   | 🟢 protagonista |     🟡 lee y comenta     |    ⚪ ausente    |     ⚪ ausente     |
| **Etapa 3 — Análisis y plan**    | 🟢 protagonista |     🟡 challengea en muro    |    ⚪ ausente    |  🟡 entra solo si quórum lo requiere  |
| **Etapa 4 — Implementación**     | 🟢 protagonista |  🟡 mira squad, no aprueba  |    ⚪ ausente    |     ⚪ ausente     |
| **Etapa 5 — Pre-merge Gate**     | 🟢 protagonista |  🟡 puede ejecutar pruebas manuales  |    ⚪ ausente    |  🟡 si hay alerta de seguridad seria  |
| **Vista admin tenant**           |    ⚪ ausente    |     ⚪ ausente     | 🟢 protagonista |     ⚪ ausente     |

---

## Implicancias para el prototipo HTML

- **El prototipo del MVP visual debe modelar a Vera primero.** Los otros tres son secundarios y se simulan con menos detalle.
- **Diego se modela con permisos restringidos en algunas vistas** — botones grises de "aprobar", solo puede comentar. Sirve para mostrar el modelo de permisos sin construirlo de verdad.
- **Marisol tiene una pantalla propia distinta** — no entra al journey del producto, entra al panel del tenant.
- **Pablo no tiene pantalla propia** — entra a una pantalla que normalmente es de Vera (el muro), pero con permisos acotados a comentar y aprobar/rechazar.

---

## Lo que NO es una persona en este doc

- **Rob / agentes**: no son usuarios, son piezas de la pantalla. Viven en el doc de componentes UI.
- **PM / Product Manager**: ROBOK no opina sobre el qué — eso ya viene resuelto del planning. Si entra al muro, entra como "stakeholder invitado" estilo Pablo.
- **QA dedicado**: en muchos equipos modernos no existe como rol separado. Si existe, en V1 se modela como otro Developer con permisos de "marcar pruebas manuales".
