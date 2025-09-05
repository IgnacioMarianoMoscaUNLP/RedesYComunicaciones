Redes y Comunicaciones
Práctica1

1. ¿Qué es una red? ¿Cuál es el principal objetivo para construir una red?
    Una red es un conjunto de dispositivos interconectados que pueden comunicarse e intercambiar información entre sí. En el contexto de las tecnologías de información, una red de computadoras consiste en dos o más dispositivos (computadoras, servidores, impresoras, teléfonos móviles, etc.) conectados a través de medios de comunicación como cables, ondas de radio, o fibra óptica.
    Principales motivos de construir una red:

  	Compartir recursos
  	Comunicar
  	Acceso a información
  	Eficiencia operacional(coordinacion,etc)
  	Respaldo y Seguridad
  	Escalabilidad

2. ¿Qué es Internet? Describa los principales componentes que permiten su funcionamiento.
    Es una red de computadoras que interconecta billones de dispositivos (hosts y end systems) al rededor del mundo.
    Podemos ver la internet como una infraestructura que provee servicios a aplicaciones. 

3. ¿Qué son las RFCs?
    Son documentos que se llevaron a cabo para generar concenso en hacer protocolos que permitan una comunicacion generalizada, pactada que permita la intercomunicacion.

4. ¿Qué es un protocolo?
    Es una secuencia de pasos de a seguir que indican como entablar una comunicacion, llevar a cabo un cambio de informacion y de acciones a realizar. 
    """Un protocolo define el formato y el orden de los mensajes intercambiados entre dos o más entidades que se comunican, así como las acciones que se realizan en la transmisión y/o recepción de un mensaje u otro evento."""

5. ¿Por qué dos máquinas con distintos sistemas operativos pueden formar parte de una misma red?
    Porque estas se comunican a traves de la red con los mismos protocolos, por ende hablan el mismo idioma de cara al internet.

6. ¿Cuáles son las 2 categorías en las que pueden clasificarse a los sistemas finales o End	Systems? Dé un ejemplo del rol de cada uno en alguna aplicación distribuida que corra sobre Internet.
    Basicamente se dividen entre clientes y servidores.

7. ¿Cuál es la diferencia entre una red conmutada de paquetes de una red conmutada de circuitos?
    Basicamente la diferencia radica en como se trasmiten los paquetes de una comunicacion. En una conmutacion de paquetes basicamente se aprovecha al maximo los recursos para trasmitir cada paquete en un camino diferente, digamos que se usa el medio mas rapido o el primero en resolver. En cambio una red conmutada de circuitos es una comunicacion dedicada para trasmitir paquetes de un punto a otro por un solo camino que reserva los recursos  los utiliza hasta terminar de transmitir. 

  

  ![image-20250904193739183](/home/ignaciomariano/.var/app/io.typora.Typora/config/Typora/typora-user-images/image-20250904193739183.png)

  DSL (Digital Subscriber Line)
  Funcionamiento:

Utiliza las líneas telefónicas existentes de la casa
La compañía telefónica local también actúa como ISP
El módem DSL convierte datos digitales en tonos de alta frecuencia
Viajan por el cable telefónico hasta el DSLAM en la central telefónica

Componentes clave:

DSLAM: Digital Subscriber Line Access Multiplexer (en la central telefónica)
Splitter: Separa las señales de datos y teléfono en casa

Bandas de frecuencia:

0-4 kHz: Llamadas telefónicas normales
4-50 kHz: Datos subida (upstream)
50 kHz-1 MHz: Datos bajada (downstream)

Velocidades estándar:

Bajada: 24 Mbps y 52 Mbps
Subida: 3.5 Mbps y 16 Mbps
Nuevos estándares: hasta 1 Gbps agregado

Limitaciones:

Velocidad disminuye con la distancia (máximo 5-10 millas de la central)
Es asimétrico (bajada más rápida que subida)
Puede ser limitado por el proveedor según el plan contratado

2. Internet por Cable
Funcionamiento:

Usa la infraestructura existente de televisión por cable
Combina fibra óptica (central-barrio) y cable coaxial (barrio-casas)
Sistema HFC (Hybrid Fiber Coax)

Componentes:

Cable Head End: Central de la compañía de cable
CMTS: Cable Modem Termination System
Fiber Nodes: Nodos de fibra en el vecindario
Cable Modem: Módem especial en casa

Características:

Red de 500-5000 hogares por junction de vecindario
Medio compartido: Todos comparten el ancho de banda
DOCSIS 2.0: 40 Mbps bajada, 30 Mbps subida
DOCSIS 3.0: 1.2 Gbps bajada, 100 Mbps subida

