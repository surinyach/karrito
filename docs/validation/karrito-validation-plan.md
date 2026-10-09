# Karrito — Plan de validación

**SDLC:** 1.3 · Validación de la idea  
**Estado:** Plan acordado; pruebas aún no ejecutadas  
**Fecha:** 9 de octubre de 2026  
**Documentos de referencia:** [Intención del producto](../intent/statement-of-intent.md) · [Refinamiento de la idea](../ideas/karrito.md)

## 1. Objetivo

Verificar que **Karrito sustituye de forma habitual el papel y los mensajes dispersos de WhatsApp** para coordinar las compras de una familia de **3–4 personas**, con menos olvidos y confusiones y sin exigir formularios largos.

La validación es **de uso familiar, no de viabilidad comercial**: el promotor quiere desarrollar una solución propia y autohospedada, sin depender de suscripciones para ampliar funcionalidades.

## 2. Supuestos y decisiones confirmadas

- **Usuarios piloto:** 3–4 familiares, todos con Android. El **MVP incluye Android y Web**; iOS queda fuera.
- **Alojamiento:** servidor propio con Ubuntu Server, encendido durante las horas previstas de uso; no se exige disponibilidad 24/7.
- **Acceso remoto:** mediante VPN, configurada previamente por el promotor y activada manualmente por cada familiar.
- **Conectividad MVP:** se requiere comunicación con el servidor para modificar datos; mostrar errores claros y no confirmar operaciones sin guardar. Sin modo de edición offline ni sincronización diferida.
- **Sin notificaciones** por cada producto añadido; consultar los cambios al abrir la aplicación. Queda pendiente decidir la actualización en tiempo real mientras permanece abierta.
- **Prioridad:** adopción y rapidez antes que riqueza funcional; añadir un producto genérico por su nombre, dejando opcionales otros detalles.
- **Modelo de listas:** una lista permanente (compras archivadas individualmente), listas específicas (archivado manual solo sin pendientes) y una vista global de pendientes. Las entradas repetidas en varias listas son independientes.
- **Compra parcial:** registrar cantidad realmente comprada y elegir entre completar el producto o mantener la cantidad restante pendiente.
- **Precio:** opcional al completar un producto en el MVP. Gestión detallada y obtención automática quedan para después.
- **Colaboración:** permisos iguales, identidad familiar sencilla y trazabilidad básica visible. El mecanismo concreto de atribución sigue pendiente.

## 3. Hipótesis que queremos contrastar

| ID | Hipótesis | Evidencia esperada |
|---|---|---|
| H1 | La familia prefiere Karrito a papel y WhatsApp. | Uso sostenido y menos anotaciones en canales paralelos. |
| H2 | Añadir productos habituales es suficientemente rápido. | La tarea se completa con muy pocas interacciones y sin ayuda. |
| H3 | El modelo híbrido se entiende fácilmente. | Los participantes distinguen lista permanente, listas específicas y vista global. |
| H4 | La información compartida evita confusiones. | Los cambios aparecen correctamente entre usuarios y listas, sin compras duplicadas por descoordinación. |
| H5 | El autohospedaje con VPN es operativo para la familia. | Los participantes acceden y completan compras dentro del horario del servidor sin dificultades recurrentes. |

## 4. Experimentos y momentos de ejecución

### A. Antes de implementar: revisión de alternativas y prototipo

1. **Benchmark ligero:** probar los flujos de añadir, consultar y completar productos en 1–2 aplicaciones existentes como referencia de UX; no es necesario superarlas comercialmente.
2. **Prototipo navegable:** comprobar con los familiares las tareas: añadir un producto genérico, crear una lista para un propósito, localizar un pendiente en la vista global, marcar una compra parcial y finalizar una lista elegible.
3. **Observación:** anotar pasos, errores, dudas, tiempos y sugerencias; priorizar cambios que reduzcan fricción antes de codificar.

### B. Tras implementar el MVP: piloto de uso real

