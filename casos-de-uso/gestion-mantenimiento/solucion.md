# Solución Sistema de Gestión de Solicitudes de Mantenimiento de Infraestructuras

## 1) Requisitos funcionales

**RF1. Registrar solicitud de mantenimiento**
El empleado debe poder crear una solicitud indicando: tipo de incidencia, ubicación, descripción detallada y nivel de prioridad.

**RF2. Consultar y dar seguimiento a solicitudes**
Los usuarios autorizados deben poder consultar solicitudes y ver su estado, fechas y responsable/técnico asignado.

**RF3. Clasificar solicitud**
El responsable de mantenimiento debe poder clasificar la solicitud (categoría/subtipo, criticidad, validación de datos, etc.).

**RF4. Asignar técnico**
El responsable debe poder asignar uno o más técnicos responsables de una solicitud.

**RF5. Planificar intervención**
El responsable debe poder planificar fecha/ventana de intervención y registrar observaciones de planificación.

**RF6. Actualizar estado de solicitud/tarea**
El técnico debe poder cambiar el estado (p. ej., *Abierta, En curso, En espera, Resuelta, Cerrada*) y registrar avances.

**RF7. Registrar tiempo empleado**
El técnico debe registrar el tiempo dedicado (por intervención o por tramo).

**RF8. Registrar descripción de intervención**
El técnico debe documentar el trabajo realizado, materiales utilizados y resultado.

**RF9. Generar historial de mantenimiento por ubicación**
El sistema debe generar un historial (filtro por ubicación, periodo, tipo de incidencia, etc.).

**RF10. Notificar cambios de estado**
El sistema debe emitir notificaciones automáticas cuando cambie el estado de una solicitud.

**RF11. Notificar retrasos**
El sistema debe emitir notificaciones automáticas cuando una solicitud supere la fecha/ventana planificada o SLA definido.

## 2) Requisitos no funcionales

**RNF1. Seguridad y control de acceso por roles**
Autenticación obligatoria y autorización por rol (Empleado, Responsable, Técnico).

**RNF2. Trazabilidad y auditoría**
Registrar quién hizo qué y cuándo (cambios de estado, asignaciones, tiempos, ediciones).

**RNF3. Rendimiento**
Consultas de listado/búsqueda deben responder en tiempos razonables (p.ej., < 2 s en condiciones normales).

**RNF4. Disponibilidad**
Servicio disponible en horario operativo (idealmente 24/7) con tolerancia a fallos y recuperación.

**RNF5. Usabilidad**
Interfaz clara, formularios guiados, validaciones y mensajes de error comprensibles.

**RNF6. Integridad de datos**
Actualizaciones consistentes del estado y datos asociados (sin estados imposibles o transiciones inválidas).

## 3) Diagrama de casos de uso

![Diagrama de casos de uso](assets/uc-gestion-mantenimiento.png)

## 4) Casos de uso en formato expandido (tablas)

### UC-01 Registrar solicitud

| Elemento              | Descripción                                                                                                                                                                                                                           |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Requisito(s)**      | **RF1**                                                                                                                                                                                                                               |
| **Actor principal**   | Empleado                                                                                                                                                                                                                              |
| **Precondición**      | Empleado autenticado                                                                                                                                                                                                                  |
| **Curso típico**      | 1) El empleado selecciona “Nueva solicitud”. 2) Introduce tipo, ubicación, descripción, prioridad. 3) El sistema valida campos obligatorios. 4) El sistema registra la solicitud con estado “Abierta” y confirma el número de ticket. |
| **Curso alternativo** | Si faltan campos obligatorios o formato inválido, el sistema muestra error y no registra la solicitud.                                                                                                                                |

### UC-02 Consultar/seguir solicitud

| Elemento              | Descripción                                                                                                                                                                                   |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Requisito(s)**      | **RF2**                                                                                                                                                                                       |
| **Actor principal**   | Empleado / Responsable / Técnico                                                                                                                                                              |
| **Precondición**      | Usuario autenticado y con permisos                                                                                                                                                            |
| **Curso típico**      | 1) El usuario accede al listado/búsqueda. 2) Filtra por estado, ubicación, tipo, fechas. 3) Selecciona una solicitud. 4) El sistema muestra detalle, estado actual, histórico y asignaciones. |
| **Curso alternativo** | Si no hay resultados, el sistema indica “sin coincidencias” y sugiere ajustar filtros.                                                                                                        |

### UC-03 Clasificar solicitud

| Elemento              | Descripción                                                                                                                                                              |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Requisito(s)**      | **RF3**                                                                                                                                                                  |
| **Actor principal**   | Responsable de mantenimiento                                                                                                                                             |
| **Precondición**      | Solicitud en estado “Abierta” (o “En revisión”)                                                                                                                          |
| **Curso típico**      | 1) El responsable abre una solicitud. 2) Define clasificación (categoría/subtipo/criticidad) y valida datos. 3) El sistema guarda la clasificación y registra auditoría. |
| **Curso alternativo** | Si la solicitud carece de información suficiente, el responsable la marca “En espera de información” y el sistema lo refleja en el seguimiento.                          |

### UC-04 Asignar técnico

