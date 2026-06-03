# Solución - Plataforma de Viviendas Turísticas

## Requisitos funcionales

| Código   | Requisito |
|----------|-----------|
| **RF-1** | Los propietarios pueden registrar alojamientos con fotos, descripción, capacidad, servicios y precios por temporada |
| **RF-2** | Los propietarios pueden definir disponibilidad y bloquear fechas específicas |
| **RF-3** | Los clientes pueden buscar alojamientos usando filtros (ubicación, capacidad, precio, servicios) |
| **RF-4** | Los clientes pueden realizar reservas y seleccionar fechas disponibles |
| **RF-5** | El sistema evita reservas duplicadas para las mismas fechas y calcula automáticamente el precio total |
| **RF-6** | El sistema procesa pagos en línea y genera confirmación de reserva automática para ambas partes |
| **RF-7** | Los huéspedes pueden valorar y comentar el alojamiento una vez finalizada la reserva |
| **RF-8** | Los administradores pueden supervisar actividad, bloquear anuncios y gestionar incidencias |

## Requisitos no funcionales

| Código    | Requisito |
|-----------|-----------|
| **RNF-1** | La plataforma debe ser **responsiva** y funcionar en móviles, tablets y escritorio |
| **RNF-2** | Los tiempos de respuesta en búsquedas no deben superar **2 segundos** |
| **RNF-3** | Disponibilidad del sistema **99.5%** durante el año con recuperación automática ante fallos |
| **RNF-4** | Todas las comunicaciones deben ser **cifradas (HTTPS)** y cumplir **RGPD** |
| **RNF-5** | Los datos de pago deben cumplir con **PCI DSS** y estar protegidos con cifrado |
| **RNF-6** | El sistema debe soportar **1,000 usuarios concurrentes** sin degradación de rendimiento |
| **RNF-7** | Las contraseñas se almacenarán con **hash criptográfico** y se implementará **2FA** opcional |

## Diagrama de casos de usos

![Casos de uso](image.png)

## Formato expandido de los casos de uso

### UC-1: Registrar Alojamiento

| Campo | Contenido |
|-------|-----------|
| **Nombre** | Registrar Alojamiento |
| **Actor Primario** | Propietario |
| **Requisitos Satisfechos** | **RF-1:** Los propietarios pueden registrar alojamientos con fotos, descripción, capacidad, servicios y precios por temporada |
| **Descripción** | El propietario registra un nuevo alojamiento en la plataforma proporcionando información básica, fotos y características del inmueble. |
| **Precondiciones** | • El propietario debe estar registrado y autenticado en el sistema<br>• El propietario debe haber completado su perfil con información personal |
| **Postcondiciones** | • El alojamiento se crea en estado "Borrador"<br>• El propietario puede ver el alojamiento en su dashboard<br>• El propietario puede continuar editando el alojamiento |
| **Flujo Principal** | 1. Propietario accede a su panel de control<br>2. Hace clic en "Nuevo Alojamiento"<br>3. Completa formulario con: nombre, dirección, tipo, capacidad, descripción<br>4. Sube mínimo 5 fotografías (máximo 50)<br>5. Selecciona servicios disponibles<br>6. Establece precio base por noche<br>7. Hace clic en "Guardar como Borrador"<br>8. Sistema confirma el registro exitoso |
| **Flujos Alternativos** | **2a. Carga desde plantilla:**<br>- Selecciona "Cargar desde plantilla"<br>- Sistema muestra plantillas prediseñadas<br>- Completa datos restantes<br><br>**5a. Sube menos fotos:**<br>- Sistema muestra advertencia<br>- Puede continuar pero con menor visibilidad<br>- Se recomienda agregar más fotos |

---

### UC-2: Gestionar Disponibilidad

