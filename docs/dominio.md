# Dominio operativo de solicitudes y turnos

## Alcance y procedencia de las reglas

Este documento prepara la **Fase 3A** para auditoría: especifica solicitudes de
extras, líneas, plazas, asignaciones, declaraciones de fin y confirmaciones. No
diseña tablas ni desarrolla liquidación económica o gestión documental.

Las reglas rotuladas **Requisito del PDF** proceden exclusivamente del resumen
de las páginas indicado por Work; el PDF no está disponible en el repositorio y
no se afirma haberlo leído. Las **Decisiones técnicas** se identifican como
aprobadas o propuestas. Las **Decisiones pendientes** no son comportamiento
confirmado.

**Información aportada por el usuario:** actualmente no existen clientes con
varios restaurantes. La base operativa del MVP es que un Cliente corresponde a
un restaurante o local. La gestión de varios locales es una ampliación
**futura**, no exigida para el MVP. Esta aclaración no procede del PDF y no
resuelve el número de cuentas o contactos que podrán acceder a un Cliente ni la
compatibilidad entre roles.

**Decisión técnica de Work (aprobada):** Cliente y Usuario son entidades
separadas. Las solicitudes y el historial pertenecen al Cliente, no a una cuenta
de acceso. Cada servicio conserva su ubicación para que un cambio posterior del
perfil del Cliente no altere el histórico.

## Glosario operativo

| Término | Definición |
| --- | --- |
| Usuario | Identidad de acceso. No equivale por sí sola a Cliente ni a Personal. |
| Cliente | Destinatario del servicio y titular de su historial y saldo. En el MVP corresponde a un restaurante o local. |
| Personal | Persona que realiza el trabajo. |
| Función | Tipo de trabajador solicitado, por ejemplo camarero o cocinero. |
| Solicitud de extras | Petición de personal para un servicio. |
| Línea | Función y cantidad solicitadas. Su agrupación dentro de una solicitud es la propuesta pendiente D01. |
| Plaza o turno individual | Una unidad de personal solicitada; varias plazas no constituyen una única asignación. |
| Asignación | Vínculo histórico entre una plaza y un trabajador. |
| Declaración de fin | Valor introducido por Cliente o Personal para el trabajador y la asignación correspondientes. |
| Confirmación | Valor definitivo del fin y procedencia de la decisión. |

## Reglas e invariantes

1. **Requisito del PDF (pp. 1 y 4):** el Cliente solicita cantidad, tipo de
   personal, fecha y hora de inicio.
2. **Requisito del PDF (pp. 2 y 4):** Administración gestiona solicitudes y
   asigna Personal. Cada asignación corresponde a una plaza individual; por
   tanto, cubrir una plaza no equivale a cubrir las restantes.
3. **Requisito del PDF (p. 4):** el Cliente puede modificar o eliminar una
   solicitud pendiente antes de que Administración asigne Personal. La conducta
   desde una primera asignación, incluida la cobertura parcial, depende de D03.
4. **Requisito del PDF (p. 2):** «En curso» significa asignado y sin fin
   confirmado, incluso cuando la fecha del servicio es futura. No significa que
   el trabajo haya comenzado físicamente.
5. **Requisito del PDF (pp. 3–4):** Cliente y Personal pueden introducir y
   modificar sus respectivas declaraciones de fin mientras no exista
   confirmación.
6. **Requisito del PDF (pp. 3–4):** la coincidencia de ambas declaraciones
   permite confirmar el fin. Qué constituye coincidencia está pendiente en D05;
   hasta resolverla no se presupone tolerancia ni precisión.
7. **Requisito del PDF (pp. 3–4):** Administración puede fijar directamente el
   fin definitivo sin ambas declaraciones. Tras confirmar, solo Administración
   puede corregirlo.
8. **Requisito del encargo:** toda corrección definitiva conserva autor, momento,
   valor anterior y valor nuevo. La confirmación conserva además la procedencia
   de la decisión.
9. **Requisito del PDF (p. 4):** Personal consulta sus turnos y el inicio, la
   ubicación y el uniforme correspondientes.
10. **Requisito del encargo:** declaraciones y confirmación se vinculan a la
    asignación y al trabajador concretos. Una sustitución conserva el historial
    y no hereda silenciosamente las declaraciones del trabajador anterior.
11. **Decisión técnica aprobada por Work:** solicitudes e historial pertenecen
    al Cliente y el servicio conserva una copia histórica de su ubicación.
12. **Requisito del encargo:** cobertura de plazas, cierre del trabajo y
    liquidación económica son hechos distintos. Un turno con fin confirmado no
    implica que el Cliente haya pagado ni que el Personal haya cobrado.
