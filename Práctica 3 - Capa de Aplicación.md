Práctica 3 - Capa de Aplicación
DNS (Domain Name Server)

------

**1. Funcionamiento del DNS y su objetivo**
 El **DNS (Domain Name System)** es un sistema jerárquico y distribuido que traduce los **nombres de dominio** (ejemplo: `www.google.com`) en **direcciones IP** (ejemplo: `142.250.190.78`), que son las que realmente utilizan los equipos para comunicarse en la red.
 👉 **Objetivo:** facilitar la navegación en Internet permitiendo que las personas usen nombres fáciles de recordar en lugar de números difíciles de manejar.

------

**2. Root server y gTLD**

- **Root server:** son los **servidores raíz del DNS**. Existen 13 "identidades" (letras de la A a la M), distribuidas mundialmente mediante *anycast*. Son el punto más alto de la jerarquía del DNS y saben cómo dirigir las consultas hacia los servidores de dominios de nivel superior.
- **gTLD (generic Top-Level Domain):** son los **dominios genéricos de nivel superior**, como `.com`, `.org`, `.net`, `.edu`, etc. Se diferencian de los **ccTLD (country code TLD)** que son específicos de países (ej: `.ar`, `.es`, `.fr`).

------

**3. Respuesta autoritativa**
 Una **respuesta autoritativa** es la que proviene directamente del **servidor DNS responsable de la zona** (dueño de los registros). No es información cacheada ni intermedia, sino oficial.

------

**4. Diferencia entre consulta recursiva e iterativa**

- **Consulta recursiva:** el **resolver** (servidor DNS del usuario, normalmente el del ISP) se encarga de buscar toda la respuesta por el cliente. El cliente solo recibe la respuesta final.
- **Consulta iterativa:** el servidor DNS contactado devuelve la **mejor referencia que conoce** (ej: "no sé la IP, pero pregunta en estos servidores"). El cliente (o el resolver) sigue consultando paso a paso.

------

**5. Resolver**
 El **resolver** (también llamado *stub resolver* en el cliente o *recursor* en el ISP) es el componente que recibe las consultas DNS de las aplicaciones y se encarga de resolverlas, ya sea preguntando a los servidores DNS necesarios o consultando su caché.

------

**6. Tipos de registros DNS**
 a. **A** → Asocia un nombre de dominio con una **dirección IPv4**.
 b. **MX** → Indica los **servidores de correo** de un dominio.
 c. **PTR** → Hace la resolución inversa: de una dirección IP a un nombre de dominio.
 d. **AAAA** → Asocia un nombre de dominio con una **dirección IPv6**.
 e. **SRV** → Especifica la ubicación de servicios específicos (puerto, protocolo, host). Ejemplo: servidores de SIP o LDAP.
 f. **NS** → Indica los **servidores de nombres** autorizados para un dominio.
 g. **CNAME** → Crea un **alias** de un dominio apuntando a otro.
 h. **SOA (Start of Authority)** → Define la información principal de la zona: servidor maestro, contacto del admin, número de serie, tiempos de refresco, etc.
 i. **TXT** → Guarda información en texto. Se usa, por ejemplo, para SPF, DKIM y verificación de dominio.

------

**7. Múltiples servidores DNS para un dominio**
 Un dominio suele tener más de un servidor DNS para:

- **Redundancia:** si uno falla, otro responde.
- **Disponibilidad:** mejorar confiabilidad.
- **Balanceo geográfico:** responder desde ubicaciones cercanas y rápidas para los usuarios.

------

**8. Servidor primario y secundarios**

- El **primario (maestro)** almacena la **copia original** de la zona.
- Los **secundarios (esclavos)** mantienen **copias sincronizadas**.
   👉 Razón: seguridad, disponibilidad y distribuir la carga. Si el primario cae, los secundarios pueden seguir respondiendo.

------

**9. Transferencia de zona**
 Es el mecanismo mediante el cual un **servidor secundario** obtiene una **copia exacta de la zona** desde el primario.
 👉 **Finalidad:** mantener sincronizados todos los servidores que gestionan el mismo dominio, garantizando consistencia.



