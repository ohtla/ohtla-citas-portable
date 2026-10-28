# Ohtla Citas Portable

Proyecto de código abierto para gestionar citas, diseñado primero para Cloudflare y para organizaciones con una o varias sucursales. El objetivo es ofrecer un flujo de reservas pequeño y funcional que los negocios puedan compartir con sus clientes, incluso por WhatsApp, sin exigirles una cuenta.

> Estado: solo investigación y planificación. Todavía no hay código de aplicación, despliegue ni versión publicada. El alcance y la arquitectura propuestos están sujetos a revisión.

## Primera versión propuesta

Una organización configura sucursales, calendarios reservables, servicios y horarios semanales. El cliente abre un enlace público, elige un horario disponible, proporciona los datos de contacto mínimos y recibe una confirmación de la reserva. El personal puede consultar las citas y bloquear horarios. El servidor impide las reservas superpuestas.

La primera versión **no** debería depender de la Plataforma de WhatsApp Business. El negocio puede enviar el enlace público mediante su proceso habitual en WhatsApp. Los mensajes automatizados serían una integración posterior, evaluada por separado.

## Documentación

1. [Investigación del mercado y los flujos](docs/research.md): qué problemas resuelven las agendas y qué funciones importan primero.
2. [Alcance del producto](docs/product-plan.md): MVP propuesto, criterios de aceptación, trabajo diferido y decisiones pendientes.
3. [Arquitectura en Cloudflare](docs/architecture.md): multitenencia, integridad de las reservas, datos, seguridad y opciones de despliegue.
4. [Integración con WhatsApp](docs/whatsapp.md): enlaces de baja fricción frente a la API oficial.

## Principios

- Mantener el proceso de reserva breve y fácil de usar en dispositivos móviles.
- Establecer desde el inicio los límites entre organizaciones y sucursales.
- Configurar la duración de los servicios, la disponibilidad y las etiquetas, en lugar de crear una variante del sistema para cada sector.
- Preferir un núcleo pequeño y comprobable frente a integraciones que los primeros usuarios quizá no necesiten.
- No confundir la visualización de un calendario con una reserva correcta: la disponibilidad debe volver a comprobarse al confirmar la cita.

## Licencia

Este proyecto se distribuye bajo la licencia Apache 2.0. Consulte el texto oficial en [LICENSE](LICENSE).

## Estado del desarrollo

Git ya estaba inicializado en este directorio. No se ha creado el primer commit; la documentación está pendiente de revisión por parte del responsable del proyecto.
