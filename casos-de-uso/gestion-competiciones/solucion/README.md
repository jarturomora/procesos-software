# Solución del caso práctico: Plataforma de Gestión de Competiciones Deportivas Amateur

## 1. Requisitos funcionales

| ID   | Requisito funcional                         | Descripción                                                                                                                                                  |
| ---- | ------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| RF1  | Crear torneos                               | El sistema permitirá a los organizadores registrar nuevas competiciones, indicando nombre, disciplina, fechas y descripción.                                 |
| RF2  | Gestionar categorías                        | Los organizadores podrán crear y modificar categorías dentro de cada competición, según edad, nivel o modalidad.                                             |
| RF3  | Gestionar calendarios                       | Los organizadores podrán programar encuentros, establecer fechas, horarios y ubicaciones, así como modificar su planificación.                               |
| RF4  | Registrar equipos y participantes           | Los organizadores podrán dar de alta equipos o participantes y asociarlos a las competiciones correspondientes.                                              |
| RF5  | Inscribirse en competiciones                | Los deportistas podrán solicitar su inscripción en las competiciones y categorías disponibles.                                                               |
| RF6  | Consultar calendarios                       | Los deportistas podrán consultar los encuentros programados, incluyendo horarios y ubicaciones.                                                              |
| RF7  | Consultar resultados y clasificaciones      | Los usuarios podrán consultar los resultados de los encuentros y la clasificación actualizada de cada competición.                                           |
| RF8  | Registrar resultados                        | Los árbitros podrán introducir y validar los resultados de los encuentros que tengan asignados.                                                              |
| RF9  | Registrar incidencias                       | Los árbitros podrán documentar incidencias ocurridas durante los encuentros.                                                                                 |
| RF10 | Registrar sanciones                         | Los árbitros podrán registrar las sanciones impuestas a jugadores o equipos durante los encuentros.                                                          |
| RF11 | Actualizar clasificaciones                  | El sistema actualizará automáticamente las clasificaciones cuando se registre o corrija un resultado válido, de acuerdo con el reglamento de la competición. |
| RF12 | Generar estadísticas de rendimiento         | El sistema calculará estadísticas de equipos y participantes a partir de los resultados registrados.                                                         |
| RF13 | Enviar comunicaciones                       | Los organizadores podrán enviar avisos a los inscritos sobre cambios de horarios, ubicaciones o reglamentos.                                                 |
| RF14 | Publicar noticias                           | Los organizadores podrán publicar noticias relacionadas con las competiciones.                                                                               |
| RF15 | Consultar resultados históricos             | Los usuarios podrán consultar resultados de competiciones finalizadas.                                                                                       |
| RF16 | Supervisar competiciones y corregir errores | Los administradores podrán revisar la información registrada y corregir errores detectados.                                                                  |
| RF17 | Gestionar reclamaciones                     | Los administradores podrán consultar, revisar y resolver reclamaciones presentadas por los participantes.                                                    |
| RF18 | Validar inscripciones                       | El sistema verificará la existencia de plazas y el cumplimiento de los requisitos de la categoría antes de aceptar una inscripción.                          |

## 2. Requisitos no funcionales

