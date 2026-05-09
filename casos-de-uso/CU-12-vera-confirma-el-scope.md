# CU-12. Vera confirma el scope que Rob propone para una historia

## Identidad

- **Actor**: Vera (Dev Lead)
- **Pantalla principal**: E1. Confirmación de scope
- **Pantallas secundarias**: D1 (entrada via CU-09), E2. Análisis profundo en curso (salida).
- **Pre-condición**: Vera delegó un ticket en D1 (CU-09). Rob ya inferió un scope inicial sobre el mapa de componentes — un subconjunto de nodos pre-highlighteados con justificación.
- **Post-condición**: Scope confirmado. ROBOK arranca el análisis profundo (transición a E2).
- **Etapa del journey**: Etapa 3 — Análisis profundo y plan
- **Frecuencia esperada**: una vez por historia delegada (varias veces por sprint).

## Disparador

Vera apretó "Delegar de una" en D1 (CU-09). ROBOK la lleva automáticamente a E1 con el scope que Rob infirió.

## Flujo principal (happy path)

1. Vera ve E1 — Confirmación de scope. La pantalla muestra el mapa de componentes en modo selección, con los componentes que Rob propone tocar **ya highlighteados**: `pricing` y `billing-api`. Contador en el tope: "2 componentes en scope".
2. Junto al mapa, un panel lateral con la justificación de Rob: "Propongo estos componentes porque la spec menciona refactor del cálculo de precio (`pricing`) y el endpoint público de cotización vive en `billing-api`. NO incluyo `promise-worker` porque el cambio es síncrono."
3. Vera lee la justificación. Coincide con su entendimiento.
4. Vera click en "Confirmar scope".
5. ROBOK marca el scope como confirmado y transiciona a E2 — Análisis profundo en curso (cubierto en CU-14). Rob arranca a leer en profundidad solo los componentes confirmados.

Aplicación del principio: Rob propone X. Vera confirma o corrige antes de que Rob actúe. Sin esta confirmación, Rob no profundiza ni quema tokens analizando todo el producto.

## Variantes

### V1. Vera quita un componente que Rob incluyó de más

Vera ve que Rob incluyó `billing-api` pero el cambio en realidad no toca el endpoint, solo la lógica interna de `pricing`. Click en el nodo `billing-api` para deseleccionarlo. El contador baja a "1 componente en scope". Vera click en "Confirmar scope". Rob va a profundizar solo `pricing`.

### V2. Vera quiere ver detalle de un componente antes de confirmar

Vera hover/click en el nodo `pricing` → tooltip o panel lateral muestra: ubicación del repo, tamaño aproximado, ADRs recientes que lo tocaron, último commit. Vera lee, vuelve, confirma.

## Caminos alternativos / errores

- **Si Rob no pudo inferir scope inicial** (módulo sin contexto bien indexado): E1 carga con scope vacío y un mensaje "No pude inferir scope inicial. Marcalo manualmente." — el flujo continúa como CU-13 (selección manual).
- **Si Vera no confirma y cierra E1**: el ticket queda en estado "Para Rob — Esperando scope" y vuelve al backlog enriquecido con esa marca. Vera puede retomar después haciendo click en la card en D1.

## Decisiones del usuario en este flujo

- Aceptar el scope de Rob, ajustarlo (CU-13), o cancelar.
- Pedir o no pedir más detalle de un componente antes de confirmar.

## Componentes UI involucrados

- 1. Header con badge ambient
- 8. Mapa de componentes (en modo selección — nodos highlighteados, contador en tope)

## Notas para el prototipo HTML

- E1 reusa el componente 8 (mapa de componentes) pero en modo selección. Los nodos pre-highlighteados son clickeables para deseleccionar.
- Estados sugeridos como archivos separados:
  - `e1-scope-inferido.html` (scope inicial propuesto por Rob — 2 nodos highlighteados)
  - `e1-scope-ajustado.html` (V1: Vera removió un nodo)
  - `e1-scope-vacio.html` (caso del error: Rob no infirió nada)
- Para el contador y la justificación: panel lateral fijo en el lado derecho.

## Referencias

- Journey: ROBOK_v5.md §1.7 — Etapa 3, paso B–C del flujo detallado y nota sobre "Inferencia con confirmación humana".
- Inventario: 03-inventario-pantallas.md §E1.
- Componentes UI: 04-componentes-ui.md §8 (modo selección).
- Principios involucrados: P3 (inferencia con confirmación humana — Rob propone, Vera confirma antes de que Rob profundice), P5 (demostrar entendimiento — Rob justifica por qué propone esos componentes).
