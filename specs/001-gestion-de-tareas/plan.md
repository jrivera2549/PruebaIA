# Implementation Plan: Gestión de tareas

**Branch**: `001-gestion-de-tareas` | **Date**: 2026-09-01 | **Spec**: [spec.md](spec.md)

**Input**: Feature specification from `/specs/001-gestion-de-tareas/spec.md`

## Summary

Construir una aplicación web de una sola página para gestionar tareas personales en el navegador. Los usuarios pueden crear, leer, actualizar y eliminar tareas; filtrar por prioridad y estado; marcar completadas con un clic; ver contadores separados de tareas pendientes y completadas; y conservar sus tareas entre sesiones mediante almacenamiento local. La interfaz será responsiva (mobile-first), accesible mediante teclado, sin dependencias externas ni servidores, y todo el código incluirá comentarios en español.

## Technical Context

**Language/Version**: JavaScript ES6+, HTML5, CSS3 (Flexbox y Grid)

**Primary Dependencies**: Ninguna — HTML, CSS y JavaScript nativos del navegador

**Storage**: `localStorage` API del navegador (almacenamiento local)

**Testing**: Pruebas manuales documentadas para operaciones CRUD (sin automatización, según constitución)

**Target Platform**: Navegadores modernos con soporte para ES6, `localStorage` y CSS Grid/Flexbox

**Project Type**: Aplicación web de una sola página (SPA)

**Performance Goals**: 
- Render de lista completa en < 1 segundo
- Filtros se aplican instantáneamente (< 100 ms)
- Cambio de estado de tarea con clic único, feedback inmediato

**Constraints**: 
- Cero dependencias externas
- Todos los recursos (HTML, CSS, JS) locales en el repositorio
- Código con comentarios en español
- Cumplimiento de accesibilidad (navegación con teclado, etiquetas ARIA, contraste)

**Scale/Scope**: 
- Usuario local único
- Volumen esperado de tareas: centenas (no miles)
- Plataformas: móvil y escritorio, navegadores estándar

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

✓ **Principio I (Código limpio y mantenible)**: Toda decisión arquitectónica y regla de negocio será comentada en español.

✓ **Principio II (Interfaz accesible y responsiva)**: Diseño mobile-first, navegación por teclado, etiquetas y estados accesibles, contraste WCAG AA.

✓ **Principio III (Simplicidad tecnológica)**: Solo HTML5, CSS3 y JavaScript vanilla. Prohibidas librerías, frameworks, CDNs y dependencias externas.

✓ **Principio IV (Persistencia local)**: Datos persistidos en `localStorage` con formato JSON documentado y recuperación resiliente ante corrupción.

✓ **Principio V (Pruebas manuales CRUD)**: El 100% de operaciones CRUD tendrá casos de prueba documentados (precondiciones, pasos, esperado/observado).

✓ **Principio VI (Autocontención)**: Aplicación ejecutable abriendo `index.html` en navegador, sin servidores ni recursos remotos.

---

*No hay conflictos con la constitución vigente. Todas las restricciones se incorporan en el diseño.*

## Project Structure

### Documentation (this feature)

```text
specs/001-gestion-de-tareas/
├── spec.md                    (Especificación aclarada)
├── plan.md                    (Este documento)
├── tasks.md                   (Tareas de implementación)
├── checklists/
│   └── requirements.md        (Validación de calidad)
└── test-cases/
    └── crud-manual-tests.md   (Pruebas manuales CRUD)
```

### Application Structure

```text
index.html                      (Estructura HTML)
styles.css                      (Estilos CSS, mobile-first)
app.js                          (Lógica de aplicación)
```

**Data Model** (en memoria + localStorage):

```javascript
// Estructura de una tarea
{
  id: "YYYYMMDD-HHMMSS-<counter>",  // Identificador único y creciente
  title: "Título obligatorio",      // Obligatorio
  description: "",                  // Opcional
  priority: "Media",                // Alta, Media, Baja (defecto: Media)
  status: "Pendiente",              // Pendiente, En Progreso, Completada (defecto: Pendiente)
  createdAt: Date.now()             // Marca de tiempo, sin edición
}
```

**Persistencia**:

- Clave `localStorage`: `taskManagerTasks`
- Formato: Array JSON de tareas
- Recuperación: Lectura con validación ante corrupción; lista vacía si fallos

## Technical Approach

### Phase 0: Research & Initialization (< 1 día)

