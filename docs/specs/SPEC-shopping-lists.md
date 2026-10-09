# Karrito — Especificación de capacidad: `shopping-lists`

**SDLC:** 1.4 · Especificación de requisitos (`spec-driven-development`)  
**Estado:** Definición funcional aprobada; especificación preparada para revisión documental  
**Fecha:** 9 de octubre de 2026  
**Alcance:** MVP · Modelo híbrido de listas

**Documentos de referencia:** [Statement of Intent](../intent/statement-of-intent.md) · [Refinamiento de la idea](../ideas/karrito.md) · [Plan de validación](../validation/karrito-validation-plan.md) · [SPEC-family-access](SPEC-family-access.md)

> Esta especificación registra las decisiones funcionales tomadas después del refinamiento inicial. No define la arquitectura, el esquema de datos ni los mecanismos técnicos de sincronización. Cuando una regla afecta al producto comprado o a su registro histórico, establece el contrato observable y remite los detalles a `shopping-items` o `purchase-tracking`.

## 1. Objetivo y límites de la capacidad

Permitir que los familiares organicen compras mediante **una lista permanente compartida** y **múltiples listas específicas por propósito**, consulten una **vista global de pendientes** y conserven un historial comprensible, sin que los datos de unas listas interfieran con otras.

**Dentro del MVP:** creación, consulta, edición, eliminación y archivado manual de listas específicas; lista permanente no eliminable ni archivable; presentación y ordenación de listas; vista global de pendientes; consulta del historial de listas y del historial general de compras.

**Fuera del alcance de esta capacidad:** atributos detallados de entradas/productos, asignación a supermercados y filtros por tienda (`shopping-items`); registro de cantidades efectivamente compradas, compras parciales, precio opcional y detalle del evento de compra (`purchase-tracking`); mecanismos técnicos de identidad y sesiones (`family-access`). No se incluyen estadísticas avanzadas ni recuperación de listas eliminadas.

**Dependencias funcionales:** `family-access` aporta perfil activo y permisos compartidos. `shopping-lists` define el contenedor, su ciclo de vida y su relación con las vistas. `shopping-items` y `purchase-tracking` aportan respectivamente las entradas pendientes y los registros de compra para calcular si una lista puede archivarse.

## 2. Requisitos funcionales

| ID | Requisito verificable | Prioridad |
|---|---|---|
| SL-01 | Existe **una única lista permanente**, compartida por toda la familia, siempre activa. No se puede archivar ni eliminar. | MVP |
| SL-02 | Cualquier perfil activo puede **crear varias listas específicas** e independientes entre sí y de la permanente. | MVP |
| SL-03 | Una lista específica solo puede **archivarse manualmente**, mediante una acción explícita de finalización. | MVP |
| SL-04 | Cuando se completa una entrada de la **lista permanente**, su compra se incorpora al historial individual y deja de figurar como pendiente; la lista permanece activa. | MVP |
| SL-05 | Existe una **vista global unificada** de entradas pendientes, que identifica la lista de origen. No constituye una nueva lista persistente. | MVP |
| SL-06 | Una misma referencia a producto puede aparecer en diferentes listas como **entradas independientes**; completar o modificar una no altera automáticamente las de otras listas. | MVP |
| SL-07 | Las **listas específicas archivadas** se pueden consultar en modo solo lectura y **eliminar definitivamente** previa confirmación. | MVP |
| SL-08 | Todos los perfiles activos tienen **los mismos permisos** para gestionar listas y consultar historiales. | MVP |
| SL-09 | Crear una lista específica exige **nombre obligatorio** y permite **descripción** y **fecha prevista de compra** opcionales; se crea vacía y activa. | MVP |
| SL-10 | Cualquier perfil puede editar nombre, descripción y fecha prevista de una **lista activa**; las archivadas no son editables. | MVP |
| SL-11 | Se puede **eliminar una lista específica activa**, aunque contenga pendientes, **previa confirmación**. No se permite recuperar esa lista. | MVP |
| SL-12 | Al eliminar una lista activa se **conservan en el historial general las compras ya realizadas** (incluida la parte adquirida de compras parciales); se descartan sus entradas o cantidades pendientes. | MVP |
| SL-13 | Las compras conservadas de una lista activa eliminada mantienen el **nombre de la lista original** y muestran que su origen fue una «lista eliminada». | MVP |
| SL-14 | La acción **«Finalizar compra»** solo está disponible cuando una lista específica tiene **al menos una compra registrada y ningún producto pendiente**; una lista sin compras se elimina, no se archiva. | MVP |
| SL-15 | Se permiten **listas distintas con nombres idénticos**, incluidas listas creadas en fechas diferentes. | MVP |
| SL-16 | El selector de listas específicas muestra siempre **nombre y fecha de creación**; muestra también la **fecha prevista** cuando exista. | MVP |
| SL-17 | En el selector de listas activas, la **lista permanente ocupa el primer lugar**; después aparecen las listas específicas por **fecha de creación descendente**. | MVP |
| SL-18 | Existen **dos consultas históricas**: (a) listas específicas archivadas completas y (b) historial general de compras de la lista permanente, listas activas, archivadas y listas activas eliminadas. | MVP |

