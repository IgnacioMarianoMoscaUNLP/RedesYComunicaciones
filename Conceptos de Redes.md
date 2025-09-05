### **Conceptos de Redes**

**<u>Store and Foward Transmission</u>:** Basicamente trata de como los conmutadores de paquetes (sea router, switches, etc) trasmiten los paquetes. **Reciben el paquete en su totalidad antes de retrasmitirlo al link de salida**.  Podemos pensar que un parquete tiene **L** bits totales, y que la tasa de trasmision de un link es de **R bits/sec**, es decir que el tiempo de trasmision de un paquete será **L/R segundos**. Un ejemplo sería pensar en un router que trasmite de A a B (end systems) y que el delay de trasmisión será 2L/R si hace store and foward, pero sino será L/R si puede enviar sin necesidad de tener el paquete completo. Esto en un entorno en el que las condiciones permitan que no haya otro tipo de delay (congestion de red por ejemplo). ejemplo de enviar tres paquetes, será un total de 4L/R enviar de A-router-B ya que en 2L/R llego el primer paquete a B (destino) y llegó paquete 2 a router, en 3L/R llegó paquet 2 a B y llego paquete 3 a router, y en 4L/R llego el paquete 3 a B. 

Podemos generalizar esto a un caso en el que un paquete se envia de un origen a un destino y que pasa por un camino de **N** links de un tiempo **R** de retrasmisión.

 - d(end to end) = N*L/R
 - Con esto podemos razonar por ejemplo cuanto de delay habría en enviar **P** paquetes en una serie de N links

## 📘 Resumen – *Delay, Loss, and Throughput in Packet-Switched Networks*

### 1.4.1 Tipos de retraso (review rápido)

- **Retraso de procesamiento (dproc):** verificar errores de bit, consultar tablas de reenvío. Normalmente muy pequeño, pero limita el **throughput máximo de un router**.
- **Retraso de transmisión (dtrans = L/R):** tiempo en “empujar” todos los bits de un paquete al enlace. Puede ser insignificante (LANs) o muy grande (modems).
- **Retraso de propagación (dprop):** depende de la distancia y la velocidad de propagación de la señal en el medio (~velocidad de la luz).
- **Retraso de cola (dqueue):** tiempo en espera en la cola antes de ser transmitido. Muy variable → depende de la carga.

------

### 1.4.2 Retraso de cola y pérdida de paquetes

🔹 **Retraso de cola (dqueue):**

- Es el más **complejo y variable** → depende del tráfico y capacidad.
- Definición clave:
  - Tasa de llegada de paquetes: **a** (packets/seg).
  - Longitud del paquete: **L** bits.
  - Tasa de transmisión del enlace: **R** (bits/seg).
  - **Intensidad de tráfico:** ρ = La / R.
- **Casos:**
  - Si ρ > 1 → la cola crece sin límite → *delay tiende a ∞*.
  - Si ρ ≤ 1 → el retraso depende de la naturaleza del tráfico:
    - Llegadas periódicas uniformes → casi sin cola.
    - Llegadas en ráfagas → mayor retraso promedio.
- **Fenómeno clave:** cuando ρ → 1, un pequeño aumento en tráfico produce un gran aumento en retraso (igual que en un embotellamiento de autos 🚗).

🔹 **Pérdida de paquetes:**

- En la práctica, las colas tienen **capacidad finita**.
- Si llega un paquete y la cola está llena → **se descarta** (loss).
- Probabilidad de pérdida ↑ a medida que ρ se acerca a 1.
- Los protocolos de transporte (ej. TCP) pueden **retransmitir** los paquetes perdidos.

------

### 1.4.3 Retraso extremo a extremo (end-to-end delay)

Si entre origen y destino hay N–1 routers, con colas vacías y parámetros homogéneos:
$$
d_{end-to-end} = N \cdot (d_{proc} + d_{trans} + d_{prop})
$$
Donde:

- $d_{trans} = \frac{L}{R}$.

🔹 **Traceroute**: herramienta práctica para medir rutas y delays:

- Envía paquetes especiales con TTL crecientes.
- Cada router devuelve una respuesta → se mide el **RTT (round-trip time)** hasta cada salto.
- Permite ver rutas, retrasos y dónde aparecen demoras (ejemplo: salto transatlántico con gran dprop).

