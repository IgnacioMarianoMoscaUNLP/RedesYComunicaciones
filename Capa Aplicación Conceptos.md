## **Capa Aplicación Conceptos**

Las aplicaciones han sido el motor principal de uso cotidiano de la red de internet en el mundo. Hogares, escuelas, gobiernos, empresas, negocios han hecho del internet parte integral de sus actividades diarias.

Un concepto que explico la adopción de sistemas informáticos a nivel masivo, ya no solo en universidades o centros de inteligencia, sino en oficinas, colegios, hogares, etc. Es el concepto de Killer App que nos indica aquellas aplicaciones que fuero motivo y son de comprar una computadora o celular solo para correrla. Hoy en día está plagado de este tipo de apps, tal como whatssap, facebook, mercadolibre, etc. Aunque el concepto tuvo mucho más sentido en el auge del internet porque aún no había un uso masivo de internet, sino que se masifica a través de este tipo de aplicaciones  que surgen.



### **<u>Principios de las aplicaciones de red</u>**



La comunicación de los sistemas finales (hosts) es en concreto a través de la capa de aplicación, sin está no se podrían llegar a entender ni si quiera iniciar un intercambio de mensaje.

**Arquitecturas**

Desde la perspectiva de los desarrolladores la arquitectura de la red está hecha y diseñada para proveer un conjunto específico de servicios a las aplicaciones. 

Por otro lado la arquitectura de la aplicación es determinada por el desarrollador de la app y dicta como esta misma app se estructura de igual manera en los diferentes end systems.

Hay dos grandes paradigmas en los cuáles basar la arquitectura de una aplicación al diseñarla:

- **Client-Server arquitectura**
  - Siempre hay un host activo (always-on host) que es el Servidor
  - El server es el que sirve  a las consultas de los otros hosts que son los clientes
  - El ejemplo clásico de esta arquitectura es en el que hay una app web corriendo en un user agent de un cliente y una app web que responde las peticiones y está siempre activa en un server
  - Otra característica importante es que los servidores tiene un dirección fija y bien definida en el protocolo IP
  - Los **data centers** son eslabones claves en esta rede de servidores y clientes porque permiten replicar un mismo servidores en diferentes puntos para balancear cargas y tener una disponibilidad en tiempo y espacio eficiente
- **Peer-to-Peer (P2P) architecture**
  - No hay casi una gestión de servidores, y mucho menos que se alojen data centers
  - La comunicación es entre pares que cambian información de punto a punto, es decir, que se conectan uno a otro y cambian la data.
  - Conecta de manera directa a los hosts. 
  - Los pares no son propiedad del proveedor del servicio, sino que son diferentes sistemas finales que están en cualquier lado, ya sea oficinas, computadoras personales, en universidad, casas, colegios.
  - Como la arquitectura está hecha para que la comunicación no pase por servers específicos, se llama peer-to-peer
  - Un ejemplo popular de una app p2p es BitTorrent para compartir archivos
  - Una realidad es que es una arquitectura muy escalable, porque si bien es de a pares la comunicación, algo que sucede es que las computadoras que solicitaron un recurso, luego tienen la capacidad de compartirlo a todos los host que le soliciten, siendo así el incremento de pares que replican recursos a través de la red. 
  - Si la arquitectura no presenta un desafío en relación a costos y ancho de banda, si que presenta problemas en cuanto a la seguridad, performmance, confiabilidad, gracias a la estructura descentralizada que posee. 

**Comunicación de procesos**

Básicamente es tener en la cabeza que la comunicación entre end systems se basa en la comunicación de dos procesos relacionados que corren en los diferentes end systems, que se comunican entre si a través de protocolos con mensajes que son transmitidos a través de la red.

- Procesos de cliente y servidor:
  - Entender que siempre en una comunicación entre aplicaciones de sistemas finales, son procesos los que se comunican, uno es un proceso cliente y el otro un proceso servidor
  - En un p2p el que envia la información es el proceso servidor y el que la recibe el proceso cliente
  - Por definición podemos decir que en el contexto de una sesión de comunicación entre un par de procesos, el que inicia la comunicación es etiquetado como cliente. el proceso que espera a ser contactado para iniciar la sesión es el servidor



**La interfaces entre Procesos y la red de computadoras**

Los procesos envían y reciben mensajes a  la red a través de una interface de software llamada **socked**. 

Los socked permiten indicar en que punto de los host, tanto del emisor como receptor, se efectuán la comunicación, es decir, en que punto o dirección de un sistema final se efectuán las recepciones de los mensajes.

**Direccionamiento de procesos**

Las dirección para identificar un proceso en una comunicación entre host está dada por la dirección ip que identifica el host concreto dentro de la red y además por el socket que indica donde está escuchando el proceso receptor.



