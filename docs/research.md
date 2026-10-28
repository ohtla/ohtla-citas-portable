# Agendas de citas: investigación y definición del problema

**Hallazgo principal:** La dificultad no está en mostrar un calendario, sino en transformar los horarios de trabajo en espacios realmente disponibles, reservar uno sin condiciones de carrera y mantener informados al cliente y al negocio cuando cambian los planes. Un enlace compartible es el punto de partida de menor fricción; las asignaciones avanzadas, los pagos y la mensajería son útiles cuando un flujo real los requiere.

Investigación consultada el 28 de octubre de 2026. Las funciones, políticas, tarifas y limitaciones de las plataformas pueden cambiar.

## El problema del negocio

Hoy muchos negocios coordinan citas por chat o teléfono. Una conversación típica solicita el servicio, la sucursal, un horario preferido y una confirmación. El personal compara agendas manualmente, propone alternativas y corrige conflictos. Los problemas recurrentes son las reservas duplicadas, los mensajes perdidos, los horarios inconsistentes entre sucursales, las ausencias y los errores de zona horaria.

Un sistema de agenda sustituye ese intercambio por un flujo controlado:

1. El negocio define qué se puede reservar, dónde, durante cuánto tiempo y en qué horarios.
2. El cliente elige un espacio disponible y proporciona solo los datos necesarios.
3. El sistema confirma la reserva, muestra sus detalles y actualiza la agenda del negocio.
4. Ambas partes pueden gestionar cancelaciones o cambios sin perder el registro de la cita.

## Lo que muestran los productos existentes

| Función | Evidencia | Implicación para este proyecto |
| --- | --- | --- |
| Páginas públicas de reserva, tipos de servicio o evento, disponibilidad, intervalos de preparación, límites de reserva, preguntas al cliente, cancelación y reprogramación | [Funciones de Cal.com](https://cal.com/pricing) | Empezar por la página de reservas, duración, horarios, excepciones y cancelación; no copiar todas las reglas avanzadas. |
| Horarios del personal, servicios, reservas en línea y varias sucursales | [Ayuda de Square Appointments](https://squareup.com/help/us/en/topic/appointments) y [funciones de Square](https://squareup.com/us/en/pricing) | Modelar por separado las organizaciones, sucursales y agendas reservables, aunque la interfaz inicial sea sencilla. |
| Solicitudes de confirmación y recordatorios para reducir ausencias | [Guía de notificaciones de Square](https://squareup.com/help/us/en/article/6729-customer-confirmations-with-square-appointments) | Considerar los recordatorios como una mejora posterior; implican entrega, consentimiento y costos. |
| Asignación entre miembros del equipo, citas colectivas, pagos y análisis | [Funciones de Cal.com](https://cal.com/pricing) | Son posibilidades de crecimiento, no requisitos para la primera cita confiable. |

Estas son observaciones sobre funciones, no una afirmación de que todos los sectores necesiten el mismo proceso. Una clínica, un servicio de tutoría, un salón y una consultoría pueden diferir en datos iniciales, capacidad, privacidad, pagos y políticas de cancelación. El primer producto debe ofrecer algunos puntos estables de configuración, no un constructor general de flujos.

## Capacidades mínimas

Un sistema pequeño pero útil necesita:

- Un enlace público de reserva, apto para móviles y asociado con una organización y una sucursal.
- Un servicio con nombre y duración, y una agenda que represente a una persona o recurso reservable.
- Horarios semanales, cierres en fechas concretas y la zona horaria del negocio.
- Disponibilidad calculada en el servidor que excluya las citas existentes y los bloqueos manuales.
- Prevención atómica de conflictos cuando dos clientes eligen el mismo horario.
- Un formulario breve, una confirmación clara y una vista de citas para el negocio.
- Una forma de cancelar o modificar manualmente una cita; de lo contrario, la agenda dejará de reflejar la realidad.
- Aislamiento entre organizaciones, acceso básico del personal y protección frente a envíos automatizados al formulario público.

El [plan del producto](product-plan.md) convierte estos puntos en una propuesta de alcance. La [nota de arquitectura](architecture.md) explica por qué la prevención de conflictos y las zonas horarias son indispensables incluso en un MVP pequeño.

## La forma de empezar con menos fricción

Utilizar una URL que el negocio pueda copiar en su sitio web, perfil social, código QR o conversación de WhatsApp. El cliente no debería necesitar una cuenta ni instalar una aplicación. El formulario puede limitarse a nombre y un método de contacto, salvo que un caso real justifique más campos.

No convertir WhatsApp en una dependencia técnica para reservar. Un [enlace para iniciar un chat](https://faq.whatsapp.com/5913398998672934) abre la conversación y puede mostrar un texto prellenado, pero no envía el mensaje ni reserva un horario por sí solo. La reserva debe realizarse en el servidor de esta aplicación. Véase [Integración con WhatsApp](whatsapp.md).

## Supuestos que deben validarse con negocios piloto

- ¿Es aceptable elegir una sucursal y una agenda con nombre, o el sistema debe asignar al personal automáticamente?
- ¿Es necesaria la reprogramación autónoma en la primera versión, o bastan la cancelación y una nueva reserva asistida por el personal?
- ¿Qué método de contacto prefieren los clientes para las confirmaciones?
- ¿Son indispensables los anticipos, la capacidad superior a una persona, los formularios de ingreso o la sincronización con calendarios externos para el primer sector piloto?

Son preguntas de investigación, no compromisos implícitos del MVP. Conviene probar el flujo con algunos negocios de sectores distintos antes de ampliar el modelo de datos.