13. **Decisión técnica aprobada:** el backend autoriza cada acción por rol,
    recurso, pertenencia y estado; ocultar controles en la interfaz no basta.

No se definen todavía duración facturable, descansos, mínimos, suplementos,
cancelaciones, ausencias ni efectos económicos.

## Tabla de estados y acciones

Los nombres describen el flujo verificable y no anticipan un esquema de base de
datos. «Parcialmente cubierta» es una condición de cobertura, no un estado de
cierre.

| Ámbito / estado observable | Significado confirmado | Acciones confirmadas | Salida o condición |
| --- | --- | --- | --- |
| Solicitud pendiente, sin asignaciones | Petición registrada sin Personal asignado. | Cliente propietario modifica o elimina; Administración gestiona y asigna. | Una asignación inicia «En curso» para su plaza; D03 decidirá el bloqueo global. |
| Plaza sin cubrir | Unidad solicitada sin asignación vigente. | Administración puede asignar Personal. | Pasa a plaza asignada. |
| Solicitud parcialmente cubierta | Al menos una plaza está asignada y al menos otra no lo está. | Administración continúa gestionando y asignando. | La edición/eliminación del Cliente no se afirma: depende de D03. |
| Asignación «En curso» | Personal asignado y fin aún no confirmado, aunque el inicio sea futuro. | Cliente y Personal declaran o modifican su fin; Administración puede fijar el definitivo. | Coincidencia según D05 o cierre directo de Administración. |
| Fin discrepante | Existen declaraciones no coincidentes y no hay confirmación. | Cliente y Personal pueden modificar su propia declaración; Administración puede fijar el definitivo. | Coincidencia según D05 o cierre directo administrativo. |
| Fin confirmado | Existe un valor definitivo con procedencia. | Consulta; solo Administración puede corregir. | Una corrección genera nueva trazabilidad sin borrar valores anteriores. |
| Liquidación | Estado económico separado, aún no especificado. | Fuera de Fase 3A. | No cambia automáticamente por completar el turno. |

## Matriz de permisos del flujo operativo

«Propio» para Cliente significa que el recurso pertenece a su entidad Cliente;
para Personal, que la asignación vincula a ese trabajador. Toda fila se aplica
en backend.

| Rol | Recurso propio o ajeno | Acción | Condición de estado | Resultado |
| --- | --- | --- | --- | --- |
| Cliente | Cliente propio | Crear solicitud de extras | Sin condición adicional confirmada | Permitido con cantidad, función, fecha e inicio. |
| Cliente | Solicitud propia | Consultar | Cualquier estado | Permitido. |
| Cliente | Solicitud ajena | Consultar, modificar o eliminar | Cualquier estado | Denegado. |
| Cliente | Solicitud propia | Modificar o eliminar | Pendiente y sin asignaciones | Permitido. |
| Cliente | Solicitud propia | Modificar o eliminar | Con una o más asignaciones | Pendiente de D03; no se concede implícitamente. |
| Cliente | Asignación de una plaza de solicitud propia | Crear o modificar su declaración de fin | Sin fin confirmado | Permitido para esa asignación y trabajador. |
| Cliente | Asignación ajena | Consultar o declarar fin | Cualquier estado | Denegado. |
| Cliente | Asignación propia | Modificar fin definitivo | Confirmado | Denegado. |
| Personal | Asignación propia | Consultar turno, inicio, ubicación y uniforme | Cualquier momento visible | Permitido. |
| Personal | Asignación ajena | Consultar o declarar fin | Cualquier estado | Denegado. |
| Personal | Asignación propia | Crear o modificar su declaración de fin | Sin fin confirmado | Permitido; no altera la declaración del Cliente. |
| Personal | Asignación propia | Modificar fin definitivo | Confirmado | Denegado. |
| Administración | Cualquier solicitud o plaza | Consultar, gestionar y asignar Personal | Según integridad del flujo; cancelaciones/sustituciones dependen de D07 | Permitido. |
| Administración | Cualquier asignación | Fijar directamente el fin definitivo | Sin fin confirmado; no requiere ambas declaraciones | Permitido y auditado. |
| Administración | Cualquier asignación | Corregir fin definitivo | Confirmado | Permitido con autor, momento, valor anterior y nuevo. |

La visibilidad temporal exacta de turnos para Personal y el alcance de
«gestionar» fuera de las acciones citadas no están definidos por el resumen.

## Decisiones pendientes

Ninguna propuesta de esta tabla está confirmada por el usuario. Debe resolverse
antes de aprobar el modelo afectado.