------

### 1.4.4 Throughput (rendimiento de red)

- **Definición:** tasa efectiva de transferencia de datos extremo a extremo.
  - **Instantáneo:** velocidad en un instante dado.
  - **Promedio:** $\text{Throughput} = \frac{F}{T}$ (F = bits totales, T = tiempo total).

🔹 **Casos básicos:**

- En un camino con 2 enlaces (server → router → client):
  $$
  \text{Throughput} = \min(R_s, R_c)
  $$
  donde Rs = tasa servidor→router, Rc = router→cliente.
   → El **cuello de botella** (bottleneck link) limita el throughput.

- Con N enlaces:
  $$
  \text{Throughput} = \min(R_1, R_2, ..., R_N)
  $$

- En la **Internet moderna**, los enlaces del core son de alta capacidad → el **acceso** (Rs o Rc) suele ser el cuello de botella.

- Con múltiples descargas que comparten un enlace de capacidad R:

  - Si hay M flujos, cada uno recibe ≈ R/M (si comparten de manera justa).

📌 Ejemplo:

- F = 32 Mbits, Rs = 2 Mbps, Rc = 1 Mbps → throughput = 1 Mbps → tiempo = 32 seg.
- Si 10 descargas comparten un enlace de 5 Mbps → cada una recibe 0.5 Mbps.

------

## 🔑 Puntos clave

1. **Delay total** = suma de procesamiento + transmisión + propagación + cola.
2. **Retraso de cola** depende de la intensidad de tráfico ρ = La/R.
3. Si ρ > 1 → colapso (colas infinitas, pérdida masiva).
4. **Packet loss** ocurre porque las colas son finitas.
5. **End-to-end delay** se calcula sumando los delays en cada nodo.
6. **Throughput** lo determina el **enlace más lento** o los flujos que comparten un cuello de botella.



**<u>Queuing Delays and Packet Loss</u>**

- Revisar pagina 35 del pdf del libro de Kurose, es importante para entender trasmision de paquetes en el camino de un end system a otro.

**<u>Forwarding tables  and Routing Protocols</u>**

- Pagina 36 del pdf

### **<u>1.5 Protocol layering</u>**

<u>**Protocol layering**</u>

Cada protocolo pertenece a una de las capas. En cada capa suceden servicios que se ofrecen a la siguiente capa (Modelo de servicio), y también estos servicios se proveen de servicios de las capas adyacentes. 

Los protocolos de capa pueden ser implementados vía software, hardware o una combinación de ambos. 

Los protocolos de la capa de aplicación son todos hechos en software en los end systems, por eso les decimos Protocolos de capa de transporte.

La capa física y la capa de conexión de datos son reponsables de conmutar (recibir y retrasmitir paquetes) paquetes a puntos concretos, son implementados tipicamente en placas de interface de red y se asocian con puntos de conexión concretos. 

La capa de red combina soft and hard. 

Podemos entender que hay parte de cada capa, protocolos de diferentes capas en cada end system de la red. 



Algunas desventajas o críticas a la estructura en capas es que por ejemplo varias capas tienen un sistema de recuperación de errores, por ende se duplicado la funcionalidad. También sucede que protocolos de una capa necesitan datos que provee, o recopilan protocolos de otras capas, haciendo que se pierda la encapsulación entre capas.

Podemos llamar **Stack Protocolo** al conjunto de protocolos de diferentes capas que se unen para trabajar juntos. 

**VER EN P1  DESARROLLO DE FUNCIÓN DE CADA CAPA**

#### **<u>Redes bajo ataque</u>**

El capítulo introduce el tema de **seguridad en redes**, explicando que la Internet, esencial para individuos e instituciones, también es un espacio vulnerable a múltiples amenazas. Se presentan los ataques más comunes:

1. **Malware**
   - Programas maliciosos que ingresan a un host a través de Internet.
   - Pueden borrar archivos, robar información privada (contraseñas, números, datos) o convertir al dispositivo en parte de un **botnet**.
   - Muchos son **autorreplicantes**, propagándose rápidamente entre equipos.
