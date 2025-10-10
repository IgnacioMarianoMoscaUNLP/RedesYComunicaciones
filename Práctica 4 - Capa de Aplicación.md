Práctica 4 - Capa de Aplicación
Correo Electrónico
1. ¿Qué protocolos se utilizan para el envío de mails entre el cliente y su servidor de correo? ¿Y entre servidores de correo?

  SMTP, HTTP(web mails)

  Entre servidores tenemos SMTP

2. ¿Qué protocolos se utilizan para la recepción de mails? Enumere y explique características y diferencias entre las alternativas posibles.

  ### Protocolos:

  1. **POP3 (Post Office Protocol v3)**
     - Descarga los mensajes del servidor al dispositivo cliente.
     - Por defecto, borra los mensajes del servidor (aunque se puede configurar para dejarlos una copia).
     - Ventajas: simple, consume poco espacio en el servidor.
     - Desventajas: mala sincronización entre varios dispositivos, ya que el correo se guarda localmente.
  2. **IMAP (Internet Message Access Protocol)**
     - Mantiene los mensajes almacenados en el servidor y solo muestra copias sincronizadas en el cliente.
     - Permite organizar carpetas, marcar como leído/no leído, mover correos, etc., directamente en el servidor.
     - Ventajas: sincronización completa entre varios dispositivos.
     - Desventajas: requiere conexión frecuente y espacio en el servidor.
  3. **HTTP (Webmail, apps)**
     - Usado por servicios como Gmail, Outlook.com o apps móviles.
     - El acceso al correo se hace a través de una interfaz web o aplicación que se comunica con el servidor mediante HTTP/HTTPS.
     - Ventajas: accesible desde cualquier navegador/dispositivo, no necesita configuración de cliente de correo.
     - Desventajas: dependes de estar en línea y de la plataforma del proveedor.

------

  ✅ **Diferencias clave:**

  - **POP3**: descarga → pensado para un solo dispositivo.
  - **IMAP**: sincroniza → ideal para múltiples dispositivos.
  - **HTTP**: acceso vía web/app → depende del proveedor y no requiere cliente dedicado.

3. Utilizando la VM y teniendo en cuenta los siguientes datos, abra el cliente de correo (Thunderbird) y configure dos cuentas de correo. Una de las cuentas utilizará POP para solicitar al servidor los mails recibidos para la misma mientras que la otra utilizará IMAP.
    Al crear cada una de las cuentas, seleccionar Manual config y luego de configurar las
    mismas según lo indicado, ignorar advertencias por uso de conexión sin cifrado.
    ● Datos para POP
    Cuenta de correo: alumnopop@redes.unlp.edu.ar
    Nombre de usuario: alumnopop
    Contraseña: alumnopoppass
    Puerto: 110
    ● Datos para IMAP
    Cuenta de correo: alumnoimap@redes.unlp.edu.ar
    Nombre de usuario: alumnoimap
    Contraseña: alumnoimappass
    Puerto: 143
    ● Datos comunes para ambas cuentas
    Servidor de correo entrante (POP/IMAP):
    • Nombre: mail.redes.unlp.edu.ar
    • SSL: None
    • Autenticación: Normal password
    Servidor de correo saliente (SMTP):
    • Nombre: mail.redes.unlp.edu.ar
    • Puerto: 25
    • SSL: None
    • Autenticación: Normal password
    a. Verificar el correcto funcionamiento enviando un email desde el cliente de
    una cuenta a la otra y luego desde la otra responder el mail hacia la primera.
    b. Análisis del protocolo SMTP

  i. Utilizando Wireshark, capture el tráfico de red contra el servidor de
  correo mientras desde la cuenta alumnopop@redes.unlp.edu.ar envía
  un correo a alumnoimap@redes.unlp.edu.ar
  ii. Utilice el filtro SMTP para observar los paquetes del protocolo SMTP
  en la captura generada y analice el intercambio de dicho protocolo
  entre el cliente y el servidor para observar los distintos comandos
  utilizados y su correspondiente respuesta. Ayuda: filtre por protocolo
  SMTP y sobre alguna de las líneas del intercambio haga click derecho
  y seleccione Follow TCP Stream. . .

```sql
Handshake inicial SMTP:

220 → el servidor se presenta.

EHLO [172.28.0.1] → el cliente se identifica.

El servidor responde con sus capacidades (PIPELINING, SIZE, 8BITMIME, etc.).

Flujo de envío del correo:

MAIL FROM:<alumno...@...> → dirección del remitente.

RCPT TO:<alumnopop@...> → destinatario.

DATA → empieza el cuerpo del mensaje.

Luego se ve el contenido fragmentado (DATA fragment, 2473 bytes).

. seguido de 250 2.0.0 Ok: queued → el servidor acepta el mensaje y lo pone en cola.

Cierre de la conexión:

QUIT / 221 Bye.
```

  c. Usando el cliente de correo Thunderbird del usuario
  alumnopop@redes.unlp.edu.ar envíe un correo electrónico
  alumnoimap@redes.unlp.edu.ar el cual debe tener: un asunto, datos en el
  body y una imagen adjunta.
  i. Verifique las fuentes del correo recibido para entender cómo se utiliza
  el header “Content-Type: multipart/mixed“ para poder realizar el envío
  de distintos archivos adjuntos.

El encabezado **MIME** (Multipurpose Internet Mail Extensions) es el mecanismo que permite que un correo electrónico no se limite solo a texto plano, sino que pueda contener **imágenes, archivos adjuntos, formatos enriquecidos, e incluso múltiples partes dentro de un mismo mensaje**.

Lo que ves en tu captura es un correo **multipart/mixed**, que significa que el mensaje contiene **varias partes distintas**, típicamente:

- **Texto plano (text/plain) o HTML**
- **Archivos adjuntos (image/jpeg, pdf, etc.)**

------

### ¿Cómo funciona el encabezado MIME en tu ejemplo?

El cliente (Thunderbird en tu caso) genera algo como esto:

```
MIME-Version: 1.0
Content-Type: multipart/mixed; boundary="----gMown6kb0NY9Ewfek4wv1UXL"
```