| Campo | Contenido |
|-------|-----------|
| **Nombre** | Gestionar Disponibilidad |
| **Actor Primario** | Propietario |
| **Requisitos Satisfechos** | **RF-2:** Los propietarios pueden definir disponibilidad y bloquear fechas específicas |
| **Descripción** | El propietario define las fechas disponibles para reservas, configura temporadas con precios diferenciados y bloquea fechas específicas (mantenimiento, uso personal). |
| **Precondiciones** | • El propietario debe tener al menos un alojamiento registrado<br>• El alojamiento debe estar publicado o en estado editable<br>• El propietario debe estar autenticado |
| **Postcondiciones** | • Las fechas se actualizan en el calendario del sistema<br>• Los precios por temporada quedan configurados<br>• Las fechas bloqueadas no aparecen en búsquedas de clientes |
| **Flujo Principal** | 1. Propietario selecciona un alojamiento<br>2. Accede a pestaña "Disponibilidad"<br>3. Ve calendario de 12 meses<br>4. Define temporadas (Baja, Media, Alta) con multiplicador de precio<br>5. Bloquea fechas específicas indicando motivo<br>6. Establece número mínimo de noches<br>7. Hace clic en "Guardar cambios"<br>8. Sistema confirma actualización |
| **Flujos Alternativos** | **4a. Disponibilidad rápida:**<br>- Carga archivo CSV con fechas y precios<br>- Sistema valida formato<br>- Importa automáticamente<br><br>**5a. Patrón recurrente:**<br>- Selecciona "Bloqueo recurrente"<br>- Define: cada X domingo, del X al Y de cada mes<br>- Sistema aplica automáticamente |

---

### UC-3: Buscar Alojamientos

| Campo | Contenido |
|-------|-----------|
| **Nombre** | Buscar Alojamientos |
| **Actor Primario** | Cliente |
| **Requisitos Satisfechos** | **RF-3:** Los clientes pueden buscar alojamientos usando filtros (ubicación, capacidad, precio, servicios) |
| **Descripción** | El cliente busca alojamientos usando múltiples filtros (ubicación, fechas, capacidad, precio, servicios) y visualiza resultados en lista o mapa. |
| **Precondiciones** | • El cliente debe estar registrado (búsqueda sin autenticarse es posible)<br>• Debe haber al menos 5 alojamientos publicados en el sistema |
| **Postcondiciones** | • Se muestran resultados según criterios de búsqueda<br>• Cliente puede ver detalles de cada alojamiento<br>• Cliente puede proceder a crear una reserva |
| **Flujo Principal** | 1. Cliente accede a página de búsqueda<br>2. Ingresa ubicación (ciudad, región o dirección)<br>3. Selecciona fechas de entrada y salida<br>4. Especifica número de huéspedes<br>5. Establece rango de precio (opcional)<br>6. Selecciona servicios deseados (opcional)<br>7. Hace clic en "Buscar"<br>8. Sistema valida disponibilidad para esas fechas<br>9. Muestra resultados ordenados por relevancia, precio o calificación<br>10. Resultado muestra: foto, nombre, precio/noche, calificación, ubicación |
| **Flujos Alternativos** | **2a. Búsqueda avanzada:**<br>- Accede a "Búsqueda Avanzada"<br>- Filtra por tipo de vivienda, capacidad exacta, amenidades específicas<br>- Usa mapa para delimitación geográfica<br><br>**9a. Ordena resultados:**<br>- Hace clic en selector de orden<br>- Elige: Menor precio, Mayor precio, Mejor calificación, Más cercano |

---

### UC-4: Crear Reserva

| Campo | Contenido |
|-------|-----------|
| **Nombre** | Crear Reserva |
| **Actor Primario** | Cliente |
| **Requisitos Satisfechos** | **RF-4:** Los clientes pueden realizar reservas y seleccionar fechas disponibles<br>**RF-5:** El sistema evita reservas duplicadas para las mismas fechas y calcula automáticamente el precio total |
| **Descripción** | El cliente selecciona un alojamiento y crea una reserva especificando fechas de entrada/salida y cantidad de huéspedes, generando una reserva en estado pendiente de pago. |
| **Precondiciones** | • Cliente debe estar autenticado<br>• Alojamiento debe estar disponible para las fechas seleccionadas<br>• Cliente debe haber visto detalles del alojamiento<br>• La capacidad debe permitir el número de huéspedes |
| **Postcondiciones** | • Se crea reserva en estado "Pendiente de pago"<br>• Sistema genera número de confirmación temporal<br>• Cliente procede a procesar pago<br>• Propietario recibe notificación de nueva reserva |
| **Flujo Principal** | 1. Cliente visualiza detalles del alojamiento<br>2. Selecciona o confirma fechas (entrada/salida)<br>3. Confirma número de huéspedes<br>4. Sistema valida disponibilidad en tiempo real<br>5. Sistema calcula precio total (base × noches × multiplicador temporada + impuestos - descuentos)<br>6. Muestra desglose de costos<br>7. Cliente completa información: nombre, teléfono, mensaje al propietario (opcional)<br>8. Acepta términos y condiciones<br>9. Hace clic en "Confirmar y pagar"<br>10. Sistema crea reserva en estado "Pendiente de pago" |
| **Flujos Alternativos** | **5a. Aplica código promocional:**<br>- Ingresa código promocional<br>- Sistema valida código (vigencia, usos)<br>- Recalcula precio con descuento<br><br>**7a. Huésped recurrente:**<br>- Sistema ofrece descuento de lealtad (5-10%)<br>- Cliente acepta o rechaza<br><br>**7b. Chat previo con propietario:**<br>- Cliente inicia chat antes de confirmar<br>- Hace preguntas específicas<br>- Propietario responde en tiempo real |

