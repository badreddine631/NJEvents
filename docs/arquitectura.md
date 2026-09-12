# Arquitectura general propuesta

> **Decisión técnica propuesta:** este documento pertenece a la Fase 2 y está
> pendiente de auditoría de Work. Describe una dirección, no una implementación
> ni una aprobación. El repositorio continúa sin stack instalado.

## Forma del sistema y stack

Se propone un **monolito modular**: una única aplicación Django desplegable, con
frontend y backend bajo el mismo origen. El stack recomendado por Work es Python
3.13, Django 5.2 LTS, PostgreSQL 17, templates de Django, HTMX y Tailwind CSS
compilado. Los parches exactos y la compatibilidad efectiva de todas las
dependencias se verificarán al preparar el entorno; esta fase no instala nada.

La interfaz será responsive. Administración se optimizará principalmente para
ordenador por la densidad de sus tareas; Cliente y Personal, principalmente para
móvil. Esto no excluye que los tres perfiles funcionen en ambos tamaños.

## Módulos y responsabilidad

| Módulo | Responsabilidad y datos de los que es autoridad |
| --- | --- |
| `accounts` | Identidad, usuario propio mínimo, autenticación y pertenencia a roles; no confunde el rol Personal con `is_staff`. |
| `profiles` | Datos de negocio de clientes y personal, contactos, locales y disponibilidad cuando se definan. |
| `operations` | Solicitudes, plazas, asignaciones, declaraciones de fin, confirmaciones y sus transiciones. |
| `pricing` | Tarifas de venta, retribuciones, excepciones y copia de la tarifa aplicada a cada operación. |
| `finance` | Movimientos, cobros, pagos, gastos, bonos, multas y saldos derivados, sin decidir aún sus reglas abiertas. |
| `documents` | Metadatos, clasificación, vínculo y permisos de documentos almacenados privadamente; contratos sin resolver su firma. |
| `audit` | Registro inmutable o trazable de acciones relevantes, especialmente correcciones y autoría. |
| `reporting` | Consultas, calendarios, estadísticas y exportaciones; lee de los módulos autoridad y no reimplementa reglas. |

Las dependencias siguen la propiedad anterior: `operations` consulta identidades
y perfiles, y solicita cálculos a `pricing`; los hechos confirmados pueden
originar movimientos mediante operaciones explícitas de `finance`; `documents`
vincula sus metadatos a entidades ajenas sin apropiarse de ellas; `audit`
registra acciones de todos los módulos; `reporting` compone lecturas. Las vistas,
templates, admin e informes coordinan o presentan casos de uso, pero nunca
duplican validaciones, autorización ni reglas de negocio.

## Estructura futura del repositorio

Esta tabla es orientativa para una fase de implementación. **No representa
carpetas creadas en esta fase.**

| Ruta futura | Finalidad |
| --- | --- |
| `config/` | Configuración Django, URLs y puntos de entrada del proyecto. |
| `apps/accounts/` | Módulo de cuentas. |
| `apps/profiles/` | Módulo de perfiles. |
| `apps/operations/` | Núcleo operativo. |
| `apps/pricing/` | Tarifas y cálculo de importes. |
| `apps/finance/` | Contabilidad y movimientos. |
| `apps/documents/` | Gestión segura de documentos. |
| `apps/audit/` | Trazabilidad transversal. |
| `apps/reporting/` | Consultas e informes. |
| `templates/` | Templates compartidos y fragmentos HTMX. |
| `assets/` | Fuentes de estilos Tailwind y otros recursos compilables. |
| `static/` | Salida estática preparada para servir en despliegue. |
| `tests/` | Pruebas transversales y recorridos críticos. |
| `docs/` | Documentación y ADR. |

## Recorrido de una operación

1. Una petición HTTP, formulario normal o interacción HTMX entra por una vista.
2. El formulario valida forma, tipos y campos; no decide por sí solo el negocio.
3. El backend autentica y autoriza por rol, propietario del recurso, acción y
   estado actual.
4. Una operación de negocio explícita y reutilizable comprueba invariantes,
   calcula el resultado y solicita la auditoría necesaria. El admin automático
   debe invocar el mismo camino y no puede eludir cierres ni movimientos.
5. Los cambios relacionados se persisten en una transacción PostgreSQL. Las
   acciones con efectos económicos incorporarán protección frente a reintentos
   para impedir duplicados.
6. La vista devuelve HTML completo o un fragmento HTMX; la presentación no se
   convierte en fuente de verdad.

## Decisiones arquitectónicas propuestas

- **Datos:** PostgreSQL se usará desde el primer entorno funcional y en pruebas
  de integración. Los importes serán decimales; las fechas, completas y con zona
  horaria; cada resultado conservará las tarifas efectivamente aplicadas.
- **Identidad:** se definirá un usuario propio mínimo antes de la primera
  migración. Django gestionará contraseñas y sesiones en servidor.
- **Autorización:** el backend combinará rol, propiedad del recurso, acción y
  estado. Los grupos no bastan para proteger recursos propios y el rol Personal
  es distinto del indicador técnico `is_staff`.
- **Negocio e integridad:** los casos críticos serán operaciones explícitas,
  reutilizables por cualquier interfaz. Correcciones y autoría se auditarán; las
  escrituras relacionadas serán transaccionales y las de efecto económico se
  protegerán ante reintentos.
- **Documentos:** el contenido residirá en almacenamiento privado; metadatos y
  permisos quedarán en la base de datos. Toda descarga se autorizará en backend
  con mínimo acceso.
- **Pruebas y controles posteriores:** se proponen pytest y pytest-django,
  PostgreSQL para integración, Playwright para recorridos críticos y, en fases
  posteriores, Ruff y CI. Se priorizarán permisos, transiciones, dinero, tiempo,
  concurrencia y reintentos.
- **Despliegue:** se propone una aplicación en contenedor, PostgreSQL y
  almacenamiento privado, con entornos separados, secretos externos y copias de
  seguridad cuya restauración sea comprobable. Los proveedores se elegirán solo
  cuando haya una necesidad concreta.

El contexto y el contraste de alternativas constan en el
[ADR 0001](decisiones/0001-arquitectura-y-stack.md).

## Decisiones pendientes

El **modelo lógico detallado**, cardinalidades, restricciones y matriz de estados
corresponden a la Fase 3. En esa fase deberán aclararse agrupación de solicitudes,
contactos y locales, edición tras asignación parcial, vigencia de tarifas,
cancelaciones y sustituciones. Antes de implementar cálculos se resolverán
precisión y redondeo, descansos, impuestos, pagos parciales y anticipos, y efectos
de bonos y multas. Antes del flujo contractual o de altas se decidirán firma y
datos requeridos. Antes de producción se fijarán conservación documental,
proveedores, servicios auxiliares, copias, observabilidad y versiones exactas.

Nada de lo anterior queda resuelto por esta propuesta.
