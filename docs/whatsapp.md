# Integración con WhatsApp: empezar con un enlace

**Recomendación para revisión:** Al principio, WhatsApp debería ser un canal para compartir la URL de reservas, no el motor de reservas ni una dependencia necesaria para ejecutar la aplicación. Así, el negocio puede seguir utilizando sus conversaciones habituales mientras la aplicación controla la disponibilidad y la integridad de cada cita.

Investigación consultada el 28 de octubre de 2026. Las políticas, los requisitos de incorporación y las tarifas de Meta deben revisarse nuevamente justo antes de implementar mensajes de pago.

## Fase 1: sin API de WhatsApp

El negocio copia una URL pública y la envía manualmente en una conversación de WhatsApp o la comparte mediante su perfil, estado o código QR. El cliente reserva en el navegador. No hacen falta una cuenta de desarrollador de Meta, webhooks, plantillas aprobadas ni integración de facturación por mensajes.

Un [enlace para iniciar un chat de WhatsApp](https://faq.whatsapp.com/5913398998672934) puede abrir una conversación con texto prellenado. **No** envía el texto automáticamente, no confirma su entrega y no crea una reserva. Si se ofrece tras confirmar una cita, la interfaz debe indicar que el cliente todavía tiene que pulsar «Enviar». La aplicación debe mostrar la referencia de la reserva sin depender de WhatsApp.

Contrapartida: la configuración es mínima, pero los recordatorios y cambios de estado siguen siendo manuales. Esto solo es aceptable para el primer piloto si los negocios pueden gestionar sus citas sin notificaciones automáticas.

## Fase 2: Plataforma de WhatsApp Business, si hace falta

La [Plataforma de WhatsApp Business](https://whatsappbusiness.com/developers/developer-hub/) ofrece API, webhooks, números de prueba e integración directa o mediante un proveedor asociado. Puede servir para confirmaciones, recordatorios, cancelaciones y respuestas, pero cada organización necesita un modelo de incorporación y titularidad del número. La [aplicación WhatsApp Business y la Plataforma son productos distintos](https://whatsappbusiness.com/resources/faq/); compartir enlaces durante un piloto no habilita automáticamente los mensajes mediante API.

Según las [funciones de la Plataforma](https://whatsappbusiness.com/products/business-platform-features/), los mensajes iniciados por el negocio requieren consentimiento y plantillas. Su [política de mensajería](https://whatsappbusiness.com/policy/) limita las respuestas sin plantilla a la ventana de atención de 24 horas y exige plantillas aprobadas fuera de ella. La [página de precios](https://whatsappbusiness.com/products/platform-pricing/) describe cargos según los mensajes entregados, el mercado y la categoría. Como las tarifas y los beneficios gratuitos cambian, este plan no fija un costo: habría que calcularlo con la tabla oficial vigente y el volumen estimado del piloto antes de activar el servicio.

Una integración oficial necesitaría:

- Configuración de la cuenta y el número de cada organización, con almacenamiento seguro de credenciales.
- Consentimiento explícito del destinatario y evidencia de autorización para mensajes salientes.
- Plantillas de tipo «utility» aprobadas para recordatorios o confirmaciones iniciados fuera de la ventana de atención.
- Verificación de webhooks, procesamiento idempotente de eventos, reintentos y seguimiento de entregas.
- Una alternativa clara de atención humana cuando falle la automatización o el cliente pida ayuda.
- Un modelo de uso y costos por organización; no se presupone un único número emisor para todas.

No utilizar automatizaciones no oficiales de WhatsApp Web como base del producto: harían que un flujo crítico dependiera de una sesión de cliente sin soporte para este uso. La base de datos de la aplicación sigue siendo la fuente de verdad; un mensaje es una notificación, nunca una prueba de que el horario quedó reservado.

## Criterio para decidir

Agregar la API oficial solo cuando los pilotos demuestren que compartir enlaces manualmente y consultar el panel del negocio no alcanza, y después de decidir quién controla el número de WhatsApp de cada organización y quién paga los mensajes. En ese momento puede evaluarse un flujo de reservas dentro de WhatsApp o mediante Flows frente al flujo en el navegador, sin suponer que sea más sencillo.
