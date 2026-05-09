# Casos de uso de ROBOK — Índice

Tabla maestra de los 35 casos de uso esperados según `05-guia-casos-de-uso.md`. Estado de cada uno.

**Leyenda de estado:**
- ⏳ pendiente — todavía no se escribió
- 📝 draft — escrito, pendiente de revisión
- ✅ revisado — releído por humano, sin objeciones
- 🏁 final — congelado para construir el prototipo

## Resumen por etapa

| Etapa | Total | Pendiente | Draft | Revisado | Final |
|---|---:|---:|---:|---:|---:|
| Etapa 0 — Acceso | 3 | 0 | 3 | 0 | 0 |
| Etapa 1 — Onboarding | 4 | 0 | 4 | 0 | 0 |
| Etapa 2 — Selección | 4 | 0 | 4 | 0 | 0 |
| Etapa 3 — Análisis y plan | 8 | 0 | 8 | 0 | 0 |
| Etapa 4 — Implementación | 8 | 0 | 8 | 0 | 0 |
| Etapa 5 — Pre-merge Gate | 5 | 0 | 5 | 0 | 0 |
| Vistas auxiliares y admin | 3 | 0 | 3 | 0 | 0 |
| **Total** | **35** | **0** | **35** | **0** | **0** |

## Tabla maestra

### Etapa 0 — Acceso

| ID | Título | Actor | Pantalla principal | Estado | Archivo |
|---|---|---|---|---|---|
| CU-01 | Vera entra a ROBOK por primera vez y elige un producto | Vera | A2. Selector de productos | 📝 draft | [CU-01](./CU-01-vera-entra-y-elige-producto.md) |
| CU-02 | Marisol entra como Admin y accede al panel del tenant | Marisol | H1. Panel del tenant | 📝 draft | [CU-02](./CU-02-marisol-entra-al-panel-del-tenant.md) |
| CU-03 | Vera cambia entre dos productos sin volver al login | Vera | C1. Vista del producto | 📝 draft | [CU-03](./CU-03-vera-cambia-entre-productos.md) |

### Etapa 1 — Onboarding del producto

| ID | Título | Actor | Pantalla principal | Estado | Archivo |
|---|---|---|---|---|---|
| CU-04 | Vera onboardea un producto recién provisionado: carga contexto y mapea equipo | Vera | B1. Onboarding paso de carga de contexto | 📝 draft | [CU-04](./CU-04-vera-onboardea-producto-y-mapea-equipo.md) |
| CU-05 | Vera mira el indexado en curso con narración explícita y entiende qué está pasando | Vera | B2. Onboarding indexado en curso | 📝 draft | [CU-05](./CU-05-vera-mira-el-indexado-en-curso.md) |
| CU-06 | Vera lee el insight inicial proactivo y lo confirma | Vera | B3. Insight inicial proactivo | 📝 draft | [CU-06](./CU-06-vera-confirma-el-insight-inicial.md) |
| CU-07 | Vera lee el insight inicial, encuentra errores, y los corrige conversando con Rob | Vera | B3. Insight inicial proactivo | 📝 draft | [CU-07](./CU-07-vera-corrige-el-insight-conversando.md) |

### Etapa 2 — Selección y priorización

| ID | Título | Actor | Pantalla principal | Estado | Archivo |
|---|---|---|---|---|---|
| CU-08 | Vera entra al sprint actual y revisa el backlog enriquecido | Vera | D1. Sprint actual — backlog enriquecido | 📝 draft | [CU-08](./CU-08-vera-revisa-el-backlog-enriquecido.md) |
| CU-09 | Vera marca un ticket para Rob desde el backlog enriquecido (camino "delegar de una") | Vera | D1 | 📝 draft | [CU-09](./CU-09-vera-delega-de-una-un-ticket.md) |
| CU-10 | Vera marca un ticket en pausa para profundizar después | Vera | D1 | 📝 draft | [CU-10](./CU-10-vera-pausa-un-ticket-para-profundizar.md) |
| CU-11 | Vera entra al detalle de un ticket anotado para ver más contexto | Vera | D2. Detalle de ticket anotado | 📝 draft | [CU-11](./CU-11-vera-entra-al-detalle-del-ticket.md) |

### Etapa 3 — Análisis profundo y plan