Ese **boundary (separador)** es clave. Marca dónde empieza y termina cada parte del mensaje.

Tu captura se interpreta así:

```
--gMown6kb0NY9Ewfek4wv1UXL     ← Primera frontera (boundary)
(Encapsulated multipart part: text/plain)
... aquí va el cuerpo de texto ...

--gMown6kb0NY9Ewfek4wv1UXL     ← Segunda frontera
(Encapsulated multipart part: image/jpeg)
... aquí va la imagen en base64 o binario ...

--gMown6kb0NY9Ewfek4wv1UXL--   ← Frontera final (con doble --)
```

------

### ¿Por qué aparece aunque enviaste desde POP3 a IMAP?

Porque **POP3 e IMAP solo definen cómo se recibe/almacena el correo**, pero **el envío siempre lo hace SMTP**, y **es el cliente de correo (Thunderbird) el que decide cómo construir el mensaje** en formato MIME según el contenido.

- Si solo envías texto → `Content-Type: text/plain`
- Si hay adjuntos → `Content-Type: multipart/mixed` con boundaries
- Si hay HTML + texto plano → `multipart/alternative`
- Si hay imágenes embebidas en HTML → `multipart/related`

------

### En resumen claro:

> **El encabezado MIME le dice al cliente de correo cómo interpretar y separar las diferentes partes del mensaje (texto, imágenes, adjuntos).**
>  Se basa en un *boundary* que actúa como separador entre cada sección, y cada parte tiene su propio `Content-Type`.



  ii. Extraiga la imagen adjunta del mismo modo que lo hace el cliente de
  correo a partir de los fuentes del mensaje.

Básicamente es abrir el paquete en wireshark que contiene la imagen codificada en base64, guardarla como archivo .b64, luego convertirla a jpg y listo.

  4. Análisis del protocolo POP

    a. Utilizando Wireshark, capture el tráfico de red contra el servidor de correo
    mientras desde la cuenta alumnoimap@redes.unlp.edu.ar le envía una
    correo a alumnopop@redes.unlp.edu.ar y mientras
    alumnopop@redes.unlp.edu.ar recepciona dicho correo.
    b. Utilice el filtro POP para observar los paquetes del protocolo POP en la
    captura generada y analice el intercambio de dicho protocolo entre el cliente
    y el servidor para observar los distintos comandos utilizados y su
    correspondiente respuesta.


Esta captura muestra claramente **una sesión POP3 completa** entre un cliente de correo (por ejemplo Thunderbird) y el servidor POP (en este caso, probablemente *Dovecot* en 172.28.0.1).

Vamos a interpretar paso a paso qué ocurre:

------

### 📨 Flujo típico de POP3 que se observa

| Línea                | Dirección          | Comando POP3                                 | Significado                                                 |
| -------------------- | ------------------ | -------------------------------------------- | ----------------------------------------------------------- |
| `+OK Dovecot ready.` | Servidor → Cliente | Banner                                       | El servidor POP indica que está listo para recibir comandos |
| `CAPA`               | Cliente → Servidor | Solicita capacidades                         | El cliente pregunta qué funciones soporta el servidor       |
| `+OK ...`            | Servidor → Cliente | Respuesta                                    | Envía lista de capacidades                                  |
| `AUTH PLAIN`         | Cliente            | Método de autenticación                      |                                                             |
| `<cadena base64>`    | Cliente            | Usuario/contraseña codificados               |                                                             |
| `+OK Logged in.`     | Servidor           | Autenticación exitosa                        |                                                             |
| `STAT`               | Cliente            | Solicita cantidad de mensajes y tamaño total |                                                             |
| `+OK 4 5141`         | Servidor           | Hay **4 mensajes**, total **5141 bytes**     |                                                             |
| `LIST`               | Cliente            | Pide tamaño individual de cada mensaje       |                                                             |
| `+OK 4 messages:`    | Servidor           | Lista de mensajes (id y tamaño)              |                                                             |
| `UIDL`               | Cliente            | Pide identificadores únicos de cada mensaje  |                                                             |
| `RETR 3`             | Cliente            | Descarga el mensaje número 3                 |                                                             |
| `+OK 778 octets`     | Servidor           | Envía el mensaje                             |                                                             |
| `RETR 4`             | Cliente            | Descarga el mensaje número 4                 |                                                             |
| `QUIT`               | Cliente            | Cierra la sesión                             |                                                             |
| `+OK Logging out.`   | Servidor           | Finaliza                                     |                                                             |

------

### ✅ ¿Qué significa todo esto en resumen?

> **El cliente POP se conecta, se autentica, consulta qué mensajes hay, descarga (RETR) uno o más mensajes, y finalmente cierra la sesión con QUIT.**

Todo el flujo es **pull/download**, ya que POP3 **solo sirve para recibir correos**, a diferencia de SMTP que se usa para enviar.

------



    5. Análisis del protocolo IMAP
    
    a. Utilizando Wireshark, capture el tráfico de red contra el servidor de correo
    mientras desde la cuenta alumnopop@redes.unlp.edu.ar le envía un correo a
    alumnoimap@redes.unlp.edu.ar y mientras alumnoimap@redes.unlp.edu.ar
    recepciona diredecho correo.
    b. Utilice el filtro IMAP para observar los paquetes del protocolo IMAP en la
    captura generada y analice el intercambio de dicho protocolo entre el cliente
    y el servidor para observar los distintos comandos utilizados y su
    correspondiente respuesta.


​    

Perfecto, esta captura muestra una sesión **IMAP** activa entre el cliente (172.28.0.90) y el servidor (172.28.0.1). Vamos a interpretarla.

------

### 📨 ¿Qué está pasando en esta captura IMAP?

| Elemento         | Significado                                                  |
| ---------------- | ------------------------------------------------------------ |
| `* 3 EXISTS`     | El servidor informa que hay **3 correos en el buzón**.       |
| `UID FETCH`      | El cliente pide información de cada mensaje (tamaño, flags, encabezados, cuerpo, etc.). |
| `BODY.PEEK[...]` | El cliente **lee parcialmente el contenido** del mensaje **sin marcarlo como leído** (a diferencia de `BODY[...]`). |
| `FLAGS`          | Indica el estado del mensaje (*visto*, *borrador*, *respondido*, etc.). |
| `NOOP`           | Comando “ping” → mantiene viva la conexión sin hacer nada.   |
| `IDLE`           | El cliente entra en **modo escucha**, esperando notificaciones de nuevos mensajes en tiempo real (push). |
| `DONE`           | Sale del modo IDLE para ejecutar nuevos comandos.            |

