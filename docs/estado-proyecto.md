# Estado del proyecto

## Inventario observado

Al comenzar esta fase el 12/09/2026, el checkout contenía únicamente `.gitkeep`
versionado, sin cambios locales, en la rama asignada `work`. Git tenía un commit
(`5936aba19bc1843c8004ad958913f156748c6517`) y no había remotos configurados.
No se observó una aplicación ni una copia del PDF. La fase añadió solo los cinco
archivos documentales y de convenciones autorizados; `.gitkeep` se preservó.
Este inventario se conserva como registro histórico del inicio de la Fase 1.

## Estado de las fases

- **Fase 1:** aprobada por Work. Su corrección de trazabilidad quedó incorporada
  en `main` mediante el commit `e2b87ecc3bb69c13c0bbf870d9a3cc95b7c30cce`.
- **Fase 2:** arquitectura y stack documentales aprobados por Work e incorporados
  en `main` mediante el commit de fusión
  `9a024bf5d785e6b1afb44e84acfa54a7f0cc9d3e`.
- **Fase 3A:** especificación de solicitudes de extras y turnos preparada en
  [`dominio.md`](dominio.md), pendiente de auditoría de Work.
- **Fase 3 completa:** abierta; no se ha aprobado ni diseñado el modelo de datos.

La arquitectura aprobada se desarrolla en [`arquitectura.md`](arquitectura.md) y
se razona en el [`ADR 0001`](decisiones/0001-arquitectura-y-stack.md). La
aprobación no resuelve las decisiones aplazadas ni autoriza implementación.

## Base funcional disponible

Lo siguiente es un **resumen facilitado y verificado por Work**; no consta una
lectura directa del PDF durante esta fase:

- **Requisito del PDF (pp. 1–2):** existen los roles Administración, Personal y
  Cliente; hay precios de venta por cliente y función, retribuciones individuales
  por trabajador y excepciones por servicio.
- **Requisito del PDF (pp. 2–3):** Administración gestiona perfiles,
  calendarios y asignaciones; puede crear, rechazar y modificar solicitudes,
  además de asignar Personal. «En curso» significa asignado sin cierre
  confirmado.
- **Requisito del PDF (pp. 3–4):** Cliente y Personal declaran el fin; si
  coinciden, queda confirmado. Administración puede fijar directamente el valor
  definitivo y, después, solo Administración puede corregirlo.
- **Requisito del PDF (p. 4):** Cliente solicita cantidad, tipo, fecha y hora de
  inicio, y puede editar o eliminar solicitudes pendientes antes de asignarlas.
  Personal consulta sus propios turnos en curso y completados, incluido inicio,
  ubicación y uniforme.
- **Requisito del PDF (pp. 3–4):** se contemplan perfiles, documentación,
  contratos, saldos e historial. Administración registra cobros, pagos, bonos y
  multas.
- **Requisito del PDF (pp. 3–5):** gastos, contabilidad y estadísticas forman
  parte del producto; el envío de datos para altas está por concretar. La
  prioridad indicada es Administración, después Cliente y Personal, y por último
  las secciones restantes.

## Decisión técnica de Fase 2 (aprobada, no implementada)

Work aprobó un monolito modular con Django 5.2 LTS y Python 3.13, PostgreSQL 17,
templates, HTMX y Tailwind CSS compilado. Las versiones de parche exactas deberán
validarse al preparar el entorno. No se han instalado dependencias ni creado
estructuras.

La decisión contempla sesiones en servidor; permisos por rol y recurso
aplicados en backend; documentos privados; importes decimales; tiempos con zona
horaria; y un historial de revisiones para las correcciones. El modelo de dominio
distinguiría explícitamente **solicitud**, **plaza**, **asignación**,
**declaración**, **confirmación** y **movimiento**. El detalle operativo revisable se documenta en [`dominio.md`](dominio.md); sus
decisiones pendientes no quedan aprobadas por describirse allí. El usuario aclaró
que, para el MVP, un Cliente corresponde a un restaurante o local; separar
Cliente y cuenta, conservar la ubicación histórica y dejar varios locales como
futuro no resuelve aún cuántas cuentas o contactos acceden ni la compatibilidad
de roles.

## Pendientes funcionales principales

No se resuelven en la Fase 3A:

- agrupación de solicitudes;
- número de cuentas y contactos por Cliente, y compatibilidad de roles;
- bloqueo o edición después de una asignación parcial;
- precisión monetaria, redondeos y descansos;
- vigencia temporal de tarifas;
- cancelaciones y sustituciones;
- impuestos;
- pagos parciales y anticipos;
- efecto contable y sobre saldos de bonos y multas;
- firma de contratos;
- datos requeridos y momento del proceso de altas;
- plazos y reglas de conservación documental.

## Flujo y siguiente control

Work dirige la arquitectura y la auditoría, Codex ejecuta únicamente la tarea
acotada y el usuario traslada las evidencias a Work. Las **Fases 1 y 2 están
aprobadas**. La **Fase 3A está preparada y pendiente de auditoría** y la Fase 3
completa permanece abierta. Se aplica el STOP: no aprobar esta entrega, no
continuar con el modelo de tablas, no instalar el stack y no implementar
funcionalidad.
