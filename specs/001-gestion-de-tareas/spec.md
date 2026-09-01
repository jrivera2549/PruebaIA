# Feature Specification: Gestión de tareas

**Feature Branch**: `001-gestion-de-tareas`

**Created**: 2026-08-31

**Status**: Draft

**Input**: User description: "Construir una aplicación web de gestión de tareas con creación, edición, eliminación, filtros, cambio rápido de estado, contador y persistencia entre sesiones."

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Administrar tareas (Priority: P1)

Como usuario, quiero crear, consultar, editar y eliminar mis tareas para mantener una lista fiable de trabajo pendiente.

**Why this priority**: El CRUD es el valor central de la aplicación y permite gestionar el ciclo de vida completo de una tarea.

**Independent Test**: Se puede crear una tarea, comprobar que aparece, modificar sus datos y eliminarla con confirmación, sin depender de otros flujos.

**Acceptance Scenarios**:

1. **Given** que no hay tareas o que la lista está visible, **When** el usuario registra un título obligatorio, una descripción opcional, una prioridad y un estado, **Then** la nueva tarea aparece en la lista con prioridad Media y estado Pendiente cuando no se seleccionaron otros valores.
2. **Given** una tarea existente, **When** el usuario edita su título, descripción, prioridad o estado y guarda, **Then** la lista muestra los valores actualizados sin perder los demás datos.
3. **Given** una tarea existente, **When** el usuario solicita eliminarla, **Then** se muestra un diálogo nativo de confirmación y la tarea solo desaparece si confirma.
4. **Given** una tarea existente, **When** el usuario cancela la confirmación de eliminación, **Then** la tarea permanece sin cambios.

---

### User Story 2 - Organizar y completar tareas (Priority: P2)

Como usuario, quiero encontrar tareas por prioridad o estado y marcar una tarea como completada con un clic para concentrarme en lo que importa.

**Why this priority**: La clasificación y el cambio rápido de estado reducen el esfuerzo para revisar una lista creciente.

**Independent Test**: Con varias tareas de distintas prioridades y estados, se puede aplicar cada filtro, comprobar los resultados y cambiar el estado de una tarea con un solo clic.

**Acceptance Scenarios**:

1. **Given** varias tareas con prioridades distintas, **When** el usuario selecciona Mostrar todas, Alta, Media o Baja, **Then** se muestran inmediatamente todas las tareas o solo las de la prioridad elegida.
2. **Given** varias tareas con estados distintos, **When** el usuario selecciona Mostrar todas, Pendientes, En Progreso o Completadas, **Then** se muestran inmediatamente todas las tareas o solo las del estado elegido.
3. **Given** una tarea no completada, **When** el usuario activa su control de completado, **Then** el estado cambia a Completada y el contador se actualiza sin recargar la página.
4. **Given** filtros de prioridad y estado activos, **When** el usuario cambia uno de ellos, **Then** la lista refleja inmediatamente la combinación actual de ambos filtros.

---

### User Story 3 - Consultar el estado y conservar la información (Priority: P3)

Como usuario, quiero ver cuántas tareas están pendientes y completadas y conservar mis tareas entre sesiones para confiar en la aplicación.

**Why this priority**: El contador da una visión rápida del avance y la persistencia evita repetir el trabajo después de cerrar la página.

**Independent Test**: Se crean tareas en distintos estados, se comprueba el contador, se cierra o recarga la aplicación y se verifica que las tareas y sus estados siguen disponibles.

**Acceptance Scenarios**:

1. **Given** una lista con tareas pendientes y completadas, **When** el usuario la consulta, **Then** el contador muestra por separado las cantidades de tareas pendientes y completadas.
2. **Given** una tarea guardada, **When** el usuario recarga la aplicación o vuelve a abrirla en el mismo navegador, **Then** la tarea conserva título, descripción, prioridad, estado y fecha de creación.
3. **Given** que no existen tareas guardadas, **When** el usuario abre la aplicación, **Then** se muestra un estado vacío claro y el contador indica cero tareas pendientes y cero completadas.

---

### Edge Cases