| ID | Título | Actor | Pantalla principal | Estado | Archivo |
|---|---|---|---|---|---|
| CU-12 | Vera confirma el scope que Rob propone para una historia | Vera | E1. Confirmación de scope | 📝 draft | [CU-12](./CU-12-vera-confirma-el-scope.md) |
| CU-13 | Vera ajusta el scope agregando un componente que Rob no consideró | Vera | E1 | 📝 draft | [CU-13](./CU-13-vera-ajusta-el-scope.md) |
| CU-14 | Vera espera el análisis profundo y observa la narración | Vera | E2. Análisis profundo en curso | 📝 draft | [CU-14](./CU-14-vera-espera-el-analisis-profundo.md) |
| CU-15 | Vera lee el plan en el muro y discute con Rob hasta cerrarlo | Vera | E3. Muro de discusión del plan | 📝 draft | [CU-15](./CU-15-vera-discute-el-plan-en-el-muro.md) |
| CU-16 | Diego comenta en el muro y Rob ajusta el plan en respuesta | Diego | E3 | 📝 draft | [CU-16](./CU-16-diego-comenta-en-el-muro.md) |
| CU-17 | Vera aprueba el plan: elige modo y se cumple el quórum | Vera | E4. Modo + quórum | 📝 draft | [CU-17](./CU-17-vera-aprueba-con-modo-y-quorum.md) |
| CU-18 | Vera cancela un plan; queda guardado para retomar después | Vera | E3 | 📝 draft | [CU-18](./CU-18-vera-cancela-un-plan.md) |
| CU-19 | Pablo es invitado al muro por requisito de quórum (módulo de auth) | Pablo | E3 | 📝 draft | [CU-19](./CU-19-pablo-invitado-al-muro-por-quorum.md) |

### Etapa 4 — Implementación con squad

| ID | Título | Actor | Pantalla principal | Estado | Archivo |
|---|---|---|---|---|---|
| CU-20 | Vera ve el squad arrancar y se familiariza con la pantalla F1 | Vera | F1. Vista del squad activo | 📝 draft | [CU-20](./CU-20-vera-ve-arrancar-el-squad.md) |
| CU-21 | Vera hace drill-down en un agente para ver qué está haciendo | Vera | F2. Drill-down de agente | 📝 draft | [CU-21](./CU-21-vera-hace-drill-down-en-agente.md) |
| CU-22 | Vera recibe una notificación interrupting de un checkpoint y lo resuelve | Vera | F3. Pantalla de checkpoint | 📝 draft | [CU-22](./CU-22-vera-resuelve-un-checkpoint.md) |
| CU-23 | Vera aprueba un fix de test propuesto en checkpoint (propose-diff-then-approve) | Vera | F3 | 📝 draft | [CU-23](./CU-23-vera-aprueba-fix-de-test.md) |
| CU-24 | Vera usa double-texting para dar instrucciones al squad sin reiniciarlo | Vera | F1 | 📝 draft | [CU-24](./CU-24-vera-usa-double-texting.md) |
| CU-25 | Vera ve el costo desbordarse y el squad pausa automáticamente | Vera | F1 | 📝 draft | [CU-25](./CU-25-vera-ve-costo-desbordarse.md) |
| CU-26 | El squad termina, Vera recibe el reporte de cierre | Vera | G1. Reporte de cierre | 📝 draft | [CU-26](./CU-26-squad-termina-reporte-de-cierre.md) |
| CU-27 | Diego mira el squad activo (sin permisos de aprobar) y aprende cómo Rob descompone | Diego | F1 | 📝 draft | [CU-27](./CU-27-diego-mira-el-squad-activo.md) |

### Etapa 5 — Pre-merge Gate

| ID | Título | Actor | Pantalla principal | Estado | Archivo |
|---|---|---|---|---|---|
| CU-28 | Vera ve las validaciones automatizadas pasar y el PR draft abrirse | Vera | G2. Pre-merge Gate dashboard | 📝 draft | [CU-28](./CU-28-validaciones-pasan-pr-draft-abre.md) |
| CU-29 | Vera ve el plan de pruebas manuales en Jira (espejado en ROBOK) | Vera | G2 | 📝 draft | [CU-29](./CU-29-vera-ve-plan-de-pruebas-manuales.md) |
| CU-30 | Diego marca una prueba manual como fallida y se abre mini-muro | Diego | G3. Mini-muro de fallo | 📝 draft | [CU-30](./CU-30-diego-marca-prueba-fallida.md) |
| CU-31 | Vera resuelve el mini-muro: clasifica como "fix directo" y vuelve al squad | Vera | G3 | 📝 draft | [CU-31](./CU-31-vera-clasifica-como-fix-directo.md) |
| CU-32 | El PR se mergea y Rob escucha el pipeline post-merge, notificando un fallo | Vera | G2 | 📝 draft | [CU-32](./CU-32-pr-mergea-pipeline-post-merge-falla.md) |

