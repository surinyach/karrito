# Karrito — Refinamiento de la idea

**SDLC:** 1.2 · Idea Refinement (`idea-refine`)  
**Estado:** Dirección y alcance del MVP aprobados  
**Fecha:** 9 de octubre de 2026  
**Documento de origen:** [`../intent/statement-of-intent.md`](../intent/statement-of-intent.md)

## Problema

**¿Cómo podemos conseguir que una familia organice sus compras desde una única herramienta compartida, sin los olvidos y las confusiones provocados por el papel y los mensajes dispersos de WhatsApp?**

**Usuarios piloto:** 3–4 familiares. **Resultado deseado:** que adopten Karrito como medio habitual para anotar, consultar y completar sus compras, dejando de necesitar listas paralelas.

## Dirección elegida

**Una aplicación web y móvil de listas familiares compartidas, optimizada para añadir productos con el mínimo esfuerzo.** La prioridad del primer incremento es la adopción cotidiana, no la sofisticación del catálogo ni la optimización de precios.

- **Captura rápida:** un nombre basta para añadir un producto; los demás detalles pueden completarse después. Evitar formularios largos. Las sugerencias rápidas se estudiarán sin convertir el catálogo avanzado en requisito del MVP.
- **Modelo híbrido desde el principio:** lista permanente para compras puntuales, listas creadas para propósitos concretos y vista unificada de pendientes.
- **Colaboración silenciosa:** información compartida y actualizada al abrir la aplicación, sin notificar cada producto añadido. La actualización en directo mientras permanece abierta se concretará en la especificación.
- **Evolución incremental:** conservar la posibilidad de añadir funcionalidades de precios y catálogo en futuras iteraciones sin condicionar la utilidad del MVP a integraciones externas.

### Alternativas consideradas

| Alternativa | Decisión | Motivo |
|---|---|---|
| Solo una lista familiar permanente | Descartada para el MVP | No cubre compras organizadas por propósito y exigiría cambiar el flujo habitual después. |
| Lista permanente + listas específicas | **Elegida** | Atiende tanto necesidades puntuales como compras planificadas. |
| Catálogo y precios avanzados desde el lanzamiento | Pospuesta | Aumentaría la fricción y complejidad antes de validar la adopción. |

## Alcance aprobado del MVP

1. **Lista permanente compartida:** siempre activa; cada producto marcado como completado genera su registro de compra archivado individualmente. La lista en sí nunca se archiva.
2. **Listas por propósito:** cualquier familiar puede crear listas independientes. Una lista solo puede archivarse **manualmente** cuando no queden productos pendientes; los pendientes deben completarse o eliminarse antes.
3. **Vista global de pendientes:** muestra las entradas de todas las listas con su lista de origen. Cada entrada conserva su identidad y estado; los productos iguales de listas distintas **no se fusionan**. Las actualizaciones se reflejan en su lista de origen.
4. **Productos y compra rápida:** se puede añadir un producto genérico por nombre, sin exigir marca, supermercado ni precio. La cantidad y otras indicaciones se concretarán según las necesidades de uso. Se mantienen las vistas/organización por supermercado cuando esté indicado.
5. **Seguimiento de compras:** marcar productos como comprados, incluyendo **compras parciales**: registrar la cantidad realmente adquirida y decidir si se completa la entrada o si el resto sigue pendiente. Al completar la compra se identifica el supermercado real si aún no estaba seleccionado. Nunca registrar como adquiridas unidades no compradas.
6. **Colaboración y acceso sencillo:** 3–4 familiares inicialmente, con los mismos permisos, identidad básica persistente y trazabilidad visible básica de quién realizó las acciones. El mecanismo que evite atribuciones erróneas se decidirá en diseño.
7. **Sincronización y consulta:** los cambios son coherentes entre dispositivos y vistas y pueden consultarse al abrir la aplicación; sin notificaciones al añadir productos.
8. **Historial básico:** consultar las compras individuales de la lista permanente y las listas archivadas; permitir su eliminación definitiva. La granularidad y retención de la auditoría requieren especificación.
9. **Precio opcional:** puede registrarse al completar una compra, pero **no es obligatorio** para marcarla como comprada. El modelo detallado de precios queda fuera del MVP.

## No haremos en el MVP

- **Catálogo avanzado** de marcas, variantes, presentaciones y disponibilidad por tienda: no es necesario para validar las listas compartidas.
- **Gestión avanzada de precios:** cálculo detallado por unidad, descuentos, históricos comparativos y analítica; el precio simple será opcional.
- **Obtención automática de precios y promociones** de Mercadona, Alcampo, Ametller Origen y Carrefour: requiere una validación independiente de fuentes y fiabilidad.
- **Comparación o recomendación automática de supermercados:** no forma parte del problema principal de adopción.
- **Estadísticas e historial avanzado:** se prioriza un historial básico comprensible.
- **Notificaciones por cada producto añadido:** se consideran intrusivas para el uso familiar previsto.

## Hipótesis y validación

| Hipótesis por comprobar | Cómo validarla |
|---|---|
| Los familiares sustituirán papel y WhatsApp. | Piloto de **4 semanas** con 3–4 familiares; observar qué canal usan realmente. |
| Añadir un producto es suficientemente rápido. | Pruebas con productos habituales y usuarios reales; medir tiempo e interacciones. |
| El modelo híbrido se comprende sin explicaciones largas. | Observar creación de listas, navegación y vista global sin asistencia. |
| La sincronización evita olvidos y confusiones. | Revisar incidencias de productos duplicados, omitidos o marcados por error durante el piloto. |

**Indicadores propuestos, aún no aprobados como umbrales formales:** al menos **80 %** de los productos anotados directamente en Karrito y **menos de 5 segundos** para añadir un producto habitual. El éxito principal es el uso sostenido por los familiares sin recurrir a listas paralelas.

**Riesgo principal:** que la incorporación de productos sea menos cómoda que WhatsApp. **Mitigación:** priorizar la captura rápida y probarla con la familia antes de añadir complejidad.

## Cuestiones abiertas para la validación y especificación

- ¿Qué nivel mínimo de detalle se necesita al introducir productos y cómo se realiza la compra parcial sin alargar el flujo?
- ¿Basta con actualizar al abrir la aplicación o también se requiere actualización inmediata mientras está abierta?
- ¿Qué información mínima del precio opcional se captura inicialmente, y cómo se selecciona con rapidez el supermercado real al comprar?
- ¿Cómo se garantiza una atribución razonable de acciones con un acceso familiar sencillo?
- ¿Qué comportamiento tendrá el historial básico frente a la eliminación definitiva y los registros de actividad?
- ¿Qué valores de uso observados justifican ampliar el catálogo, las funciones de precios o las notificaciones?

## Próximo paso

**Etapa 1.3 · Validación:** contrastar el flujo propuesto con los familiares y concretar las métricas del piloto. Después, usar `spec-driven-development` para definir requisitos y criterios de aceptación del primer incremento.

> **Trazabilidad:** este documento **refina y sustituye el alcance provisional de MVP** del Statement of Intent de la etapa 1.1. No invalida la visión a largo plazo ni los requisitos aplazados para iteraciones posteriores.
