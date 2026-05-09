# CU-13. Vera ajusta el scope agregando un componente que Rob no consideró

## Identidad

- **Actor**: Vera (Dev Lead)
- **Pantalla principal**: E1. Confirmación de scope
- **Pantallas secundarias**: D1 (entrada), E2 (salida).
- **Pre-condición**: Vera está en E1 con el scope inferido por Rob, pero detecta que falta un componente que el cambio sí va a tocar.
- **Post-condición**: Scope ampliado y confirmado. Rob arranca el análisis profundo sobre el set actualizado.
- **Etapa del journey**: Etapa 3 — Análisis profundo y plan
- **Frecuencia esperada**: 1 de cada 3 historias delegadas (cuando Rob falló en detectar una dependencia que el humano sabe).

## Disparador

Vera entra a E1 (después de CU-09), revisa el scope que Rob propone y ve que falta un componente importante.

## Flujo principal (happy path)

1. Vera ve E1 con el mapa en modo selección. Rob highlighteó `pricing` y `billing-api` (2 componentes). Justificación de Rob en panel lateral: "propongo estos componentes porque la spec menciona refactor del cálculo de precio".
2. Vera lee la justificación. Detecta el faltante: el cambio en pricing también afecta a `promise-worker` porque el worker recalcula precios al expirar promesas. Rob no lo notó porque el método compartido vive en `pricing` y la dependencia desde `promise-worker` es por import indirecto.
3. Vera click en el nodo `promise-worker` en el mapa. El nodo se highlightea. El contador sube a "3 componentes en scope".
4. ROBOK detecta el cambio y muestra una mini-justificación inline: "Agregaste `promise-worker`. ¿Querés dejar una nota para Rob sobre por qué?". Vera escribe: "el worker recalcula precios al expirar promesas, usa el método del refactor".
5. Vera click en "Confirmar scope".
6. ROBOK transiciona a E2 — Análisis profundo en curso. Rob arranca el análisis sobre los 3 componentes confirmados, con la nota de Vera incorporada al contexto del análisis.

## Variantes

### V1. Vera reemplaza un componente por otro

Vera quita `billing-api` (Rob lo había puesto de más) y agrega `promise-worker`. El flujo es idéntico — ROBOK acepta cualquier combinación de altas y bajas siempre que haya al menos un componente en scope al confirmar.

### V2. Vera agrega varios componentes a la vez

El cambio es transversal y toca 4 componentes adicionales. Vera click cada nodo. ROBOK no limita el número, pero si el contador supera N (ej. 8 componentes), aparece un banner amarillo: "Scope amplio — el análisis profundo va a ser más caro y lento. ¿Querés revisar si hay forma de descomponer la historia?". Vera puede continuar igual o volver a Jira a partir el ticket.

### V3. Sin nota libre

El input de "nota para Rob" es opcional. Vera puede agregar/quitar componentes sin justificar. Rob analiza con el set confirmado y la justificación queda implícita en el cambio.

## Caminos alternativos / errores

- **Si Vera quita todos los componentes**: el botón "Confirmar scope" se deshabilita. Mensaje "Necesitás al menos un componente para que Rob pueda analizar."
- **Si el componente que Vera quiere agregar no está en el mapa** (porque no fue indexado o es externo): el nodo aparece como "🔗 Dependencia externa" y se puede agregar al scope con una nota. Rob va a analizar el contrato pero no el código (no está en el repo).

## Decisiones del usuario en este flujo

- Qué componentes agregar / quitar.
- Si dejar una nota para Rob o no.
- Si el scope se está volviendo demasiado amplio: continuar o partir el ticket.

## Componentes UI involucrados

- 1. Header con badge ambient
- 8. Mapa de componentes (modo selección, con multi-selección)

## Notas para el prototipo HTML

- El contador de scope debe actualizarse al click. Para el prototipo, una variante por estado:
  - `e1-scope-ajustado-3-componentes.html` (Vera agregó `promise-worker`)
  - `e1-scope-amplio.html` (V2: scope con 8 componentes y banner amarillo)
  - `e1-scope-con-nota.html` (caja de nota libre desplegada)
- Reutilizar el componente 8 con multi-selección visual (nodos con borde grueso de selección).

## Referencias

- Journey: ROBOK_v5.md §1.7 — Etapa 3, ramas C–D del flujo detallado ("Líder corrige incluye/excluye componentes").
- Inventario: 03-inventario-pantallas.md §E1 (estado "scope con ediciones").
- Componentes UI: 04-componentes-ui.md §8.
- Principios involucrados: P3 (el humano lidera — el scope final lo decide Vera, no Rob), P5 (demostrar entendimiento — Vera deja una nota que Rob va a usar como contexto).