---

### UC-5: Procesar Pago

| Campo | Contenido |
|-------|-----------|
| **Nombre** | Procesar Pago |
| **Actor Primario** | Cliente |
| **Requisitos Satisfechos** | **RF-6:** El sistema procesa pagos en línea y genera confirmación de reserva automática para ambas partes |
| **Descripción** | El cliente realiza el pago de la reserva a través de una pasarela de pago segura (Stripe, PayPal u otros), confirmando la reserva e iniciando el proceso de notificación. |
| **Precondiciones** | • Debe existir una reserva en estado "Pendiente de pago"<br>• El cliente debe estar autenticado<br>• El monto total debe haber sido validado por el sistema |
| **Postcondiciones** | • Pago se procesa exitosamente<br>• Reserva cambia a estado "Confirmada"<br>• Cliente recibe confirmación por email<br>• Propietario recibe notificación de nueva reserva confirmada<br>• Ambos pueden ver detalles completos de la reserva |
| **Flujo Principal** | 1. Cliente en pantalla de pago ve desglose final y opciones de pago<br>2. Selecciona método de pago (Tarjeta crédito, PayPal, etc.)<br>3. Ingresa datos (nombre titular, número, vencimiento, CVV)<br>4. Marca "Recordar tarjeta" (opcional)<br>5. Hace clic en "Pagar"<br>6. Sistema envía solicitud a pasarela de pago<br>7. Pasarela procesa pago (2-5 segundos)<br>8. Si es aprobado: actualiza reserva a "Confirmada"<br>9. Genera número de confirmación permanente<br>10. Envía email de confirmación con código QR para check-in |
| **Flujos Alternativos** | **2a. Usa tarjeta guardada:**<br>- Sistema lista tarjetas previas<br>- Cliente selecciona una<br>- Sistema pide solo CVV de confirmación<br><br>**7a. Pago rechazado - Tarjeta expirada:**<br>- Sistema notifica que tarjeta expiró<br>- Solicita método de pago alternativo<br>- Cliente puede intentar otra tarjeta<br><br>**7b. Fondos insuficientes:**<br>- Pasarela rechaza por fondos insuficientes<br>- Cliente puede intentar otra tarjeta o método |

---

### UC-6: Valorar Alojamiento

| Campo | Contenido |
|-------|-----------|
| **Nombre** | Valorar Alojamiento |
| **Actor Primario** | Cliente/Huésped |
| **Requisitos Satisfechos** | **RF-7:** Los huéspedes pueden valorar y comentar el alojamiento una vez finalizada la reserva |
| **Descripción** | El huésped valora su estancia con una calificación numérica (1-5 estrellas) en múltiples dimensiones y proporciona comentarios y fotos después de completar la reserva. |
| **Precondiciones** | • La reserva debe estar en estado "Completada"<br>• Mínimo 1 hora debe haber pasado desde checkout<br>• El huésped debe estar autenticado<br>• No debe haber valoración previa para esta reserva |
| **Postcondiciones** | • Valoración se publica en perfil del alojamiento<br>• Propietario recibe notificación de nueva valoración<br>• Propietario puede responder a la valoración<br>• Rating promedio del alojamiento se actualiza |
| **Flujo Principal** | 1. Sistema envía email 24 horas después de checkout: "¿Cómo fue tu estancia?"<br>2. Huésped hace clic en enlace o accede a historial de reservas<br>3. Selecciona la reserva completada<br>4. Sistema muestra formulario con calificaciones por categoría (Limpieza, Comunicación, Exactitud, Checkin, Servicios)<br>5. Escribe comentario (mínimo 10, máximo 1000 caracteres)<br>6. Selecciona fotografías para compartir (opcional)<br>7. Revisa antes de enviar<br>8. Hace clic en "Enviar valoración"<br>9. Sistema publica inmediatamente en perfil del alojamiento |
| **Flujos Alternativos** | **1a. Iniciación manual:**<br>- Huésped accede a dashboard<br>- Busca "Mis valoraciones"<br>- Selecciona alojamiento<br>- Procede desde paso 4<br><br>**6a. Sube fotos:**<br>- Selecciona hasta 5 fotos de su teléfono<br>- Sistema valida formato (JPG, PNG)<br>- Se cargan con la valoración |

