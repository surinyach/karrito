# Karrito — Especificación de capacidad: `family-access`

**SDLC:** 1.4 · Especificación de requisitos (`spec-driven-development`)  
**Estado:** Requisitos funcionales confirmados; documento listo para revisión y aprobación formal  
**Fecha:** 9 de octubre de 2026  
**Alcance:** MVP · Un único espacio familiar

**Fuentes del producto:** [Statement of Intent](../intent/statement-of-intent.md) · [Refinamiento de la idea](../ideas/karrito.md) · [Plan de validación](../validation/karrito-validation-plan.md)

> Esta especificación incorpora las decisiones posteriores a los documentos de descubrimiento: **se descarta el acceso mediante invitaciones** y se sustituye por **selección libre de perfiles familiares, sin contraseña**, similar a Netflix. Describe los requisitos de esta capacidad, no su arquitectura ni su implementación.

## 1. Objetivo y alcance

Permitir que los 3–4 familiares del piloto utilicen Karrito mediante perfiles sencillos, sin registro ni credenciales, manteniendo una identificación básica de las acciones realizadas en el espacio compartido.

**Dentro del MVP:** creación y gestión de perfiles, selección y persistencia del perfil activo, permisos compartidos y trazabilidad básica visible.  
**Fuera del MVP:** autenticación con contraseña o correo, invitaciones, verificación de identidad real, roles administrativos, gestión de múltiples familias y recuperación de cuentas mediante proveedores externos.

**Dependencias funcionales:** `family-access` no depende de otras capacidades. Proporciona el contexto del **perfil activo** y la atribución de acciones a `shopping-lists`, `shopping-items` y `purchase-tracking`.

## 2. Requisitos funcionales

| ID | Requisito verificable | Prioridad |
|---|---|---|
| FA-01 | Karrito dispone de **un único espacio familiar** compartido por todos los perfiles del MVP. | MVP |
| FA-02 | Al entrar por primera vez, el familiar puede **seleccionar un perfil existente** sin introducir contraseña, correo ni código de invitación. | MVP |
| FA-03 | Cualquier familiar puede **crear un perfil** con nombre obligatorio y avatar opcional. | MVP |
| FA-04 | Si no se elige avatar, el nuevo perfil recibe automáticamente un **avatar predeterminado**. | MVP |
| FA-05 | Los **nombres de perfiles activos son únicos**, sin distinguir mayúsculas y minúsculas; se valida tanto al crear como al renombrar. | MVP |
| FA-06 | Karrito **recuerda el último perfil seleccionado** en cada instalación móvil o navegador y lo recupera al volver a abrir la aplicación. | MVP |
| FA-07 | El familiar puede **cambiar de perfil** desde la configuración, sin contraseña. | MVP |
| FA-08 | **Todos los perfiles tienen los mismos permisos** para gestionar perfiles y colaborar en las demás capacidades, sin roles administrativos. | MVP |
| FA-09 | Cualquier familiar puede **modificar** el nombre y el avatar de cualquier perfil. | MVP |
| FA-10 | Cualquier familiar puede **eliminar un perfil**, previa confirmación. Un perfil eliminado deja de estar disponible para su selección. | MVP |
| FA-11 | No se permite **eliminar el último perfil activo**; debe permanecer al menos uno. | MVP |
| FA-12 | Eliminar un perfil **no elimina ni modifica las listas, compras o acciones históricas** en las que intervino. El historial conserva su nombre previo y muestra «Perfil eliminado». | MVP |
| FA-13 | Las **acciones relevantes** quedan asociadas al perfil seleccionado e incluyen, al menos, autor, tipo de acción, elemento afectado y fecha/hora. | MVP |
| FA-14 | Los familiares pueden **consultar un historial de actividad** con la atribución y fecha de acciones relevantes, sin sobrecargar la interfaz principal. | MVP |
| FA-15 | Si el perfil recordado en un dispositivo ha sido eliminado, la aplicación **no lo utiliza** y solicita seleccionar otro perfil activo. | MVP |

**Acciones relevantes para FA-13/14:** crear, modificar y eliminar productos y listas; marcar compras o registrar cambios de compra; finalizar o archivar listas; y operaciones de gestión de perfiles. La información exacta de los cambios registrados se precisará en los contratos de cada capacidad, sin alterar estos requisitos mínimos.

