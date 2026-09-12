# Instrucciones para agentes

Estas instrucciones se aplican a todo el repositorio. Antes de trabajar, lee las
instrucciones superiores aplicables, este archivo, `README.md`,
`docs/estado-proyecto.md`, `docs/arquitectura.md` y
`docs/decisiones/0001-arquitectura-y-stack.md`, e inspecciona el estado de Git
sin destruir ni mezclar trabajo previo.

## Fase vigente y alcance

- La fase 1 está aprobada por Work.
- La fase 2 es exclusivamente documental y queda pendiente de auditoría de Work.
- El alcance autorizado se limita a modificar `README.md`, `AGENTS.md` y
  `docs/estado-proyecto.md`, y a crear `docs/arquitectura.md` y
  `docs/decisiones/0001-arquitectura-y-stack.md`.
- **STOP:** al completar y aportar las evidencias de esta fase, detente. No
  autorices ni inicies fases futuras, ni instales o implementes el stack.

## Trazabilidad

Cada afirmación relevante debe clasificarse con claridad:

- **Requisito del PDF:** regla funcional respaldada por la fuente, conservando
  su referencia de página. Si se pospone, conserva su procedencia y su
  obligatoriedad dentro del producto completo, y se indica cuándo está previsto
  abordarlo.
- **Requisito del encargo:** condición impuesta para la tarea o su entrega.
- **Decisión técnica:** propuesta o elección aprobada, indicando siempre su
  estado; una propuesta no equivale a una aprobación.
- **Mejora:** optimización opcional que no altera silenciosamente el negocio.
- **Futuro:** funcionalidad nueva, no exigida por el PDF y fuera del MVP.

No inventes reglas para cubrir ambigüedades. Registra cada ambigüedad o decisión
pendiente como pendiente de resolución, sin convertirla en funcionalidad futura
ni en decisión aprobada, y solicita su validación en la fase correspondiente.

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