| ID    | Requisito no funcional    | Descripción                                                                                                                                                       |
| ----- | ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RNF1  | Seguridad                 | El sistema deberá autenticar a los usuarios y limitar el acceso a las funciones según su rol: deportista, organizador, árbitro o administrador.                   |
| RNF2  | Protección de datos       | Las comunicaciones deberán utilizar HTTPS y el tratamiento de datos personales deberá ajustarse al RGPD y a la normativa española aplicable.                      |
| RNF3  | Rendimiento               | Las consultas de calendarios, resultados y clasificaciones deberán responder en menos de 3 segundos en el 95 % de las solicitudes bajo una carga normal definida. |
| RNF4  | Disponibilidad            | La plataforma deberá mantener una disponibilidad mensual mínima del 99,5 %, excluyendo ventanas de mantenimiento previamente anunciadas.                          |
| RNF5  | Usabilidad                | La interfaz deberá permitir realizar las operaciones principales de forma sencilla y ser adaptable a dispositivos móviles y ordenadores.                          |
| RNF6  | Accesibilidad             | La interfaz web deberá satisfacer los criterios de conformidad WCAG 2.2 de nivel AA.                                                                              |
| RNF7  | Integridad y consistencia | El sistema deberá impedir inscripciones duplicadas, resultados incompatibles y actualizaciones inconsistentes de las clasificaciones.                             |
| RNF8  | Trazabilidad              | El sistema deberá registrar el usuario, fecha y operación realizada en modificaciones de resultados, sanciones y decisiones administrativas.                      |
| RNF9  | Escalabilidad             | La arquitectura deberá permitir incorporar nuevas disciplinas, competiciones y participantes sin rediseñar las funcionalidades principales.                       |
| RNF10 | Recuperación ante fallos  | El sistema deberá realizar copias de seguridad periódicas y disponer de un procedimiento documentado de restauración de información.                              |

## 3. Actores del sistema

Identificamos cuatro actores principales:

- Deportista: se inscribe en competiciones, consulta información y presenta reclamaciones.
- Organizador: configura competiciones, categorías y calendarios; registra participantes, publica noticias y envía comunicaciones.
- Árbitro: registra resultados, incidencias y sanciones.
- Administrador: supervisa el funcionamiento de las competiciones, corrige errores y resuelve reclamaciones.

El sistema ejecuta también funciones automáticas, como actualizar las clasificaciones o calcular estadísticas. Estas funciones no requieren representar al propio sistema como actor externo.

## 4. Diagrama de casos de uso en PlantUML

El diagrama representa las funcionalidades principales e incorpora las relaciones `<<include>>` y `<<extend>>`, utilizando su dirección correcta según UML.

```



```

### 4.1. Justificación de las relaciones

Relaciones de inclusión (`include`):

- Inscribirse en competición incluye Validar inscripción. Siempre será necesario comprobar los requisitos y la disponibilidad antes de aceptar una inscripción.
- Registrar resultado incluye Actualizar clasificación. Cuando se confirma un resultado válido, se recalcula la clasificación.
- Registrar resultado incluye Generar estadísticas. Los datos del encuentro confirmado se incorporan al cálculo estadístico.

Relaciones de extensión (`extend`):

- Registrar incidencia extiende Registrar resultado cuando, durante la elaboración del acta, el árbitro necesita añadir información sobre un incidente. También puede registrar incidencias de manera independiente durante el encuentro.
- Registrar sanción extiende Registrar incidencia cuando una incidencia documentada da lugar a una sanción. El árbitro también puede acceder directamente al registro de sanciones si ya existe la incidencia correspondiente.

La distinción fundamental es que `include` representa comportamiento incorporado obligatoriamente al caso base, mientras que `extend` incorpora comportamiento bajo determinadas condiciones.

## 5. Casos de uso en formato expandido

Cada tabla incluye el requisito funcional asociado, los actores, las precondiciones, el curso típico de acciones y al menos un curso alternativo.

### CU-01. Crear torneo

| Elemento            | Descripción                                                                                                                                                                                                   |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Requisito funcional | RF1 – Crear torneos                                                                                                                                                                                           |
| Actor principal     | Organizador                                                                                                                                                                                                   |
| Precondiciones      | El organizador está autenticado y tiene permisos de gestión.                                                                                                                                                  |
| Curso típico        | 1. El organizador selecciona «Crear torneo». 2. Introduce nombre, disciplina, fechas y descripción. 3. El sistema valida los datos. 4. El organizador confirma la creación. 5. El sistema registra el torneo. |
| Curso alternativo   | Si faltan datos obligatorios o las fechas son inconsistentes, el sistema muestra un error y solicita corregirlos.                                                                                             |
| Postcondición       | El torneo queda registrado y disponible para su configuración.                                                                                                                                                |

