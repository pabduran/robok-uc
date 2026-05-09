---
name: robok-glosario-guard
description: Use this skill when generating or editing ANY ROBOK documentation, including use cases, design docs, prototype HTML, READMEs, comments, or any text that will reference ROBOK concepts. This is a guard skill that runs BEFORE producing content to verify terminology consistency. It complements (does not replace) the more specific skills robok-casos-de-uso and robok-prototipo-html. Use proactively whenever ROBOK content is being authored.
---

# Skill — Guardián del glosario y la consistencia conceptual de ROBOK

Esta skill es un **chequeo previo y posterior** a generar contenido ROBOK. Su trabajo es evitar la deriva semántica: que después de N sesiones aparezcan 5 nombres distintos para "el plan", o pantallas inventadas que no están en el inventario.

## Cuándo se aplica

**Activala proactivamente** cuando vas a producir o editar:

- Casos de uso
- Pantallas HTML del prototipo
- Documentos de diseño
- READMEs
- Comentarios técnicos
- Issues, PRs, mensajes de commit que mencionen conceptos del producto
- Cualquier texto que contenga vocabulario del producto

Si la sesión es solo conversacional ("explicame qué es el muro de discusión"), no hace falta activarla — pero si vas a *escribir algo que va a quedar*, sí.

## Pasos

### 1. Antes de generar contenido

Tené abiertos (al menos mentalmente):

- `02-glosario-y-modelo-conceptual.md` — términos canónicos + anti-glosario.
- `03-inventario-pantallas.md` — las 18 pantallas y sus códigos.
- `04-componentes-ui.md` — los 15 componentes y sus nombres canónicos.

### 2. Mientras generás

Aplicá la siguiente checklist mental por cada concepto que mencionás:

| Categoría | Verificación |
|---|---|
| Entidades de negocio (producto, sprint, ticket, plan, ADR) | ¿Está exactamente con ese nombre en el glosario? |
| Pantallas (muro, mapa, squad, etc) | ¿Coincide con el código del inventario (E3, C3, F1)? |
| Componentes UI (header, status bar, card de agente) | ¿Coincide con el nombre canónico del doc 04? |
| Roles humanos | ¿Es Vera / Diego / Marisol / Pablo? Nunca "el usuario". |
| Roles del squad | ¿Es Architect / Implementer / Tester / Reviewer / Doc-writer / Security Reviewer? Con el emoji canónico (🎨 🔨 🧪 👀 📝 🛡️). |
| Modos de costo | ¿Express / Estándar / Económico / Sprint-pace? |
| Etapas | ¿Etapa 1 / 2 / 3 / 4 / 5 con esos números? |
| Estados | ¿Usás los emojis canónicos (🟢 🟡 🔴 ⏸️ ✅ 🛑)? |

### 3. Después de generar — auditoría rápida

Antes de cerrar el output, releé el texto buscando los **8 anti-términos del anti-glosario**:

| ❌ Si encontrás | ✅ Reemplazá por |
|---|---|
| "el bot" | "Rob" o "ROBOK" o "el squad" |
| "conversación con Rob" (cuando es Etapa 3) | "muro de discusión" |
| "ejecutar un comando" | "delegar una historia" |
| "run" / "execution" | "squad activo" o "historia en curso" |
| "logs" (cuando es la timeline editorial) | "timeline" o "drill-down" |
| "configurar el agente" (cuando es rol) | "configurar el rol del squad" |
| "workflow" / "pipeline" (interno de ROBOK) | "Plan" o "Etapa" |
| "aprobar el documento" | "aprobar el plan en el muro" |
| "confirmar" (al final del onboarding) | "validar conversando" o "validar preguntando" |
| "el usuario" | nombre propio (Vera/Diego/Marisol/Pablo) |