Limitaciones:

Velocidad variable según usuarios activos simultáneos
Necesita protocolos para evitar colisiones
Performance depende de la actividad del vecindario

3. FTTH (Fiber to the Home)
Concepto:

Fibra óptica directamente hasta la casa
Velocidades de gigabits por segundo
Mayor velocidad disponible actualmente

Arquitecturas:
Direct Fiber:

Una fibra dedicada por casa desde la central

PON (Passive Optical Network):

ONT: Optical Network Terminator (en casa)
Splitter: Combina múltiples casas (<100) en una fibra compartida
OLT: Optical Line Terminator (en la central)
Todos los paquetes del OLT se replican en el splitter

AON (Active Optical Network):

Basado en switched Ethernet

4. 5G Fixed Wireless
Características:

Internet inalámbrico de alta velocidad
No requiere cableado desde la central
Usa tecnología de beam-forming
Módem 5G conectado a router WiFi doméstico
Señal desde estación base del proveedor

Puntos Clave:
Asimetría:

Todas las tecnologías tienen velocidades de bajada > subida
Refleja patrones típicos de uso residencial

Aprovechamiento de infraestructura:

DSL: Red telefónica existente
Cable: Red de TV por cable existente
FTTH: Nueva infraestructura de fibra
5G: Torres celulares

Evolución tecnológica:

DSL → Cable → FTTH → 5G Fixed Wireless
Tendencia hacia mayor velocidad y menor dependencia de cables físicos



8. Analice qué tipo de red es una red de telefonía y qué tipo de red es Internet.

   1. La red telefónica es de tipo conmutación por circuitos ya que cada comunicación (llamadas) reserva un medio de recursos para iniciar y mantener la comunicación hasta que termine.
   2. Internet es de tipo de comuntación de paquetes, ya que cada paquete llega a destino por diferentes caminos(recursos)
9. Describa brevemente las distintas alternativas que conoce para acceder a Internet en su hogar.

  1. DSL: Red telefónica existente

    Cable: Red de TV por cable existente
    FTTH: Nueva infraestructura de fibra
    5G: Torres celulares
    
    Podríamos diferenciar tambien los medios fisicos de conexion: ctwisted-pair copper wire, coaxial cable, fiber optic.
10. ¿Qué ventajas tiene una implementación basada en capas o niveles? 

    Nos permite discutir sobre sobre partes de un sistema grande y  complejo que están bien definidas y estructuradas.

    Esto nos lleva a entender que es un sistema modular sencillo de entender y que permite hacer cambios de implementación fácilmente en servicios de una capa específica sin perjudicar demás capas.  

    Podemos cambiar la implementación interna de un servicio, lo cual no significa que cambie el servicio en si. Mientras el servicio que da una capa a otra no cambie, el sistema funciona correctamente. 

    Por ende en un sistemas grande y complejo que se actualiza constantemente, la habilidad de cambiar la implementación de un servicio sin afectar otros componentes del sistema es un importante ventaja de hacer implementación asada en capas.

    

11. ¿Cómo se llama la PDU de cada una de las siguientes capas: Aplicación, Transporte, Red y Enlace?

    - Aplicación: **Mensaje**

    - Transporte: **Segmento**

    - Red: **Datagrama**

    - Enlace: **Frames**

      ![image-20250905164306194](/home/ignaciomariano/.var/app/io.typora.Typora/config/Typora/typora-user-images/image-20250905164306194.png)

12. ¿Qué es la encapsulación? Si una capa realiza la encapsulación de datos, ¿qué capa del nodo receptor realizará el proceso inverso?

    - Routers y switches son ambos conmutadores de paquetes. No está configurados para implementar todas las capas del stack de internet, sino que implementan las necesarias. Switches implementan solo la de enlace y la fisica, en cambio los routers además están preparados para usar el protocolo IP, por ende implementar la capa de red.
    - La encapsulación se refiere a la manera en que en cada capa (en la ida del origen al destino) se pasa carga el contenido del mensaje con una serie de encabezado que serán de uso para la misma capa del host destino y además por la siguiente capa. basicamente cada capa agrega los encabezados necesarios encapsulando el paquete de la capa anterior dentro de los encanbezados que agrega, siendo así que que se encapsulan los diferentes paquetes de cada capa para que luego en el host destino se termine de desencapsular por capas los paquetes. Algo a dejar en claro que la carga un mensaje queda contenida en el paquete de la siguiente capa. 
    - El concepto de encapsulación se puede hacer complejo si por ejemplo en la trasmisión de mensajes demasiado largos se debe dividir en múltiples paquetes que luego se deben reconstruir en el host destino.