### CU-02. Gestionar categorías

| Elemento            | Descripción                                                                                                                                                                                    |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Requisito funcional | RF2 – Gestionar categorías                                                                                                                                                                     |
| Actor principal     | Organizador                                                                                                                                                                                    |
| Precondiciones      | Existe un torneo registrado.                                                                                                                                                                   |
| Curso típico        | 1. El organizador selecciona un torneo. 2. Accede a las categorías. 3. Crea o modifica una categoría indicando modalidad, edad o nivel. 4. El sistema valida los datos. 5. Guarda los cambios. |
| Curso alternativo   | Si la categoría ya existe con los mismos criterios, el sistema informa de la duplicidad.                                                                                                       |
| Postcondición       | Las categorías quedan actualizadas.                                                                                                                                                            |

### CU-03. Gestionar calendario

| Elemento            | Descripción                                                                                                                                                                                                               |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Requisito funcional | RF3 – Gestionar calendarios                                                                                                                                                                                               |
| Actor principal     | Organizador                                                                                                                                                                                                               |
| Precondiciones      | El torneo y sus categorías están definidos.                                                                                                                                                                               |
| Curso típico        | 1. El organizador selecciona una competición. 2. Define encuentro, fecha, hora y ubicación. 3. El sistema verifica conflictos de calendario. 4. El organizador confirma. 5. El sistema publica el calendario actualizado. |
| Curso alternativo   | Si existe un conflicto de horario o instalación, el sistema informa y solicita otra fecha u ubicación.                                                                                                                    |
| Postcondición       | El calendario refleja la programación vigente.                                                                                                                                                                            |

### CU-04. Registrar equipos y participantes

| Elemento            | Descripción                                                                                                                                                                                        |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Requisito funcional | RF4 – Registrar equipos y participantes                                                                                                                                                            |
| Actor principal     | Organizador                                                                                                                                                                                        |
| Precondiciones      | Existe una competición y el organizador tiene permisos para gestionarla.                                                                                                                           |
| Curso típico        | 1. El organizador accede al registro. 2. Selecciona el tipo de participante: equipo o individual. 3. Introduce los datos requeridos. 4. El sistema verifica la información. 5. Guarda el registro. |
| Curso alternativo   | Si el equipo o participante ya está registrado, el sistema informa y evita duplicarlo.                                                                                                             |
| Postcondición       | El equipo o participante queda registrado en la plataforma, pendiente de inscripción si corresponde.                                                                                               |

### CU-05. Inscribirse en competición

| Elemento               | Descripción                                                                                                                                                                                                                                                                             |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Requisitos funcionales | RF5 – Inscribirse en competiciones; RF18 – Validar inscripciones                                                                                                                                                                                                                        |
| Actor principal        | Deportista                                                                                                                                                                                                                                                                              |
| Precondiciones         | El deportista está autenticado y existe una competición con inscripciones abiertas.                                                                                                                                                                                                     |
| Curso típico           | 1. El deportista consulta las competiciones. 2. Selecciona un torneo y una categoría. 3. Solicita la inscripción. 4. El sistema ejecuta «Validar inscripción» (`include`). 5. El sistema confirma que se cumplen las condiciones. 6. Registra la inscripción y muestra la confirmación. |
| Curso alternativo      | Si no hay plazas, el deportista no cumple los requisitos o ya está inscrito, el sistema rechaza la solicitud e informa del motivo.                                                                                                                                                      |
| Postcondición          | La inscripción queda registrada y ocupa una plaza disponible.                                                                                                                                                                                                                           |

### CU-06. Consultar calendario

| Elemento            | Descripción                                                                                                                                                                |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Requisito funcional | RF6 – Consultar calendarios                                                                                                                                                |
| Actor principal     | Deportista o árbitro                                                                                                                                                       |
| Precondiciones      | Existen competiciones con calendarios publicados.                                                                                                                          |
| Curso típico        | 1. El usuario accede al calendario. 2. Selecciona la competición. 3. El sistema muestra encuentros, fechas, horarios y ubicaciones. 4. El usuario consulta la información. |
| Curso alternativo   | Si todavía no se ha publicado el calendario, el sistema muestra un mensaje informativo.                                                                                    |
| Postcondición       | El usuario obtiene la planificación vigente.                                                                                                                               |