### Vistas auxiliares y admin

| ID | Título | Actor | Pantalla principal | Estado | Archivo |
|---|---|---|---|---|---|
| CU-33 | Vera entra al mapa de componentes para entender el sistema antes de planning | Vera | C3. Mapa de componentes | 📝 draft | [CU-33](./CU-33-vera-entra-al-mapa-de-componentes.md) |
| CU-34 | Vera revisa una versión antigua del mapa para auditar una decisión de hace 6 meses | Vera | C3 | 📝 draft | [CU-34](./CU-34-vera-revisa-version-antigua-del-mapa.md) |
| CU-35 | Marisol provisiona un nuevo producto en el panel del tenant y asigna un Dev Lead | Marisol | H1 | 📝 draft | [CU-35](./CU-35-marisol-provisiona-producto.md) |

## Mapa de casos de uso por persona

| Persona | CU donde es protagonista |
|---|---|
| **Vera** (Dev Lead) | CU-01, CU-03, CU-04, CU-05, CU-06, CU-07, CU-08, CU-09, CU-10, CU-11, CU-12, CU-13, CU-14, CU-15, CU-17, CU-18, CU-20, CU-21, CU-22, CU-23, CU-24, CU-25, CU-26, CU-28, CU-29, CU-31, CU-32, CU-33, CU-34 |
| **Diego** (Developer) | CU-16, CU-27, CU-30 |
| **Marisol** (Admin de ROBOK) | CU-02, CU-35 |
| **Pablo** (Stakeholder externo) | CU-19 |

## Mapa de casos de uso por pantalla

| Pantalla | CU que la usan como principal |
|---|---|
| A1. Login / SSO | (entrada de CU-01, CU-02; sin CU propio) |
| A2. Selector de productos | CU-01 |
| B1. Onboarding paso de carga de contexto | CU-04 |
| B2. Onboarding indexado en curso | CU-05 |
| B3. Insight inicial proactivo | CU-06, CU-07 |
| C1. Vista del producto | CU-03 |
| C2. Mis historias en curso | (secundaria, sin CU propio en V1) |
| C3. Mapa de componentes | CU-33, CU-34 |
| C4. Configuración del producto | (secundaria, sin CU propio en V1) |
| D1. Sprint actual — backlog enriquecido | CU-08, CU-09, CU-10 |
| D2. Detalle de ticket anotado | CU-11 |
| E1. Confirmación de scope | CU-12, CU-13 |
| E2. Análisis profundo en curso | CU-14 |
| E3. Muro de discusión del plan | CU-15, CU-16, CU-18, CU-19 |
| E4. Modo + quórum | CU-17 |
| F1. Vista del squad activo | CU-20, CU-24, CU-25, CU-27 |
| F2. Drill-down de agente | CU-21 |
| F3. Pantalla de checkpoint | CU-22, CU-23 |
| G1. Reporte de cierre | CU-26 |
| G2. Pre-merge Gate dashboard | CU-28, CU-29, CU-32 |
| G3. Mini-muro de fallo | CU-30, CU-31 |
| H1. Panel del tenant | CU-02, CU-35 |

## Próxima sesión

🏁 **Los 35 CU están en draft.** El siguiente bloque de trabajo es:

1. **Auditoría de los 35 CU** (sesión de revisión cruzada): leer en orden, marcar lo que cambia de "📝 draft" a "✅ revisado" y dejar feedback puntual. Apuntar a tener todos en ✅ antes de arrancar el prototipo.
2. **Arrancar el prototipo HTML — Pasada 1** (las 3 pantallas más distintivas, en orden de impacto demostrativo según doc 03):
   - F1. Vista del squad activo (cubre CU-20, CU-24, CU-25, CU-27).
   - E3. Muro de discusión del plan (cubre CU-15, CU-16, CU-18, CU-19).
   - B3. Insight inicial proactivo (cubre CU-06, CU-07).
3. Skill a usar para el prototipo: `robok-prototipo-html`.
