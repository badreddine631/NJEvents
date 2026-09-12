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
- **Fase 2:** propuesta documental de arquitectura y stack preparada y pendiente
  de auditoría de Work. El repositorio sigue sin implementación, dependencias ni
  base de datos.

La propuesta se desarrolla en [`arquitectura.md`](arquitectura.md) y la decisión
se razona, sin estar aprobada, en el
[`ADR 0001`](decisiones/0001-arquitectura-y-stack.md). Este estado no duplica
esos documentos ni autoriza una fase posterior.

## Base funcional disponible

Lo siguiente es un **resumen facilitado y verificado por Work**; no consta una
lectura directa del PDF durante esta fase:

- **Requisito del PDF (pp. 1–2):** existen los roles Administración, Personal y
  Cliente; hay precios de venta por cliente y función, retribuciones individuales
  por trabajador y excepciones por servicio.
- **Requisito del PDF (pp. 2–3):** Administración gestiona perfiles, solicitudes,
  asignaciones y calendarios semanales por cliente y trabajador. «En curso»
  significa asignado sin cierre confirmado.
- **Requisito del PDF (pp. 3–4):** Cliente y Personal declaran el fin; si
  coinciden, queda confirmado. Administración puede fijar directamente el valor
  definitivo y, después, solo Administración puede corregirlo.
- **Requisito del PDF (p. 4):** Cliente solicita cantidad, tipo, fecha y hora de
  inicio, y puede editar o eliminar solicitudes pendientes antes de asignarlas.
  Personal consulta inicio, ubicación y uniforme.
- **Requisito del PDF (pp. 3–4):** se contemplan perfiles, documentación,
  contratos, saldos e historial. Administración registra cobros, pagos, bonos y
  multas.
- **Requisito del PDF (pp. 3–5):** gastos, contabilidad y estadísticas forman
  parte del producto; el envío de datos para altas está por concretar. La
  prioridad indicada es Administración, después Cliente y Personal, y por último
  las secciones restantes.

## Propuesta técnica de Fase 2 (no aprobada ni implementada)

Se propone un monolito modular con Django 5.2 LTS y Python 3.13, PostgreSQL 17,
templates, HTMX y Tailwind CSS compilado. Las versiones de parche exactas deberán
validarse al preparar el entorno. No se han instalado dependencias ni creado
estructuras.

La propuesta contempla sesiones en servidor; permisos por rol y recurso
aplicados en backend; documentos privados; importes decimales; tiempos con zona
horaria; y un historial de revisiones para las correcciones. El modelo de dominio
distinguiría explícitamente **solicitud**, **plaza**, **asignación**,
**declaración**, **confirmación** y **movimiento**. Todo ello sigue siendo una
propuesta pendiente de revisión, no una decisión técnica aprobada.

## Pendientes funcionales principales

No se resuelven en esta fase:

- agrupación de solicitudes y definición de contactos y locales;
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
acotada y el usuario traslada las evidencias a Work. La **Fase 1 está aprobada**
y la **Fase 2 queda pendiente de auditoría de Work**. Hasta recibir esa revisión
se aplica el STOP: no comenzar la Fase 3, no instalar el stack y no implementar
funcionalidad.