2. **Ataques de Denegación de Servicio (DoS/DDoS)**
   - Buscan inutilizar un servidor o red legítima.
   - Tres formas principales:
     - **Vulnerability attack**: explotar fallos del sistema con mensajes específicos.
     - **Bandwidth flooding**: saturar el ancho de banda con un gran volumen de tráfico.
     - **Connection flooding**: abrir numerosas conexiones falsas que bloquean las legítimas.
   - Los **ataques distribuidos (DDoS)**, con miles de hosts comprometidos, son especialmente dañinos.
3. **Sniffing de paquetes**
   - Un atacante puede copiar todo el tráfico en una red (sobre todo en entornos inalámbricos o LANs de difusión).
   - Permite capturar contraseñas, datos sensibles y comunicaciones privadas.
   - Los sniffers son difíciles de detectar porque son **pasivos**.
   - La defensa más eficaz: **cifrado**.
4. **Suplantación de identidad (IP spoofing)**
   - Consiste en enviar paquetes falsificados con direcciones o contenidos inventados, haciéndose pasar por otro usuario o dispositivo.
   - Puede engañar a routers, servidores u otros hosts.
   - La solución requiere **autenticación de extremo a extremo**.

## 📘 Resumen – Historia de las Redes de Computadoras y de Internet

### 1.7.1 Desarrollo de la conmutación de paquetes (1961–1972)

- En los 60 predominaba la **red telefónica conmutada por circuitos**.
- Se investigó la **conmutación de paquetes** como alternativa para tráfico “ráfaga”.
- Pioneros: Leonard Kleinrock (MIT), Paul Baran (Rand), Donald Davies (NPL, Inglaterra).
- **ARPAnet** nace en 1969 con 4 nodos (UCLA, SRI, UC Santa Barbara, Univ. Utah).
- Primer protocolo: **NCP (Network Control Protocol)**.
- En 1972 se crea el primer programa de correo electrónico.

### 1.7.2 Redes propietarias e interconexión (1972–1980)

- Surgen más redes: ALOHANet, packet-satellite, packet-radio, Telenet, Cyclades, Tymnet, IBM SNA.
- Se plantea la necesidad de **interconectar redes** → surge el concepto de **“internetting”**.
- Cerf y Kahn diseñan la **arquitectura TCP** (transportar datos de extremo a extremo).
- De TCP se separa **IP** y aparece también **UDP**.
- A fines de los 70 ya estaban definidos los tres protocolos clave: **TCP, UDP e IP**.
- También nace **Ethernet** (Metcalfe y Boggs, basado en ALOHAnet).

### 1.7.3 Proliferación de redes (1980–1990)

- ARPAnet tenía ~200 hosts (1979), y a fines de los 80 ya había >100.000 en la Internet pública.
- Redes académicas: **BITNET, CSNET, NSFNET** (esta última con backbone de 56 kbps → 1.5 Mbps).
- **1983:** ARPAnet adopta oficialmente **TCP/IP** (reemplazando NCP).
- Década clave:
  - **DNS** (1984, RFC 1034).
  - Control de congestión en TCP (Jacobson, 1988).
- En paralelo, Francia lanza **Minitel** (basado en X.25), con servicios online en hogares mucho antes que el Internet comercial en EE. UU.

### 1.7.4 Explosión de Internet (1990s)

- ARPAnet desaparece; **NSFNET permite uso comercial (1991)** y luego es reemplazada por ISPs (1995).
- **World Wide Web** (Tim Berners-Lee, 1989–1991): HTML, HTTP, servidor y navegador.
- Aparición de navegadores gráficos (Mosaic → Netscape).
- Empresas comienzan a usar la Web para comercio.
- “Killer apps” de los 90:
  - E-mail
  - Web / e-commerce
  - Mensajería instantánea
  - P2P de música (Napster).
- Boom y caída de las **punto-com (1995–2001)**.

### 1.7.5 El nuevo milenio

- Internet + **smartphones** transforman la sociedad.
- Avances clave:
  - **Banda ancha masiva** (DSL, cable, fibra, 5G).
  - **Internet móvil** y apps basadas en ubicación.
  - Redes sociales (Facebook, Twitter, WeChat, etc.).
  - Proveedores de servicios online crean **redes privadas globales** (Google, Microsoft).
  - Migración de servicios a la **nube** (AWS, Azure, Alibaba Cloud).