------

### ✅ En resumen más claro:

> **El cliente IMAP se conecta, consulta cuántos mensajes hay, descarga encabezados/cuerpo parcial de los correos con `UID FETCH`, y luego se queda en modo `IDLE` esperando nuevos mensajes automáticamente.**
>  Es un comportamiento típico de un cliente moderno como Thunderbird o Outlook, que **sincroniza en tiempo real** en lugar de descargar todo como hace POP3.

------



6. IMAP vs POP
     a. Marque como leídos todos los correos que tenga en el buzón de entrada de
       alumnopop y de alumnoimap. Luego, cree una carpeta llamada POP en la
       cuenta de alumnopop y una llamada IMAP en la cuenta de alumnoimap.
       Asegúrese que tiene mails en el inbox y en la carpeta recientemente creada
       en cada una de las cuentas.
       b. Cierre la sesión de la máquina virtual del usuario redes e ingrese
       nuevamente identificándose como usuario root y password packer, ejecute el
       cliente de correos. De esta forma, iniciará el cliente de correo con el perfil del
       superusuario (diferente del usuario con el que ya configuró las cuentas antes
       mencionadas). Luego configure las cuentas POP e IMAP de los usuarios
       alumnopop y alumnoimap como se describió anteriormente pero desde el
       cliente de correos ejecutado con el usuario root. Responda:
       i. ¿Qué correos ve en el buzón de entrada de ambas cuentas? ¿Están
       marcados como leídos o como no leídos? ¿Por qué?
       ii. ¿Qué pasó con las carpetas POP e IMAP que creó en el paso
       anterior?
       c. En base a lo observado. ¿Qué protocolo le parece mejor? ¿POP o IMAP?
       ¿Por qué? ¿Qué protocolo considera que utiliza más recursos del servidor?   ¿Por qué?

     ------

     ### i. ¿Qué correos ve en el buzón de entrada de ambas cuentas? ¿Están marcados como leídos o no leídos? ¿Por qué?

     - **Cuenta POP:** Al configurar la cuenta nuevamente desde otro perfil (root), el cliente descargará nuevamente *todos los correos que sigan disponibles en el servidor*, pero **no tiene forma de saber si antes estaban leídos o no**, ya que POP no sincroniza estados. Por eso, **aparecerán como no leídos**.
     - **Cuenta IMAP:** IMAP trabaja directamente contra el servidor, por lo que al abrir la cuenta desde el nuevo cliente, **verás los mismos correos con el mismo estado** (si los marcaste como leídos antes, seguirán leídos). Esto ocurre porque IMAP **sincroniza el estado del buzón completo**.

     ------

     ### ii. ¿Qué pasó con las carpetas POP e IMAP que creó en el paso anterior?

     - **En POP:** La carpeta **POP no aparecerá**, porque POP no sincroniza la estructura de carpetas del servidor —solo descarga el contenido del Inbox.
     - **En IMAP:** La carpeta **IMAP sí aparecerá exactamente como la dejaste**, junto con los correos que hubiese dentro, porque IMAP mantiene la estructura completa del buzón en el servidor.

     ------

     ### iii. ¿Qué protocolo parece mejor? ¿Cuál usa más recursos del servidor? ¿Por qué?

     - **Mejor para sincronización entre dispositivos:** *IMAP*, porque mantiene estados, carpetas y cambios directamente en el servidor.
     - **Más liviano para el servidor:** *POP*, porque solo entrega los correos al cliente y no mantiene sincronización constante.
     - **Más costoso para el servidor:** *IMAP*, porque requiere mantener almacenamiento, sincronización de carpetas y estados para cada cliente conectado.

     ### 📌 Diferencia clave con POP3 (por si lo necesitás para un informe):

     | POP3                                                       | IMAP                                                         |
     | ---------------------------------------------------------- | ------------------------------------------------------------ |
     | Descarga mensajes y (opcionalmente) los borra del servidor | Mantiene los mensajes en el servidor                         |
     | Conexión corta y puntual (STAT → LIST → RETR → QUIT)       | Conexión persistente con IDLE para recibir cambios en tiempo real |
     | No soporta múltiples carpetas                              | Soporta carpetas, estados (leído/no leído), flags            |
     | Cliente tipo “buzón local”                                 | Cliente tipo “sincronización”                                |

     ------

     

7. ¿En algún caso es posible enviar más de un correo durante una misma conexión TCP?
     Considere:
       ● Destinatarios múltiples del mismo dominio entre MUA-MSA y entre MTA-MTA
       ● Destinatarios múltiples de diferentes dominios entre MUA-MSA y entre
       MTA-MTA

     ###  Contexto breve

     En SMTP (protocolo de envío de correo):

     - Cada **conexión TCP** permite enviar **una o más transacciones SMTP**.

     - Cada transacción SMTP comienza con:

       ```
       MAIL FROM:<remitente>
       RCPT TO:<destinatario1>
       RCPT TO:<destinatario2>
       DATA
       (mensaje)
       .
       
       ```

     Un mismo mensaje puede tener múltiples destinatarios si están en la misma transacción/

     MUA → MSA (cliente → servidor saliente)

     #### Destinatarios múltiples del mismo dominio:

     ✅ **Sí**, se puede enviar un solo mensaje con varios destinatarios en una **misma conexión TCP**.
      El cliente (MUA) puede usar varios `RCPT TO:` dentro de la misma transacción SMTP.

     #### 🔹 Destinatarios múltiples de diferentes dominios:

     ✅ **También sí**, pero:

     - En la práctica el MUA **envía todo al mismo MSA** (su servidor saliente),
     - El MSA luego se encargará de separar por dominios.
        Por tanto, desde el **MUA al MSA** puede ir todo en **una misma conexión TCP**, con múltiples `RCPT TO:` (sin importar los dominios).

     ####  Destinatarios múltiples del mismo dominio:

     ✅ **Sí**, perfectamente posible.
      El servidor remitente puede mantener **una sola conexión TCP** al MTA del dominio destino y enviar en una sola transacción todos los destinatarios de ese dominio:

     ​	

     ```
     MAIL FROM:<remitente>
     RCPT TO:<user1@dominio.com>
     RCPT TO:<user2@dominio.com>
     DATA
     ...
     .
     
     ```

     ####  Destinatarios múltiples de diferentes dominios:

     ❌ **No.**
      El protocolo SMTP requiere que cada conexión se dirija a **un único servidor destino (un dominio MX específico)**.
      Entonces, si hay destinatarios en distintos dominios (por ejemplo `@gmail.com` y `@outlook.com`), el MTA remitente debe abrir **una conexión TCP separada por dominio**.