1. Imagine que usted es el administrador del dominio de DNS de la UNLP (unlp.edu.ar). A
     su vez, cada facultad de la UNLP cuenta con un administrador que gestiona su propio
     dominio (por ejemplo, en el caso de la Facultad de Informática se trata de info.unlp.edu.ar).
     Suponga que se crea una nueva facultad, Facultad de Redes, cuyo dominio será
     redes.unlp.edu.ar, y el administrador le indica que quiere poder manejar su propio dominio.
     ¿Qué debe hacer usted para que el administrador de la Facultad de Redes pueda gestionar el dominio de forma independiente? (Pista: investigue en qué consiste la delegación de dominios). Indicar qué registros de DNS se deberían agregar.

     La **delegación de dominios** en DNS consiste en que un dominio superior (en este caso `unlp.edu.ar`) indica en sus registros que un subdominio (`redes.unlp.edu.ar`) será gestionado por otros servidores DNS específicos.

     ```sql
     redes.unlp.edu.ar.   IN NS   ns1.redes.unlp.edu.ar.
     redes.unlp.edu.ar.   IN NS   ns2.redes.unlp.edu.ar.
     ns1.redes.unlp.edu.ar.   IN A    192.0.2.10
     ns2.redes.unlp.edu.ar.   IN A    192.0.2.11
     
     ```

     Esto se hace con **registros NS** en la zona de `unlp.edu.ar`.

     ###### En el servidor DNS de `redes.unlp.edu.ar` (mantenido por la facultad)

     El administrador de Redes deberá configurar su **propio archivo de zona**, con:

     - Registro **SOA** (Start of Authority) para la zona `redes.unlp.edu.ar`.

     - Registros **NS** que apunten a sus servidores (`ns1.redes.unlp.edu.ar`, `ns2.redes.unlp.edu.ar`).

     - Sus propios **A, AAAA, MX, CNAME, etc.**, según lo que necesite (por ejemplo `www.redes.unlp.edu.ar`, `mail.redes.unlp.edu.ar`, etc.).

     - ```sql
       $ORIGIN redes.unlp.edu.ar.
       @       IN SOA   ns1.redes.unlp.edu.ar. admin.redes.unlp.edu.ar. (
                      2025092301 ; serial
                      3600       ; refresh
                      1800       ; retry
                      1209600    ; expire
                      86400 )    ; minimum TTL
               IN NS    ns1.redes.unlp.edu.ar.
               IN NS    ns2.redes.unlp.edu.ar.
       
       ns1     IN A     192.0.2.10
       ns2     IN A     192.0.2.11
       www     IN A     192.0.2.20
       
       ```

       