**Servicios de Transporte disponibles en las aplicaciones**

El socket es el medio de comunicación entre la capa de aplicación y la capa de transporte.

La capa de aplicación pushea el mensaje a través del socket correspondiente y la capa de transporte lo recibe.

Desde la capa de aplicación podemos elegir de que manera (protocolo) se transporta la información dependiendo de las necesidades que tengamos:

- **Transferencia de datos confiable**

  - Se basa en que hay aplicaciones que no pueden tolerar fallas y pérdidas en los datos que envían, por ende se sirven de protocolos específicos que controlan la conexión, controlan que no haya errores en los bits de los paquetes, que no se pierdan paquete, para que la recepción de los mismos se concreten en su totalidad, así no se pierde información
  - Por otro lado hay aplicaciones que son tolerantes a fallos que no necesitan este tipo de protocolos, porque las fallas no son cruciales. Ejmplos puede ser las llamadas con video o audio donde la pérdida de datos genera una pérdida de calidad en la llamada pero no la impide en su totalidad, por ende no es necesario un protocolo confiable sino uno que sea rápido para mantener una comunicación en vivo eficiente.

- **Rendimiento**

  - Esta caracterísitica responde a la necesidad de que algunas apps necesitan que la tasa de envío de bits tenga un humbral mínimo para que funcionen correctamente. Exigen cierta eficiencia para tener un funcionamiento optimo.
  - Las aplicaciones **Sensibles al ancho de banda** son las que necesitan ese umbral mínimo en la tasa de envío de bits por segundo para un funcionamiento
  - Muchas de estas además son **aplicaciones elásticas** que adaptan su estándares de rendimiento al ancho de banda disponible. Por ejemplo se adapta la calidad de transmisión de un video a una tasa de bits mayor o menor según el ancho de banda disponible, que involucra la cantidad servicios que estén usando este ancho de banda y la calidad (cantidad de bits) que necesiten cada uno.

- **Tiempo de respuesta**

  Se basa en la necesidad de las aplicaciones que necesitan tiempos de repuesta cortos. Son apps que fijan o que necesitan requisitos en el tiempo de envío y recepción de paquetes para que la comunicación sea optima en relación al tiempo. Por ejemplo se puede requerir que cada paquete del emisor llegue en no más de x mili segundo al socket donde escucha el proceso receptor. Retardos en algunas apps generan un entorno no realista que afecta a la experiencia de los usuarios de los sistemas finales

- **Seguridad**

  Es una característica escencial en comunicación entre sistemas finales que compartan información sensible. Hay protocolos de la capa de transporte que se encarga de encriptar el mensaje del emisor y descencriptarlo en el receptor. Además hay maneras de configurar identificación de sistema final a sistema final que mantiene la confidencialidad de la información compartida.

#### **Servicios de transporte que provee la Internet**

- TCP
  - Servicio orientado a conexión: treehandshake para entablar conexión y permitir un flujo continió de emisión y recepción de mensajes entre el cliente y el servidor. Mantiene la conexión hasta que la aplicación la finaliza. 
  - Servicio confiable de entrega de datos: Podemos descansar en tcp para enviar datos de manera que no haya  pérdidas de datos y errores, y que la recepción delos mismos paquetes sea en orden. Se confía en tcp para que no pérdida ni duplicado de bytes.
  - Además tcp brinda el control de congestión que activa mecanismos para ralentizar o achicar el ancho de banda de la comunicación, no solo en beneficio de tener una comunicación fiable frente a la pérdida de paquetes, sino que también en beneficio del funcionamiento de la red en si misma frente al masivo envío de paquetes en la misma.
- UDP
  - procolo sin muchos servicios, ya que no provee confiabilidad en la recepción del paquete, no mantiene conexión ni inicia una, no controla un recepción ordenada.
  - No provee control de congestión, por ende los paquetes llegan al receptor cuando es posible, digamos en cuanto la red congestionada lo permita, por ende no hay restricciones en el tiempo para la recepción de los mismos.
- TLS: No es un protocolo en si mismo, si no que se une a TCP para hacer complemento a la seguridad y confiabilidad del protocolo. Podemos decir que es parte de la capa de aplicación pero interactúa con la de transporte. Su función es encriptar el mensaje y pasarlo por un socket específico que indica que el mensaje es encriptado, tcp se encarga de transportarlo al mismo socket de tls del lado del sistema receptor, para que aquí se realice la de-encriptación del mensaje y se pase al proceso receptor. 

**Servicios que no aseguran los protocolos de tranporte**