| Elemento              | Descripción                                                                                                                                                 |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Requisito(s)**      | **RF4**                                                                                                                                                     |
| **Actor principal**   | Responsable de mantenimiento                                                                                                                                |
| **Precondición**      | Solicitud clasificada                                                                                                                                       |
| **Curso típico**      | 1) El responsable selecciona “Asignar técnico”. 2) Elige técnico(s) disponible(s). 3) El sistema registra asignación y notifica a los técnicos (si aplica). |
| **Curso alternativo** | Si no hay técnicos disponibles, el sistema permite dejar la solicitud “Pendiente de asignación” y registrar motivo.                                         |

### UC-05 Planificar intervención

| Elemento              | Descripción                                                                                                                                    |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| **Requisito(s)**      | **RF5**                                                                                                                                        |
| **Actor principal**   | Responsable de mantenimiento                                                                                                                   |
| **Precondición**      | Solicitud asignada (o lista para planificar)                                                                                                   |
| **Curso típico**      | 1) El responsable define fecha/ventana y prioridad operativa. 2) Guarda la planificación. 3) El sistema actualiza el plan y deja trazabilidad. |
| **Curso alternativo** | Si hay conflicto de agenda o recursos, el sistema avisa y el responsable ajusta la planificación.                                              |

### UC-06 Actualizar estado

| Elemento              | Descripción                                                                                                                                                                                              |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Requisito(s)**      | **RF6, RF10**                                                                                                                                                                                            |
| **Actor principal**   | Técnico                                                                                                                                                                                                  |
| **Precondición**      | Técnico asignado a la solicitud                                                                                                                                                                          |
| **Curso típico**      | 1) El técnico abre la solicitud. 2) Cambia el estado (p.ej., “En curso”, “En espera”, “Resuelta”). 3) El sistema guarda el cambio y **incluye** el envío de notificación de cambio de estado (**RF10**). |
| **Curso alternativo** | Si el técnico intenta una transición inválida (p.ej., “Cerrada” sin “Resuelta”), el sistema rechaza y muestra mensaje.                                                                                   |

### UC-07 Registrar tiempo empleado

| Elemento              | Descripción                                                                                                                                   |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| **Requisito(s)**      | **RF7**                                                                                                                                       |
| **Actor principal**   | Técnico                                                                                                                                       |
| **Precondición**      | Solicitud asignada                                                                                                                            |
| **Curso típico**      | 1) El técnico selecciona “Registrar tiempo”. 2) Indica duración, fecha, y notas. 3) El sistema guarda el registro y lo asocia a la solicitud. |
| **Curso alternativo** | Si el tiempo es negativo o supera límites definidos, el sistema valida y solicita corrección.                                                 |

### UC-08 Registrar intervención realizada

| Elemento              | Descripción                                                                                                                                                       |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Requisito(s)**      | **RF8**                                                                                                                                                           |
| **Actor principal**   | Técnico                                                                                                                                                           |
| **Precondición**      | Solicitud en curso o lista para cierre                                                                                                                            |
| **Curso típico**      | 1) El técnico selecciona “Registrar intervención”. 2) Describe acciones, materiales y resultado. 3) El sistema guarda la intervención y la asocia a la solicitud. |
| **Curso alternativo** | Si falta información mínima requerida, el sistema no permite guardar y solicita completar campos.                                                                 |

### UC-09 Generar historial por ubicación

| Elemento              | Descripción                                                                                                                                                                                                                           |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Requisito(s)**      | **RF9**                                                                                                                                                                                                                               |
| **Actor principal**   | Responsable de mantenimiento                                                                                                                                                                                                          |
| **Precondición**      | Existen solicitudes registradas                                                                                                                                                                                                       |
| **Curso típico**      | 1) El responsable selecciona “Historial por ubicación”. 2) Define ubicación y rango de fechas. 3) El sistema genera el historial con solicitudes, estados, tiempos e intervenciones. 4) El responsable visualiza/exporta (si aplica). |
| **Curso alternativo** | Si no hay registros para la ubicación/periodo, el sistema muestra “sin datos” y permite cambiar filtros.                                                                                                                              |

### UC-10 Notificar retraso

| Elemento              | Descripción                                                                                                                                                             |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Requisito(s)**      | **RF11**                                                                                                                                                                |
| **Actor principal**   | Sistema (evento automático)                                                                                                                                             |
| **Precondición**      | Existe planificación o SLA para la solicitud                                                                                                                            |
| **Curso típico**      | 1) El sistema ejecuta **Detectar retraso** (<<include>>). 2) Si se confirma retraso, genera notificación a responsable y/o técnico. 3) Registra el evento en auditoría. |
| **Curso alternativo** | Si el retraso ya fue notificado recientemente, el sistema evita duplicados (según política) y solo registra el chequeo.                                                 |

## 5) Matriz de Trazabilidad

| Requisito | Caso(s) de uso               |
| --------- | ---------------------------- |
| RF1       | UC-01                        |
| RF2       | UC-02                        |
| RF3       | UC-03                        |
| RF4       | UC-04                        |
| RF5       | UC-05                        |
| RF6       | UC-06                        |
| RF7       | UC-07                        |
| RF8       | UC-08                        |
| RF9       | UC-09                        |
| RF10      | UC-06 (incluye notificación) |
| RF11      | UC-10                        |