8. Indique sí es posible que el MSA escuche en un puerto TCP diferente a los
     convencionales y qué implicancias tendría.

     Sí, **técnicamente puede**.
      El servidor de correo (por ejemplo, Postfix, Exim o Sendmail) puede configurarse para escuchar en cualquier puerto TCP (por ejemplo, el **2525**, 1025, 8025, etc.).

     ------

     ### ⚙️ Implicancias de hacerlo

     #### 1. 🔐 **Configuración en el cliente (MUA)**

     - Los clientes (como Thunderbird, Outlook o un MUA programado) deben configurarse **manualmente** para usar ese puerto distinto.
     - Si el MUA intenta usar el puerto 587 por defecto, **fallará la conexión** si el MSA no lo escucha allí.

     #### 2. 🌐 **Compatibilidad con cortafuegos, ISPs y redes**

     - Algunos **firewalls o proveedores de Internet** bloquean el puerto 25 para evitar spam.
     - El puerto 587 está estandarizado y permitido para envío de correo autenticado.
     - Usar un puerto no estándar puede **evitar bloqueos**, pero también puede **romper compatibilidad** o requerir reglas específicas en routers o firewalls.

     #### 3. 📤 **Normas y estándares (RFC 6409)**

     - Según el estándar, el **puerto 587** es el designado para *Message Submission* (MSA).
     - Escuchar en otro puerto **viola el estándar**, aunque puede hacerse sin problema técnico.
        Simplemente el sistema **no será interoperable por defecto**.

     #### 4. 🧩 **Casos de uso válidos**

     - Servidores internos o pruebas locales (por ejemplo, `localhost:2525` en desarrollo).
     - Evitar conflictos o bloqueos por políticas de red.
     - Enrutamiento especial o sistemas cerrados (intranets).

9. Indique sí es posible que el MTA escuche en un puerto TCP diferente a los
     convencionales y qué implicancias tendría.

     El **MTA (Mail Transfer Agent)** es el servidor que:

     - **Recibe** correos de otros servidores (MTA → MTA).
     - **Reenvía o entrega** esos correos al siguiente salto o al dominio destino.

     Ejemplos: Postfix, Exim, Sendmail, qmail, etc.

     ------

     ### ⚙️ Puerto convencional del MTA

     | Comunicación     | Puerto TCP | Estándar                                                |
     | ---------------- | ---------- | ------------------------------------------------------- |
     | MTA ↔ MTA (SMTP) | **25**     | ✅ Oficial y obligatorio según los estándares (RFC 5321) |