**Excepciones legítimas** (donde sí podés usar términos "prohibidos"):
- "Pipeline de CI" del cliente → sí se llama así, es externo.
- "Workflow de GitHub Actions" → sí, externo.
- "Logs del sistema operativo / del runtime" → sí, en docs técnicos de infra.

El anti-glosario aplica a **conceptos internos del producto**, no a tecnologías externas con nombre propio.

### 4. Si descubrís un hueco

Si mientras escribís te das cuenta de que necesitás un término, una pantalla o un componente que NO existe en los docs:

1. **Detenete.** No lo inventes.
2. **Avisá al usuario** con esta forma:
   > "Necesito el término/pantalla/componente X para esto. No está en el [doc 02 / doc 03 / doc 04]. ¿Lo agrego antes de seguir? Mi propuesta sería: <descripción breve>."
3. **Esperá confirmación.**
4. Una vez aprobado, agregalo al doc base correspondiente, después seguí.

Esa fricción es deliberada. La sobreingeniería en docs nace de inventar términos sin pensarlos.

## Reglas adicionales de coherencia

### Nombres en pantalla vs en docs

El doc 02 define dos niveles:

- **Nombre técnico/conceptual** (en docs): "Muro de discusión", "Vista del squad activo", "Backlog enriquecido".
- **Nombre en pantalla** (en el HTML del prototipo): puede ser más corto. "Muro", "Squad", "Sprint actual".

Cuando estés escribiendo:
- **En casos de uso, design docs, READMEs** → usá el nombre técnico completo.
- **En el HTML del prototipo (botones, breadcrumbs, títulos)** → usá el nombre en pantalla del doc 02.

### Idiomas

- Producto/UI principal en **español**.
- Roles del squad en **inglés** (Architect, Implementer, etc) — esto está fijado en el doc 02 por consistencia técnica.
- Términos de tecnología externa en su forma original (FastAPI, K8s, GitHub Actions, etc).

### Versiones

Si mencionás features:

- **V1** → es lo que está construido o en construcción. Está en los docs base.
- **V2** → roadmap a corto plazo (auto-fix sobre pipeline, embed Slack/Teams, etc). Mencionable como "esto vendrá en V2", nunca describible como "esto pasa".
- **V3+** → roadmap especulativo. Solo se menciona como "más adelante" o "fuera del alcance actual".

Si un caso de uso o pantalla parece requerir V2, ese contenido se posterga, no se escribe a medias.

## Output esperado de esta skill

Esta skill **no produce archivos propios**. Su trabajo es:

1. **Antes de generar**: avisar al usuario qué docs base estás consultando.
2. **Durante**: hacer cumplir las reglas mientras generás.
3. **Después**: ofrecer una mini-auditoría del output con cualquier inconsistencia detectada.

### Formato sugerido del reporte de auditoría

Cuando termines de generar contenido, agregá al final (en colapsable o como nota separada):

```
✅ Auditoría de glosario:
- Términos del glosario usados: 12 (todos consistentes)
- Pantallas referenciadas: F1, E3, C1 (todas en el inventario)
- Componentes referenciados: 1, 4, 5, 6 (todos en el doc 04)
- Anti-términos detectados: 0
- Personas mencionadas: Vera, Diego (con sus roles correctos)
```

Si encontrás algo:

```
⚠️ Auditoría de glosario:
- Anti-término detectado: "el bot" en línea 47 → reemplazar por "Rob"
- Pantalla no inventariada: "Settings dialog" → no está en el inventario.
  ¿Querías referenciar C4. Configuración del producto?
```

## Caso especial: el usuario pide "ignorar las reglas"

Si el usuario te pide explícitamente flexibilizar (ej. "usá el nombre que quieras", "no me importa el glosario por ahora, solo dame algo rápido"), respetá la decisión pero:

1. **Avisá brevemente** que estás saliéndote del marco.
2. **Marcá el output** como `[draft sin auditar]` para que después se pueda revisar.

No es desobediencia; es que el usuario sabe que pierde la red de seguridad.