| ID | Pregunta | Propuesta de Work (no aprobada) | Impacto | Momento límite |
| --- | --- | --- | --- | --- |
| D01 | ¿Cómo se agrupan funciones y cantidades? | Una solicitud para el mismo Cliente, ubicación e inicio, con líneas por función y plazas individuales. | Identidad, cardinalidades, edición y cobertura. | Antes de aprobar el modelo de solicitudes. |
| D02 | ¿Cuántas cuentas y contactos acceden a un Cliente? | Sin propuesta cerrada. Varios locales queda fuera del MVP, pero eso no responde cuentas/contactos. | Autorización, comunicaciones y cardinalidades. | Antes de aprobar cuentas, perfiles y permisos. |
| D03 | ¿Qué se edita tras una asignación parcial y qué ocurre si se retiran todas? | Bloquear toda la solicitud desde la primera asignación; concretar el posible desbloqueo. | Estados, permisos, historial y concurrencia. | Antes de aprobar transiciones y modelo operativo. |
| D04 | ¿El inicio es previsto o real? ¿Cómo se tratan descansos y precisión de duración? | Sin propuesta cerrada; no aplicar descuentos, mínimos ni suplementos implícitos. | Cierre, duración y futuros cálculos. | Antes de aprobar campos temporales y cualquier cálculo. |
| D05 | ¿Cuándo coinciden dos finales? | Igualdad exacta del mismo instante con precisión acordada, sin tolerancia implícita. | Confirmación automática, interfaz y almacenamiento temporal. | Antes de aprobar la transición a confirmado. |
| D07 | ¿Cómo funcionan cancelaciones, ausencias y sustituciones? ¿Hacen falta tramos separados? | Sin propuesta cerrada. | Historial de asignación, declaraciones, cobertura y cálculo futuro. | Antes de aprobar sustituciones y modelo afectado. |
| D13 | ¿Cómo funcionan cuentas, compatibilidad de roles y activación del acceso? | Sin propuesta cerrada. | Identidad, autorización y ciclo de acceso. | Antes del modelo de cuentas y la primera migración. |

## Casos de aceptación escritos

Son ejemplos documentales, no tests ejecutados.

| Caso | Ejemplo y resultado esperado | Procedencia | Pendientes condicionantes |
| --- | --- | --- | --- |
| Dos camareros y un cocinero | El Cliente pide esas cantidades, funciones, fecha e inicio. Deben distinguirse tres plazas; no se presupone todavía si hay una solicitud con dos líneas. | PDF pp. 1 y 4; distinción plaza/asignación del encargo. | D01 define agrupación. |
| Cobertura parcial | Administración asigna un trabajador a una de las plazas de camarero. Esa plaza queda asignada y las otras dos siguen sin cubrir; no existen asignaciones colectivas. La edición global del Cliente no se decide. | PDF pp. 2 y 4; encargo. | D03. |
| Acceso ajeno | Un Cliente intenta consultar una solicitud perteneciente a otro Cliente. El backend lo deniega, aunque conozca su identificador. | Decisión técnica aprobada de autorización; encargo. | D02 y D13 concretarán cuentas, sin rebajar el aislamiento. |
| Finales coincidentes | Cliente y Personal declaran un fin que satisface la regla de coincidencia que se apruebe; se permite confirmar y se conserva su procedencia. | PDF pp. 3–4. | D05 define igualdad y precisión. |
| Finales discrepantes | Las declaraciones difieren: no se confirma por coincidencia; cada parte puede cambiar la suya mientras siga sin confirmar y Administración puede fijar el fin. | PDF pp. 3–4. | D05 define la discrepancia efectiva. |
| Cierre administrativo directo | Administración fija el valor definitivo sin esperar ambas declaraciones; quedan valor, procedencia, autor y momento. | PDF pp. 3–4; trazabilidad del encargo. | D04 afecta la representación temporal. |
| Cambio después del cierre | Cliente o Personal intenta modificar una declaración o el fin tras confirmar. El backend lo deniega; Administración sí puede corregir conservando valores anterior y nuevo, autor y momento. | PDF pp. 3–4; encargo. | D04 afecta la representación, no el permiso. |
| Fin al día siguiente | Un servicio empieza una fecha y su fin se declara en la siguiente. Se conserva un instante de fin capaz de representar el cambio de día; duración, descansos y cobro no se infieren. | PDF pp. 3–4; separación exigida por el encargo. | D04. |
| Sustitución con historial | Una sustitución no borra la asignación anterior ni transfiere sus declaraciones al nuevo trabajador; cada declaración sigue vinculada a su trabajador y asignación. El mecanismo y posibles tramos no se presuponen. | Requisito del encargo. | D07. |