### CU-07. Consultar resultados y clasificaciones

| Elemento            | Descripción                                                                                                                                                                                                         |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Requisito funcional | RF7 – Consultar resultados y clasificaciones                                                                                                                                                                        |
| Actor principal     | Deportista o administrador                                                                                                                                                                                          |
| Precondiciones      | Existe una competición registrada.                                                                                                                                                                                  |
| Curso típico        | 1. El usuario selecciona la competición. 2. Accede a resultados o clasificaciones. 3. El sistema recupera la información disponible. 4. Muestra los resultados confirmados y la tabla de clasificación actualizada. |
| Curso alternativo   | Si todavía no existen resultados oficiales, el sistema informa de que están pendientes de publicación.                                                                                                              |
| Postcondición       | Se muestra la información deportiva disponible.                                                                                                                                                                     |

### CU-08. Registrar resultado

| Elemento               | Descripción                                                                                                                                                                                                                                                                                     |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Requisitos funcionales | RF8 – Registrar resultados; RF11 – Actualizar clasificaciones; RF12 – Generar estadísticas                                                                                                                                                                                                      |
| Actor principal        | Árbitro                                                                                                                                                                                                                                                                                         |
| Precondiciones         | El árbitro está asignado al encuentro y este admite el registro de resultados.                                                                                                                                                                                                                  |
| Curso típico           | 1. El árbitro selecciona el encuentro. 2. Introduce el resultado. 3. El sistema valida los datos conforme a la disciplina. 4. El árbitro confirma el acta. 5. El sistema guarda el resultado. 6. Ejecuta «Actualizar clasificación» (`include`). 7. Ejecuta «Generar estadísticas» (`include`). |
| Curso alternativo      | Si el resultado contiene datos incompatibles con las reglas deportivas, el sistema solicita corregirlos antes de guardar.                                                                                                                                                                       |
| Postcondición          | El resultado confirmado queda registrado y sus efectos se reflejan en la clasificación y las estadísticas.                                                                                                                                                                                      |

### CU-09. Registrar incidencia

| Elemento            | Descripción                                                                                                                                                                                          |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Requisito funcional | RF9 – Registrar incidencias                                                                                                                                                                          |
| Actor principal     | Árbitro                                                                                                                                                                                              |
| Precondiciones      | Existe un encuentro al que está asignado el árbitro.                                                                                                                                                 |
| Curso típico        | 1. El árbitro selecciona el encuentro. 2. Accede a «Registrar incidencia». 3. Introduce el tipo, la descripción y el momento del incidente. 4. El sistema valida los datos. 5. Guarda la incidencia. |
| Curso alternativo   | Si los datos están incompletos, el sistema solicita corregirlos. Si el incidente requiere sanción, se puede ejecutar «Registrar sanción» (`extend`).                                                 |
| Postcondición       | La incidencia queda asociada al encuentro.                                                                                                                                                           |

### CU-10. Registrar sanción

| Elemento            | Descripción                                                                                                                                                                                                      |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Requisito funcional | RF10 – Registrar sanciones                                                                                                                                                                                       |
| Actor principal     | Árbitro                                                                                                                                                                                                          |
| Precondiciones      | Existe un encuentro y una incidencia que puede dar lugar a sanción.                                                                                                                                              |
| Curso típico        | 1. El árbitro selecciona la incidencia. 2. Indica el jugador o equipo afectado. 3. Selecciona la sanción aplicable y su motivo. 4. Confirma el registro. 5. El sistema guarda la sanción vinculada al encuentro. |
| Curso alternativo   | Si la sanción no es válida según las reglas configuradas, el sistema la rechaza y solicita otra opción.                                                                                                          |
| Postcondición       | La sanción queda registrada y disponible para su supervisión.                                                                                                                                                    |

