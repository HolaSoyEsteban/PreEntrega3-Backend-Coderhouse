# PreEntrega 3 - Mejorando la arquitectura del servidor

## Consignas generales
- Profesionalizar el servidor
- Aplicar una arquitectura profesional para nuestro servidor
- Aplicar prácticas como patrones de diseño, mailing, variables de entorno, etc.

## Modificaciones en la capa de persistencia y lógica de negocio
- Modificar nuestra capa de persistencia para aplicar los conceptos de Factory (opcional), DAO y DTO.
- El DAO seleccionado (por un parámetro en línea de comandos como lo hicimos anteriormente) será devuelto por una Factory para que la capa de negocio opere con él. (Factory puede ser opcional)
- Implementar el patrón Repository para trabajar con el DAO en la lógica de negocio.
- Modificar la ruta /current para evitar enviar información sensible. Enviar un DTO del usuario solo con la información necesaria.
- Crear un middleware que pueda trabajar en conjunto con la estrategia “current” para hacer un sistema de autorización y delimitar el acceso a los siguientes endpoints:
  - Solo el administrador puede crear, actualizar y eliminar productos.
  - Solo el usuario puede enviar mensajes al chat.
  - Solo el usuario puede agregar productos a su carrito.

## Implementación de modelo Ticket y proceso de compra
- Crear un modelo Ticket que contará con todas las formalizaciones de la compra. Los campos serán:
  - Id (autogenerado por mongo)
  - code: String debe autogenerarse y ser único
  - purchase_datetime: Deberá guardar la fecha y hora exacta en la cual se formalizó la compra (básicamente es un created_at)
  - amount: Number, total de la compra.
  - purchaser: String, contendrá el correo del usuario asociado al carrito.
- Implementar la ruta /:cid/purchase en el router de carts, que permitirá finalizar el proceso de compra de dicho carrito.
  - La compra debe corroborar el stock del producto al momento de finalizarse.
  - Si el producto tiene suficiente stock para la cantidad indicada en el producto del carrito, restarlo del stock del producto y continuar.
  - Si el producto no tiene suficiente stock para la cantidad indicada en el producto del carrito, no agregar el producto al proceso de compra.
- Utilizar el servicio de Tickets para generar un ticket con los datos de la compra.
- En caso de existir una compra no completada, devolver el arreglo con los IDs de los productos que no pudieron procesarse.
- Una vez finalizada la compra, el carrito asociado al usuario que compró deberá contener solo los productos que no pudieron comprarse. Es decir, se filtran los que sí se compraron y se quedan aquellos que no tenían disponibilidad.