### Relaciones entre listas y vistas

- Cada lista mantiene sus propias entradas y estados; la vista global **reúne** únicamente las entradas pendientes sin fusionarlas.
- Si un familiar actúa sobre una entrada desde la vista global, el cambio se aplica a **esa entrada de su lista de origen**, no a otras entradas del mismo producto.
- La organización o filtrado de entradas por supermercado se definirá en `shopping-items`, conservando estas reglas de independencia.
- Al archivar una lista específica, deja de aparecer entre las activas y puede consultarse íntegramente en el historial de listas.

## 3. Reglas de negocio e invariantes

1. **Una lista permanente por espacio familiar.** Nunca se archiva ni se elimina; no requiere un formulario de creación por los usuarios.
2. **Una lista específica es independiente** y tiene un ciclo de vida funcional `activa → archivada` por acción manual. También puede eliminarse mientras está activa.
3. **Archivado condicionado:** no deben quedar entradas pendientes y tiene que haber, como mínimo, una compra registrada; no es suficiente que la lista esté vacía.
4. Un producto comprado parcialmente solo deja de ser pendiente cuando el familiar decide considerar satisfecha la entrada. Si quedan unidades pendientes, la lista **no** puede archivarse.
5. **Eliminar una lista activa no borra hechos de compra ya realizados.** Las cantidades pendientes se descartan; se conserva el origen histórico de las compras realizadas.
6. Las **listas archivadas son inmutables** para operaciones de edición. Pueden consultarse o eliminarse definitivamente.
7. La **eliminación definitiva de una lista archivada y su historial asociado** es distinta de la eliminación de una lista activa: la primera retira el registro archivado y las compras asociadas a ese registro del historial consultable; la segunda conserva las compras ya hechas. El tratamiento de los eventos de auditoría se señala como cuestión abierta.
8. **Los nombres no son identificadores únicos.** Varias listas llamadas «Compra semanal» pueden coexistir y deben distinguirse por los metadatos que se muestran.
9. La **fecha prevista de compra es informativa**: llegar o superar esa fecha no archiva, elimina ni completa automáticamente una lista.
10. Los cambios confirmados desde Android o Web deben reflejarse coherentemente en sus vistas. No se indica que una operación haya tenido éxito sin confirmación de persistencia en el servidor.

## 4. Flujos y criterios de aceptación

### CA-SL-01 — Lista permanente

- **Dado** un espacio familiar inicializado, **cuando** se consultan las listas activas, **entonces** aparece una única lista permanente en primera posición.
- **Dado** un familiar con permisos compartidos, **cuando** intenta eliminar o archivar la lista permanente, **entonces** esa operación no está disponible o se rechaza claramente.
- **Dada** una entrada de la lista permanente marcada como comprada, **cuando** se confirma su registro, **entonces** deja de estar pendiente y la compra aparece en el historial individual, sin archivar la lista.

### CA-SL-02 — Alta y edición de una lista específica

- **Dado** un familiar, **cuando** crea una lista con nombre válido y omite descripción y fecha prevista, **entonces** se crea una lista específica vacía y activa.
- **Dada** una lista activa, **cuando** se modifican nombre, descripción o fecha prevista, **entonces** los nuevos valores aparecen en las vistas pertinentes y se registra la acción atribuida al perfil activo.
- **Dadas** dos listas llamadas «Compra semanal», **cuando** se consultan, **entonces** ambas pueden coexistir sin compartir entradas ni estados y muestran su fecha de creación para distinguirlas.
- **Dada** una lista archivada, **cuando** se intenta modificarla, **entonces** la operación se rechaza.

### CA-SL-03 — Selector y fechas