### CU-11. Actualizar clasificación

| Elemento            | Descripción                                                                                                                                                                                                          |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Requisito funcional | RF11 – Actualizar clasificaciones                                                                                                                                                                                    |
| Actor principal     | Árbitro (inicia indirectamente mediante el registro de resultados).                                                                                                                                                  |
| Precondiciones      | Se ha confirmado o corregido un resultado válido.                                                                                                                                                                    |
| Curso típico        | 1. El sistema recupera el resultado confirmado. 2. Consulta las reglas de puntuación de la competición. 3. Recalcula los puntos y posiciones de los equipos o participantes. 4. Guarda la clasificación actualizada. |
| Curso alternativo   | Si el recálculo no puede completarse, el sistema conserva la clasificación anterior, registra el error y deja la actualización pendiente.                                                                            |
| Postcondición       | La clasificación refleja los resultados confirmados.                                                                                                                                                                 |

### CU-12. Generar estadísticas de rendimiento

| Elemento            | Descripción                                                                                                                                                                                                                        |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Requisito funcional | RF12 – Generar estadísticas de rendimiento                                                                                                                                                                                         |
| Actor principal     | Árbitro (inicia indirectamente al confirmar resultados).                                                                                                                                                                           |
| Precondiciones      | Existe un resultado confirmado con datos suficientes.                                                                                                                                                                              |
| Curso típico        | 1. El sistema recupera los resultados de la competición. 2. Calcula estadísticas correspondientes a la disciplina deportiva. 3. Actualiza los indicadores de rendimiento de los participantes o equipos. 4. Guarda la información. |
| Curso alternativo   | Si faltan datos necesarios para calcular una estadística, el sistema mantiene el indicador como no disponible y registra la incidencia.                                                                                            |
| Postcondición       | Las estadísticas disponibles están actualizadas.                                                                                                                                                                                   |

### CU-13. Enviar comunicaciones

| Elemento            | Descripción                                                                                                                                                                                                                      |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Requisito funcional | RF13 – Enviar comunicaciones                                                                                                                                                                                                     |
| Actor principal     | Organizador                                                                                                                                                                                                                      |
| Precondiciones      | Existen participantes inscritos en la competición.                                                                                                                                                                               |
| Curso típico        | 1. El organizador selecciona una competición. 2. Elige los destinatarios. 3. Redacta una comunicación sobre horarios, ubicaciones o reglamentos. 4. Confirma el envío. 5. El sistema distribuye el mensaje y registra su estado. |
| Curso alternativo   | Si algún destinatario no puede recibir la comunicación, el sistema registra el fallo y permite reintentar el envío.                                                                                                              |
| Postcondición       | La comunicación queda registrada y enviada o pendiente de reintento.                                                                                                                                                             |

### CU-14. Publicar noticias

| Elemento            | Descripción                                                                                                                                                                                                       |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Requisito funcional | RF14 – Publicar noticias                                                                                                                                                                                          |
| Actor principal     | Organizador                                                                                                                                                                                                       |
| Precondiciones      | Existe una competición y el organizador dispone de permisos de publicación.                                                                                                                                       |
| Curso típico        | 1. El organizador accede a las noticias. 2. Selecciona «Nueva noticia». 3. Introduce título y contenido. 4. Selecciona la competición asociada. 5. Publica la noticia. 6. El sistema la muestra en la plataforma. |
| Curso alternativo   | Si faltan datos obligatorios, el sistema informa y solicita completarlos.                                                                                                                                         |
| Postcondición       | La noticia queda publicada y vinculada a una competición.                                                                                                                                                         |

### CU-15. Consultar resultados históricos

