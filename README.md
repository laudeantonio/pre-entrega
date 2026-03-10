
# Sistema de Gestión de Tickets y Escalamiento Crítico
Nombre del Caso: Sistema automatizar la recepción de Incidencias técnicos, clasificarlos por prioridad y días festivos, generar un ID de seguimiento único y alertar de inmediato al equipo seleccionado para la prioridad.

Recepción_Ticket_Soporte (Webhook): Punto de entrada que escucha peticiones externas (GET) y recibe el JSON con los datos del email, usuario, tipo de problema y nivel de escalamiento.

Procesador_Datos_y_Generador_ID (JavaScript): Limpia la información recibida y genera un ID de ticket único basado en la fecha y el nombre.

Sincronizador_Ticket_Habil (Merge): Nodo de control que espera recibir tanto los datos del ticket como la confirmación de "Día Laboral" antes de permitir el avance del flujo.

Clasificador_Prioridad_SLA (Switch): Evalúa el nivel de escalamiento del ticket para dirigirlo hacia el canal de atención correspondiente (Alta, Media o Baja).

Consultor_Horario_Bogotá (HTTP Request): Realiza una consulta dinámica a una API externa para obtener la fecha, hora y el día de la semana exacto en la zona horaria de Colombia.

Formateador_Día_Semana (Edit Fields): Extrae y organiza los campos específicos de la respuesta de tiempo (como day_of_week) para facilitar su lectura en los nodos posteriores.

¿Es_Día_Laboral? (If): Filtra el flujo validando si el día actual es hábil (Lunes a Viernes) o si corresponde a un fin de semana (Sábado o Domingo). Este if se crea para decidir si genera el ticket o tiene que realizar el ticket un día hábil.

Unión_Datos_Rechazo (Merge): Consolida la información del usuario con los datos de tiempo para generar una notificación de rechazo personalizada y coherente.

Gmail_Notificación_Fin_de_Semana (Gmail): Envía un correo electrónico automático informando al usuario que su ticket no puede ser procesado por ser día no hábil. Contiene un código en html para él envió del correo sea visualmente llamativo.

Alerta_Discord_Crítica (HTTP Request): Dispara una notificación inmediata a un canal de Discord mediante un Webhook cuando el ticket es clasificado como prioridad "Alta".

Gmail_Confirmación_Recibido (Gmail): Envía un comprobante al usuario con los detalles de su ticket. Contiene un código en html para él envió del correo sea visualmente llamativo.

Base_Datos_Google_Sheets (Google Sheets): Registra cada incidencia procesada en una hoja de cálculo, asegurando la trazabilidad y persistencia de todos los reportes recibidos.