10. Ejercicio integrador HTTP, DNS y MAIL
      Suponga que registró bajo su propiedad el dominio redes2024.com.ar y dispone de 4
      servidores:
      ● Un servidor DNS instalado configurado como primario de la zona
      redes2024.com.ar. (hostname: ns1 - IP: 203.0.113.65).
      ● Un servidor DNS instalado configurado como secundario de la zona
      redes2024.com.ar. (hostname: ns2 - IP: 203.0.113.66)

      ● Un servidor de correo electrónico (hostname: mail - IP: 203.0.113.111).
      Permitirá a los usuarios envíar y recibir correos a cualquier dominio de
      Internet.
      ● Un servidor WEB para el acceso a un webmail (hostname: correo - IP:
      203.0.113.8). Permitirá a los usuarios gestionar vía web sus correos
      electrónicos a través de la URL https://webmail.redes2024.com.ar
      a. ¿Qué información debería informar al momento del registro para hacer visible
      a Internet el dominio registrado?
    
    - Los servidores de nombres (nameservers) autoritativos para el dominio (al menos 2). Los indica ahi ns1 y ns2
    
    - Los record de la ip en registros A de los servers autoritativos
    
    - Persona o entidad responsable (datos administrativos/tecnicos) según el registrador (contacto, e-mail,etc..)
    
      Con eso el registro público (TLD) tendrá punteros a tus servidores DNS autoritativos, y el dominio será resoluble en internet.
    
      b. ¿Qué registros sería necesario configurar en el servidor de nombres? Indique
      toda la información necesaria del archivo de zona. Puede utilizar la siguiente
      tabla de referencia (evalúe la necesidad de usar cada caso los siguientes
      campos): Nombre del registro, Tipo de registro, Prioridad, TTL, Valor del
      registro.
    
    ```python
    $TTL 3600
    @   IN  SOA ns1.redes2024.com.ar. hostmaster.redes2024.com.ar. (
            20251007 ; serial YYYYMMDDnn
            3600     ; refresh
            600      ; retry
            604800   ; expire
            86400 )  ; minimum
    
        IN  NS  ns1.redes2024.com.ar.
        IN  NS  ns2.redes2024.com.ar.
    
    ns1 IN  A   203.0.113.65
    ns2 IN  A   203.0.113.66
    
    mail    IN  A   203.0.113.111
    correo  IN  A   203.0.113.8
    webmail IN  CNAME correo.redes2024.com.ar.    ; opcional alias
    
    ; MX - indicar el servidor de correo que recibe mail para el dominio
    @   IN  MX  10 mail.redes2024.com.ar.
    
    ; SPF (TXT) - ejemplo para permitir sólo al mail server 203.0.113.111
    @   IN  TXT "v=spf1 ip4:203.0.113.111 -all"
    
    ; DKIM: registro TXT para la clave pública (selector ejemplo 'mail1')
    mail1._domainkey  IN TXT "v=DKIM1; k=rsa; p=MIIBIjANB..." 
    
    ; DMARC
    _dmarc IN TXT "v=DMARC1; p=reject; rua=mailto:postmaster@redes2024.com.ar; ruf=mailto:forensic@redes2024.com.ar; pct=100"
    
    ; PTR (reverse) — se configura en la delegación de la IP (proveedor), no en la zona forward.
    
    ```
    
      c. ¿Es necesario que el servidor de DNS acepte consultas recursivas?
      Justifique.
    
    No para los servidores autoritativos públicos. 
    
    Explicación:
    
    - Tus servidores ns1 y ns2 son autoritativos para redes2024.com.ar. NO necesitan ofrecer resolución recursirva para el público general. 
    
    - Permitir **recursión** en servidores autoritativos abiertos es peligroso: riesgo de abuso (amplificación/reflejo de DDoS) y carga innecesaria. 
    
    - Recomendación: deshabilitar recursión en los servidores autoritativos expuestos a internet. 
    
      Si necesitas resolución recursiva para usuarios internos, configurar un resolver recursivo separado (solo accesible desde la red interna)
    
      
    
      d. ¿Qué servicios/protocolos de capa de aplicación configuraría en cada
      servidor?

    - ns1 – servidor DNS primario (autoritativo)

      - Servicio: DNS autoritativo (puerto 53 udp/tcp) con registros de cambio de zona
      - Opcional: SSH para administración
    
    - ns2 – servidor DNS secundario (autoritativo)
    
      - Servicio: DNS autoritativo (puerto 53)
      - Opcional: SSH para admin
    
    - mail — server de correo (MTA + MDA + submission
    
      - SMTP para recepción y envío entre MTAs - puerto 25 (tcp)
    
      - Submission (MSA) para clientes autenticados 
    
      - SMTPS (opcional) - si se usa SMTP sobre TLS implícito
    
      - IMAP (MDA) para accesos de clientes si el MTA aloja buzones - puerto 143 o 993 para IMAPS 
    
      - POP3 si se usa en lugar de IMAP - puerto 110/995 para pop3s
    
      - ### DKIM Signer/Verifier (interno), Milter si se usa
    
        Esto se refiere a los mecanismos de **autenticación y reputación del correo saliente y entrante**.
    
        - **DKIM (DomainKeys Identified Mail):**
          - **¿Qué es?** Una tecnología de firma digital que permite a una organización asumir  la responsabilidad de un mensaje de una manera que puede ser verificada  por los proveedores de correo (Gmail, Outlook, etc.) y otros receptores.
          - **Signer (Firmador):** Es el proceso **interno** que **firma digitalmente** todo correo electrónico que sale de tu dominio. Toma el contenido del  mensaje, genera una "huella digital" única y la cifra con una clave  privada que solo tu servidor conoce. Esta firma se añade a las cabeceras del correo.
          - **Verifier (Verificador):** Es el proceso que **comprueba la firma DKIM** de los correos entrantes. Utiliza la clave pública publicada en los  registros DNS del dominio remitente para descifrar la firma. Si coincide con la huella digital del mensaje recibido, prueba que el correo es  legítimo y no fue alterado en tránsito.
        - **Milter (Mail Filter):**
          - **¿Qué es?** Es una API (Application Programming Interface) que permite conectar  aplicaciones externas al proceso de entrega de correo del agente de  transferencia de correo (MTA), como Postfix o Sendmail.
          - **Relación con DKIM:** Normalmente, las tareas de **firmar (Signer)** y **verificar (Verifier) DKIM** no las hace el MTA principal por sí solo. En su lugar, se usa un  servicio externo especializado (como OpenDKIM, Rspamd, Amavis) que se  conecta al MTA usando el protocolo **Milter**. Cuando un correo pasa por el servidor, el MTA "consulta" al servicio  Milter para que realice la firma (si es saliente) o la verificación (si  es entrante).
    
    ###  Autenticación SASL/LDAP/DB para usuarios
    
    Esto se refiere a **cómo los usuarios (o las aplicaciones) prueban su identidad para enviar correo**.
    
    Cuando configuras tu cliente de correo (Outlook, Thunderbird) o una aplicación envía un correo, debe autenticarse para demostrar que tiene permiso  para usar el servidor. Aquí hay tres métodos comunes:
    
    - **SASL (Simple Authentication and Security Layer):**
    
      - **¿Qué es?** No es un método de autenticación en sí, sino un **marco** que permite negociar y utilizar diferentes mecanismos de autenticación  (como LOGIN, PLAIN, CRAM-MD5) de forma segura entre el cliente y el  servidor. Es la "capa" que gestiona el proceso de "inicio de sesión".
    
    - **LDAP (Lightweight Directory Access Protocol):**
    
      - **¿Qué es?** Un protocolo para acceder y mantener servicios de directorio de  información distribuida (como usuarios y contraseñas). Ejemplos son  Active Directory (Windows) o OpenLDAP (Linux).
      - **En la autenticación:** El servidor de correo no almacena las contraseñas, sino que, cuando un  usuario intenta autenticarse, el servidor consulta un servidor LDAP  externo para verificar el nombre de usuario y la contraseña. Es  centralizado y muy común en entornos empresariales.
    
    - **DB (Database / Base de Datos):**
    
      - **¿Qué es?** El servidor de correo almacena las credenciales de los usuarios  (nombres y contraseñas cifradas) directamente en una base de datos  local, como **MySQL, PostgreSQL o SQLite**. La autenticación se realiza consultando esta base de datos.
    
      ###  Webadmin/IMAPS for admin (opcional)
    
      Esto se refiere a las **interfaces para administrar el servidor y, potencialmente, el correo de los usuarios**.
    
      - **Webadmin (Administración Web):**
        - **¿Qué es?** Una interfaz web (como **Roundcube, Afterlogic Webmail Lite, o incluso un panel como SOGo**) que permite a los usuarios finales acceder a su correo, gestionar carpetas, contactos, etc., desde un navegador.
        - **Para Admin:** También puede referirse a un panel de control web específico para el administrador del sistema (como **Mailu, iRedAdmin, o Virtualmin**) para gestionar dominios, cuentas de correo, alias, etc., sin necesidad de usar la línea de comandos.
      - **IMAPS (Internet Message Access Protocol Secure):**
        - **¿Qué es?** El protocolo IMAP, pero sobre una conexión cifrada con SSL/TLS. Permite a los clientes de correo (Outlook, Thunderbird, móviles) acceder y  gestionar sus mensajes almacenados en el servidor de forma segura.
        - **For Admin (opcional):** Si bien IMAPS es principalmente para usuarios finales, la nota "for admin" puede significar dos cosas:
          1. Que el administrador también usa IMAPS para acceder a su propio correo.
          2. (Menos común) Que el acceso IMAPS es una funcionalidad que el administrador puede optar por habilitar o no para los usuarios.
    
       e. Para cada servidor, ¿qué puertos considera necesarios dejar abiertos a
      Internet?. A modo de referencia, para cada puerto indique: servidor, protocolo
      de transporte y número de puerto.
    
    - ns1/ns1 
    
      - udp 53 (dns queries)
      - tcp 53 (transferencias de zona axfr/ixrf y consultas largas)
    
    - mail
    
      - tcp 25 – smtp entre mtas
      - tcp 587 — submission (MUA -> MSA) con starttls y autenticación 
      - tcp 465 — smtps (opcional, si soporta)
      - tcp 143 - imap 
      - tcp 993 - imaps (sobre tls)
      - tcp 110 – pop3 
      - tcp 995 - pop3S (pop3 sobre tls)
      - tcp 22 - ssh (solo si es estrictamente necesario; idealmente restringido por ip/vpn)
      - tcp 443 – si el servidor de mail ofrece webadmin/https (opcional)
    
    - Correo (webmail)
    
      - tcp 443 - https (webmail.redes.2024.com.ar) obligatorio para web seguro
      - tcp 80 - http (solo para redirect para https)
      - tcp 22 – ssh (admin, restringido)
    
      Notas adicionales:
    
      ​	limitar acceso ssh por firewall (permitir solo IPs administrativas o usa vpn)
    
      ​	Habilitar rate-limiting, TLS (Letś Encrypt o certificado público), y protecció contra abuso
    
      ​	Evitar exponer servicions no necesarios al público
    
      f. ¿Cómo cree que se conectaría el webmail del servidor web con el servidor de
      correo? ¿Qué protocolos usaría y para qué?
    
    El webmail intectúa con el servidor de correo usando:
    
    - Para envío de correos 
      - SMTP Submission: webmail conecta al puerto 587 en mail.redes2024.com.ar usando autenticación (SASL) y STARTTLS
      - Alternativamente LMTP o un socket local si el webmail está en la misma máquina que el MTA/MDA
    - Para lectura/gestión de buzón 
      - imap(143/993)– elwebmail  actúa como cliente imap para acceder a buzones en en MDA; Imap permite manejo de carpetas, flags, búsqueda. 
      - POP3 podría usarse, pero IMAP es el adecuado para webmail porque mantiene estado y carpetas
    - Para idexación/utilidades: a veces se usan apis locales, LMTP para entrega local, o acceso a la base de datos de usuarios
    
      g. ¿Cómo se podría hacer para que cualquier MTA reconozca como válidos los
      mails provenientes del dominio redes2024.com.ar solamente a los que llegan
      de la dirección 203.0.113.111? ¿Afectaría esto a los mails enviados desde el
      Webmail? Justifique.
    
    Hay que usar una combinación de estándares de autenticación de correo
    
    - SPF (sender policy framework)–publicar un registro TXT en DNS:
    
    - ```
      @ IN TXT "v=spf1 ip4:203.0.113.111 -all"
      
      ```
    
      Esto indica que **solo** 203.0.113.111 está autorizado a enviar en nombre de `redes2024.com.ar`. Los MTAs receptores que implementen SPF y apliquen política `-all` marcarán o rechazarán envíos desde otras IPs.
    
    - DKIM (DomainKeys identified Mail)- configurar firma dkim en el MTA:
    
      - Generar clave privada en el servidor que envía 
      - Publicar la clave pública en DNS
      - Firmar salientes. Receptores que validen DKIM confirmarán autenticidad del contenido
    
    - Dmarc - publicar DMARC en DNS (apoya SPF + DKIM)
    
    - PT (reverse dns) – asegurar que la ip tenga un ptr haciendo match con mail.redes2024.com.ar
    
    Afectaría al Webmail:
    
    Depende de cómo el webmail envíe los correos:
    
    - Si el webmail envía directamente los mensajes a destino con su propia ip sin relé por la ip del correo entonces SPF los marcaría como no autorizados y serían rechazados por receptores que aplican -all. Por lo tanto sí afectaría
    
    - **afectaría**.
    
      Para evitar ese problema, configurar el webmail para **usar 203.0.113.111 como MSA (relay)**: el webmail conecta al MSA en `mail.redes2024.com.ar:587` y el MSA reenvía usando la IP 203.0.113.111; así los mensajes **sí cumplen SPF/DKIM**.
    
      Alternativa: incluir también la IP 203.0.113.8 en el SPF, pero esto *rompe* la política de “solo 203.0.113.111” y reduce control. Mejor práctica: **forzar que todo envío salga por el MSA `203.0.113.111`**.
    
      h. ¿Qué característica propia de SMTP, IMAP y POP hace que al adjuntar una
      imagen o un ejecutable sea necesario aplicar un encoding (ej. base64)?
    
    ​	La razón es **transparencia 7-bit** y la **limitación de transporte de datos binarios**:
    
    - **SMTP/IMAP/POP históricamente fueron diseñados para texto ASCII de 7 bits** (líneas terminadas en CRLF y caracteres imprimibles).
    - Los archivos binarios (imágenes, ejecutables) contienen bytes que no son ASCII 7-bit y pueden corromperse si se transmiten sin codificar.
    - **Solución**: usar codificación binario-a-texto (p. ej. **base64**) dentro de MIME (Multipurpose Internet Mail Extensions). MIME convierte el binario a texto ASCII seguro para transporte, y el receptor decodifica al restaurar el binario original.
    
    Así: la **limitación de los protocolos** sobre contenido 7-bit y la **necesidad de preservar integridad** requieren codificación.
    
      i. ¿Se podría enviar un mail a un usuario de modo que el receptor vea que el
      remitente es un usuario distinto? En caso afirmativo, ¿Cómo? ¿Es una
      indicación de una estafa? Justifique
    
    ​	**Sí, es posible.**
    
    - El campo **From:** del mensaje es un encabezado que **cualquiera puede establecer** en el cuerpo del correo SMTP. Un emisor puede poner `From: presidente@empresa.com` aunque el envío venga de otra IP.
    - Además, el **envelope sender** (MAIL FROM en SMTP) puede diferir del `From:` header visible en el cliente. El receptor suele ver el `From:` header.
    
    **Cómo se hace técnicamente:**
    
    - Simplemente enviar un mensaje con ese `From:` en la cabecera. Herramientas, scripts o servidores mal configurados permiten forjarlo.
    
    **¿Es indicación de estafa?**
    
    - **No siempre**, pero **sí es una bandera de riesgo**. El spoofing de remitente se usa frecuentemente en phishing y spam.
    - Por eso existen **SPF, DKIM y DMARC**: para verificar que el remitente realmente está autorizado y que el mensaje no fue alterado. Si un mensaje aparece como procedente de `redes2024.com.ar` pero no pasa SPF/DKIM/DMARC, es muy probable que sea fraudulento.
    
    Conclusión: **se puede**, y **es sospechoso**; la presencia de políticas de autenticación ayuda a distinguir legítimo de malicioso.
    
    ------
    
      j. ¿Se podría enviar un mail a un usuario de modo que el receptor vea que el
      destinatario es un usuario distinto? En caso afirmativo, ¿Cómo? ¿Por qué no
      le llegaría al destinatario que el receptor ve? ¿Es esto una indicación de una
      estafa? Justifique
    
    
    
    ​	Interpretación: el usuario que **recibe** (visualmente) un mensaje con un destinatario diferente al real.
    
    - **Cabezas relevantes**:
      - **RCPT TO** es el destinatario a nivel SMTP (envelope recipient).
      - **To:** (cabecera) es lo que muestra el cliente al usuario.
    
    **Si** el emisor manipula la cabecera `To:` (p. ej. `To: juan@example.com`) pero en el **envelope** usó otro destinatario (`RCPT TO: maria@otrodominio.com`), puede ocurrir:
    
    - El **receptor real** (maria@otrodominio.com
    
      k. ¿Qué protocolo usará nuestro MUA para enviar un correo con remitente
      redes@info.unlp.edu.ar? ¿Con quién se conectará? ¿Qué información será
      necesaria y cómo la obtendría?
    
    **Protocolo que usa el MUA:** **SMTP (submission)** — normalmente a través del **MSA** en puerto **587** (o 465).
    
    **¿Con quién se conectará?** Al **servidor SMTP configurado en el cliente**, es decir el MSA que el usuario tenga configurado (normalmente el servidor de correo saliente del proveedor/empresa). No se conecta directamente al MX del dominio del remitente; se conecta al servidor saliente (configurado manualmente o por autodiscover).
    
    **Información necesaria por el MUA:**
    
    - **Servidor SMTP (host)**: nombre o IP del MSA (por ejemplo `mail.redes2024.com.ar` o el MSA del dominio `info.unlp.edu.ar` si el remitente es de UNLP).
    - **Puerto**: 587 (o 465 si SMTPS).
    - **Método TLS**: STARTTLS o TLS implícito.
    - **Credenciales**: usuario y contraseña (autenticación SASL).
    - **From:** dirección (a nivel de header) — `redes@info.unlp.edu.ar`.
    
    **Cómo obtiene esa información:**
    
    - **Configuración manual** por el usuario en el cliente.
    - O **autodiscover/registro** en el proveedor (p. ej. configuración automática en clientes modernos).
    - El MSA, una vez aceptado el mail, hace una **consulta DNS (MX)** del dominio del destinatario para entregar el mensaje a los MTAs destino.
    
      l. Dado que solo disponemos de un servidor de correo, ¿qué sucederá con los
      mails que intenten ingresar durante un reinicio del servidor?
      m. Suponga que contratamos un servidor de correo electrónico en la nube para
      integrarlo con nuestra arquitectura de servicios.
    
    **Comportamiento normal de otros MTAs:** cuando intentan entregar un mail y no pueden conectar (conexión TCP fallida), el MTA remoto **no desecha el correo inmediatamente**: lo coloca en su cola y **reintenta** según su política (por ejemplo cada 15–30 minutos inicialmente, durante varios días — típicamente 2–5 días) antes de generar un *bounce* permanente (DSN).
    
    **Conclusión práctica:**
    
    - Durante un **reinicio corto**, la mayoría de remitentes volverán a intentar y el correo llegará cuando el servidor vuelva.
    - **Riesgo:** si el reinicio es prolongado y el remitente tiene políticas agresivas o desconocidas, podría generarse un rebote.
    - **Mejora:** contratar/usar un **backup MX** (servidor secundario) que acepte correo y lo almacene hasta que el primario vuelva; o usar un servicio de relé en la nube que retenga correo en la caída.
    
      i. ¿Cómo configuraría el DNS para que ambos servidores de correo se
      comporten de manera de dar un servicio de correo tolerante a fallos?
    
    ## Opción 1 — **MX múltiple (backup MX)**
    
    - Publicar **dos (o más) registros MX** con distintas prioridades:
    
      ```
      @ IN MX 10 mail.redes2024.com.ar.     ; primario (203.0.113.111)
      @ IN MX 20 mail-cloud.redes2024.com.ar. ; secundario en la nube (IP del proveedor cloud)
      
      ```
    
      - Comportamiento: los MTAs intentan primero el MX con prioridad 10; si no responde, prueban el siguiente (20). El MX secundario puede aceptar y **almacenar** mensajes hasta que el primario esté disponible, o reenviarlos cuando el primario vuelva.
    
      **Requisitos extra:**
    
      - El MX secundario debe estar configurado para **aceptar correo para `redes2024.com.ar`** (o reenviarlo), y preferentemente reenviar/forward cuando el primario vuelva.
      - Actualizar **SPF** para incluir la IP del servicio en la nube (`ip4:a.b.c.d`) o usar `include:` del proveedor.
      - Gestionar **DKIM**: el servidor en la nube debe poder **firmar** o aceptar correo sin romper la verificación; normalmente se crean claves DKIM en ambos o se comparte la clave pública en DNS.
      - **DMARC** y políticas deben ser coherentes.
    
      ## Opción 2 — **Balanceo activo / reparto de carga**
    
      - Ambos servidores (on-prem y cloud) actúan como **MX con la misma prioridad** (misma preferencia) y comparten buzones (replicación). Esto exige sincronización de usuarios y buzones (IMAP replication, Dovecot replication, o usar un storage compartido).
      - Más complejo, pero permite alta disponibilidad sin almacenar en el secundario.
    
      ## Recomendaciones prácticas
    
      1. **Backup MX (Opción 1)** es la forma clásica y sencilla: MX adicional con prioridad mayor. Asegúrate que el MX secundario **no reenvíe inmediatamente a recursión** (evitar loops) y que tenga procedimientos para entregar al primario cuando vuelva.
      2. **SPF/DKIM/DMARC**: actualizar SPF para incluir IPs/cloud provider; garantizar DKIM en ambos (mismas claves o múltiples selectores) y consistencia DMARC.
      3. **Certificados TLS**, HELO/EHLO coherentes y PTR configurado donde corresponda.
      4. **Pruebas**: simular caída del primario y comprobar que el secundario recibe, almacena y reenvía correctamente.
    
    
    
    
    
    
    
      - Utilizando la herramienta Swaks envíe un correo electrónico con las siguientes
          características:
          ● Dirección destino: Dirección de correo de alumnoimap@redes.unlp.edu.ar
          ● Dirección origen: redesycomunicaciones@redes.unlp.edu.ar
          ● Asunto: SMTP-Práctica4
          ● Archivo adjunto: PDF del enunciado de la práctica
          ● Cuerpo del mensaje: Esto es una prueba del protocolo SMTP
          a. Analice tanto la salida del comando swaks como los fuentes del mensaje
          recibido para responder las siguientes preguntas:
          i. ¿A qué corresponde la información enviada por el servidor destino
          como respuesta al comando EHLO? Elija dos de las opciones del
          listado e investigue la funcionalidad de la misma.
          ii. Indicar cuáles cabeceras fueron agregadas por la herramienta swaks.
          iii. ¿Cuál es el message-id del correo enviado? ¿Quién asigna dicho
          valor?
          iv. ¿Cuál es el software utilizado como servidor de correo electrónico?
          v. Adjunte la salida del comando swaks y los fuentes del correo
          electrónico.
          b. Descargue de la plataforma la captura de tráfico smtp.pcap y la salida del
          comando swaks smtp.swaks para responder y justificar los siguientes
          ejercicios.
          i. ¿Por qué el contenido del mail no puede ser leído en la captura de
          tráfico?
          c. Realice una consulta de DNS por registros TXT al dominio info.unlp.edu.ar y
          entre dichos registros evalúe la información del registro SPF. ¿Por qué cree
          que aparecen muchos servidores autorizados?
    
      d. Realice una consulta de DNS por registros TXT al dominio outlook.com y
      analice el registro correspondiente a SPF. ¿Cuáles son los bloques de red
      autorizados para enviar mails?. Investigue para qué se utiliza la directiva
      "~all"
      12. Observar el gráfico a continuación y teniendo en cuenta lo siguiente , responder:
    
          ![image-20250924160700776](/home/ignaciomariano/.var/app/io.typora.Typora/config/Typora/typora-user-images/image-20250924160700776.png)
    
          ● El usuario juan@misitio.com.ar en PC-A desea enviar un mail al usuario
          alicia@example.com
          ● Cada organización tiene su propios servidores de DNS y Mail
          ● El servidor ns1 de misitio.com.ar no tiene la recursión habilitada
          a. El servidor de mail, mail1, y de HTTP, www, de example.com tienen la misma
          IP, ¿es posible esto? Si lo es, ¿cómo lo resolvería?
          b. Al enviar el mail, ¿por cuál registro de DNS consultará el MUA?
          c. Una vez que el mail fue recibido por el servidor smtp-5, ¿por qué registro de
          DNS consultará?
          d. Si en el punto anterior smtp-5 recibiese un listado de nombres de servidores
          de correo, ¿será necesario realizar una consulta de DNS adicional? Si es
          afirmativo, ¿por qué tipo de registro y de cuál servidor preguntaría?
    
          e. Indicar todo el proceso que deberá realizar el servidor ns1 de misitio.com.ar
          para obtener los servidores de mail de example.com.
          f. Teniendo en cuenta el proceso de encapsulación/desencapsulación y
          definición de protocolos, responder V o F y justificar:
          ● Los datos de la cabecera de SMTP deben ser analizados por el
          servidor DNS para responder a la consulta de los registros MX
          ● Al ser recibidos por el servidor smtp-5 los datos agregados por el
          protocolo SMTP serán analizados por cada una de las capas
          inferiores
          ● Cada protocolo de la capa de aplicación agrega una cabecera con
          información propia de ese protocolo
          ● Como son todos protocolos de la capa de aplicación, las cabeceras
          agregadas por el protocolo de DNS puede ser analizadas y
          comprendidas por el protocolo SMTP o HTTP
          ● Para que los cliente en misitio.com.ar puedan acceder el servidor
          HTTP www.example.com y mostrar correctamente su contenido
          deben tener el mismo sistema operativo.
          g. Un cliente web que desea acceder al servidor www.example.com y que no
          pertenece a ninguno de estos dos dominios puede usar a ns1 de
          misitio.com.ar como servidor de DNS para resolver la consulta.
          h. Cuando Alicia quiera ver sus mails desde PC-D, ¿qué registro de DNS
          deberá consultarse?
          i. Indicar todos los protocolos de mail involucrados, puerto y si usan TCP o
          UDP, en el envío y recepción de dicho mail