- **Dadas** varias listas activas, **cuando** se abre el selector, **entonces** la permanente aparece antes que las específicas y estas se ordenan por fecha de creación, de más reciente a más antigua.
- **Dada** una lista con fecha prevista, **cuando** aparece en el selector, **entonces** se muestran nombre, fecha de creación y fecha prevista; si carece de fecha prevista, no se inventa ninguna.
- **Dada** una lista cuya fecha prevista ya ha pasado, **cuando** se consulta, **entonces** conserva su estado y no se archiva automáticamente.

### CA-SL-04 — Vista global e independencia

- **Dada** una entrada pendiente en la lista permanente y otra idéntica en «Compra semanal», **cuando** se abre la vista global, **entonces** aparecen **dos entradas** diferenciadas por su lista de origen.
- **Dada** una entrada visible en la vista global, **cuando** un familiar la marca como comprada, **entonces** se actualiza únicamente su entrada de origen; otras entradas equivalentes permanecen sin cambios.
- **Dadas** varias listas con pendientes, **cuando** se consulta una lista particular, **entonces** solo se muestran sus propias entradas, sin incorporar entradas de otras listas.

### CA-SL-05 — Archivado manual de listas específicas

- **Dada** una lista con alguna entrada pendiente, **cuando** se intenta finalizarla, **entonces** la acción está deshabilitada o se rechaza con una explicación.
- **Dada** una lista vacía o sin compras realizadas, **cuando** se intenta archivarla, **entonces** no se permite; se ofrece la posibilidad de eliminarla.
- **Dada** una lista con al menos una compra registrada y ninguna entrada pendiente, **cuando** un familiar selecciona «Finalizar compra», **entonces** la lista pasa al historial y deja de aparecer entre las activas.
- **Dada** una compra parcial con cantidad restante pendiente, **cuando** se intenta finalizar la lista, **entonces** sigue bloqueada; si el familiar marca la entrada como satisfecha con la cantidad realmente adquirida, se reevalúa la condición de archivado.

### CA-SL-06 — Eliminación de lista activa

- **Dada** una lista específica activa, **cuando** un familiar solicita eliminarla, **entonces** se solicita confirmación antes de ejecutar la operación.
- **Dada** una lista activa con compras registradas y entradas pendientes, **cuando** se confirma su eliminación, **entonces** desaparece como lista activa, las entradas pendientes se descartan y las compras realizadas permanecen en el historial general.
- **Dada** una compra histórica conservada de una lista eliminada, **cuando** se consulta, **entonces** se muestra su nombre de lista original y la indicación «lista eliminada».
- **Dada** una lista activa que nunca ha registrado compras, **cuando** se elimina, **entonces** no se añade una lista vacía al historial de archivadas.

### CA-SL-07 — Historial y eliminación de archivadas

- **Dada** una lista archivada, **cuando** se abre desde el historial de listas, **entonces** pueden consultarse sus compras, cantidades, precios disponibles y atribuciones, pero no modificarse.
- **Dadas** compras de la lista permanente, listas activas, archivadas y activas posteriormente eliminadas, **cuando** se consulta el historial general, **entonces** aparecen las compras conservadas de esos orígenes.
- **Dada** una lista archivada, **cuando** se solicita eliminarla definitivamente, **entonces** se requiere confirmación y se advierte de la eliminación de su historial asociado.
- **Dada** una lista archivada cuya eliminación definitiva se ha confirmado, **cuando** se consulta de nuevo el historial, **entonces** no aparece la lista ni su historial de compras asociado.

### CA-SL-08 — Permisos, auditoría y conectividad

- **Dado** cualquier perfil activo, **cuando** gestiona una lista, **entonces** puede realizar todas las acciones permitidas para su estado, sin permisos administrativos adicionales.
- **Dada** una creación, edición, eliminación o finalización de lista, **cuando** el servidor confirma la operación, **entonces** la actividad registra al menos perfil autor, tipo de acción, lista afectada y fecha/hora, conforme a `family-access`.
- **Dada** una pérdida de conexión con el servidor o la VPN, **cuando** se intenta modificar una lista, **entonces** no se presenta un éxito falso; se muestra un error claro y no se presupone persistencia local.

## 5. Requisitos no funcionales aplicables

