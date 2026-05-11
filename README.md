# Modelado de Comportamiento en Sistemas de E-commerce

# Explicación técnica

Para el diseño de este diagrama nos hemos basado en el procedimiento de compra que utiliza la empresa Amazon para tener una base sólida sobre la que trabajar.

Comenzando desde que el usuario finaliza la compra, al darle a validar el sistema valida el stock y la sesión simultáneamente:

**STOCK**

- Si no hay stock manda un mensaje de error.  
- Si hay stock continua.


**SESIÓN**

- Si la sesión no es válida pide el correo y contraseña del usuario y valida enviando un PIN, este PIN si se introduce incorrectamente más de 3 veces se bloquea y no puede realizar la compra.  
- Si la sesión es válida continua.

Si la sesión y el stock son validados correctamente sigue el procedimiento llegando a una pasarela de pago segura donde pide al usuario el numero de tarjeta, el cvv (el codigo de verificación de la tarjeta) y la fecha de caducidad.

Si algo no es correcto vuelve a pedirlo hasta un máximo de 3 intentos, si no lo valida en esos tres intentos lo bloquea y no puede realizar la compra.

Si es correcto, simultáneamente registra el pedido, genera el pdf con la factura y notifica al cliente de que la compra ha sido un exito, para terminar envía un mensaje de confirmación y finaliza la compra.

# Imagen del diagrama diseñado

<img width="1002" height="1024" alt="DiagramaCompra" src="https://github.com/user-attachments/assets/d5a9ae3b-622a-4a8f-ad98-3964221e96b7" />

# Justificación del uso de nodos de sincronización

El uso es fundamental para representar el procesamiento paralelo y la concurrencia

1. En primer lugar utilizamos un nodo de bifurcación (fork) para poder validar de manera simultánea dos o más tareas independientes como lo son validar el stock y la sesión.  
2. Antes de pasar a la pasarela de pago segura utilizamos un join que actúa como una “llave de paso”, por lo que el flujo no puede avanzar al pago hasta que ambas condiciones se hayan completado correctamente.  
3. Para recoger tanto el nº de la tarjeta, como el CVV y la fecha de caducidad de la tarjeta utilizaremos una bifurcación para que compruebe que todos los datos son correctos antes de continuar el flujo.  
4. Para el procesamiento post-venta cuando el pago es validado, dividimos el flujo para realizar simultáneamente el registro del pedido en la base de datos, la generación de un pdf con la factura y la notificación al cliente de que la compra se realizó con éxito y asegurarnos que el cliente solo recibe el mensaje de confirmación.

# Bibliografía

O. OMG, "especificación formal de UML", OMG, . \[Online\]. Available: https://www.omg.org/spec/UML/. \[Accessed: 05-11-2026\].

I. IBM, "UML activity diagrams", IBM, 2021\. \[Online\]. Available: https://www.ibm.com/docs/en/rhapsody/9.0.1?topic=diagrams-uml-activity. \[Accessed: 05-11-2026\].

V. Visual Paradigm, "UML Activity Diagram Tutorial", Visual Paradigm, . \[Online\]. Available: https://www.visual-paradigm.com/guide/uml/what-is-activity-diagram/. \[Accessed: 05-11-2026\].
