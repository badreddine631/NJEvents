# ADR 0001: arquitectura y stack

- **Estado:** PROPUESTO, pendiente de auditoría de Work
- **Fecha:** 2026-09-12
- **Alcance:** Fase 2 documental; sin implementación

## Problema

NJ Events necesita servir tres experiencias responsive —Administración centrada
en ordenador, Cliente y Personal centradas en móvil— y mantener una única fuente
de verdad para solicitudes, asignaciones, cierres, precios, movimientos,
documentos y auditoría. El backend debe hacer cumplir permisos por rol, recurso,
acción y estado, proteger documentos y preservar la integridad de operaciones
económicas. El equipo necesita una base sencilla de desplegar y evolucionar sin
separar prematuramente interfaces y reglas.

## Opciones consideradas

| Opción | Ventajas | Costes y riesgos |
| --- | --- | --- |
| Django integrado (templates, HTMX y PostgreSQL) | Autenticación, sesiones, formularios, ORM, transacciones y admin en un solo sistema; HTML progresivo y un despliegue. | Acopla la interfaz web al monolito; exige impedir que el admin salte casos de uso; la interactividad muy rica puede requerir otro enfoque. |
| Next.js con PostgreSQL | Buen ecosistema para interfaces React y renderizado en servidor. | Hay que elegir y ensamblar acceso a datos, autenticación, autorización, auditoría y disciplina transaccional; aumenta la superficie de decisiones para este dominio. |
| Frontend separado con API | Clientes desacoplados y contrato reutilizable para múltiples consumidores. | Dos aplicaciones, contrato y despliegues; duplica coordinación de autenticación, errores y validación y resulta prematuro sin una necesidad confirmada de API pública o múltiples clientes. |

## Decisión propuesta

Adoptar un **monolito modular Django con PostgreSQL**, Python 3.13, Django 5.2
LTS, PostgreSQL 17, templates Django, HTMX y Tailwind CSS compilado. Frontend y
backend se servirán bajo el mismo origen. Los límites y responsabilidades están
en la [arquitectura general](../arquitectura.md) y no se repiten aquí.

Las fuentes oficiales identificadas para verificar esta propuesta son las notas de
[Django 5.2](https://docs.djangoproject.com/en/5.2/releases/5.2/) (LTS y soporte
de Python 3.13), la documentación de
[bases de datos de Django 5.2](https://docs.djangoproject.com/en/5.2/ref/databases/)
(compatibilidad PostgreSQL), la política de
[versionado de PostgreSQL](https://www.postgresql.org/support/versioning/), la
[documentación de HTMX](https://htmx.org/docs/) y la instalación mediante CLI de
[Tailwind CSS](https://tailwindcss.com/docs/installation/tailwind-cli). La
consulta en vivo no pudo completarse en este entorno por bloqueo de red; por
ello, los parches exactos y la compatibilidad conjunta se verificarán de nuevo
al preparar el entorno, sin instalar paquetes durante esta fase.

## Razones y consecuencias

La opción concentra reglas, autorización y transacciones en un backend y reduce
la complejidad operativa inicial. Django ofrece piezas coherentes para sesiones,
contraseñas, formularios, administración y persistencia; PostgreSQL permite usar
el mismo motor desde la primera integración hasta producción. Templates y HTMX
cubren interacciones progresivas sin crear ahora una API separada, mientras
Tailwind compilado permite una interfaz responsive sin depender de un CDN en
producción.

El coste es aceptar un despliegue y un ciclo de cambios compartidos, mantener
fronteras modulares mediante disciplina y probar que ningún acceso —incluido el
admin— evita las operaciones del dominio. HTMX no sustituye una arquitectura de
estado cliente si la interfaz llegara a exigirla. El equipo también asumirá la
operación de PostgreSQL, almacenamiento privado, compilación de CSS y copias
restaurables.

## Cuándo reconsiderar

La decisión se revisará si aparecen consumidores externos con un contrato API
estable, varias aplicaciones cliente independientes, equipos con despliegues
realmente autónomos, requisitos de escalado incompatibles por módulo o una
experiencia cliente cuyo estado e interacción no puedan sostenerse razonablemente
con HTML progresivo. Esas circunstancias justificarían evaluar una API o un
frontend separado; no implican que sean necesarios hoy.

## Decisiones aplazadas

- Proveedores de alojamiento, almacenamiento, correo, observabilidad y demás
  servicios auxiliares, hasta concretar sus necesidades.
- Proveedor y mecanismo de firma contractual, hasta definir validez y flujo.
- Reglas económicas pendientes: precisión y redondeo, descansos, impuestos,
  cancelaciones, anticipos y efectos de bonos y multas.
- Datos y momento del proceso de altas.
- Versiones de parche exactas de Python, Django, PostgreSQL, HTMX, Tailwind y
  dependencias auxiliares, que se comprobarán al preparar el entorno.