13. Describa cuáles son las funciones de cada una de las capas del stack TCP/IP o protocolo de Internet.

    nternet protocolo stack**:

    - Capa de aplicación
      - Donde residen las aplicaciones web y los protocolos que permiten que se comuniquen.
      - Http: Permite solicitud y respuesta de documentos web
      - SMTP: provee trasmisión de mensajes de correo electrónico
      - FTP: Permite trasmición de archivos entre sistemas finales
      - DNS: Permite abstraer a las personas de las direcciones ip para que se en cambio dominios que es mucho más fácil de comprender y recordad.
    - Capa de transporte: transporta los mensajes de la capa de aplicación a entre los punto de entrada de  aplicación.
      - TCP: Ofrece conexión orientada a servicios. Estos servicios son garantizar que llegan los mensajes al destino, flujo de control al enviar (controlar recibir y enviar a una velocidad acorde entre las partes). También particiona mensaje largos en pequeños segmentos y provee un mecanismo de control de congestión, que permite indicar cuando la red esta congestionada, es decir que los paquetes tardan y se quedan parados en algún punto del camino. 
      - UDP: Provee conexiones sin servicios a las aplicaciones. No provee servicios de confiabilidad (es decir que garanticen recepción al destino), con ofrece un control de flujo y tampoco de control de congestión. 
    - Capa de Red: Mueve los paquetes de la capa de red , es decir datagramas, de un host a otro. La capa de transporte le pasa a la red un segmento y una dirección destino (host destino). 
      - IP: Define los campos de los datagramas y como deben ser usados por los end systems y los routers. 
      - También hay protocolos de ruteo que determinan las rutas que toman los datagramas del host origen  al host destino.
      - Como Internet es una red de redes entonces en cada red se puede configurar un protocolo de ruteo distinto. El admin puede configurar el protocolo que desee. 
    - Capa de Enlace: Dirige un datagrama a través de una serie de routers entre el origen y el destino. Usa los servicios de la capa de enlace para mover un paquete de un nodo a otro (host o router). En particular, en cada nodo se recibe el paquete y se decide cual es el siguiente nodo en la ruta. Una vez que se envió al nodo final antes del host destino, este nodo pasa el datagrama a la capa de red. La capa de enlace usa diferentes protocolos para enviar un frame a otro nodo, la mayoría son protocolos de confianza que hacen una conexión punto a punto entre nodos, a diferencia de la de tcp que si bien hace controles, la realidad es que pasa por varios protocolos y nodos distintos hasta llegar a destino. 
    - Capa Física: Básicamente mueve los bits, que son parte de los frames de la capa de enlace, de un nodo al adyacente. Los protocolos son independientes del link que se use, en particular del medio que se use para compartir los bits (twisted pair copper wire, single-mode fiber optics). Por ejemplo ethernet puede ser cable presando o un cable coaxial, que hacen el mismo trabajo pero trasmiten de diferente manera. 

14. Compare el modelo OSI con la implementación TCP/IP

## Modelo OSI vs TCP/IP

**Estructura de capas:**

- **OSI**: 7 capas (Física, Enlace, Red, Transporte, Sesión, Presentación, Aplicación)
- **TCP/IP**: 4 capas (Acceso a red, Internet, Transporte, Aplicación)

**Origen y propósito:**

- **OSI**: Modelo teórico desarrollado por la ISO como estándar internacional para entender cómo funcionan las redes
- **TCP/IP**: Modelo práctico desarrollado por DARPA, base real de Internet

**Enfoque:**

- **OSI**: Modelo de referencia conceptual, más detallado y específico en la separación de funciones
- **TCP/IP**: Modelo implementado, más simple y orientado a la práctica

**Correspondencia de capas:**

- Las capas de Sesión, Presentación y Aplicación de OSI se combinan en la capa de Aplicación de TCP/IP
- La capa de Red de OSI equivale a la capa de Internet en TCP/IP
- Las capas Física y Enlace de OSI se agrupan en Acceso a red en TCP/IP

**Uso actual:**

- **OSI**: Se usa principalmente para enseñar conceptos de redes y como referencia teórica
- **TCP/IP**: Es el modelo que realmente gobierna Internet y las redes modernas

**Flexibilidad:**

- **OSI**: Más rígido en su estructura
- **TCP/IP**: Más flexible y adaptable a diferentes tecnologías

En resumen, OSI es excelente para entender los conceptos, pero TCP/IP es lo que realmente usamos en la práctica diaria de las redes.