---

### UC-7: Supervisar Actividad

| Campo | Contenido |
|-------|-----------|
| **Nombre** | Supervisar Actividad |
| **Actor Primario** | Administrador |
| **Requisitos Satisfechos** | **RF-8:** Los administradores pueden supervisar actividad, bloquear anuncios y gestionar incidencias |
| **Descripción** | El administrador monitorea la actividad general de la plataforma, revisa métricas clave en tiempo real, accede a información de usuarios, alojamientos, reservas e ingresos, e identifica anomalías. |
| **Precondiciones** | • El usuario debe tener rol de Administrador<br>• Usuario debe estar autenticado<br>• Debe haber actividad en la plataforma |
| **Postcondiciones** | • Administrador puede ver dashboards actualizados<br>• Puede identificar anomalías o problemas<br>• Puede acceder a información detallada para tomar acciones |
| **Flujo Principal** | 1. Administrador accede a "Panel de Control"<br>2. Ve dashboard con métricas en tiempo real (usuarios activos, reservas, ingresos, calificación promedio)<br>3. Selecciona período de análisis (Hoy, Semana, Mes, Personalizado)<br>4. Sistema genera gráficos actualizados<br>5. Accede a secciones: Usuarios, Alojamientos, Reservas, Ingresos, Valoraciones<br>6. Puede filtrar y buscar información<br>7. Genera reportes en PDF/Excel |
| **Flujos Alternativos** | **1a. Acceso a alertas:**<br>- Sistema muestra alertas prioritarias<br>- Usuarios reportados, alojamientos reportados, patrones sospechosos<br>- Administrador puede priorizar investigaciones<br><br>**5a. Revisa usuario específico:**<br>- Busca usuario por email/nombre<br>- Ve perfil completo, historial de transacciones<br>- Puede ver todas sus reservas como cliente y propietario |

---

### UC-8: Bloquear Anuncios

| Campo | Contenido |
|-------|-----------|
| **Nombre** | Bloquear Anuncios |
| **Actor Primario** | Administrador |
| **Requisitos Satisfechos** | **RF-8:** Los administradores pueden supervisar actividad, bloquear anuncios y gestionar incidencias |
| **Descripción** | El administrador revisa anuncios de alojamientos reportados o en revisión, y bloquea aquellos que violen normas de uso, notificando al propietario de las acciones requeridas. |
| **Precondiciones** | • El usuario debe tener rol de Administrador<br>• Usuario debe estar autenticado<br>• Debe existir alojamiento a revisar<br>• El alojamiento debe estar publicado |
| **Postcondiciones** | • Anuncio se oculta de búsquedas (si se bloquea)<br>• Propietario recibe notificación por email<br>• Sistema registra motivo del bloqueo<br>• Anuncio se puede desbloquear después de correcciones |
| **Flujo Principal** | 1. Administrador accede a "Alojamientos" en panel de control<br>2. Filtra por: "Reportados", "En revisión", "Bloqueados"<br>3. Selecciona alojamiento para revisar<br>4. Ve detalles: fotos, descripción, servicios, valoraciones, razón de reporte<br>5. Decide: Aprobar, Rechazar temporalmente, o Bloquear<br>6. Si bloquea, selecciona motivo (Contenido inapropiado, Información falsa, Incumplimiento seguridad, Comportamiento abusivo, Otro)<br>7. Escribe mensaje para propietario explicando problema y plazo de corrección (usualmente 7 días)<br>8. Hace clic en "Bloquear alojamiento"<br>9. Sistema oculta alojamiento e informa a propietario |
| **Flujos Alternativos** | **5a. Pide correcciones sin bloquear:**<br>- Alojamiento sigue visible<br>- Propietario recibe email "Requiere atención"<br>- Plazo de 3 días para corregir<br><br>**7a. Necesita más información:**<br>- Abre chat con propietario<br>- Solicita aclaraciones<br>- Propietario tiene 24 horas para responder<br><br>**7b. Bloqueo inmediato sin plazo:**<br>- Violación crítica de normas<br>- Alojamiento se bloquea inmediatamente<br>- Propietario recibe notificación clara del motivo |
|