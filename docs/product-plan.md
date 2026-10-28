# Plan del producto: el sistema de citas confiable más pequeño

**Propuesta para revisión:** Construir un núcleo de agenda para varias organizaciones con un único flujo público de reservas. Admitir varias sucursales y agendas reservables explícitas, pero posponer la asignación automática, los pagos y la mensajería automatizada. Esto permite adaptar el producto sin convertir la primera versión en una plataforma general de flujos.

## Recorridos de usuario

**Configuración del negocio:** Una persona responsable crea una organización, agrega una sucursal con su zona horaria, define un servicio y una agenda reservable, establece los horarios semanales y copia el enlace público.

**Reserva del cliente:** El cliente abre el enlace, elige sucursal, servicio y, si hay más de una, agenda; selecciona fecha y hora, proporciona su nombre y un método de contacto, confirma y recibe una referencia. No necesita una cuenta.

**Operación diaria:** El personal autorizado consulta las próximas citas de su organización o sucursal, agrega bloqueos, cancela o modifica reservas y puede comprobar qué horarios siguen disponibles.

## Alcance de la primera versión

| Área | Comportamiento necesario | Evidencia de aceptación |
| --- | --- | --- |
| Organización | Un usuario puede pertenecer a más de una organización; los datos no se filtran entre ellas. | Pruebas de lectura y escritura entre organizaciones. |
| Sucursal | Cada organización puede tener varias sucursales, cada una con zona horaria y contexto público propios. | Las reservas y su visualización usan la zona horaria de la sucursal elegida. |
| Servicio | Se configuran nombre, duración, intervalo opcional de preparación, estado activo y sucursales donde se ofrece. | Los horarios reflejan duración e intervalo; un servicio inactivo no se puede reservar. |
| Agenda | Representa a una persona o recurso reservable con capacidad para una cita a la vez; el cliente la elige explícitamente cuando sea necesario. | No se superponen reservas confirmadas ni bloqueos en la misma agenda. |
| Disponibilidad | Horarios semanales y cierres o excepciones por fecha; plazo máximo para reservar y anticipación mínima configurables. | La generación de horarios contempla cierres, límites y cambios de horario de verano. |
| Reserva | El cliente elige un horario ofrecido e indica nombre y un método de contacto. El servidor revalida y confirma o informa un conflicto. | Dos intentos simultáneos producen, como máximo, una reserva para una agenda de capacidad uno. |
| Gestión | El personal puede listar citas, bloquear horarios, cancelar y crear o modificar reservas manualmente. | Los cambios de disponibilidad aparecen en el flujo público. |
| Confirmación | La interfaz muestra una referencia y los detalles de la cita; cuando sea práctico, permite descargar un archivo de calendario. | El cliente puede conservar los detalles sin depender de un proveedor de mensajes. |
| Acceso y abuso | La administración requiere acceso del personal; los envíos al formulario público tienen límites y una medida contra bots. | El acceso no autorizado a otra organización falla; los envíos automatizados están acotados. |

Este es el mínimo propuesto, no una promesa de implementarlo en un solo cambio. La capacidad inicial es de una cita por agenda; las clases grupales y las combinaciones de recursos requieren otro diseño.

## Funciones pospuestas deliberadamente

- Recordatorios automáticos por WhatsApp, SMS o correo y bots de conversación.
- Pagos, anticipos, cargos por ausencia y facturas.
- Sincronización con Google Calendar u Outlook y creación de videollamadas.
- Asignación rotativa de personal, citas colectivas, listas de espera, clases y servicios que combinan varios recursos.
- Dominios personalizados, marca blanca, formularios avanzados, analítica y complementos específicos de cada sector.
- Reprogramación autónoma hasta comprobar las reglas de cancelación y prevención de conflictos.

Cada función pospuesta tiene un costo concreto: credenciales externas, obligaciones de consentimiento, consistencia distribuida o reglas de negocio adicionales. Solo debería incorporarse después de observar una necesidad en el piloto.

## Configuración en vez de variantes por sector

Utilizar términos de dominio neutros: **organización**, **sucursal**, **servicio**, **agenda**, **regla de disponibilidad**, **bloqueo** y **reserva**. Una agenda puede representar a un profesional, una sala, un vehículo u otro recurso de capacidad uno. Las etiquetas visibles y las descripciones de servicios deben configurarse; el núcleo no debe incluir procesos exclusivos de salones, clínicas o tutorías.

Una organización es propietaria de sus sucursales, servicios, agendas, membresías del personal y reservas. Cada sucursal puede ofrecer un subconjunto de servicios y agendas. Las combinaciones complejas de recursos deben quedar como capacidad futura, no como supuesto oculto del primer esquema.

## Secuencia de entrega propuesta

1. **Base:** estructura del proyecto, ejecución local de pruebas, esquema de datos, acceso limitado por organización y ejemplo con una sucursal.
2. **Reservas confiables:** reglas de disponibilidad, bloqueos, reserva atómica, página pública y confirmación.
3. **Operación del negocio:** calendario o lista de gestión, cambios manuales, cancelación, varias organizaciones y sucursales, y pruebas de acceso.
4. **Preparación del piloto:** pruebas de uso con negocios reales, accesibilidad, controles contra abuso, pruebas de zonas horarias y concurrencia, copias de seguridad y observabilidad.
5. **Integraciones opcionales:** añadir la API de WhatsApp u otros proveedores de mensajería solo si la evidencia del piloto justifica la configuración y el costo operativo.

Cada etapa debe poder utilizarse y comprobarse de forma independiente. Esta fase de planificación no incluye conectar una cuenta productiva de WhatsApp, desplegar ni publicar el repositorio.

## Decisiones pendientes de la persona responsable

1. ¿Qué primer sector y región definirán el piloto, aunque el núcleo siga siendo genérico?
2. ¿Bastan inicialmente la cancelación y una nueva reserva asistida por el personal?
3. ¿Es aceptable que el cliente elija una agenda con nombre o debe asignarse al personal automáticamente desde la primera versión?
4. ¿Qué canal de contacto y confirmación se prefiere antes de disponer de mensajes automatizados?

Estas decisiones afectan la experiencia de usuario y el orden de implementación; el plan no las resuelve de forma implícita.