| Elemento            | Descripción                                                                                                                                                                   |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Requisito funcional | RF15 – Consultar resultados históricos                                                                                                                                        |
| Actor principal     | Deportista                                                                                                                                                                    |
| Precondiciones      | Existen competiciones finalizadas cuyos resultados se conservan.                                                                                                              |
| Curso típico        | 1. El deportista accede al histórico. 2. Filtra por disciplina, competición o temporada. 3. El sistema busca los registros. 4. Muestra los resultados históricos disponibles. |
| Curso alternativo   | Si no existen resultados para los criterios seleccionados, el sistema muestra un mensaje y permite modificar la búsqueda.                                                     |
| Postcondición       | El deportista consulta los resultados de competiciones anteriores.                                                                                                            |

### CU-16. Supervisar y corregir errores

| Elemento            | Descripción                                                                                                                                                                                                                                                                                                                               |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Requisito funcional | RF16 – Supervisar competiciones y corregir errores                                                                                                                                                                                                                                                                                        |
| Actor principal     | Administrador                                                                                                                                                                                                                                                                                                                             |
| Precondiciones      | El administrador está autenticado y dispone de permisos de supervisión.                                                                                                                                                                                                                                                                   |
| Curso típico        | 1. El administrador accede al panel de supervisión. 2. Selecciona una competición o registro. 3. Identifica información incorrecta. 4. Introduce una corrección justificada. 5. El sistema valida y guarda el cambio, registrando la operación. 6. Si afecta a resultados, actualiza las clasificaciones y estadísticas correspondientes. |
| Curso alternativo   | Si la modificación vulnera una regla de integridad, el sistema rechaza el cambio e informa del motivo.                                                                                                                                                                                                                                    |
| Postcondición       | La información corregida queda registrada y es trazable.                                                                                                                                                                                                                                                                                  |

### CU-17A. Presentar reclamación

| Elemento            | Descripción                                                                                                                                                                                                                                               |
| ------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Requisito funcional | RF17 – Gestionar reclamaciones                                                                                                                                                                                                                            |
| Actor principal     | Deportista                                                                                                                                                                                                                                                |
| Precondiciones      | El deportista está autenticado y está vinculado a la competición sobre la que reclama.                                                                                                                                                                    |
| Curso típico        | 1. El deportista accede a la competición. 2. Selecciona «Presentar reclamación». 3. Describe el motivo e identifica el encuentro o la decisión afectada. 4. Envía la reclamación. 5. El sistema registra la solicitud con estado «Pendiente de revisión». |
| Curso alternativo   | Si faltan datos obligatorios, el sistema solicita completarlos antes de registrar la reclamación.                                                                                                                                                         |
| Postcondición       | La reclamación queda registrada para su revisión administrativa.                                                                                                                                                                                          |

### CU-17B. Resolver reclamación

| Elemento            | Descripción                                                                                                                                                                                                                                                               |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Requisito funcional | RF17 – Gestionar reclamaciones                                                                                                                                                                                                                                            |
| Actor principal     | Administrador                                                                                                                                                                                                                                                             |
| Precondiciones      | Existe una reclamación pendiente.                                                                                                                                                                                                                                         |
| Curso típico        | 1. El administrador accede a las reclamaciones. 2. Selecciona una pendiente. 3. Revisa los datos, resultados e incidencias relacionados. 4. Registra una decisión motivada. 5. El sistema actualiza el estado de la reclamación. 6. Comunica la resolución al deportista. |
| Curso alternativo   | Si necesita información adicional, el administrador deja la reclamación pendiente y solicita la documentación necesaria.                                                                                                                                                  |
| Postcondición       | La reclamación queda resuelta o pendiente de información adicional, con su correspondiente trazabilidad.                                                                                                                                                                  |

### CU-18. Validar inscripción