**Deliverables**:
- Template HTML básico con semántica correcta
- CSS reset y variables de color/espaciado (mobile-first)
- Archivo JS vacío con comentarios de estructura
- Validación de accesibilidad manual en navegador

**Scope**:
- No código de lógica aplicativa
- Solo preparación de andamiaje
- Checklist de accesibilidad inicial (elementos, teclado)

**Success Criteria**:
- `index.html` abre en navegador sin errores
- CSS carga y reset aplicado
- Inspección de accesibilidad pasa revisión básica

---

### Phase 1: Core CRUD & Data Model (1 día)

**Deliverables**:
- Funciones de creación, lectura, actualización y eliminación de tareas
- Modelo de datos en memoria (Array de objetos)
- Renderizado dinámico de lista de tareas
- Persistencia en `localStorage`
- Modal de edición (estructura + show/hide lógico)
- Pruebas manuales CRUD documentadas y ejecutadas

**Key Functions**:
- `initializeApp()`: Carga datos desde `localStorage`, renderiza UI
- `createTask(title, description, priority, status)`: Valida título, asigna ID/fecha/defaults, persiste
- `renderTasks(tasks)`: Renderiza dinámicamente la lista
- `editTask(id, updates)`: Abre modal, actualiza datos, guarda
- `deleteTask(id)`: Pide confirmación, elimina, persiste
- `updateLocalStorage()`: Guarda array de tareas en JSON

**Acceptance Criteria**:
- Usuario puede crear tarea válida en < 30 segundos
- Usuario puede ver tarea creada en la lista
- Usuario puede editar tarea mediante modal
- Usuario puede eliminar tarea con confirmación
- Datos persisten tras recargar página
- Pruebas CRUD documentadas con pasos y resultados

---

### Phase 2: Filtros, Contadores & Refinamiento (1 día)

**Deliverables**:
- Filtro por prioridad (Mostrar todas, Alta, Media, Baja)
- Filtro por estado (Mostrar todas, Pendientes, En Progreso, Completadas)
- Combinación AND de filtros aplicada instantáneamente
- Contador separado: tareas pendientes vs completadas
- Cambio rápido de estado (toggle Completada) con clic único
- Validaciones de accesibilidad finales
- Pruebas manuales de filtros y contadores

**Key Functions**:
- `applyFilters(priorityFilter, statusFilter)`: Filtra con lógica AND
- `updateCounter()`: Calcula y muestra contadores, actualizado instantáneamente
- `toggleTaskStatus(id)`: Cambia estado a Completada/no Completada
- `handleFilterChange(type, value)`: Aplica filtro y re-renderiza
- `sortTasks(tasks)`: Ordena por fecha descendente; desempate por ID descendente

**Acceptance Criteria**:
- Cada cambio de filtro se refleja en < 1 segundo
- Contador muestra cifras actuales tras cada operación
- Cambio de estado visible inmediatamente
- Toggle de estado accesible con teclado
- Accesibilidad completa: navegación con teclado, etiquetas, contraste
- Responsive en móvil y escritorio sin scroll horizontal
- Pruebas manuales de filtros y contador documentadas

---

## Task Decomposition

(See `tasks.md` for detailed task cards and dependencies)

**Phase 0 Tasks**:
1. Crear `index.html` con estructura semántica
2. Crear `styles.css` con reset, variables y mobile-first base
3. Crear `app.js` con comentarios de estructura
4. Validar accesibilidad manual

**Phase 1 Tasks**:
1. Implementar modelo de datos (estructura de tarea, Array en memoria)
2. Implementar `createTask` y validación de título
3. Implementar `renderTasks` con dinámico HTML
4. Implementar `editTask` modal y lógica
5. Implementar `deleteTask` con confirmación
6. Implementar persistencia `localStorage`
7. Documentar y ejecutar pruebas CRUD (manuales)

**Phase 2 Tasks**:
1. Implementar filtro por prioridad
2. Implementar filtro por estado
3. Implementar combinación AND de filtros
4. Implementar contador dinámico
5. Implementar toggle de estado
6. Implementar ordenamiento (fecha DESC, ID DESC desempate)
7. Validar accesibilidad final (teclado, ARIA, contraste)
8. Documentar y ejecutar pruebas de filtros
9. Responsive testing (móvil y escritorio)

## Validation & Success Gates

**Phase 0 Exit Gate**:
- [ ] `index.html` válido y accesible
- [ ] CSS mobile-first base aplicado
- [ ] `app.js` estructura lista
- [ ] Inspección de accesibilidad básica pasa