## 3. Reglas de negocio e invariantes

1. Solo existe **un espacio familiar** durante el MVP y todos los perfiles activos operan sobre los mismos datos.
2. El **nombre es obligatorio**. Dos perfiles activos no pueden tener el mismo nombre al compararlos sin distinción de mayúsculas; `María` y `maría` entran en conflicto.
3. **Siempre existe al menos un perfil activo** una vez creado el primero. No se puede eliminar el último.
4. La eliminación de un perfil **impide utilizarlo en nuevas acciones**, pero conserva la atribución histórica de las acciones ya realizadas, identificándolo como eliminado.
5. El perfil activo se recuerda **por dispositivo o navegador**; cambiarlo en un dispositivo no obliga a cambiarlo en otro.
6. **Ningún perfil tiene privilegios adicionales**. Elegir un perfil sirve para personalizar el uso y atribuir acciones, **no para verificar la identidad real** de la persona.
7. Para operaciones que cambien datos, una respuesta de éxito solo se mostrará cuando el servidor confirme su persistencia. El funcionamiento sin conexión queda fuera del MVP.

## 4. Flujos y criterios de aceptación

### CA-01 — Alta inicial y creación de perfil

- **Dado** un Karrito sin perfiles iniciales, **cuando** se abre por primera vez, **entonces** se ofrece crear el primer perfil.
- **Dado** un nombre válido y ningún avatar elegido, **cuando** se crea el perfil, **entonces** se muestra con el avatar predeterminado.
- **Dado** que existe «María», **cuando** se intenta crear «maría» o renombrar otro perfil a ese nombre, **entonces** se rechaza la operación y se explica el motivo.

### CA-02 — Selección, persistencia y cambio

- **Dado** que existen varios perfiles, **cuando** alguien selecciona uno, **entonces** puede usar Karrito sin iniciar sesión con credenciales.
- **Dado** un perfil elegido previamente, **cuando** se cierra y vuelve a abrir la aplicación en ese mismo dispositivo o navegador, **entonces** se recupera el perfil recordado.
- **Dado** un perfil activo, **cuando** el familiar utiliza la opción de configuración para cambiarlo, **entonces** las nuevas acciones se atribuyen al nuevo perfil.
- **Dado** que el perfil recordado fue eliminado, **cuando** se vuelve a abrir la aplicación, **entonces** no se permite actuar con él y se solicita seleccionar uno vigente.

### CA-03 — Edición y permisos

- **Dado** cualquier perfil activo, **cuando** edita el nombre o avatar de otro perfil, **entonces** puede guardar el cambio si cumple las validaciones.
- **Dado** cualquier perfil activo, **cuando** utiliza las capacidades de listas o compras, **entonces** no se le exige un rol especial para ejecutar las acciones permitidas por dichas capacidades.

### CA-04 — Eliminación de perfiles

- **Dado** más de un perfil activo, **cuando** se solicita eliminar uno, **entonces** se pide confirmación antes de ejecutarlo.
- **Dado** que se confirma la eliminación, **cuando** se completa, **entonces** ese perfil no aparece como seleccionable y sus acciones previas conservan el nombre original con la etiqueta «Perfil eliminado».
- **Dado** que solo queda un perfil activo, **cuando** se intenta eliminar, **entonces** se rechaza y se explica que debe existir al menos un perfil.

### CA-05 — Historial de actividad

- **Dado** un familiar con un perfil activo, **cuando** crea, modifica o completa un elemento relevante, **entonces** el historial registra la acción, el perfil, el elemento y la fecha/hora.
- **Dado** un perfil eliminado, **cuando** se consulta una acción anterior suya, **entonces** se conserva su atribución histórica y se indica que el perfil fue eliminado.
- **Dado** que la actividad se consulta desde Android o Web, **cuando** se abre su vista, **entonces** se muestran las acciones disponibles del espacio familiar compartido.

### CA-06 — Error de conexión

- **Dado** que Karrito no puede comunicarse con el servidor, **cuando** se intenta crear, modificar o eliminar un perfil, **entonces** no se informa de éxito ni se asume que la operación se guardó; se muestra un error comprensible.

