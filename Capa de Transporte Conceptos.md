Capa de Transporte Conceptos

Su función se extiende junto al protocolo de red por exelencia, ip, ya que la capa de transporte inicia y controla el envío a través de la red mensjaes entre host terminales que intercambian mensajes por diferentes aplicaciones. 

**Servicios principales:**

Brinda una comunicación logica entre procesos de aplicación que corren en diferentes hosts. Por comunicación logica nos referimos a que estos hosts se abstraen de la diferencia física y espacial que pueda haber entre ambos, y se comunican como si residen en el mismo espacio. 

La capa no recibe en nodos de red, sino en sistemas terminales. Convierte mensajes de aplicaciones en segmentos de capa de transporte. A través de la red viaja como datagrama el mensaje que contiene el segmento que luego será analizado por la capa de transporte del sistema terminal remitente. 

**Relación entre capa de transporte y de red**

Mientras que la capa transporte brinda una conexión lógica entre los procesos de los hosts involucrados, la capa de red brinda conexión lógica entre los hosts. 

Los protocolos de la capa de transporte pueden ofrecer servicios que la capa de red no incluye por más de que sea una capa superior. 

**Overview **

La capa tiene dos grandes protocolos. **udp** siendo un protocolo no confiable, no orientado a conexión. El segundo es **tcp** que provee confiabilidad, es un servicio orientado a conexión. 

Entender que los protocolos buscan brindan una extensión del servicios de delivery que hace ip de host a host, hacerlo de proceso a proceso. Esto se conoce como **multiplexacion y demultiplexacion de capa de transporte**.

Estos protocolos brindan también control de errores chequeando ciertos registros en los headers de los segmentos. 

Estos dos últimos servicios mencionados, de multiplexacion, demultiplexacion y checksum son los únicos servicios que ofrece udp. 

Tcp en cambio ofrece más servicios. Provee confiabilidad de transporte. Usando control de flujo, número de secuencia, acks, timers, chequea que llegan todos los segmentos y en orden.  Por ende convierte el protocolo no confiable IP en confiable por así decirlo. 

Tcp además provee control de congestión. No es un servicio para la aplicación como tal, sino para el funcionamiento general de la red de internet. De manera básica digamos que esto controla el flujo de segmentos enviados a través de la red cuando hay tráfico excesivo datagramas viajando. 

**Multiplexing and Demultiplexing**

Partimos de que en un host corren varios procesos en diferentes protocolos (http, ftp, smtp, etc) por lo que además hay diferentes sockets. Los sockets son puertas por la cual se envían y reciben los datos de los procesos a la red y viceversa. Por lo que el la capa de transporte ocmo no entrega datos a procesos sino a sockets. Hay varios socket por host que se identifican cada uno por un número distinto. Varia la manera de identificar sockets entre udp y tcp. 

El proceso de recibir segmentos y pasarlos al sockets correcto es lo que se conoce como demultiplexacion que realiza la capa de transporte.

El proceso de recolectar secciones de datos de los sockets del host emisor, encapsulando cada sección de datos con headers específicos para crear segmentos y pasarlos a la capa de red es lo que conocemos con multiplexación. 

Son conceptos que como tal se dan en algun punto en diferentes capas de una manera parecida, no son sólo de la capa de tranporte

En los headers del segmenetos a multiplexar deben colocarse port origen y port destino para lograr enviar al proceso en el host destino. 

Tenemos los port reservados del 0 al 1023. Que son los port well-know utilizados por diferentes protocolos como http o ftp. 



**Multiplexing y demultiplexing sin conexion (En realidad refiere a UDP)**

​	Como tal podemos decir que un se usa la tupla doble que  incluye port e ip destino.

​	Simplemente tener en cuenta que la idea de enviar el port de origen es para que si el host remitente quiere responder, sabe a que port hacerlo. 

**Orientado a conexión multiplexacion y demultiplexacion**

TCP. Difiere en que se identifica con una tupla cuadruple que incluye source ip y port, y destino port e ip. Por lo tanto en host destino, tcp usa todos los datos para hacer demultiplexing correctamente y enviar el mensaje al proceso correspondiente.

Digamos que se identifica la conexión y los canales fisicos de cada host para pasar la data, eso sería el socket.





**Servidores web y tcp**

Los servidores pueden ejecutar un solo proceso que responde a un port particular y resolver diferentes conexiones teniendo diferentes threads que las atiendes, haciendo de alguna manera así múltiples sockets. 

Tener en cuenta que las conexiones http persistentes permiten mantener sockets y no estar creando y descartando sockets todo el tiempo, cosa que genera latencia y consumo al servidor. 

#### **Transporte de conexiones no orientadas a conexión: UDP **

No hace casi nada, simplemente hace el servicio de multiplexacion/demultiplexación y coloca el header del checksum, pero nada más. No mantiene conexiones, no hace controles ni de confirmación de llegada, no controla flujo, etc. Por esto mismo se entiende como un protocolo de mejor esfuerzo porque es rápido y se entiene como protocolo sin orientación a conexión, porque no genera ninguna conexión previa para enviar los segmentos.

Hay razones para querer usar udp en vez de tcp para diferentes aplicaciones:

- Muy poco control a nivel de aplicación sobre lo que datos se están  enviando y cuando. Esto es en referencia a que tcp hace muchos controles que atrasan los envíos de segmentos a la red. En aplicaciones de tiempo real, donde se necesita en todo momento un ratio de transferencia y que no importa si se pierden datos (pérdida de calidad de streams o videos), udp matchea con estos requerimientos, en cambio tcp no por se un protocolo que puede retrasar la entrega de segmentos. Este tipo de aplicaciones pueden aplicar funcionalidades por su cuenta para hacer ciertas cosas que no hace udp, sin dejar de usarlo para seguir teniendo buenos tiempos de entrega de segmentos de punto a punto. 
- NO requiere establecimiento de conexión. Udp no introduce por lo tanto ninguno delay para establacer alguna conexión. Dns para ser veloz corre en udp. Http no se usa con udp, sino tcp porque es un protocolo que busca la confiabilidad más que nada por su uso en páginas web. Obviamente el usar tcp en http genera retrasos para descargas de archivos de páginas web, por eso el protocolo QUIC de chrome es un avance que implementa confiabilidad junto a udp.
- Sin estado conexión. Básicamente udp no mantiene información sobre la conexión en los sistemas terminales como lo hace tcp. No mantiene párametros que regulan flujo, congestión, etc. Por esto mismo un servidor por ejemplo puede mantener un soporte mucho más rápido y con más clientes activos utilizando udp en vez de tcp, ya que el consumo de recursos para mantener conexiones sería muchísimo menor. 
- Pequeño overhead por headers pequeños. Udp mantiene 8 bytes de headers frente  a los 20 que tiene tcp.

Hay varias discusiones sobre que protocolo es mejor para que, según que app, etc. Hay investigadores que proponen que se haga control de congestión siempre, ya que muchas veces servicios que funcionan en udp, como el streaming, genera una congestión de la red que termina perjudicando a los servicios que corren sobre tcp, retrasando el envío de segmentos. 

Como se mencione, muchas aplicaciones optan por hacer uso de udp y hacer implementaciones para confiabilidad en la app misma, pudiendo así no tener restricciones de conexión que de por si impone tcp. Obviamente estás implementaciones generar que las aplicaciones use tiempo de ejecución para resolver la entrega y recepción de paquetes, digamos que es tiempo no efectivo de ejecución porque no ejecuta lógica de la app como tal. 