| Elemento            | Descripción                                                                                                                                                                                                                                                                                                                      |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Requisito funcional | RF18 – Validar inscripciones                                                                                                                                                                                                                                                                                                     |
| Actor principal     | Deportista (inicia indirectamente mediante «Inscribirse en competición»).                                                                                                                                                                                                                                                        |
| Precondiciones      | Se ha solicitado una inscripción en una competición concreta.                                                                                                                                                                                                                                                                    |
| Curso típico        | 1. El sistema recupera los datos del participante y la categoría. 2. Comprueba que el plazo de inscripción está abierto. 3. Verifica que se cumplen los requisitos de participación. 4. Comprueba la disponibilidad de plazas y la ausencia de duplicados. 5. Devuelve un resultado favorable para continuar con la inscripción. |
| Curso alternativo   | Si no se cumple alguna condición, el sistema devuelve el motivo de rechazo y no autoriza la inscripción.                                                                                                                                                                                                                         |
| Postcondición       | La solicitud queda validada o rechazada.                                                                                                                                                                                                                                                                                         |

## 6. Matriz de trazabilidad: requisitos funcionales y casos de uso

Esta matriz permite comprobar que cada requisito funcional tiene asociado al menos un caso de uso y que existe una correspondencia directa con su descripción expandida.

| Requisito                                    | Caso de uso asociado | Cobertura |
| -------------------------------------------- | -------------------- | --------- |
| RF1 – Crear torneos                          | CU-01                | Completa  |
| RF2 – Gestionar categorías                   | CU-02                | Completa  |
| RF3 – Gestionar calendarios                  | CU-03                | Completa  |
| RF4 – Registrar equipos y participantes      | CU-04                | Completa  |
| RF5 – Inscribirse en competiciones           | CU-05                | Completa  |
| RF6 – Consultar calendarios                  | CU-06                | Completa  |
| RF7 – Consultar resultados y clasificaciones | CU-07                | Completa  |
| RF8 – Registrar resultados                   | CU-08                | Completa  |
| RF9 – Registrar incidencias                  | CU-09                | Completa  |
| RF10 – Registrar sanciones                   | CU-10                | Completa  |
| RF11 – Actualizar clasificaciones            | CU-08, CU-11, CU-16  | Completa  |
| RF12 – Generar estadísticas                  | CU-08, CU-12, CU-16  | Completa  |
| RF13 – Enviar comunicaciones                 | CU-13                | Completa  |
| RF14 – Publicar noticias                     | CU-14                | Completa  |
| RF15 – Consultar resultados históricos       | CU-15                | Completa  |
| RF16 – Supervisar y corregir errores         | CU-16                | Completa  |
| RF17 – Gestionar reclamaciones               | CU-17A, CU-17B       | Completa  |
| RF18 – Validar inscripciones                 | CU-05, CU-18         | Completa  |

# 18/18

Requisitos funcionales cubiertos

# 19

Casos de uso documentados

# 5

Relaciones include y extend

## 7. Observaciones didácticas

Para utilizar esta solución como referencia en la corrección de ejercicios, destacaría tres aspectos.

Primero, la trazabilidad. Los alumnos deben comprender que no necesariamente existe una relación uno a uno entre requisitos funcionales y casos de uso. Por ejemplo, «Registrar resultado» satisface directamente RF8, pero también desencadena las funcionalidades de RF11 y RF12.

Segundo, el uso correcto de `include` y `extend`. No debemos utilizar estas relaciones simplemente para conectar funcionalidades relacionadas. `include` incorpora comportamiento obligatorio, mientras que `extend` añade comportamiento condicional en un punto de extensión. En este ejemplo, la actualización de clasificaciones es obligatoria tras confirmar un resultado; registrar una incidencia durante la elaboración del acta, en cambio, es una posibilidad.

Tercero, la coherencia entre los diagramas y los cursos de acciones. Toda relación representada en el diagrama debe poder comprenderse a partir de los flujos definidos. Las acciones automáticas deben quedar documentadas, aunque no exista un actor humano que las ejecute directamente.

Esta solución constituye una referencia completa para el nivel introductorio. Los alumnos pueden proponer otros casos de uso o agrupar funcionalidades de manera diferente y obtener igualmente la máxima calificación, siempre que su análisis sea coherente, los requisitos estén cubiertos y el modelado UML sea correcto.