Simplemente debemos entender que si bien hay manera confiable de enviar datos a sistemas terminales con tcp y de manera segura. Podemos entender que los servicios de efiencia y de tiempo  no son garantizados por estos protocolos de la capa de transporte. Es la estructura misma de la  internet la que permite eficiencia y que se desarollen aplicaciones que se ejecutan y necesitan cierta eficiencia en el ancho de banda en el tiempo de recepción. No se garantizan estos requerimientos porque dependen de la congestión de la red, de la disponibilidad de recursos, de la disponibilidad de servers, de links, etc. 

Considerar que a veces servicios que corren mejor en udp son llevados a tcp por la política de firewalls que manejar algunos servidores o sistemas terminales que prohíben el uso de udp por su falta de fiabilidad y seguridad.





#### **Protocolos de capa de aplicación**

Definen como procesos de aplicación, que corren en diferentes sistemas terminales, se pasan mensajes uno a otro. 

Protocolos de la capa definen:

- Tipos de mensajes: solicitudes o respuestas, etc..
- Sintaxis de diferentes tipos de mensajes: campos del mensaje y como son definidos estos campos
- Semántica de los campos: Significado de la información de los mismos
- Reglas que definen cuando y como un proceso envía y recibe a mensajes

Muchos de estos protocolos están definidos en las RFC y por ende son de uso público. Otros son de propiedad privada y no son de uso libre o público.

Debemos ver a los protocolos como piezas de las aplicaciones web y no como las aplicaciones web mismas, ya que estos solo definen como enviar y compartir recursos, información, datos, etc, a través de mensajes bien definidos y estructurados con su propio significado. 

**HTTP y la WEB**

Lo que llevó a la internet a un auge fue la world wide web,  que no es ni más ni menos que la internet basada en paǵinas web que comparten archivos, recursos, noticias, información, imagenes, etc. Y que dicho servicios sea de acceso fácil para los que querían consumir y querían publicar, teniendo una interfaz amigable y sencilla de entender que permitía que cualquiera pueda llegar a acceder a los sitios y realizar diferentes tipos de publicaciones. 

**HTTP**

Protocolo de transferencia de hipertexto (hypertext transfer protocol).

Es el corazón de la web. DEfinido en RFC (diferentes según la versión)

Se implementa en el proceso del cliente y el proceso del servidor, definiendo cada uno mensajes que intercambiarán el uno con el otro. 

Página web: Consiste de objetos. Un objetos es un archivo (html, imagen, js script, ccs style sheet, video, etc) que es accesible a través de una URL única. 

Cada URL consiste de dos componentes: El nombre del host del servidor que aloja el componente y el nombre que ubica al objeto en el servidor. 

- en http//www.nacho.com.edu/somethings/newTask.html
  - www.nacho.com.edu indica el nombre del host y somethings/newTask.html indica donde ubicar el objeto con su nombre en el servidor. 

Http indica como los clientes web solicitan páginas web y recursos y como los servidores web transfieren dichos requerimientos a los clientes.

Cabe recalcar que el servidor envía los recursos solicitados al cliente sin guardar ningun tipo de información del cliente. El servidor no mantiene ningún tipo de información sobre el cliente, por ende se dice que el protocolo http es un protocolo sin estados. 

La web usa la arquitectura Cliente-Servidor

**conexiones persistentes y no persistentes	**

Basicamente sin persistencia se realiza la resolución de un objeto del servidor web por conexión, es decir, habrá tantas conexiones como objetos se soliciten.

RTT es una medida de tiempo que engloba el tiempo de emisión de un paquete que pasa por toda la red hasta el receptor e incluye el tiempo de vuelta de la respuesta.

Las conexiones no persistentes hacen que se generen nuevas conexiones de manera constante, una relación lineal con la cantidad de recursos que se soliciten. Por cada conexión los buffers tcp se deben alocar y las variables tcp deben mantenerse tanto en el cliente como en el servidor. Esto puede traer un desborde en el lado del servidor que puede estar resolviendo miles de solicitudes simultáneas. Además cada objeto sufre un retraso de 2RTTs, uno para entablar la conexión TCP y otro para solicitar y recibir el objeto. 

Con HTTP/1.1 las conexiones persistentes permiten al servidor dejar la conexión tcp abiertas después de enviar una respuesta. Las solicitudes y respuestas posteriores al mismo cliente se pueden realizar en una misma conexión, evitando realizar múltiples three hand shake  para entablar conexiones. Una página entera puede ser enviada en una misma conexión. 

Es posible hacer sobre una misma conexión solicitudes de varias páginas distintas alojadas en un mismo servidor. Este puede cerrar la conexión después de una determinada cuota de tiempo transcurrida sin que haya habido uso de la conexión, es decir, que no hubo nuevas solicitudes de objetos al servidor. 

