Redes y Comunicaciones LINTI - UNLP
Práctica 2 - Capa de Aplicación
HTTP
Requerimientos
Para realizar esta práctica deberá descargar la máquina virtual provista por la
cátedra. Puede ver la URL de descarga en el sitio de la cátedra en
https://catedras.info.unlp.edu.ar/.
Una vez descargado el archivo, haciendo doble click en el mismo debería abrirse un
cuadro de diálogo que permita configurar algunos parámetros del sistema. Se pueden
aceptar los valores por defecto haciendo simplemente click en Importar.
Se recomienda hacer un snapshot de la VM antes de empezar a usarla, para poder
volver atrás en caso de que algo deje de funcionar. Los datos de acceso a la máquina virtual
son:
Usuario: redes
Contraseña: redes
Para acceso con permisos de administrador, usar el comando sudo
Aclaración: el dominio redes.unlp.edu.ar solo existe dentro de la VM provista por la
cátedra, no es válido en Internet, lo que implica que todos los ejercicios de esta práctica y
las siguientes en las que se utilice dicho dominio solo podrán ser resueltos dentro dicha VM.
Introducción

1. ¿Cuál es la función de la capa de aplicación?

   La función de la capa de aplicación es la de brindar comunicación entre procesos de diferentes sistemas finales, a través de protocolos y lineamientos que permitan intercambiar información, enviar mensajes, coordinar tareas entre sistemas finales que pueden ser esencialmente distintos. 

   Brinda servicios a las aplicaciones de usuario

   Facilita comunicación entre procesos

   *La capa de aplicación **traduce las necesidades de comunicación de los programas del usuario en mensajes comprensibles para la red** y permite que sistemas heterogéneos se coordinen y colaboren.*

   

2. Si dos procesos deben comunicarse:
    a. ¿Cómo podrían hacerlo si están en diferentes máquinas?

  Lo hacen a través de protocolos predefinidos que indican como comunicarse. Protocolos que permiten generar un mensaje comprensible para el receptor, saber a que host enviar y por que puerto se entabla la comunicación. Información que pasa a la capa de transporte y así sucesivamente por las demás capas; Para que luego el sistemas final receptor pueda dar el mensaje al proceso receptor que escucha en el socket indicado.

  b. Y si están en la misma máquina, ¿qué alternativas existen?

  Claro. La comunicación no implicaría el uso de la red. Existen diferentes mecanismos que permiten la comunicación entre procesos de un mismo sistema final:

  - Memoria compartida: escriben y leen del mismo sector de memoria
  - pipes: Uno escribe en una pipe y otro lee de esa pipe
  - colas de mensajes: SO mantiene colas en que los procesos depositan y reciben mensajes
  - sockets locales o Unix sockets: Funcionan parecido a los soket de red pero dentro del mismosistema, más rápidos porque no salen a la red. Digamos que pasan a la capa de transporte.
  - Señales digitales: para notificaciones simples entre procesos (example: comandos como kill, pause, recover process,etc)

3. Explique brevemente cómo es el modelo Cliente/Servidor. Dé un ejemplo de un sistema
    Cliente/Servidor en la “vida cotidiana” y un ejemplo de un sistema informático que siga el
    modelo Cliente/Servidor. ¿Conoce algún otro modelo de comunicación?

  El modelo Cliente/Servidor consiste en que un cliente solicita servicios o recursos y un servidor los provee.
  Ejemplo cotidiano: un cajero automático (cliente) consultando al banco (servidor).
  Ejemplo informático: un navegador web (cliente) accediendo a un servidor web.
  Otro modelo: P2P (peer-to-peer), donde todos los nodos actúan como clientes y servidores. 

4. Describa la funcionalidad de la entidad genérica “Agente de usuario” o “User agent”. 

   El **Agente de usuario (User Agent)** es el software que actúa de intermediario entre el usuario y la red, permitiéndole solicitar y recibir servicios. Ejemplos típicos son los navegadores web o clientes de correo, que formulan las solicitudes, las envían a través de la pila de protocolos y presentan los resultados al usuario.