**Phase 1 Exit Gate**:
- [ ] CRUD funcional (crear, leer, editar, eliminar)
- [ ] Persistencia en `localStorage` verificada
- [ ] Modal de edición accesible
- [ ] Pruebas CRUD documentadas y ejecutadas (100% cobertura)
- [ ] Sin errores de consola

**Phase 2 Exit Gate**:
- [ ] Filtros aplicados instantáneamente
- [ ] Contador actualizado inmediatamente
- [ ] Toggle de estado funcional
- [ ] Accesibilidad completa (teclado, ARIA, contraste WCAG AA)
- [ ] Responsive en móvil (< 768px) y escritorio (> 1024px)
- [ ] Pruebas de filtros documentadas
- [ ] Pruebas responsive documentadas
- [ ] 100% de requisitos CRUD con pruebas manuales

---

## Risk Mitigation

| Riesgo | Probabilidad | Impacto | Mitigación |
|--------|--------------|--------|-----------|
| Corrupción de datos en `localStorage` | Media | Alto | Parseo con try/catch, validación de estructura JSON, recuperación a lista vacía |
| Problemas de accesibilidad no detectados en pruebas manuales | Media | Alto | Uso de herramienta de validación automática (axe, WAVE) en navegador; prueba con lector de pantalla |
| Rendimiento degradado con 100+ tareas | Baja | Medio | Optimizar renderizado: virtual scroll o paginación si necesario (Fase 3) |
| Incompatibilidad de navegador (IE11, etc.) | Baja | Bajo | Target: navegadores modernos (Chrome, Firefox, Safari, Edge últimas 2 versiones) |
| Modal no accesible (navegación trapeada) | Medio | Alto | Validar trap de focus, orden de tabulación, cierre con ESC |

---

## Testing Strategy

**Manual CRUD Tests** (Fase 1):
- Crear tarea válida → verificar en lista
- Crear tarea sin título → verificar error
- Editar título, descripción, prioridad, estado → verificar cambios
- Eliminar tarea → verificar confirmación y desaparición
- Recargar página → verificar persistencia

**Manual Filter Tests** (Fase 2):
- Aplicar filtro de prioridad → verificar lista filtrada
- Aplicar filtro de estado → verificar lista filtrada
- Combinar ambos filtros → verificar lógica AND
- Limpiar filtros → verificar todas las tareas regresan

**Manual Counter Tests** (Fase 2):
- Crear tareas con diferentes estados → verificar contador
- Cambiar estado de tarea → verificar actualización inmediata
- Completar tarea → verificar desplazamiento entre pendientes y completadas
- Eliminar tarea → verificar contador actualizado

**Accessibility Tests**:
- Navegación completa con teclado (Tab, Enter, ESC)
- Nombres accesibles en botones y campos
- Contraste de color (WCAG AA mínimo)
- Estructura de árbol de accesibilidad válida

---

## Deliverables Summary

| Entregable | Fase | Responsable | Criterio de Aceptación |
|------------|------|-------------|------------------------|
| `index.html` | 0 | Desarrollo | Estructura semántica, sin errores |
| `styles.css` | 0 | Diseño | Mobile-first, variables de color, reset |
| `app.js` (andamiaje) | 0 | Desarrollo | Comentarios de estructura, funciones vacías |
| CRUD funcional | 1 | Desarrollo | Crear, leer, editar, eliminar tareas |
| `localStorage` persistencia | 1 | Desarrollo | Guardar/cargar sin corrupción |
| Modal de edición | 1 | Diseño + Desarrollo | Accesible, show/hide correcto |
| Pruebas CRUD | 1 | QA/Verificación | 100% cobertura, documentadas |
| Filtros (prioridad + estado) | 2 | Desarrollo | AND lógico, < 1 segundo |
| Contador dinámico | 2 | Desarrollo | Actualización inmediata, precisión |
| Toggle de estado | 2 | Desarrollo | Clic único, feedback visual |
| Accesibilidad final | 2 | QA/Verificación | Teclado, ARIA, contraste |
| Responsive design | 2 | Diseño | Móvil y escritorio sin scroll horizontal |
| Pruebas manuales integrales | 2 | QA/Verificación | Filtros, contador, responsividad |

---

## Sign-Off

This plan aligns with [Spec Kit Constitution](../../.specify/memory/constitution.md) principles and the cleared [Feature Specification](spec.md). All phases are scoped to deliver measurable, testable outcomes without external dependencies.

**Plan Status**: Ready for task decomposition (`/speckit-tasks`)

**Date**: 2026-09-01
