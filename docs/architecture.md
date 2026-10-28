# Propuesta de arquitectura diseñada para Cloudflare

**Recomendación para revisión:** Empezar con un único Cloudflare Worker de aplicación completa, que sirva archivos estáticos y rutas de API, respaldado por una base de datos D1. Mantener las reglas de agenda en un módulo de dominio pequeño y establecer una operación atómica para confirmar reservas. Añadir Durable Objects solo si el modelo elegido no puede garantizar de forma verificable la ausencia de superposiciones en D1 o si se necesita coordinación por agenda. Esta es una propuesta, no una decisión definitiva de implementación.

Cloudflare recomienda [Workers Static Assets](https://developers.cloudflare.com/workers/static-assets/) para aplicaciones nuevas; un Worker puede desplegar la interfaz y la API conjuntamente. [D1](https://developers.cloudflare.com/d1/) proporciona datos relacionales con semántica de SQLite. Los [Durable Objects](https://developers.cloudflare.com/durable-objects/best-practices/rules-of-durable-objects/) están diseñados para coordinar estado de forma consistente por entidad, incluso en sistemas de reservas, pero agregan otro componente con estado. Un producto pequeño no debería asumir esa complejidad antes de necesitarla para cumplir sus garantías.

## Límites lógicos

```text
Página pública de reservas / interfaz del personal
                       |
                  Rutas del Worker
                       |
         Acceso por organización + validación
                       |
              Dominio de la agenda
                       |
             Almacenamiento en D1
                       |
Adaptadores opcionales posteriores: WhatsApp, correo, calendarios
```

La interfaz no debe calcular la disponibilidad definitiva. Las rutas del servidor identifican la organización y sucursal públicas mediante una URL legible, validan las relaciones entre servicio y agenda, y calculan los horarios. Las rutas del personal obtienen el acceso a la organización desde la membresía autenticada; nunca confían únicamente en un identificador de organización enviado por el cliente.

## Modelo de dominio inicial

| Entidad | Propósito | Regla importante |
| --- | --- | --- |
| Organización | Límite entre clientes del sistema e identidad pública | Identificador público único en la URL e identificador interno opaco. |
| Membresía | Función del personal dentro de una organización | Toda acción del personal requiere una membresía activa. |
| Sucursal | Sede y zona horaria IANA | Pertenece a una sola organización. |
| Servicio | Oferta reservable, duración e intervalo de preparación | Solo se ofrece donde esté habilitado explícitamente. |
| Agenda | Persona o recurso reservable de capacidad uno | Pertenece a una organización y una sucursal en la primera versión. |
| Regla o excepción de disponibilidad | Intervalos semanales y cambios por fecha | Se interpreta en la zona horaria de la sucursal. |
| Bloqueo | Indisponibilidad manual | Impide reservar igual que un intervalo ocupado. |
| Reserva | Cliente, servicio, agenda, inicio, fin y estado | No hay dos intervalos activos superpuestos en una agenda. |

Todas las tablas pertenecientes a una organización incluyen `organization_id` y los índices compuestos apropiados. Las relaciones deben comprobar que sucursal, servicio, agenda y reserva pertenecen a la misma organización. La página pública solo muestra información necesaria para reservar, nunca credenciales del personal ni datos privados de otros clientes.

## Integridad de las reservas

1. Guardar los instantes reales de inicio y fin en UTC y, por separado, el identificador IANA de la zona horaria de la sucursal. Convertir los horarios locales semanales en intervalos UTC para la fecha solicitada.
2. Generar horarios candidatos teniendo en cuenta la duración del servicio, el intervalo de preparación, los bloqueos, la anticipación mínima, el plazo máximo y las reservas activas. Usar intervalos semiabiertos `[inicio, fin)` para permitir citas consecutivas.
3. Al confirmar, volver a comprobar todas las reglas en el servidor. Un horario mostrado es una invitación a reservar, no una reserva.
4. Confirmar mediante una única operación de control de concurrencia atómica. La primera implementación debe demostrar que dos solicitudes simultáneas para intervalos superpuestos no pueden tener éxito ambas. Una lectura seguida de una escritura sin atomicidad es insuficiente.
5. Si el horario deja de estar libre, devolver una respuesta clara de conflicto y alternativas actualizadas. La cancelación libera capacidad, pero conserva un registro de la reserva.

La estrategia exacta de escritura en D1 es una investigación técnica pendiente: elegir un esquema, instrucción o patrón transaccional que impida superposiciones y demostrarlo con pruebas de concurrencia en el entorno de ejecución previsto. Si las duraciones arbitrarias o combinaciones de recursos dificultan esa garantía, utilizar un Durable Object identificado por agenda como coordinador y definir qué almacenamiento es la fuente de verdad. **No** guardar reservas sin cuidado en D1 y en un Durable Object a la vez: los fallos entre ambos pasarían a ser parte del problema del producto. Cloudflare indica que los Durable Objects ofrecen coordinación fuerte, pero el orden de procesamiento no equivale necesariamente al orden de llegada; sus transacciones de almacenamiento deben usarse deliberadamente. [Guía de almacenamiento de Cloudflare](https://developers.cloudflare.com/workers/platform/storage-options/) y [reglas de Durable Objects](https://developers.cloudflare.com/durable-objects/best-practices/rules-of-durable-objects/).

Evitar las réplicas de lectura de D1 para calcular disponibilidad definitiva hasta diseñar explícitamente su consistencia. Se actualizan de forma asíncrona; la [guía de Sessions API](https://developers.cloudflare.com/d1/best-practices/read-replication/) describe marcadores para lecturas secuenciales. Un horario aparentemente libre en una réplica desactualizada nunca debe autorizar una reserva.

## Seguridad y operación básica

- Exigir membresía explícita para los permisos del personal y usar tokens opacos y revocables para cualquier enlace de cancelación del cliente.
- Reducir al mínimo los datos personales; ningún endpoint público debe exponer la lista de clientes de una organización. Documentar conservación y eliminación de datos antes de usar el sistema en producción.
- Limitar la frecuencia de envíos al formulario público y validar el token de [Turnstile en el servidor](https://developers.cloudflare.com/turnstile/get-started/server-side-validation/) si se adopta Turnstile. Un componente visual en el cliente por sí solo no brinda protección.
- Guardar las credenciales de proveedores en secretos del Worker, nunca en el código fuente ni en archivos públicos.
- Registrar la creación y cancelación de citas con datos de auditoría acotados y usar una clave de idempotencia para los reintentos.
- Probar el aislamiento entre organizaciones, las superposiciones, los cambios de horario de verano, las cancelaciones y la concurrencia antes del despliegue.

## Elecciones que aún no se hacen

Este documento no selecciona un framework de interfaz, proveedor de autenticación, ORM, calendario externo, proveedor de mensajes ni plan de precios. Antes de elegir dependencias, la primera implementación debe comparar su compatibilidad actual con Cloudflare y su facilidad de prueba local. R2, KV, Queues, Workflows y Durable Objects no son componentes iniciales solo porque Cloudflare los ofrezca.

Si posteriormente se añaden recordatorios automáticos, será pertinente disponer de ejecución programada o colas y reintentos idempotentes. No hacen falta para compartir un enlace de reservas y deben evaluarse por separado.