5. ¿Qué son y en qué se diferencian HTML y HTTP?

   Http es un protocolo para solicitar y enviar recursos de un servidor web. Indica como deben ser los mensajes, que tipo de mensajes existen, el recurso que se solicita y a quién. El contenido varia desde archivos de páginas web como html, css, javascript o inclusive imagenes, datos, json, etc. 

   Html es el formato default para compartir páginas web enteras, ya que es el archivo que estrutura una página web y que indica al user agent como representar una página web. Es un tipo de recurso que devuelven los servidores web. 

   **HTTP**: es un **protocolo de comunicación** entre cliente y servidor web. Define cómo se solicitan y transfieren recursos (HTML, CSS, JS, imágenes, JSON, etc.).

   **HTML**: es un **lenguaje de marcado** usado para estructurar y presentar contenido en páginas web. Es uno de los tipos de recurso que se transmiten usando HTTP.

6. HTTP tiene definido un formato de mensaje para los requerimientos y las respuestas.
    (Ayuda: apartado “Formato de mensaje HTTP”, Kurose).
    a. ¿Qué información de la capa de aplicación nos indica si un mensaje es de requerimiento o de respuesta para HTTP? ¿Cómo está compuesta dicha información?¿Para qué sirven las cabeceras?

b. ¿Cuál es su formato? (Ayuda:
https://developer.mozilla.org/es/docs/Web/HTTP/Headers)

Existen dos tipos primordiales de mensajes http: 

- Mensajes de solicitudes
- Mensajes de respuesta

Los mensajes http de manera default están codificados en ASCII por ende son comprensibles para el hombre. 

- **Request Line**: es la primera línea de un mensaje http 

  - Tres campos:

    - Método

      - GET: cuando un browser solicita un objeto del server, que se indica en la url
      - POST: Para solicitudes que envien contenido en body, so el server considera analizar lo que hay en el body
      - HEAD: tiene un próposito para el desarrollo, ya que se solicita un objeto pero el server responde con header y no con el objeto, en el afán de evitar cargas innecesarias en pruebas en etapa de desarrollo
      -  PUT: Es un método directo que permite actualizar objetos del servidor, se indica con la url el objeto
      -  DELETE: Permite eliminar objetos de un servidor, indicado con url 

    - URL field: Identifica el dominio del server y el recurso solicitado

    - Versión HTTP: HTTP/1.0,/1.1,/2.0,/3.0

    - **Header lines** : Son varios headers que indican cada uno algo específico relacionado a la solicitud y a la respuesta

      - Host: Especifica el sistema final en el que el objeto reside. La info esta es más que nada necesaria por los proxy caché. 
      - Connection:close indica al server que la conexión se cierra una vez que este responda a la solicitud. 
      - User-agent: específica el user agent con el cual se hace la solicitud al server. Es útil porque el server puede enviar diferentes versiones del objeto según el user agent

      ![image-20250908162157951](/home/ignaciomariano/.var/app/io.typora.Typora/config/Typora/typora-user-images/image-20250908162157951.png)



**respuesta http:**	

- staus line: Protocol, version field, status field que indica como salió la respuesta
- header lines:
  - connectino:close 
  - connection:open
  - Date: Indica la fecha en que el cliente respondió a la solicitud http
  - server: Indica que tipo de servidor genero la respuesta
  - Last-Modified: Indica fecha en que el objeto fue creado o modificado
    - este header es crucial para el manejo de las caches del cliente y el servidor
  - Content-length: el número de bytes en el objeto enviado
  - contet-type: indica el tipo de objeto enviado en el body (html, json, etc)

​	![image-20250908163124996](/home/ignaciomariano/.var/app/io.typora.Typora/config/Typora/typora-user-images/image-20250908163124996.png)

c. Suponga que desea enviar un requerimiento con la versión de HTTP 1.1 desde curl/7.74.0 a un sitio de ejemplo como www.misitio.com para obtener el recurso /index.html. En base a lo indicado, ¿qué información debería enviarse mediante encabezados? Indique cómo quedaría el requerimiento.

​	GET  /index.html HTTP/1.1

​	host: www.misitio.com

​	user-agent: curl/7.74.0



7. Utilizando la VM, abra una terminal e investigue sobre el comando curl. Analice para qué sirven los siguientes parámetros (-I, -H, -X, -s).

  - curl -I: Hace un HEAD request
  - curl -H: añade o modifica un encabezado
  - curl -X: Especifica el método HTTP
  - curl -S: mostrar errores

8. Ejecute el comando curl sin ningún parámetro adicional y acceda a
    www.redes.unlp.edu.ar. Luego responda:
    a. ¿Cuántos requerimientos realizó y qué recibió? Pruebe redirigiendo la salida
    (>) del comando curl a un archivo con extensión html y abrirlo con un
    navegador.

  Realice un requerimiento GET de la pagina index de redes

  b. ¿Cómo funcionan los atributos href de los tags link e img en html?

  - href : "*hypertext reference* 
  - <link href=… > No genera contenido visible, sino que asocia un recurso al documento (ejemplo: hoja de estilo)
  - src en <img> y no href: El atributo img es src, que apunta al archivo de la imgagen que debe cargarse y renderizarse dentro del documento.

  - **`href`** → indica la **ubicación de un recurso que se vincula o relaciona** (no se inserta en la página). Usado en `<link>`, `<a>`.
  - **`src`** → indica la **fuente de un recurso que se inserta/renderiza** en la página (imágenes, scripts, iframes, etc.).

  c. Para visualizar la página completa con imágenes como en un navegador, ¿alcanza con realizar un único requerimiento?

  No, debe hacerse una serie de requerimientos, que por lo general al usar un browser como user agent, este se encarga de hcaer todos los requerimientos necesarios, que implican html, imagenes, etc.

  d. ¿Cuántos requerimientos serían necesarios para obtener una página que tiene dos CSS, dos Javascript y tres imágenes? Diferencie cómo funcionaría un navegador respecto al comando curl ejecutado previamente.

  Son necesarios 8 requerimientos, es decir, primero se consulta el recurso html y luego se obtienen de el los recursos necesarios para cargar estilos, funcionalidad e imágenes de la página

  La diferencia de curl con un navegador es que curl hace la solicitud del recuros indicado y nada más, mientras que el navegador cuando hace la solicitud de un recurso html, luego interpreta los tags que indican la necesidad de buscar otros recursos. Es posible configurar los browsers para que estos por ejemplo no carguen css, no carguen imagenes, etc.

9. Ejecute a continuación los siguientes comandos:
    curl -v -s www.redes.unlp.edu.ar > /dev/null
    curl -I -v -s www.redes.unlp.edu.ar
    a. ¿Qué diferencias nota entre cada uno?

  Uno muestra la respuesta y el otro no.

  Con -s se indica modo silencioso, no barra de progreso

  Con -v se indica modo verboso: muestra detalle de la conexión, headers enviados y recibidos

  Redirección >/dev/null descarta el cuerpo de la respuesta

  Con la segunda respuesta -I se indica un método HEAD por ende no es necesaria la redireccion de la respuesta para que no muestre el cuerpo, ya que se reciben solo los encabezados de la respuesta y no el body

  b. ¿Qué ocurre si en el primer comando se quita la redirección a /dev/null? ¿Por	qué no es necesaria en el segundo comando?

  El comando mostraría **todo el cuerpo de la página HTML** además de la información verbosa.

  En el segundo comando no hace falta porque al usar `-I` **no hay cuerpo que mostrar** (HEAD solo devuelve headers).

  c. ¿Cuántas cabeceras viajaron en el requerimiento? ¿Y en la respuesta?

  En el req viajaron 4 cabeceras (metodo, host, UA,Accept)

  En la respuesta 8 (tipo respuesta con código, date, lastMod,Etag,Accept-ranges, content-length,contentType)

10. ¿Qué indica la cabecera Date?

    Cuando se respondió la solicitud realizada por el client

11. En HTTP/1.0, ¿cómo sabe el cliente que ya recibió todo el objeto solicitado de manera completa? ¿Y en HTTP/1.1?

    - http 1.0 tiene por defecto el cierre de conexión una vez que se responde con el objeto solicitado, por ende cuando se cierra la conexión podemos llegar a la conclusión de que el objeto se recibió en su totalidad
    - http1.1 las conexiones son persistentes por defecto, hasta que se acabe el timer del server que indica que no se recibieron nuevas solicitudes en un rango de tiempo de espera.
    - Por ende para saber si se recibió el objeto en http1.1 hay dos maneras:
      - Ver la cabecera dice el tamaño del archivo y que el body tenga ese tamaño, por ende que se recibió la totalidad del objeto en la misma respuesta o ver en las respuestas posteriores que se complete la recepción de ese objeto.
      - Transfer-Encodig: Chunked: si el servidor no conoce el tamaño de antemano, envía el objeto en "chunks", pedazos, y termina con un chunk de tamaño 0.

12. Investigue los distintos tipos de códigos de retorno de un servidor web y su significado.
    Considere que los mismos se clasifican en categorías (2XX, 3XX, 4XX, 5XX).

    https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status

13. Utilizando curl, realice un requerimiento con el método HEAD al sitio
    www.redes.unlp.edu.ar e indique:
    a. ¿Qué información brinda la primera línea de la respuesta?

    La primera línea indica el método con el cual respondió el servidor, que es http/1.1 e indica el código de respuesta por el cual sabemos que la solicitud se resolvió y dió OK (200)

    b. ¿Cuántos encabezados muestra la respuesta?

    Como la versión del protocolo de respuesta y el código de status de la misma no cuentan como header, podemos decir que hay 7 encabezados.

    c. ¿Qué servidor web está sirviendo la página?

    El servidor web que brinda la respuesta es un Apache/2.4.56 (Unix)

    d. ¿El acceso a la página solicitada fue exitoso o no?

    Si, lo podemos ver con el código 200

    e. ¿Cuándo fue la última vez que se modificó la página?

    Last-modified indica que fue Doming 19 de Marzo de 2023 a las 19 horas

    f. Solicite la página nuevamente con curl usando GET, pero esta vez indique que quiere obtenerla sólo si la misma fue modificada en una fecha posterior a la que efectivamente fue modificada. ¿Cómo lo hace? ¿Qué resultado obtuvo? ¿Puede
    explicar para qué sirve?

    Podemo hacerlo :

    *curl -H "If-Modified-Since: <day-name>, <day> <month> <year> <hour>:<minute>:<second> GMT" www.redes.unlp.edu.ar*

    En cada de que haya sido modificado nos devuelve el objeto con código 200

    Si no fue modificado responde error con código 304, que indica que el objeto no fue modificado

    La utilidad es para por ejemplo no traer objetos que tenemos en cache que no fueron modificados de la útlima vez que lo obtuvimos

    Sirve justamente para **no volver a descargar un objeto innecesariamente** y ahorrar ancho de banda, tiempo y carga en el servidor 

14. Utilizando curl, acceda al sitio www.redes.unlp.edu.ar/restringido/index.php y siga las
    instrucciones y las pistas que vaya recibiendo hasta obtener la respuesta final. Será de
    utilidad para resolver este ejercicio poder analizar tanto el contenido de cada página como
    los encabezados.

    Basicamente para enviar el usuario y clave primero se codifican juntas en base 64 con: *echo -n "usuario:clave" | base64*

    Luego se envía con curl con el encabezado "Authorization: Basic <credenciales en base64>"

    Luego simplemente vemos con los headers -I la pagina final a la que hay que enviar las credenciales con el cabezal authorization

15. Utilizando la VM, realice las siguientes pruebas:
    a. Ejecute el comando ’curl www.redes.unlp.edu.ar/extras/prueba-http-1-0.txt’ y redcopie la salida completa (incluyendo los dos saltos de línea del final).
    b. Desde la consola ejecute el comando telnet www.redes.unlp.edu.ar 80 y
    luego pegue el contenido que tiene almacenado en el portapapeles. ¿Qué ocurre luego de hacerlo?

    Se Cierra la salida una vez que se finalizó de enviar el recurso

    c. Repita el proceso anterior, pero copiando la salida del recurso
    /extras/prueba-http-1-1.txt. Verifique que debería poder pegar varias veces el mismo contenido sin tener que ejecutar el comando telnet nuevamente.

    Básicamente permite pedir dos recursos sin cerrar conexión, se mantiene abierta la conexión.

16. En base a lo obtenido en el ejercicio anterior, responda:
    a. ¿Qué está haciendo al ejecutar el comando telnet?

    El comando `telnet www.redes.unlp.edu.ar 80` establece una **conexión TCP** con el servidor web en el puerto 80. Esa conexión es el canal sobre el cual después se envía manualmente la petición HTTP (copiada desde el archivo).

    b. ¿Qué método HTTP utilizó? ¿Qué recurso solicitó?

    El método fue **`GET`** (ya viene escrito en los archivos).
     Los recursos solicitados fueron:

    - `/extras/prueba-http-1-0.txt` en el caso de HTTP/1.0
    - `/extras/prueba-http-1-1.txt` en el caso de HTTP/1.1

    c. ¿Qué diferencias notó entre los dos casos? ¿Puede explicar por qué?

    - En **HTTP/1.0**, después de la respuesta el servidor **cierra la conexión** automáticamente.
    - En **HTTP/1.1**, la conexión queda **abierta (keep-alive por defecto)** y se pueden enviar múltiples peticiones sin reabrir la conexión.

    La diferencia se debe a que en HTTP/1.0 las conexiones eran *no persistentes* por defecto, mientras que HTTP/1.1 introdujo las *conexiones persistentes* para optimizar el rendimient

    d. ¿Cuál de los dos casos le parece más eficiente? Piense en el ejercicio donde
    analizó la cantidad de requerimientos necesarios para obtener una página
    con estilos, javascripts e imágenes. El caso elegido, ¿puede traer asociado
    algún problema?

    HTTP/1.1 es más eficiente porque evita abrir una conexión TCP por cada recurso (lo cual ahorra tiempo y recursos, sobre todo si una página necesita cargar HTML, CSS, JS e imágenes).

    Problema posible: mantener conexiones abiertas consume recursos en el servidor (memoria, sockets), incluso si el cliente no hace más solicitudes → puede llevar a agotamiento de recursos si hay muchas conexiones ociosas.

17. En el siguiente ejercicio veremos la diferencia entre los métodos POST y GET. Para ello, será necesario utilizar la VM y la herramienta Wireshark. Antes de iniciar considere:
    ■ Capture los paquetes utilizando la interfaz con IP 172.28.0.1. (Menú “Capture
    ->Options”. Luego seleccione la interfaz correspondiente y presione Start).
    ■ Para que el analizador de red sólo nos muestre los mensajes del protocolo http
    introduciremos la cadena ‘http’ (sin las comillas) en la ventana de especificación de filtros de visualización (display-filter). Si no hiciéramos esto veríamos todo el tráfico que es capaz de capturar nuestra placa de red. De los paquetes que son
    capturados, aquel que esté seleccionado será mostrado en forma detallada en la sección que está justo debajo. Como sólo estamos interesados en http ocultaremos toda la información que no es relevante para esta práctica (Información de trama,
    Ethernet, IP y TCP). Desplegar la información correspondiente al protocolo HTTP bajo la leyenda “Hypertext Transfer Protocol”.
    ■ Para borrar la cache del navegador, deberá ir al menú “Herramientas->Borrar historial reciente”. Alternativamente puede utilizar Ctrl+F5 en el navegador para forzar la petición HTTP evitando el uso de caché del navegador.
    ■ En caso de querer ver de forma simplificada el contenido de una comunicación http, utilice el botón derecho sobre un paquete HTTP perteneciente al flujo capturado y seleccione la opción Follow TCP Stream.
    a. Abra un navegador e ingrese a la URL: www.redes.unlp.edu.ar e ingrese al link en la sección “Capa de Aplicación” llamado “Métodos HTTP”. En la página mostrada se visualizan dos nuevos links llamados: Método GET y Método POST. Ambos muestran un formulario como el siguiente:
    b. Analice el código HTML

    Principalmente se diferencian en que el método del formulario es GET y el otro es POST	

    c. Utilizando el analizador de paquetes Wireshark capture los paquetes
    enviados y recibidos al presionar el botón Enviar.

    

    d. ¿Qué diferencias detectó en los mensajes enviados por el cliente?

    La diferencia principal radica en donde van enviados los parámetros del formulario, ya que en el get los parámetros se colocan en la url, mientras que en el post van en el body.

    e. ¿Observó alguna diferencia en el browser si se utiliza un mensaje u otro?

    Si, se puede observar en la url de cada uno, que en el GET están los datos pasados en el form y en la url de POST no. 

18. Investigue cuál es el principal uso que se le da a las cabeceras Set-Cookie y Cookie en HTTP y qué relación tienen con el funcionamiento del protocolo HTTP.

    set-cookie: Setea una nueva cookie que será guardada en el lado del user y con la que el servidor identificará el usuario (user agent en un computador específico). La setea el servidor por lo general ya que los servidores no aceptan cookies de parte del usuario que no hayan seteado ellos.

    cookie: El user agent cada vez que haga una request luego que le hayan seteado una cookie, enviará esas cookies en el header este.

    La función es permitir a un protocolo sin estados como los http ser provechoso para aplicaciones en las que las sesiones o datos del usuario si se mantienen en el tiempo y pueden volver ser accedidas sin necesidad de estar todo el tiempo identificandose o diciendo quién es.

    

19. ¿Cuál es la diferencia entre un protocolo binario y uno basado en texto? ¿De qué tipo de protocolo se trata HTTP/1.0, HTTP/1.1 y HTTP/2?

Uno basado en binario es HTTP/2 y está hecho para que los headers y el body de los mensajes estén codificados en binario lo que permite un mejor parseo, tamaños de mensajes más pequeños, y menos propensos a tener errores. El parseo refiere a que son más fáciles de analizar por progrmas y ser comparados. 

HTTP/1.0 e /1.1 son basados en texto, es decir, ascii. Basicamente sus headers y bodys viajan en texto plano, no tienen las ventajas mencionadas sobre http/2 por ser basado en binario. 

Se puede resumir en que **textual ** es más fácil para humanos pero más costoso para máquina. En cambio en **binario** es más difícil de entender para humanos pero mucho más eficiente para máquinas. 

20.Responder las siguientes preguntas:
a. ¿Qué función cumple la cabecera Host en HTTP 1.1? ¿Existía en HTTP 1.0?
¿Qué sucede en HTTP/2? (Ayuda:
https://undertow.io/blog/2015/04/27/An-in-depth-overview-of-HTTP2.html para HTTP/2)

En HTTP1.1 cumple la función de indicar el dominio del servidor al que se solicita. También tiene la opción de indicar puerto pero en caso de no tenerlo se ve el protocolo usado para solicitud para saber a que puerto enviar el mensaje. 

El dominio sirve para asociar con hosting virtual. indica al servidor el nombre de dominio al que  el cliente quiere acceder, aunque todos los dominios compartan la misma IP.

En HTTP1.0 no existía porque cada cliente hacía solicitud a una ruta específica ya que cada servidor tenía una sola dirección IP para un solo sitio web. Entonces, con la IP a la que se conectaba el cliente ya alcanzaba para saber a qué página pedir el recurso. 

El problema empezó con el **hosting virtual** en el que un servidor con una sola IP servía a varios dominios distintos, el servidor no podía distinguir a qué dominio se refería el cliente

Para HTTP/2.0 el header host sigue siendo soportado pero ahora se utiliza como estándar el pseudo eader :authority que cumple el mismo rol. Los servidores hoy buscan primero el :authority y en caso de no estar buscar el host. 

b. En HTTP/1.1, ¿es correcto el siguiente requerimiento?
GET /index.php HTTP/1.1
User-Agent: curl/7.54.0

NO es correcto porque le falta el cabezal de host

c. ¿Cómo quedaría en HTTP/2 el siguiente pedido realizado en HTTP/1.1 si se está usando https?
GET /index.php HTTP/1.1
Host: www.info.unlp.edu.ar



```html
:method: GET
:scheme: https
:authority: www.info.unlp.edu.ar
:path: /index.php

```

Ejercicio de Parcial
curl -X ?? www.redes.unlp.edu.ar/??

> HEAD /metodos/ HTTP/??
> Host: www.redes.unlp.edu.ar
> User-Agent: curl/7.54.0
> < HTTP/?? 200 OK
> < Server: nginx/1.4.6 (Ubuntu)
> < Date: Wed, 31 Jan 2018 22:22:22 GMT
> < Last-Modified: Sat, 20 Jan 2018 13:02:41 GMT
> < Content-Type: text/html; charset=UTF-8
> < Connection: close
> a. ¿Qué versión de HTTP podría estar utilizando el servidor?
>
> Podría estar utilizando HTTP/1.0 porque cerro la conexión luego de recibir el recurso (aunque esto no es suficiente para decir) y además el cabezal host está puesto, pero sabemos que en http/1.0 era opcional por ende no necesariamente es http/1.1. 
>
> En relación a esto último podemos decir que el comando curl al hacer -H sabemos que está poniendo el header host adrede y terminar de concluir que es HTTP/1.0 el protocolo utilizado. 
>
> No queda muy claro, podría estar usando tanto la versión del protocolo 1.1 como la 1.0.
>
> b. ¿Qué método está utilizando? Dicho método, ¿retorna el recurso completo solicitado?
>
> Está utilizando el método HEAD el cual no retorna todo recurso sino los headers de la respuesta del servidor, sin el body donde vendría el recurso.
>
> c. ¿Cuál es el recurso solicitado?
>
> El recurso solicitado es el index.html de /metodos/.Digo esto porque al terminar el recurso en barra, esto según leí indica un index.html del path solicitado. 
>
> d. ¿El método funcionó correctamente?
>
> SI, el código de respuesta indica que fue exitoso.
>
> e. Si la solicitud hubiera llevado un encabezado que diga:
> If-Modified-Since: Sat, 20 Jan 2018 13:02:41 GMT
> ¿Cuál habría sido la respuesta del servidor web? ¿Qué habría hecho el
> navegador en este caso?
>
> El código de respuesta habróia sido  304 que indica que no fue modificado el recurso desde la fecha solicitada, porque con la anterior respuesta podemos ver que el recurso fue modificado por última vez en la misma fecha del cabezal If-Modified des este punto. 
>
> El servidor responde con la copia del recurso que tiene guardada en caché.