| ID | Restricción | Estado |
|---|---|---|
| RNF-SL-01 | Android y Web acceden a las mismas listas y a la misma información compartida. | Confirmado |
| RNF-SL-02 | Backend autohospedado en Ubuntu Server y acceso remoto mediante VPN. | Confirmado |
| RNF-SL-03 | Disponibilidad durante las horas de uso previstas; no se requiere operación 24/7. | Confirmado |
| RNF-SL-04 | Sin edición offline en el MVP; los fallos de conexión se comunican y los cambios no confirmados no se presentan como guardados. | Confirmado |
| RNF-SL-05 | Navegación y gestión de listas comprensibles para 3–4 familiares; evitar operaciones y formularios innecesarios. | Confirmado |
| RNF-SL-06 | No enviar notificaciones por cada producto añadido. | Confirmado |
| RNF-SL-07 | El objetivo de validación de añadir un producto habitual en menos de 5 segundos y el de ≥80 % de adopción son **métricas propuestas del piloto**, no resultados demostrados. | Propuesto, pendiente de evaluación |

## 6. Estrategia de verificación

- **Funcional:** existencia y protección de la lista permanente; altas, edición, eliminación y archivado de específicas; fechas y nombres duplicados.
- **Reglas de estado:** impedir archivado con pendientes o sin compras; permitirlo cuando todas las entradas estén resueltas y haya compras; compras parciales con y sin pendiente restante.
- **Historial:** compras individuales de lista permanente; consulta de lista archivada; conservación de compras al eliminar una activa; retirada del historial asociado al eliminar una archivada.
- **Integridad entre vistas:** una acción en la vista global solo actualiza la entrada de origen; listas con productos iguales siguen siendo independientes.
- **Colaboración:** cambios desde Android y Web con perfiles distintos; autoría atribuida y datos coherentes tras actualizar o reabrir.
- **Conectividad:** simular caída del servidor/VPN durante cambios y verificar el tratamiento de errores sin falsas confirmaciones.
- **Usabilidad:** reconocer permanentemente la lista principal; identificar listas homónimas por fecha; comprender cuándo se habilita la finalización.

Las herramientas, comandos de test, objetivos de cobertura, tecnología de persistencia y diseño de interfaces se determinarán en las fases posteriores de diseño y planificación. **No se autoriza implementación con este documento.**

## 7. Límites para futuras decisiones técnicas

**Siempre:** preservar independencia entre listas, comprobar condiciones de archivado, aplicar confirmaciones de eliminación, mantener la atribución de acciones y evitar falsas confirmaciones de escritura.  
**Consultar antes:** cambiar la política de conservación/eliminación histórica, añadir archivado automático, introducir permisos distintos o fusionar automáticamente entradas equivalentes.  
**Nunca:** archivar una lista específica con pendientes; archivar una lista sin compras; borrar la lista permanente; tratar productos no comprados como compras realizadas; borrar las compras realizadas por eliminar una lista *activa*.

**Diferido a diseño:** arquitectura, esquema de datos, API, mecanismos de actualización entre dispositivos, formato de fechas, reglas para empates de fecha/hora de creación, estructura de proyecto, convenciones de código y comandos.

## 8. Cuestiones abiertas y responsabilidades de otras capacidades

1. **Auditoría y borrado definitivo:** decidir si los eventos de actividad asociados a una lista archivada eliminada también se purgan, se anonimizan o conservan solo información mínima. Debe conciliarse con `family-access` y la expectativa de eliminación definitiva.
2. **Historial general:** concretar en `purchase-tracking` la representación de compras individuales, compras parciales, importes opcionales y referencia a una lista eliminada.
3. **Entradas de compra:** definir en `shopping-items` nombre, cantidad/unidad, supermercado opcional, ediciones, eliminación, repetición de productos y los filtros por establecimiento.
4. **Sincronización mientras la aplicación permanece abierta:** el MVP exige coherencia y datos actualizados al abrir o consultar, pero no se ha confirmado una estrategia de actualización en vivo.
5. **Detalles de presentación:** formatos de fecha, aspecto del selector e interacción de confirmaciones se diseñarán en UX. No se prescribe una distribución visual concreta.

Estas cuestiones **no invalidan** las reglas SL-01–SL-18; se resolverán en sus capacidades o en diseño sin inventar requisitos adicionales.

## 9. Criterio de cierre y siguiente paso

Se considerará funcionalmente especificada `shopping-lists` cuando las reglas SL-01–SL-18 y los escenarios CA-SL-01–CA-SL-08 estén revisados y aceptados. Esta aceptación **no autoriza desarrollar código**.

**Siguiente capacidad:** `shopping-items`, para especificar productos y entradas, cantidades, notas, supermercado y gestión desde listas o vista global.