2. Responda y justifique los siguientes ejercicios.
     a. En la VM, utilice el comando dig para obtener la dirección IP del host
     www.redes.unlp.edu.ar y responda:
     i. ¿La solicitud fue recursiva? ¿Y la respuesta? ¿Cómo lo sabe?

     Si fue recursiva la solicitud (recursion desired)

     ii. ¿Puede indicar si se trata de una respuesta autoritativa? ¿Qué
     significa que lo sea?

     si por el registro aa que indica que el servidor que realizó la respuiesta es autoritativo.

     iii. ¿Cuál es la dirección IP del resolver utilizado? ¿Cómo lo sabe?

     se ve en la seccion de respuesta en el registro de server ip y que usó el puerto 53.

     b. ¿Cuáles son los servidores de correo del dominio redes.unlp.edu.ar? ¿Por qué hay más de uno y qué significan los números que aparecen entre MX y el nombre? Si se quiere enviar un correo destinado a redes.unlp.edu.ar, ¿a qué servidor se le entregará? ¿En qué situación se le entregará al otro?

     Con dig dominio MX nos da los servidores de correo. Los numeros indican la prioridad, menor numero indica mayor prioridad, es decir que se consulta primero al servidor de mayor prioridad. Sirve para distribuir cargas y tener disponibilidad.

     c. ¿Cuáles son los servidores de DNS del dominio redes.unlp.edu.ar?

     dig dominio NS y obtenemos ns-sv-a.redes.unlp.edu.ar y ns-sv-b….

     d. Repita la consulta anterior cuatro veces más. ¿Qué observa? ¿Puede
     explicar a qué se debe?

     Por un lado podemos ver el id que va cambiando,  que sirve para identificar respuesta de una consulta concreta. 

     Segundo vemos que se usa el mecanismo de cookies para tener mayor seguridad

     e. Observe la información que obtuvo al consultar por los servidores de DNS del dominio. En base a la salida, ¿es posible indicar cuál de ellos es el primario?

     NO uso SOA para eso

     f. Consulte por el registro SOA del dominio y responda.

     Start of Authority- comienxo de autoridad de esa zona

     i. ¿Puede ahora determinar cuál es el servidor de DNS primario?

     Si ns-sv-b….

     ii. ¿Cuál es el número de serie, qué convención sigue y en qué casos es importante actualizarlo?

     

     iii. ¿Qué valor tiene el segundo campo del registro? Investigue para qué
     se usa y cómo se interpreta el valor.
     iv. ¿Qué valor tiene el TTL de caché negativa y qué significa?

     ```sql
     dominio.  IN SOA  ns1.dominio. admin.dominio. (
                      serial
                      refresh
                      retry
                      expire
                      minimum )
     
     ```

     **Servidor primario (ns1.dominio.)** → el nombre del *DNS primario* de la zona.

     **Correo del administrador (admin.dominio.)** → email del responsable (se reemplaza `.` por `@`).

     **Serial** → número de serie de la zona. Se incrementa cuando se hacen cambios, para que los secundarios actualicen.

     **Refresh** → intervalo en segundos en que los servidores secundarios consultan al primario para ver si hay cambios.

     **Retry** → tiempo de espera antes de reintentar si el refresh falla.

     **Expire** → tiempo máximo que un secundario mantiene la información sin poder contactar al primario.

     **Minimum (TTL mínimo)** → tiempo mínimo de vida para registros negativos o de caché.

     

     g. Indique qué valor tiene el registro TXT para el nombre
     saludo.redes.unlp.edu.ar. Investigue para qué es usado este registro.

     "HOLA"

     Sirve tradicionalmente para notas administrativas, pero hoy en día se usa mucho en seguridad y validación de servicios. 

     COmo por ejemplo spf que indica política de que servidores de correo están autorizados a enviar correos. 

     DKIM almacena claves públicas para verificar firmas digitales de correos.

     h. Utilizando dig, solicite la transferencia de zona de redes.unlp.edu.ar, analice la salida y responda.

     i. ¿Qué significan los números que aparecen antes de la palabra IN?
     ¿Cuál es su finalidad?

     Los tiempos que pueden durar los registros en cache para consultarlos sin hacer consulta a los servidores autoritativos.

     ii. ¿Cuántos registros NS observa? Compare la respuesta con los
     servidores de DNS del dominio redes.unlp.edu.ar que dio
     anteriormente. ¿Puede explicar a qué se debe la diferencia y qué
     significa?

     Hay más porque me devuelve todos los servidores asociados al dominio redes.unlp.edu.ar, que tiene subdominios que tambien son manejasdos o registrados por él, como es en este caso práctica.redes.unlp.edu.ar que tienen sus propios servidores pero está registrado en el servidor de redes.unlp.edu.ar por ser subdominio del mismo. 

     i. Consulte por el registro A de www.redes.unlp.edu.ar y luego por el registro A de www.practica.redes.unlp.edu.ar. Observe los TTL de ambos. Repita la operación y compare el valor de los TTL de cada uno respecto de larespuesta anterior. ¿Puede explicar qué está ocurriendo? (Pista: observar los flags será de ayuda).

     Basicamente la respuesta de practica indica un ttl menor porque no está respondida por un servidor autoritativo por ende debe ser actualizado su registro A más seguido. En cambio la consulta por www.redes. durá más tiempo por ser respondida por un servidor autoritativo

     j. Consulte por el registro A de www.practica2.redes.unlp.edu.ar. ¿Obtuvo alguna respuesta? Investigue sobre los códigos de respuesta de DNS. ¿Para qué son utilizados los mensajes NXDOMAIN y NOERROR?

     ​	 Códigos de respuesta DNS (RCODE)

     En la salida de `dig` aparecen en el **HEADER**, en el campo `status:`.
      Los más comunes:

     - **NOERROR** → la consulta se procesó correctamente.

     - **NXDOMAIN** → el nombre de dominio no existe.

     - (Otros: `SERVFAIL`, `REFUSED`, `FORMERR`, etc., pero en este caso no son relevantes).

       ###  Diferencia entre NXDOMAIN y NOERROR

       - **NXDOMAIN (Non-Existent Domain):**
         - Significa que el dominio o subdominio solicitado **no existe en absoluto** en el DNS.
         - Ejemplo: pedís `pepito.unlp.edu.ar` y no hay ningún registro para ese nombre → el servidor devuelve **NXDOMAIN**.
       - **NOERROR:**
         - Significa que la consulta fue procesada con éxito, **pero puede que no haya un registro del tipo pedido**.
         - Ejemplo: `www.unlp.edu.ar` existe, pero si pedís un registro `AAAA` y no hay ninguno definido, el servidor responde **NOERROR** con *respuesta vacía*.

       ------

     12. Investigue los comandos nslookup y host. ¿Para qué sirven? Intente con ambos
         comandos obtener:
         ● Dirección IP de www.redes.unlp.edu.ar.
         ● Servidores de correo del dominio redes.unlp.edu.ar.
         ● Servidores de DNS del dominio redes.unlp.edu.ar.

         ## `nslookup`

         - Es una **herramienta de línea de comandos** para consultar **servidores DNS** y obtener información sobre dominios.
         - Se puede usar para:
           - Resolver un **nombre de dominio a su IP** (registro A).
           - Resolver una **IP a un nombre de dominio** (registro PTR).
           - Consultar **tipos específicos de registros** (MX, NS, TXT, SOA, etc.).
           - Elegir **servidor DNS específico** para hacer la consulta.

         ## `host`

         - También es una **herramienta de consulta DNS**, más simple y enfocada a la **resolución rápida**.
         - Sirve para:
           - Obtener la **IP de un dominio** (A o AAAA).
           - Verificar **registros MX, NS, PTR, TXT**, etc.
           - Consultar servidores DNS específicos usando `host dominio servidor`.
         - Suele ser más **conciso y limpio** que `nslookup`.

     13. ¿Qué función cumple en Linux/Unix el archivo /etc/hosts o en Windows el archivo \WINDOWS\system32\drivers\etc\hosts?

         |      | Función |
         | ---- | ------- |
         |      |         |

         | Archivo `hosts` | Resuelve localmente hostnames a IP antes de ir a DNS |
         | --------------- | ---------------------------------------------------- |
         |                 |                                                      |

         | Consulta DNS | Solo se hace si el hostname no está en `hosts` (para aplicaciones normales) |
         | ------------ | ------------------------------------------------------------ |
         |              |                                                              |

         | Seguridad / bloqueo | No evita que otros hagan consultas DNS sobre ese nombre, solo afecta **tu máquina** |
         | ------------------- | ------------------------------------------------------------ |
         |                     |                                                              |

     14. Abra el programa Wireshark para comenzar a capturar el tráfico de red en la interfaz con
         IP 172.28.0.1. Una vez abierto realice una consulta DNS con el comando dig para averiguar
         el registro MX de redes.unlp.edu.ar y luego, otra para averiguar los registros NS correspondientes al dominio redes.unlp.edu.ar. Analice la información proporcionada por dig y compárelo con la captura.

         No estoy seguro de que es lo que tengo que ver pero parece que luego de hacer la consulta  a los registros NSse realizan una serie de consultas adicionales. 

     15. Dada la siguiente situación: “Una PC en una red determinada, con acceso a Internet, utiliza los servicios de DNS de un servidor de la red”. Analice:
         a. ¿Qué tipo de consultas (iterativas o recursivas) realiza la PC a su servidor de DNS?

         realiza consulta recursivas, ya que su servidor dns hará el trabajo de consultar de manera iterativa.

         b. ¿Qué tipo de consultas (iterativas o recursivas) realiza el servidor de DNS para resolver requerimientos de usuario como el anterior? ¿A quién le realiza estas consultas?

         Hace consultas iterativas a los servidores en la jerarquía de dns, según corresponda. La manera genérica sería consultar a un root por el servidor de un TLD, luego a ese TLD, el TLD le de el server del dominio especificado y asi hasta llegar al servidor autoriticativo del dominio consultado.

     16. Relacione DNS con HTTP. ¿Se puede navegar si no hay servicio de DNS?

         - **Relación DNS ↔ HTTP:**
           - Cuando escribís un URL (`http://www.ejemplo.com`) en el navegador, primero **se resuelve el nombre de dominio a una IP** mediante DNS.
           - Luego, el navegador hace la **petición HTTP** a esa IP.
         - **Navegar sin DNS:**
           - Sí, **si conocés la IP del servidor**, podés acceder directo (`http://192.0.2.10`).
           - Sin DNS, no podés usar nombres de dominio fáciles de recordar.

         ✅ En resumen: **DNS no es obligatorio para HTTP**, pero es **fundamental para usar nombres legibles por humanos**.

     17. Observar el siguiente gráfico y contestar:

         ![image-20250919113937743](/home/ignaciomariano/.var/app/io.typora.Typora/config/Typora/typora-user-images/image-20250919113937743.png)