- Si el usuario intenta guardar una tarea sin título o con un título compuesto solo por espacios, la aplicación MUST impedir el guardado y mostrar un mensaje asociado al campo.
- Si la descripción está vacía, la tarea MUST poder guardarse sin texto descriptivo.
- Si una tarea se elimina mientras hay filtros activos, la lista y el contador MUST actualizarse sin mostrar resultados obsoletos.
- Si un filtro no encuentra coincidencias, la aplicación MUST mostrar un estado vacío que distinga entre "no hay tareas" y "no hay coincidencias".
- Si los datos guardados no pueden leerse, la aplicación MUST iniciar una lista vacía sin bloquear la interfaz y MUST informar al usuario de que los datos previos no están disponibles.
- Si el usuario cambia el estado a Completada y luego lo revierte mediante edición, el contador MUST reflejar el estado actual.
- En pantallas pequeñas, ningún control ni texto de tarea MUST quedar cortado, superpuesto o inutilizable.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: La aplicación MUST mostrar las tareas ordenadas por fecha de creación, de la más reciente a la más antigua.
- **FR-002**: La aplicación MUST permitir crear una tarea con título obligatorio, descripción opcional, prioridad Alta/Media/Baja y estado Pendiente/En Progreso/Completada.
- **FR-003**: La aplicación MUST asignar Media como prioridad predeterminada y Pendiente como estado predeterminado cuando el usuario no indique otros valores.
- **FR-004**: La aplicación MUST rechazar títulos ausentes o compuestos únicamente por espacios e indicar cómo corregirlos.
- **FR-005**: La aplicación MUST permitir editar el título, la descripción, la prioridad y el estado de una tarea existente mediante edición inline o un modal simple.
- **FR-006**: La aplicación MUST solicitar confirmación mediante un diálogo nativo antes de eliminar una tarea.
- **FR-007**: La aplicación MUST eliminar una tarea solo después de que el usuario confirme la acción.
- **FR-008**: La aplicación MUST ofrecer filtros de prioridad con las opciones Mostrar todas, Solo Alta, Solo Media y Solo Baja.
- **FR-009**: La aplicación MUST ofrecer filtros de estado con las opciones Mostrar todas, Solo Pendientes, Solo En Progreso y Solo Completadas.
- **FR-010**: La aplicación MUST aplicar los filtros instantáneamente, sin recargar la página, y combinar correctamente el filtro de prioridad con el de estado.
- **FR-011**: La aplicación MUST permitir cambiar una tarea a Completada con un solo clic y actualizar inmediatamente su representación y el contador.
- **FR-012**: La aplicación MUST mostrar un contador separado de tareas pendientes y tareas completadas, actualizado después de cada alta, edición, cambio de estado o eliminación.
- **FR-013**: La aplicación MUST conservar las tareas y sus atributos entre sesiones mediante almacenamiento local del navegador.
- **FR-014**: La aplicación MUST recuperar de forma segura los datos guardados y mostrar un estado vacío utilizable si no hay datos válidos disponibles.
- **FR-015**: La interfaz MUST ser responsiva con enfoque mobile-first y mantener todas las funciones en móvil y escritorio.
- **FR-016**: Los controles MUST ser navegables mediante teclado, tener etiquetas o nombres accesibles, mostrar estados comprensibles y mantener contraste suficiente.
- **FR-017**: El código MUST incluir comentarios en español cuando expliquen decisiones, reglas de negocio o comportamientos no evidentes.
- **FR-018**: La solución MUST funcionar sin librerías, frameworks, CDNs, fuentes remotas ni servicios externos.
- **FR-019**: El 100% de las funciones CRUD críticas MUST contar con pruebas manuales documentadas que incluyan precondiciones, pasos, datos, resultado esperado y resultado observado.

### Key Entities *(include if feature involves data)*

- **Tarea**: Unidad de trabajo del usuario; contiene identificador, título, descripción, prioridad, estado y fecha de creación.
- **Prioridad**: Clasificación de una tarea con uno de los valores Alta, Media o Baja.
- **Estado**: Situación de una tarea con uno de los valores Pendiente, En Progreso o Completada.
- **Filtro**: Criterio seleccionado para limitar la lista por prioridad, por estado o por ambos.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Un usuario puede crear una tarea válida y verla en la lista en menos de 30 segundos, sin recargar la página.
- **SC-002**: El 100% de las operaciones CRUD críticas ejecutadas durante la validación manual tiene un caso documentado con resultado esperado y observado.
- **SC-003**: El 100% de los cambios de filtros se refleja en la lista en menos de 1 segundo y sin navegación ni recarga visible.
- **SC-004**: El 100% de las tareas creadas y guardadas correctamente conserva sus datos después de recargar o reabrir la aplicación en el mismo navegador.
- **SC-005**: En una revisión manual con teclado, el 100% de los controles de creación, edición, filtrado, completado y eliminación puede enfocarse y utilizarse sin ratón.
- **SC-006**: En pruebas con una pantalla móvil y una de escritorio, el 100% de los flujos principales permanece visible, legible y operativo sin desplazamiento horizontal.
- **SC-007**: En una prueba con al menos 50 tareas, el usuario puede identificar las tareas de una prioridad y un estado concretos en menos de 10 segundos.
- **SC-008**: Al menos 90% de los usuarios de prueba puede completar el flujo de crear, completar y eliminar una tarea en el primer intento, sin ayuda.

## Assumptions

- La aplicación está destinada a un único usuario local; no incluye cuentas, autenticación, permisos ni sincronización entre dispositivos.
- El almacenamiento local del navegador es suficiente para el volumen esperado de uso y no se requiere un servidor.
- La fecha de creación se asigna automáticamente al registrar una tarea y no se edita desde la interfaz.
- La edición puede resolverse con un formulario inline o con un modal simple; se elegirá la opción que mantenga menor complejidad y mejor accesibilidad.
- El diálogo nativo de confirmación es el mecanismo aceptado para evitar eliminaciones accidentales.
- Las pruebas manuales documentadas se conservarán como evidencia del proyecto y se actualizarán cuando cambien los flujos CRUD.
- El alcance no incluye búsqueda por texto, etiquetas, ordenamiento configurable, arrastrar y soltar, exportación, importación ni notificaciones.
- Los navegadores objetivo son navegadores modernos con soporte estándar para almacenamiento local, HTML, CSS y JavaScript.
