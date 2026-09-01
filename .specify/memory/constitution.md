<!--
Sync Impact Report
- Version change: uninitialized scaffold -> 1.0.0
- Modified principles: none; all six principles are newly defined.
- Added sections: Additional Constraints; Development Workflow, Review Process,
  Quality Gates.
- Removed sections: none.
- Follow-up TODOs: confirm the project's original ratification date.
-->

# Spec Kit Constitution

## Core Principles

### I. Código limpio y mantenible
El código MUST ser claro, modular y fácil de mantener. Los comentarios que expliquen
decisiones, reglas de negocio o comportamiento no evidente MUST estar escritos en español.
Los comentarios MUST mantenerse sincronizados con el código y no sustituir nombres expresivos
ni una estructura comprensible. Esto reduce el coste de mantenimiento y hace el proyecto
accesible para su equipo objetivo.

### II. Interfaz accesible y responsiva
La interfaz MUST diseñarse con enfoque mobile-first y MUST adaptarse a teléfonos, tabletas y
escritorios sin pérdida de contenido ni funcionalidad. Todos los flujos interactivos MUST ser
utilizables mediante teclado, exponer nombres y estados accesibles, mantener contraste legible
y asociar correctamente etiquetas con controles. Esta regla garantiza una experiencia usable
para más personas y en más dispositivos.

### III. Simplicidad tecnológica
El proyecto MUST implementarse únicamente con HTML, CSS y JavaScript vanilla, salvo una
enmienda explícita de esta constitución. El diseño MUST preferir la solución más simple que
cubra el requisito, evitando abstracciones, herramientas o capas innecesarias. Esta restricción
mantiene bajo el coste operativo y facilita que el proyecto sea comprendido y ejecutado.

### IV. Persistencia local
Los datos de la aplicación MUST persistir en `localStorage` para simular una base de datos.
Las operaciones de lectura, creación, actualización y eliminación MUST usar un formato de datos
documentado y manejar de forma determinista la ausencia, corrupción o incompatibilidad de datos.
La persistencia local mantiene el proyecto autocontenido y permite conservar el estado entre
sesiones sin incorporar un servidor.

### V. Pruebas manuales de funciones críticas
El 100% de las funciones críticas de creación, lectura, actualización y eliminación (CRUD)
MUST contar con pruebas manuales documentadas antes de considerarse terminadas. Cada prueba
MUST indicar precondiciones, pasos, datos usados y resultado esperado y observado; los fallos
MUST registrarse y resolverse antes de cerrar el trabajo. Esta evidencia protege los flujos
principales en un proyecto sin dependencia de un arnés de pruebas externo.

### VI. Autocontención y cero dependencias externas
El proyecto MUST funcionar sin paquetes, frameworks, CDNs, fuentes remotas ni servicios externos
en tiempo de ejecución. Todos los recursos necesarios MUST residir en el repositorio y la
aplicación MUST poder abrirse y utilizarse con las capacidades estándar del navegador. Esto
reduce puntos de fallo, mejora la reproducibilidad y preserva la autonomía del proyecto.

## Restricciones adicionales

La solución MUST conservar una separación clara entre estructura HTML, presentación CSS y
comportamiento JavaScript. Las decisiones que introduzcan una excepción tecnológica MUST
documentarse y aprobarse mediante una enmienda a esta constitución.

## Flujo de desarrollo, revisión y puertas de calidad

Cada cambio MUST verificarse en los tamaños de pantalla soportados, revisarse con teclado y
validarse contra los criterios de accesibilidad aplicables. Antes de cerrar un cambio que afecte
al CRUD, MUST actualizarse y ejecutarse su conjunto de pruebas manuales documentadas, incluida
la persistencia tras recargar la página. La revisión MUST comprobar también que no se añadieron
dependencias externas ni datos fuera de `localStorage`.

## Governance
<!-- Example: Constitution supersedes all other practices; Amendments require documentation, approval, migration plan -->

Esta constitución prevalece sobre prácticas contradictorias del proyecto. Toda enmienda MUST
proponer el texto afectado, explicar su motivación e impacto, registrar la fecha y actualizar
el informe de impacto. Los cambios MUST revisarse antes de aplicarse y los cambios incompatibles
MUST incluir un plan de migración o una justificación explícita de por qué no aplica.

La versión usa Semantic Versioning: MAJOR para eliminar o redefinir principios de forma
incompatible, MINOR para añadir principios o ampliar materialmente la gobernanza, y PATCH para
aclaraciones sin cambio semántico. Cada revisión del proyecto MUST comprobar el cumplimiento de
los principios y de las puertas de calidad; cualquier incumplimiento MUST quedar registrado con
una acción correctiva o una excepción aprobada.

**Version**: 1.0.0 | **Ratified**: TODO(RATIFICATION_DATE) | **Last Amended**: 2026-08-27
