Práctica 5
Capa de Transporte - Parte I
1. ¿Cuál es la función de la capa de transporte?

2. La capa de transporte en el modelo TCP/IP (o equivalente en el enfoque descendente de Kurose) tiene como responsabilidad principal **ofrecer servicios de comunicación de extremo a extremo (host-a-host, proceso a proceso)** entre aplicaciones que residen en los hosts finales. 

   Algunas funciones específicas de la capa de transporte son:

   1. **Multiplexación / Demultiplexación**
      - Recibir datos de múltiples procesos/aplicaciones en el host origen, agregarlos en segmentos con identificadores (puertos) (multiplexación). [PivIT Global+3CSE WUSTL+3University of Cape Town Computer Science+3](https://www.cse.wustl.edu/~jain/cse473-22/ftp/i_3tcpz.pdf?utm_source=chatgpt.com)
      - Entregar los datos recibidos al proceso/aplicación correcta en el host destino (demultiplexación), usando los números de puerto. [University of Cape Town Computer Science+2CSE WUSTL+2](https://www.cs.uct.ac.za/mit_notes/networks/htmls/chp03.html?utm_source=chatgpt.com)
   2. **Segmentación y reensamblado**
      - Dividir (segmentar) los datos de la capa de aplicación en trozos (segmentos) adecuados para transporte. [Medium+3University of Cape Town Computer Science+3CSE WUSTL+3](https://www.cs.uct.ac.za/mit_notes/networks/htmls/chp03.html?utm_source=chatgpt.com)
      - En el extremo receptor, reensamblar los segmentos para reconstruir el flujo de datos original para la aplicación. [University of Crne Gore+3UTSA Pressbooks+3CSE WUSTL+3](https://utsa.pressbooks.pub/networking/chapter/the-transport-layer/?utm_source=chatgpt.com)
   3. **Control de errores, confiabilidad y recuperación**
      - Detectar errores en los segmentos (por ejemplo mediante checksum). [University of Crne Gore+3CSE WUSTL+3UTSA Pressbooks+3](https://www.cse.wustl.edu/~jain/cse473-22/ftp/i_3tcpz.pdf?utm_source=chatgpt.com)
      - En protocolos confiables (ej. TCP), retransmitir segmentos perdidos o corruptos, implementar confirmaciones (ACKs), temporizadores, etc. [Gaia+4CSE WUSTL+4UTSA Pressbooks+4](https://www.cse.wustl.edu/~jain/cse473-22/ftp/i_3tcpz.pdf?utm_source=chatgpt.com)
   4. **Control de flujo**
      - Regular la velocidad de envío para no saturar al receptor (evitar que el receptor sea desbordado). [UTSA Pressbooks+2CSE WUSTL+2](https://utsa.pressbooks.pub/networking/chapter/the-transport-layer/?utm_source=chatgpt.com)
   5. **Control de congestión** (en TCP)
      - Regular el envío de segmentos de manera que no se congestione la red. Este es un aspecto más avanzado que Kurose abarca al tratar TCP en detalle. [University of Cape Town Computer Science+3University of Crne Gore+3CSE WUSTL+3](https://www.ucg.ac.me/skladiste/blog_44233/objava_64433/fajlovi/Computer Networking _ A Top Down Approach%2C 7th%2C converted.pdf?utm_source=chatgpt.com)
   6. **Conexión (establecimiento/terminación) — en protocolos orientados a conexión (TCP)**
      - TCP establece y termina la conexión mediante un protocolo de “handshake” (por ejemplo SYN, SYN-ACK, ACK). [UTSA Pressbooks+3University of Crne Gore+3CSE WUSTL+3](https://www.ucg.ac.me/skladiste/blog_44233/objava_64433/fajlovi/Computer Networking _ A Top Down Approach%2C 7th%2C converted.pdf?utm_source=chatgpt.com)
      - Realizar cierre ordenado de la conexión para asegurar que todos los datos sean entregados. [CSE WUSTL+1](https://www.cse.wustl.edu/~jain/cse473-22/ftp/i_3tcpz.pdf?utm_source=chatgpt.com)

   En resumen, la capa de transporte abstrae detalles de la red subyacente y da una interfaz orientada a procesos finales (aplicaciones) con calidad de servicio definida (fiabilidad, orden, control, etc.).

3. Describa la estructura del segmento TCP y UDP.

   ### Segmento TCP (cabecera + datos)

   La cabecera TCP tiene un tamaño mínimo de 20 bytes, y puede extenderse si hay opciones. [University of Cape Town Computer Science+3Wikipedia+3CSE WUSTL+3](https://en.wikipedia.org/wiki/Transmission_Control_Protocol?utm_source=chatgpt.com)
    Los campos más relevantes son:

   - **Puerto origen (Source Port)**: 16 bits — identifica el puerto de la aplicación de envío. [Wikipedia+1](https://en.wikipedia.org/wiki/Transmission_Control_Protocol?utm_source=chatgpt.com)
   - **Puerto destino (Destination Port)**: 16 bits — identifica el puerto de la aplicación receptora. [Wikipedia+1](https://en.wikipedia.org/wiki/Transmission_Control_Protocol?utm_source=chatgpt.com)
   - **Número de secuencia (Sequence Number)**: 32 bits — indica la posición de los datos (bytes) en el flujo de datos. [Wikipedia+2CSE WUSTL+2](https://en.wikipedia.org/wiki/Transmission_Control_Protocol?utm_source=chatgpt.com)
   - **Número de acuse/confirmación (Acknowledgment Number)**: 32 bits — en segmentos con ACK, indica el próximo número de secuencia esperado. [Wikipedia+2CSE WUSTL+2](https://en.wikipedia.org/wiki/Transmission_Control_Protocol?utm_source=chatgpt.com)
   - **Offset de datos / Data Offset**: 4 bits — indica cuántas “palabras” de 32 bits (i.e. cuántas unidades de 4 bytes) ocupa la cabecera TCP (incluyendo opciones). [Wikipedia+2University of Crne Gore+2](https://en.wikipedia.org/wiki/Transmission_Control_Protocol?utm_source=chatgpt.com)
   - **Reservado (Reserved)**: 3 bits (o más) — usualmente cero, para uso futuro. [Wikipedia+1](https://en.wikipedia.org/wiki/Transmission_Control_Protocol?utm_source=chatgpt.com)
   - **Flags / Bits de control (Control Bits / Flags)**: 9 bits típicos (URG, ACK, PSH, RST, SYN, FIN, etc.). [University of Cape Town Computer Science+3Wikipedia+3CSE WUSTL+3](https://en.wikipedia.org/wiki/Transmission_Control_Protocol?utm_source=chatgpt.com)
   - **Ventana (Window Size)**: 16 bits — cantidad máxima de datos (bytes) que el receptor puede aceptar sin confirmar (para control de flujo). [University of Crne Gore+3Wikipedia+3CSE WUSTL+3](https://en.wikipedia.org/wiki/Transmission_Control_Protocol?utm_source=chatgpt.com)
   - **Checksum**: 16 bits — para verificación de errores de cabecera + datos. [University of Cape Town Computer Science+3Wikipedia+3CSE WUSTL+3](https://en.wikipedia.org/wiki/Transmission_Control_Protocol?utm_source=chatgpt.com)
   - **Puntero urgente (Urgent Pointer)**: 16 bits — si se usa la bandera URG, señala el límite de datos urgentes. [Wikipedia+1](https://en.wikipedia.org/wiki/Transmission_Control_Protocol?utm_source=chatgpt.com)
   - **Opciones (Options, variable longitud)**: puede incluir varias opciones (por ejemplo, TCP timestamp, SACK, máximo tamaño de segmento, ventana escalada). [Wikipedia+2CSE WUSTL+2](https://en.wikipedia.org/wiki/Transmission_Control_Protocol?utm_source=chatgpt.com)
   - **Relleno (Padding)**: para que la cabecera termine en un múltiplo de 32 bits. [Wikipedia+1](https://en.wikipedia.org/wiki/Transmission_Control_Protocol?utm_source=chatgpt.com)
   - **Datos / Payload**: los datos transportados por TCP (parte de la capa de aplicación) siguen después de la cabecera.

   **Flujo de datos**: TCP trata el transporte como un flujo de bytes (stream), no como “mensajes”. [University of Crne Gore+2University of Cape Town Computer Science+2](https://www.ucg.ac.me/skladiste/blog_44233/objava_64433/fajlovi/Computer Networking _ A Top Down Approach%2C 7th%2C converted.pdf?utm_source=chatgpt.com)

   ### Segmento UDP (datagrama UDP)

   La estructura de la cabecera UDP es mucho más simple y fija, ocupando 8 bytes. [University of Cape Town Computer Science+3Wikipedia+3CSE WUSTL+3](https://en.wikipedia.org/wiki/User_Datagram_Protocol?utm_source=chatgpt.com)

   Los campos son:

   - **Puerto origen (Source Port)**: 16 bits — opcionalmente 0 si no usa puerto origen identificable. [Wikipedia+1](https://en.wikipedia.org/wiki/User_Datagram_Protocol?utm_source=chatgpt.com)
   - **Puerto destino (Destination Port)**: 16 bits — puerto de la aplicación destino. [Wikipedia+1](https://en.wikipedia.org/wiki/User_Datagram_Protocol?utm_source=chatgpt.com)
   - **Longitud (Length)**: 16 bits — longitud total del datagrama UDP (cabecera + datos). [Wikipedia+2University of Crne Gore+2](https://en.wikipedia.org/wiki/User_Datagram_Protocol?utm_source=chatgpt.com)
   - **Checksum**: 16 bits — para detectar errores en cabecera + datos (se puede usar opcionalmente, depende de implementación). [Wikipedia+2University of Crne Gore+2](https://en.wikipedia.org/wiki/User_Datagram_Protocol?utm_source=chatgpt.com)
   - **Datos / Payload**: el contenido de la aplicación (mensaje) que UDP transporta.

   Principales diferencias con TCP:

   - UDP es *no orientado a conexión* (connectionless): no establece conexión antes del envío ni mantiene estado de conexión. [Wikipedia+2University of Crne Gore+2](https://en.wikipedia.org/wiki/User_Datagram_Protocol?utm_source=chatgpt.com)
   - No ofrece mecanismos de retransmisión, control de flujo, ordenamiento, confirmaciones, etc. Lo asume “mejor esfuerzo”. [University of Crne Gore+3Wikipedia+3CSE WUSTL+3](https://en.wikipedia.org/wiki/User_Datagram_Protocol?utm_source=chatgpt.com)
   - Cabecera fija de 8 bytes, sin opciones complejas. [Wikipedia+2](https://en.wikipedia.org/wiki/User_Datagram_Protocol?utm_source=chatgpt.com)

4. ¿Cuál es el objetivo del uso de puertos en el modelo TCP/IP?

   Los números de **puerto** juegan un rol crucial para lograr la multiplexación/demultiplexación **proceso a proceso** dentro de los hosts finales. Es decir, permiten que múltiples aplicaciones en la misma máquina compartan la red (la misma dirección IP) sin confundirse entre sí. [Gaia+5University of Crne Gore+5CSE WUSTL+5](https://www.ucg.ac.me/skladiste/blog_44233/objava_64433/fajlovi/Computer Networking _ A Top Down Approach%2C 7th%2C converted.pdf?utm_source=chatgpt.com)

   Algunos puntos clave del uso de puertos:

   - Un puerto identifica un **servicio o aplicación específica** en el host destino (o de origen). Por ejemplo, HTTP en el puerto 80, DNS en el puerto 53, etc. [Gaia+4University of Crne Gore+4University of Cape Town Computer Science+4](https://www.ucg.ac.me/skladiste/blog_44233/objava_64433/fajlovi/Computer Networking _ A Top Down Approach%2C 7th%2C converted.pdf?utm_source=chatgpt.com)
   - Junto con la dirección IP del host, los puertos permiten constituir una *tupla* (IP origen + puerto origen, IP destino + puerto destino) que identifica un “socket” o “conexión lógica” entre procesos finales. [UTSA Pressbooks+4Wikipedia+4CSE WUSTL+4](https://en.wikipedia.org/wiki/Transmission_Control_Protocol?utm_source=chatgpt.com)
   - Gracias a los puertos, la capa de transporte puede **demultiplexar** el tráfico entrante: cuando un segmento TCP o UDP llega a un host, su puerto destino le dice a cuál proceso/aplicación entregar esos datos. [Medium+3University of Cape Town Computer Science+3University of Crne Gore+3](https://www.cs.uct.ac.za/mit_notes/networks/htmls/chp03.html?utm_source=chatgpt.com)
   - También permite **multiplexar** múltiples flujos de diferentes aplicaciones hacia la red, de modo que todos viajen por la misma interfaz física/ip, pero sean distinguidos mediante puertos. [Medium+3CSE WUSTL+3University of Crne Gore+3](https://www.cse.wustl.edu/~jain/cse473-22/ftp/i_3tcpz.pdf?utm_source=chatgpt.com)
   - Facilita el establecimiento simultáneo de múltiples conexiones o sesiones desde un mismo host hacia distintos destinos o incluso al mismo destino (con distinto puerto origen). [University of Cape Town Computer Science+3Wikipedia+3CSE WUSTL+3](https://en.wikipedia.org/wiki/Transmission_Control_Protocol?utm_source=chatgpt.com)

   En suma: **los puertos permiten que la capa de transporte ofrezca servicios proceso a proceso**, discriminando tráfico entre distintas aplicaciones, pese a que compartan la misma infraestructura de red.

5. Compare TCP y UDP en cuanto a:

6. **a. Confiabilidad**

   - **TCP:** Proporciona confiabilidad. Usa ACKs, retransmisiones, números de secuencia y reordenamiento.
   - **UDP:** No es confiable. No retransmite, no confirma ni ordena.

   **b. Multiplexación / Demultiplexación**

   - **TCP:** Usa puertos origen y destino, igual que UDP, pero cada conexión se identifica con una *cuádrupla* (IP origen, puerto origen, IP destino, puerto destino).
   - **UDP:** También usa puertos para multiplexar, pero no mantiene estado de conexión ni identifica flujos con una cuádrupla persistente.

   **c. Orientación a la conexión**

   - **TCP:** Orientado a conexión. Necesita handshake (SYN, SYN-ACK, ACK) antes de enviar datos.
   - **UDP:** No orientado a conexión. Envía directamente sin establecer estado previo.

   **d. Control de congestión**

   - **TCP:** Sí implementa control de congestión (ej. slow start, AIMD, etc.).
   - **UDP:** No tiene control de congestión. Envía lo que le manden sin autorregulación.

   **e. Uso de puertos**

   - **TCP:** Usa puertos para identificar procesos y mantener conexiones únicas mediante tuplas de 4 elementos.
   - **UDP:** Usa puertos únicamente para multiplexar/demultiplexar mensajes, sin establecer conexión ni estado.

7. La PDU de la capa de transporte es el segmento. Sin embargo, en algunos contextos
  suele utilizarse el término datagrama. Indique cuando.

  Entiendo que en udp se suele usar datagrama. Se usa principalmente en discusiones para diferentciar los protocolos y porque udp es un protocolo que no hace casi nada, por lo que es casi nula su presencia y es un transporte liviano que casi no tiene presencia entre la capa de aplicacion y la de red (protocolo ip)

8. Describa el saludo de tres vías de TCP. ¿UDP tiene esta característica?

9. Investigue qué es el ISN (Initial Sequence Number). Relaciónelo con el saludo de tres
  vías.

10. Investigue qué es el MSS. ¿Cuándo y cómo se negocia?

11. Utilice el comando ss (reemplazo de netstat) para obtener la siguiente información de su
   PC:
   a. Para listar las comunicaciones TCP establecidas.
   b. Para listar las comunicaciones UDP establecidas.
   c. Obtener sólo los servicios TCP que están esperando comunicaciones
   d. Obtener sólo los servicios UDP que están esperando comunicaciones.
   e. Repetir los anteriores para visualizar el proceso del sistema asociado a la conexión.
   f. Obtenga la misma información planteada en los items anteriores usando el comando
   netstat.

12. ¿Qué sucede si llega un segmento TCP con el flag SYN activo a un host que no tiene
    ningún proceso esperando en el puerto destino de dicho segmento (es decir, el puerto
    destino no está en estado LISTEN)?
    a. Utilice hping3 para enviar paquetes TCP al puerto destino 22 de la máquina virtual
    con el flag SYN activado.
    b. Utilice hping3 para enviar paquetes TCP al puerto destino 40 de la máquina virtual
    con el flag SYN activado.
    c. ¿Qué diferencias nota en las respuestas obtenidas en los dos casos anteriores?
    ¿Puede explicar a qué se debe? (Ayuda: utilice el comando ss visto anteriormente).
    1Redes y Comunicaciones

LINTI - UNLP
11. ¿Qué sucede si llega un datagrama UDP a un host que no tiene ningún proceso
esperando en el puerto destino de dicho datagrama (es decir, que dicho puerto no está en
estado LISTEN)?
a. Utilice hping3 para enviar datagramas UDP al puerto destino 5353 de la máquina
virtual.
b. Utilice hping3 para enviar datagramas UDP al puerto destino 40 de la máquina
virtual.
c. ¿Qué diferencias nota en las respuestas obtenidas en los dos casos anteriores?
¿Puede explicar a qué se debe? (Ayuda: utilice el comando ss visto anteriormente).
12. Investigue los distintos tipos de estado que puede tener una conexión TCP.
Ver
https://users.cs.northwestern.edu/~agupta/cs340/project2/TCPIP_State_Transition_Diagram
.pdf
13. Dada la siguiente salida del comando ss, responda:
    ![image-20251010164821622](/home/ignaciomariano/.var/app/io.typora.Typora/config/Typora/typora-user-images/image-20251010164821622.png)
    a. ¿Cuántas conexiones hay establecidas?
    b. ¿Cuántos puertos hay abiertos a la espera de posibles nuevas conexiones?
    c. El cliente y el servidor de las comunicaciones HTTPS (puerto 443), ¿residen en la
    misma máquina?
    d. El cliente y el servidor de la comunicación SSH (puerto 22), ¿residen en la misma
    máquina?
    2Redes y Comunicaciones

LINTI - UNLP
e. Liste los nombres de todos los procesos asociados con cada comunicación. Indique
para cada uno si se trata de un proceso cliente o uno servidor.
f. ¿Cuáles conexiones tuvieron el cierre iniciado por el host local y cuáles por el
remoto?
g. ¿Cuántas conexiones están aún pendientes por establecerse?
14. Dadas las salidas de los siguientes comandos ejecutados en el cliente y el servidor,
    responder:
    ![image-20251010164737239](/home/ignaciomariano/.var/app/io.typora.Typora/config/Typora/typora-user-images/image-20251010164737239.png)
    a. ¿Qué segmentos llegaron y cuáles se están perdiendo en la red?
    b. ¿A qué protocolo de capa de aplicación y de transporte se está intentando conectar el
    cliente?
    c. ¿Qué flags tendría seteado el segmento perdido?

15. Use CORE para armar una topología como la siguiente, sobre la cual deberá realizar:
    a. En ambos equipos inspeccionar el estado de las conexiones y mantener abiertas
    ambas ventanas con el comando corriendo para poder visualizar los cambios a
    medida que se realiza el ejercicio.
    Ayuda: watch -n1 ’ss -nat’.
    b. En Servidor, utilice la herramienta ncat para levantar un servicio que escuche en el
    puerto 8001/TCP. Utilice la opción -k para que el servicio sea persistente. Verifique el
    estado de las conexiones.
    ![image-20251010164704603](/home/ignaciomariano/.var/app/io.typora.Typora/config/Typora/typora-user-images/image-20251010164704603.png)
    c. Desde CLIENTE1 conectarse a dicho servicio utilizando también la herramienta
    ncat. Inspeccione el estado de las conexiones.
    d. Iniciar otra conexión desde CLIENTE1 de la misma manera que la anterior y verificar
    el estado de las conexiones. ¿De qué manera puede identificar cada conexión?
    e. En base a lo observado en el item anterior, ¿es posible iniciar más de una conexión
    desde el cliente al servidor en el mismo puerto destino? ¿Por qué? ¿Cómo se
    garantiza que los datos de una conexión no se mezclarán con los de la otra?
    f. Analice en el tráfico de red, los flags de los segmentos TCP que ocurren cuando:
    i. Cierra la última conexión establecida desde CLIENTE1. Evalúe los estados de las
    conexiones en ambos equipos.
    ii. Corta el servicio de ncat en el servidor (Ctrl+C). Evalúe los estados de las
    conexiones en ambos equipos.
    iii. Cierra la conexión en el cliente. Evalúe nuevamente los estados de las
    conexiones.