1. **Duración propuesta:** cuatro semanas, con los 3–4 familiares.
2. **Uso ordinario:** sustituir los canales de compra existentes en la medida de lo posible, sin prohibir su uso como respaldo.
3. **Medición ligera:** registrar altas de productos en Karrito y anotar periódicamente las altas hechas solo en papel/WhatsApp; recoger incidencias de sincronización, acceso, duplicados y olvidos.
4. **Revisión final:** evaluar métricas y opiniones; decidir qué mejorar en la siguiente iteración Agile.

El piloto de cuatro semanas **no puede considerarse ejecutado ni validado** hasta disponer de un MVP funcional. La validación previa con prototipos no prueba todavía adopción a largo plazo.

## 5. Indicadores de éxito propuestos

| Indicador | Objetivo inicial | Método |
|---|---|---|
| Adopción | **≥ 80 %** de nuevas anotaciones de productos realizadas directamente en Karrito. | Altas en Karrito / (altas en Karrito + anotaciones realizadas exclusivamente fuera de Karrito), durante el piloto. |
| Rapidez | **< 5 segundos** para añadir un producto habitual. | Cronometrar la tarea desde la decisión de añadirlo hasta la confirmación, en pruebas de usabilidad. |
| Comprensión | Los 3–4 usuarios completan los flujos básicos sin ayuda recurrente. | Observación de tareas y feedback cualitativo. |
| Consistencia | Sin incidencias importantes de sincronización que provoquen compras erróneas. | Registro y revisión de incidencias. |
| Acceso | Utilización habitual en Android y pruebas funcionales de la Web, mediante VPN cuando corresponda. | Pruebas de acceso y reporte de dificultades. |
| Desconexión | Ningún cambio fallido se muestra como guardado correctamente. | Prueba deliberada de pérdida de conexión/VPN. |

Estos objetivos se utilizarán como **referencias de evaluación**, no como resultados ya demostrados ni como garantías técnicas. Ajustarlos requiere una decisión explícita basada en lo observado.

## 6. Riesgos y mitigaciones

| Riesgo | Mitigación durante la validación |
|---|---|
| WhatsApp o papel siguen siendo más rápidos. | Reducir pasos y repetir pruebas de incorporación rápida. |
| El modelo híbrido genera confusión. | Ensayar navegación, nombres de listas y estados con usuarios reales. |
| La VPN se olvida o el servidor no está encendido. | Verificar acceso remoto y anotar incidentes y horarios reales de uso. |
| Pérdida de conexión o cambios no reflejados. | Comprobar mensajes de error y consistencia tras reconectar. |
| La trazabilidad sencilla atribuye acciones erróneamente. | Probar el cambio de identidad y revisar el mecanismo en la especificación. |
| Se añaden demasiadas funcionalidades antes de probar el producto. | Mantener catálogo avanzado, precios avanzados y estadísticas fuera del MVP. |

## 7. Criterio para cerrar la validación

- **Antes del desarrollo:** revisar el prototipo con los usuarios y registrar los problemas principales; incorporar los cambios relevantes a la especificación del MVP.
- **Después del piloto:** comparar métricas con objetivos, recoger feedback y decidir entre mantener, corregir o ampliar el producto.
- **En Agile:** una hipótesis no validada no bloquea necesariamente toda implementación; se conserva como riesgo explícito y se vuelve a evaluar con software funcionando.

## 8. Fuera de esta etapa

No se seleccionan frameworks, arquitectura, base de datos ni estrategia de despliegue. No se implementa código. La obtención automática de precios, su comparación entre supermercados, el catálogo avanzado y la analítica detallada corresponden a iteraciones posteriores.

## 9. Próximo paso

**Etapa 1.4 · Especificación (`spec-driven-development`):** comprobar primero si el alcance debe dividirse en capacidades verificables; después redactar especificaciones con requisitos y criterios de aceptación, distinguiendo hechos confirmados de decisiones de diseño pendientes.
