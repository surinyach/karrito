# Karrito — Statement of Intent

**Etapa del SDLC:** 1.1 · Descubrimiento del problema (`interview-me`)  
**Estado:** Intención del producto confirmada por el promotor  
**Fecha:** 9 de octubre de 2026

## 1. Problema

Actualmente, la familia organiza sus compras mediante listas en papel, mensajes de WhatsApp y notas personales. La información queda repartida entre varios lugares y personas, lo que genera olvidos, confusiones y falta de coordinación sobre qué productos están pendientes o ya se han comprado.

## 2. Usuarios y contexto

Los usuarios iniciales serán los miembros de una misma familia. Todos deben poder participar en la gestión de las compras desde una interfaz web o móvil, incluso si tienen poca familiaridad con aplicaciones complejas.

## 3. Visión del producto

**Karrito** será una aplicación familiar de compras que centralice la información en un espacio compartido, permita organizar compras puntuales o planificadas, reutilizar un catálogo de productos y conservar un historial fiel de las compras realizadas.

La prioridad es **hacer que la compra familiar sea fácil de coordinar y difícil de olvidar**, sin aumentar el trabajo de quienes apuntan y compran los productos.

## 4. Resultado esperado y criterio inicial de éxito

- Sustituir el uso disperso de papel y mensajes por una fuente compartida de información.
- Permitir a todos los familiares conocer y actualizar los productos pendientes.
- Evitar confusiones sobre qué lista contiene cada producto y qué se ha comprado realmente.
- Mantener una experiencia sencilla para añadir, comprar y consultar productos.

**Validación inicial:** La familia consigue organizar sus compras habituales en Karrito, sin necesitar listas paralelas en papel o WhatsApp. Los indicadores cuantitativos de éxito y las pruebas con usuarios se definirán durante el refinamiento de la idea.

## 5. Alcance funcional previsto para el MVP

1. **Lista permanente:** Espacio compartido siempre activo para compras puntuales. Cada producto completado se archiva individualmente; la lista no se archiva.
2. **Listas por propósito:** Creación de listas independientes, por ejemplo «Compra semanal». Se archivan manualmente y únicamente cuando no quedan productos pendientes; los pendientes deben completarse o eliminarse antes.
3. **Vista global y por establecimiento:** Consulta consolidada de todos los productos pendientes, identificando la lista de origen. Las entradas de distintas listas siguen siendo independientes, incluso cuando representan el mismo producto.
4. **Catálogo reutilizable:** Productos genéricos y variantes de marca o presentación. Al añadir una entrada puede dejarse la marca y/o el supermercado sin definir, para que quien compre los concrete después.
5. **Establecimientos:** Los establecimientos habituales iniciales son Mercadona, Alcampo, Ametller Origen y Carrefour. Se podrán asociar productos y consultar las compras por establecimiento.
6. **Compra y precios manuales:** Registro de cantidad efectivamente comprada, unidad, precio unitario y total calculado; el importe final podrá corregirse para reflejar descuentos u ofertas.
7. **Compras parciales:** Quien compra decide si una cantidad inferior a la prevista satisface la necesidad o si la cantidad restante sigue pendiente. El historial registra la compra real, nunca unidades no compradas.
8. **Colaboración familiar:** Todos los miembros tienen los mismos permisos. Se prioriza un acceso sencillo mediante invitación e identificación persistente, con atribución visible de las acciones a cada familiar.
9. **Historial:** Consulta de compras individuales y listas específicas archivadas, con posibilidad de eliminación definitiva por los usuarios.

## 6. Restricciones y prioridades

- **Usabilidad primero:** La experiencia debe resultar intuitiva y rápida, especialmente desde el móvil.
- **Web y móvil:** Ambas plataformas forman parte de la visión inicial.
- **Información coherente:** Los cambios han de reflejarse entre vistas y miembros de la familia, sin mantener listas duplicadas.
- **Simplicidad técnica:** La arquitectura se decidirá después; no se adoptan tecnologías ni patrones en esta etapa.
- **Trazabilidad:** Es necesario reconocer quién realizó cada acción; el mecanismo concreto de identificación está pendiente de diseño.

## 7. Fuera del MVP

- Obtención y actualización **automática** de precios desde supermercados.
- Comparación **automática** de promociones, ofertas y disponibilidad entre establecimientos.
- Recomendaciones automáticas sobre dónde comprar.

Estas capacidades podrán evaluarse para versiones futuras, tras verificar su viabilidad y el valor aportado.

## 8. Cuestiones pendientes para la siguiente actividad

- Validar con la familia qué flujos resultan realmente imprescindibles y cuáles complicarían el uso.
- Concretar métricas de éxito y priorizar el mínimo conjunto de funcionalidades que pruebe el valor del producto.
- Determinar cómo equilibrar la identificación sencilla de familiares con una atribución fiable de las acciones.
- Precisar reglas de historial y trazabilidad cuando un usuario elimina información definitivamente.
- Analizar la viabilidad y condiciones de futuras integraciones de precios con supermercados.

## 9. Próximo paso

**Etapa 1.2 · Refinamiento de la idea (`idea-refine`)**: contrastar alternativas, riesgos y alcance antes de elaborar la especificación formal de requisitos. Este documento expresa **la intención del producto** y no sustituye al futuro PRD ni a las especificaciones funcionales detalladas.