**User servers iteraction: Cookies**

Las cookies están definidas en una RFC y permite a los sitios mantener un rastro de los usuarios. Mayormente tienen un uso comercial hoy en día.

Están compuestas por 4 partes:

- Header de la cookies: Uno en la response http y luego de manera sucesiva en las request http. 
- El archivo cookie se mantiene en lado del usuario pero es de utilidad en el lado del servidor. 
- Además a una base de datos del sitio web que guarda datos de las cookies. 

Sitios de comercio como Amazon, Mercadolibre, etc.. Generan cookies incluso sin necesidad de que estes registrado, con el motivo de que en el tiempo y en futuras visitas sepán por el id de una cookie que se guardo en el user agent que usas (firefox, chrome) que además ellos identifican en su base de datos en la cual guardan todas las páginas que visitaste en Amazon.com y así tener un perfil para hacerte ofertas, conocer tus intereses, saber cuando te conectas, etc.

Entonces las cookies permiten de alguna manera crear una sesión de usuario o mantenerla en el tiempo en un protocolo que es sin estados como es http. 

Claro esta que las cookies pueden ser invasivas por la cantidad de información que guardan sobre la actividad y los íntereses de los usuarios, dejando una línea muy delgada entre el uso de datos públicos y datos que son privados. 

**WEB CACHING**

Un **Web cache** o **proxy server** es una entidad de red que responde solicitudes HTTP en lugar del servidor de origen.

- Guarda en disco copias de objetos solicitados recientemente.
- Los navegadores pueden configurarse para enviar primero todas las solicitudes al cache.

**Funcionamiento básico:**

1. El navegador se conecta al cache y solicita un objeto.
2. Si el objeto ya está guardado, el cache lo devuelve al cliente.
3. Si no está, el cache solicita el objeto al servidor de origen.
4. Al recibirlo, lo guarda y lo reenvía al navegador.

👉 El cache actúa como **servidor** para el navegador y como **cliente** frente al servidor de origen.

**Beneficios principales:**

- **Menor tiempo de respuesta**: si el objeto está en el cache y la conexión local es rápida.
- **Reducción de tráfico en el enlace de acceso** (por ejemplo, entre una universidad y la red pública). Menos tráfico implica menos necesidad de aumentar ancho de banda → reducción de costos.
- Mejora del rendimiento general de Internet al reducir tráfico global.

**Ejemplo numérico (sin cache):**

- Red institucional: LAN de 100 Mbps conectada a Internet por enlace de 15 Mbps.
- Solicitudes: 15 por segundo, objetos de 1 Mbit.
- Intensidad de tráfico en el acceso: 1 (saturación).
- Resultado: retrasos de minutos, inaceptable.

**Soluciones:**

1. **Aumentar ancho de banda** (ej. de 15 a 100 Mbps): caro, aunque reduce el tiempo a ≈ 2 segundos (el retraso de Internet).
2. **Instalar un cache en la red local**: con una tasa de aciertos (hit rate) del 40%, solo el 60% de los objetos atraviesan el enlace.
   - La intensidad del enlace baja a 0.6 → retrasos despreciables en el acceso.
   - Tiempo promedio de respuesta ≈ **1.2 segundos**, mejor que la solución de ampliar ancho de banda y más barata.

**CDNs (Content Distribution Networks):**

- Extienden la idea del cache a gran escala con servidores distribuidos en distintas regiones del mundo.
- Pueden ser **compartidos** (Akamai, Limelight) o **dedicados** (Google, Netflix).

## Conditional GET

**Problema del cache:** los objetos guardados pueden quedar **desactualizados** respecto al servidor de origen.

**Solución en HTTP:** el **Conditional GET**.

- Es una solicitud GET que incluye el encabezado `If-Modified-Since`.
- El cache guarda no solo el objeto, sino también su fecha de última modificación (`Last-Modified`).

**Ejemplo:**

1. El cache solicita un objeto y el servidor responde con:

   - Objeto + encabezado `Last-Modified: <fecha>`.

2. El cache lo guarda junto con esa fecha.

3. Una semana después, otro cliente pide el mismo objeto → el cache envía:

   ```html
   GET /objeto HTTP/1.1
   Host: servidor
   If-Modified-Since: <fecha_guardada>
   
   ```

   4.El servidor responde:

   - Si el objeto **no cambió** → `304 Not Modified`, sin enviar el objeto (ahorro de ancho de banda).
   - Si **cambió** → reenvía el objeto actualizado con nueva fecha.

   👉 Así, el cache garantiza que los clientes reciban contenido **actualizado** sin necesidad de transferir objetos innecesariamente, optimizando **ancho de banda y tiempo de respuesta**.