a. Si la PC-A, que usa como servidor de DNS a "DNS Server", desea obtener la IP de www.unlp.edu.ar, cuáles serían, y en qué orden, los pasos que se ejecutarán para obtener la respuesta.
b. ¿Dónde es recursiva la consulta? ¿Y dónde iterativa?

### a. Pasos para obtener la IP de [www.unlp.edu.ar](https://www.unlp.edu.ar) (en orden)

1. **Consulta Recursiva desde PC-A:** La PC-A envía una **consulta recursiva** a su servidor DNS local ("DNS Server" en el diagrama). Le pide la IP de `www.unlp.edu.ar`.
2. **Proceso Iterativo del "DNS Server":** El "DNS Server" no tiene la respuesta en su caché, por lo que inicia el proceso iterativo:
   - **Paso 2.1:** Consulta a un **servidor Raíz (Root Server)**. Le pregunta: "¿Quién es autoritativo para `.ar`?".
   - **Paso 2.2:** El servidor Raíz responde con una **referencia** a los servidores TLD (de primer nivel) para `.ar`.
   - **Paso 2.3:** El "DNS Server" consulta a uno de los servidores TLD de **.ar**. Le pregunta: "¿Quién es autoritativo para `edu.ar`?".
   - **Paso 2.4:** El servidor TLD de **.ar** responde con una referencia a los servidores autoritativos para `edu.ar`.
   - **Paso 2.5:** El "DNS Server" consulta a uno de los servidores autoritativos de **[edu.ar](https://edu.ar)**. Le pregunta: "¿Quién es autoritativo para `unlp.edu.ar`?".
   - **Paso 2.6:** El servidor de **[edu.ar](https://edu.ar)** responde con una referencia a los servidores autoritativos para `unlp.edu.ar`.
   - **Paso 2.7:** El "DNS Server" consulta a uno de los servidores autoritativos de **[unlp.edu.ar](https://unlp.edu.ar)**. Le pregunta: "¿Cuál es la dirección IP de `www.unlp.edu.ar`?".
   - **Paso 2.8:** El servidor autoritativo de **[unlp.edu.ar](https://unlp.edu.ar)** responde con el **registro A** (la dirección IP) de `www.unlp.edu.ar`.
3. **Respuesta Final a PC-A:** El "DNS Server" ahora tiene la respuesta final. Le envía esta IP a la PC-A como respuesta a su consulta recursiva inicial.

------

### b. ¿Dónde es recursiva la consulta? ¿Y dónde iterativa?

- **Consulta Recursiva:**
  - Se produce en un **único lugar**: entre la **PC-A y su "DNS Server" local**.
  - La PC le "delega el trabajo" al servidor DNS, esperando una respuesta definitiva (la IP o un error).
- **Consultas Iterativas:**
  - Se producen entre el **"DNS Server" local y los servidores de la jerarquía DNS** (Root, TLD `.ar`, Autoritativo `edu.ar`, Autoritativo `unlp.edu.ar`).
  - En cada paso, el "DNS Server" consulta a un servidor de nivel superior, y este le responde con una **referencia** al siguiente servidor más específico. El "DNS Server" es quien debe realizar la siguiente consulta.

18. ¿A quién debería consultar para que la respuesta sobre www.google.com sea autoritativa?

    Para obtener una respuesta autoritativa y técnica sobre el dominio `www.google.com`, debes consultar el **DNS (Sistema de Nombres de Dominio)**, específicamente los **servidores de nombres autoritativos** para el dominio `google.com`.

    ### Pasos para obtener una respuesta autoritativa:
    1. **Consulta los servidores de nombres autoritativos para `google.com`:**
       - Usa el comando `dig` (en Linux/macOS) o `nslookup` (en Windows) para encontrar los servidores autoritativos:
         ```bash
         dig NS google.com
         ```
         o
         ```bash
         nslookup -type=NS google.com
         ```
       - Esto devolverá una lista de servidores como:
         ```
         ns1.google.com
         ns2.google.com
         ns3.google.com
         ns4.google.com
         ```

    2. **Consulta directamente a uno de estos servidores autoritativos:**
       - Realiza una consulta DNS directamente a uno de los servidores de Google:
         ```bash
         dig @ns1.google.com www.google.com
         ```
         o
         ```bash
         nslookup www.google.com ns1.google.com
         ```
       - La respuesta incluirá la dirección IP autoritativa de `www.google.com`.

    ### ¿Por qué esto es "autoritativo"?
    - Los servidores de nombres autoritativos (`ns1.google.com`, etc.) son la fuente oficial de información para el dominio `google.com`. Cualquier otro servidor DNS (como el de tu proveedor de internet) obtiene los datos de estos servidores y los almacena en caché, pero la respuesta final y confiable proviene directamente de los servidores autoritativos.

    ### Alternativa rápida:
    Si solo necesitas la dirección IP de `www.google.com`, una consulta DNS normal (`dig www.google.com` o `nslookup www.google.com`) suele ser suficiente, pero técnicamente no es "autoritativa" si la respuesta proviene de un caché DNS intermedio.

    ### Resumen:
    - **Fuente autoritativa:** Servidores DNS de Google (`ns1.google.com`, `ns2.google.com`, etc.).
    - **Herramientas:** Usa `dig` o `nslookup` para consultarlos directamente.
    - **Propósito:** Obtener información técnica exacta (IPs, registros DNS) sin depender de cachés intermedios.

19. ¿Qué sucede si al servidor elegido en el paso anterior se lo consulta por www.info.unlp.edu.ar? ¿Y si la consulta es al servidor 8.8.8.8?

    La función principal de un **servidor autoritativo** es servir **únicamente** la información de los dominios para los que es responsable. Para  cualquier otra consulta, su rol es redirigirte hacia la jerarquía  correcta, no resolverla por ti. La resolución (recursión) es el trabajo  de los **resolvers DNS** (como los de tu ISP o los públicos de Google/Cloudflare).

    Basicamente no responderá con respuesta porque no se pondrá a hacer recursión porque no está habilitado para hacer esto. 

    

    

    Ejercicio de parcial

20. En base a la siguiente salida de dig, conteste las consignas. Justifique en todos los
    casos.
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 4, ADDITIONAL: 4
    ;; QUESTION SECTION:
    ;ejemplo.com. IN _MX_
    ;; ANSWER SECTION:
    ejemplo.com. 1634 IN _MX_ 10 srv01.ejemplo.com. (1)
    ejemplo.com. 1634 IN _MX 5 srv00.ejemplo.com. (2)
    ;; AUTHORITY SECTION:
    ejemplo.com. 92354 IN NS_ ss00.ejemplo.com.
    ejemplo.com. 92354 IN _NS_ ss02.ejemplo.com.
    ejemplo.com. 92354 IN _NS_ ss01.ejemplo.com.
    ejemplo.com. 92354 IN _NS_ ss03.ejemplo.com.
    ;; ADDITIONAL SECTION:
    srv01.ejemplo.com. 272 IN _A_ 64.233.186.26
    srv01.ejemplo.com. 240 IN _AAAA_ 2800:3f0:4003:c00::1a
    srv00.ejemplo.com. 272 IN _A_ 74.125.133.26
    srv00.ejemplo.com. 240 IN _AAAA_ 2a00:1450:400c:c07::1b
    a. Complete las líneas donde aparece __ con el registro correcto.
    b. ¿Es una respuesta autoritativa? En caso de no serlo, ¿a qué servidor le preguntaría para obtener una respuesta autoritativa? NO, no está el registro aa. Al servidor que me aparezca si hago consulta dig indicando SOA le consultaría para obtener respuesta autoritativa
    c. ¿La consulta fue recursiva? ¿Y la respuesta? Si fue recursiva la request, pero no se puede afirmar nunca al 100% que una respuesta es recursiva porque si bien indica que el servidor que respondió acepta recursión, no se puede afirmar nunca si fue recursiva
    d. ¿Qué representan los valores 10 y 5 en las líneas (1) y (2).

    Indican la prioridad en la cual se debe enviar un correo, el de menor numero indica que el correo primero se envia al servidor ese por tener mayor prioridad,.