## 5. Requisitos no funcionales aplicables

| ID | Restricción | Fuente / estado |
|---|---|---|
| RNF-FA-01 | Compatibilidad del MVP con **Android y Web**. | Confirmada |
| RNF-FA-02 | Backend **autohospedado en Ubuntu Server**; acceso remoto de los familiares mediante **VPN** configurada previamente. | Confirmada |
| RNF-FA-03 | El servicio debe estar disponible **durante las horas previstas de uso**, sin exigencia de funcionamiento 24/7. | Confirmada |
| RNF-FA-04 | **No hay edición offline**: indicar fallos de conexión y no dar por guardados cambios no confirmados por el servidor. | Confirmada |
| RNF-FA-05 | La selección de perfil y su uso habitual deben ser **simples y rápidos**, sin formularios largos ni credenciales. | Confirmada (sin umbral cuantitativo específico para perfiles) |
| RNF-FA-06 | La atribución por perfil es **trazabilidad funcional**, no autenticación fuerte ni garantía de autoría real. | Limitación aceptada |

## 6. Estrategia de verificación

- **Pruebas funcionales:** crear, seleccionar, editar y eliminar perfiles; validación de nombres duplicados; avatar predeterminado; protección del último perfil.
- **Pruebas de persistencia:** cerrar y reabrir Android y navegador; cambiar de perfil; invalidar un perfil recordado por eliminación desde otro dispositivo.
- **Pruebas multiusuario:** varios dispositivos compartiendo perfiles y registrando actividad sin perder la atribución al perfil seleccionado en cada uno.
- **Pruebas de integridad:** comprobar que eliminar un perfil no borra listas, compras ni atribución histórica.
- **Pruebas de conectividad:** pérdida de conexión/VPN durante una operación de escritura; mensaje de error y ausencia de falsos éxitos.
- **Pruebas de usabilidad:** los familiares pueden seleccionar su perfil y empezar a utilizar Karrito sin instrucciones extensas.

Los frameworks de testing, comandos, cobertura objetivo y automatización se definirán en las etapas de diseño y planificación; **no se han elegido aún**.

## 7. Límites para la implementación futura

**Siempre:** respetar las invariantes; validar entradas; atribuir correctamente las acciones; conservar el historial al eliminar perfiles; verificar los criterios de aceptación.  
**Consultar antes:** introducir autenticación, roles, múltiples familias, cambios en la política de conservación de actividad o nuevas dependencias tecnológicas.  
**Nunca:** afirmar que un perfil sin contraseña autentica a una persona; eliminar automáticamente historial de compras al borrar perfiles; permitir borrar el último perfil activo.

**Detalles técnicos diferidos intencionalmente:** stack, estructura física de código, comandos de construcción y test, convenciones de estilo, mecanismos de almacenamiento de perfil y representación de eventos. Su ausencia no autoriza a un agente a elegirlos sin pasar por diseño aprobado.

## 8. Cuestiones abiertas y dependencias con otras capacidades

1. **Conservación y eliminación de eventos:** cuando se borre definitivamente una lista o compra, ¿deben desaparecer también sus eventos del historial? Resolverlo al especificar `shopping-lists` y `purchase-tracking`.
2. **Datos exactos de cada evento:** definir en las otras capacidades qué detalle del cambio se muestra (más allá de autor, acción, objeto y fecha).
3. **Reglas de nombres:** la comparación sin mayúsculas está confirmada; tratamiento de espacios, acentos y nombres de perfiles eliminados que se vuelvan a utilizar queda por definir si resulta necesario.
4. **Diseño técnico:** persistencia por dispositivo, seguridad de acceso VPN y consistencia de operaciones concurrentes se concretarán en arquitectura, sin modificar los comportamientos aprobados.

## 9. Criterio de cierre y siguiente paso

Esta capacidad está **especificada funcionalmente** cuando las reglas FA-01–FA-15 y los escenarios CA-01–CA-06 son revisados y aprobados por el promotor. La aprobación de este documento **no autoriza implementación**.

**Siguiente capacidad:** `shopping-lists` — modelo híbrido de listas, archivado, independencia y vista global (en coordinación con `shopping-items`).
