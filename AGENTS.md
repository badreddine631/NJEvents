# Instrucciones para agentes

Estas instrucciones se aplican a todo el repositorio. Antes de trabajar, lee las
instrucciones superiores aplicables, este archivo, `README.md` y
`docs/estado-proyecto.md`, e inspecciona el estado de Git sin destruir ni mezclar
trabajo previo.

## Fase vigente y alcance

- La fase 1 es exclusivamente documental y queda pendiente de auditoría de Work.
- El alcance autorizado se limita a `README.md`, `AGENTS.md`, `.gitignore`,
  `.editorconfig` y `docs/estado-proyecto.md`.
- **STOP:** al completar y aportar las evidencias de esta fase, detente. No
  autorices ni inicies fases futuras, ni instales o implementes el stack.

## Trazabilidad

Cada afirmación relevante debe clasificarse con claridad:

- **Requisito del PDF:** regla funcional respaldada por la fuente, conservando
  su referencia de página.
- **Requisito del encargo:** condición impuesta para la tarea o su entrega.
- **Decisión técnica:** elección aprobada expresamente; no presentar una
  propuesta como si ya lo fuera.
- **Mejora:** optimización opcional que no altera silenciosamente el negocio.
- **Futuro:** trabajo fuera de la fase vigente o asunto aún no resuelto.

No inventes reglas para cubrir ambigüedades. Registra los pendientes y solicita
su validación en la fase correspondiente.

## Seguridad y calidad

- El backend será responsable de hacer cumplir permisos por rol y por recurso,
  así como todas las reglas de negocio; la interfaz nunca será la única barrera.
- Nunca incluyas secretos, credenciales ni datos personales reales en código,
  documentación, ejemplos, pruebas o historial Git.
- Mantén privados los documentos y aplica el mínimo acceso necesario cuando se
  implemente su almacenamiento.
- Cuando exista código, añade y ejecuta pruebas para permisos, transiciones de
  estado, cálculos monetarios y temporales, y demás reglas críticas.
- Entrega estado inicial y final, rama y SHA, archivos afectados, diff revisado,
  comandos con sus resultados, comprobaciones omitidas y limitaciones. Confirma
  que el cambio respeta el alcance antes de detenerte.
