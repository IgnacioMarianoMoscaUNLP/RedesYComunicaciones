**Conceptos Correo Electrónico**

- Posee tres componentes principales:	
  - User Agent: Permiten a los usuarios finales leer, responder, reenviar, guardar y componer mensajes. tenemos microsoft outlook, apple ail, app de gmail. Cuando el usuario termina de escribir un mail, el user agent es el que se encarga de enviar el mensaje a su mail server, donde será encolado para ser enviado al destinatario por el servidor. 
  - Mail servers: Son el núcleo de la infaestructura del email. Como dijimos son los que reciben mails del user agent para que se envie a l servidor de mail del destinatario o entregar directamente al user agent del destinatario. Los destinatarios tiene su propio mailbox localizados en uno de los mail servers. Cuando el usuario final quiere acceder a sus mensajes, el servidor autentica al usuario y luego permite descargar los mensajes en la mailbox. 
    - cuando los ervidores no pueden enviar mails a otro servidor, los guardan en la cola del mailbox del usuario para ser enviados luego en varios intentos. Cuando no se pudo enviar luego de unos días, el servidor elimina el mensaje de la cola y notifica al emisor. 
  - Simple Mail Transfer Protocol (SMTP): es el protocolo principal en la capa de correo eléctronico. Utiliza el protocolo confiable tcp para enviar mails del servidor de mail del emisor al del destinatario. 
    - tiene dos lados, el del cliente  que es el que se ejecuta en el servidor del emisor, y un lado servidor qu ees el que se ejecuta cuando recibe el servidor de mail del receptor. Ambas facetas corren en todos los servidores de correo que existen. 

**SMTP**

**SMTP original (RFC 821, 1982)**: fue diseñado en una época en la que las comunicaciones eran muy básicas. Solo permitía enviar texto en **ASCII de 7 bits**, sin soporte directo para caracteres extendidos ni binarios.

**Problema**: al crecer el uso del correo, la gente necesitaba enviar **archivos binarios** (imágenes, documentos, audio) y además **caracteres no ASCII** (acentos, caracteres de otros alfabetos). Eso no era posible con SMTP puro de 7 bits.

**Solución: MIME (RFC 2045, 1996 y siguientes)**: no se cambió el límite del protocolo SMTP (seguía siendo 7 bits por compatibilidad), sino que se inventó una capa de codificación.

- Se definieron codificaciones como **Base64** (convierte binario en texto ASCII seguro de 7 bits) y **Quoted-Printable** (para textos con algunos caracteres no ASCII).
- Los adjuntos (binarios) se convierten a texto ASCII válido de 7 bits antes de ser enviados.

**Extensión posterior: 8BITMIME (RFC 1652, 1994)**:
 Algunos servidores modernos soportan enviar directamente contenido de **8 bits** sin codificación, pero no todos lo implementan. Por eso MIME sigue siendo la base para compatibilidad universal.

Algo importante es tener en cuenta que el servicio nunca delega mensajes en servidores intermedios, quiero decir que las conexiones tcp para enviar mails entre servidores de correo son de punto a punto, si tengo un servidor en argentina y el otro en bielorrusia, se hará la conexion entre estos dos mediante tcp, incluso si uno de los servidores está caído, el mensaje quedará en la cola del servidor emisor. 

Pasos de conexiones en el correo:

- Primero el cliente que quiere enviar mail inicia conexión tcp en el puerto 25 al servidor de correo. Lo intenta hasta que logré la conexión.  Una vez que están conectados, se inicia una negociación de la capa. Digamos que se presentan antes de iniciar la transmisión del mensaje. 
  - En este proceso de presentación el cliente smtp indica el email del emisor y el email del receptor.  
  - Luego de la "presentación" se envía el mensaje. 
  - SMTP cuenta con tcp para enviar el mensaje de manera confiable sin errores. 
  - El cliente puede enviar múltiples mensajes a través de esta conexión, sino indica el cierre de la misma.

**Formato de Mensajes de correo **

Los mensajes de correo tiene una estructura de headers que brindan información que dan contexto al mensaje, en el rpotocolo son definidos antes del cuerpo del mensaje como tal. 

Los headers y el body se separán por medio de una línea blanca. 

Los headers son muy similares a como se conforman en HTTP, además hay algunos default y otros opcionales.

- From: 
- To:
- Subject:

Cabe recalcar que los headers no son parte de los comandos del protocolo SMTP, sino son parte del mensaje de correo. 

**Mail Access Protocols**

- **Problema**: No es práctico que el servidor de correo esté en la PC del usuario porque debería estar siempre encendida y conectada.
- **Solución**: El correo se almacena en un **servidor de correo compartido y siempre disponible**, al que el usuario accede desde su dispositivo mediante un **agente de usuario** (app o programa de correo).
- **Proceso de envío**: El remitente primero deposita su mail en su propio servidor; luego, ese servidor se encarga de reenviarlo al servidor del destinatario, incluso reintentando si falla la conexión.
- **Acceso del destinatario**: Como SMTP solo envía (push), para recuperar mensajes se usan **HTTP (webmail o apps)** o **IMAP (clientes como Outlook)**, que además permiten organizar y gestionar los correos en carpetas.

👉 En resumen, da a conocer por qué existen protocolos de **acceso al correo (HTTP, IMAP)** además de **SMTP**, y cómo permiten al usuario recibir y manejar sus mensajes sin necesidad de tener un servidor propio.

