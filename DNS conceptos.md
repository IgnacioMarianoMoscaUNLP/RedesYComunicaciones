**DNS conceptos**

Identificamos sitios mediante hosts, etiquetas alfanumericas que nos dicen algunos datos pero son escasos o pobres en relacion a como podrían ser usados para saber del dominio. Por ejemplo un .ar nos dice el país del dominio pero no mucho más. Por ende los routers necesitan algo más que un host, por eso se usan las direcciones ip. 

**Servicios que brinda dns**

El principal servicio que brinda el sistema de nombre de dominio (domain name system ) es el de traducir nombres de dominio a direcciones ip y vicervesa. Se distribuye este servicios en servidores y además participa en la capa de usuario. 

Proceso para obtener la dirección ip por ejemplo de www.someschool.edu es el siguiente:

- La pc del solicitante corre el servicio cliente de la aplicacion dns
- el user agent extrae el hostname de la url y  se lo pasa a la app dns cliente que corre en el host origen.
- El dns cliente pasa la query que contiene el hostname a solicitar al server dns (por lo general al isp)
- Eventualmente el dns client recibirá una respuesta que incluirá la ip del hostname solicitado
- Una vez que el user agent recibe la ip del dns, puede iniciar una conexion para hacer consultas http

Como sabemos esto de consultar al dns por una ip de un hostname trae retardo para generar conexiones a través del internet, pero se puede reducir teniendo copias de los registros ip relacionados a una ip en diferentes servidores dns distribuidos.

otros servicios de dns son los siguientes: 

**Host aliasing**: Permite que un host con un nombre complicado tenga alias más fáciles de recordar. DNS devuelve tanto la dirección IP como el nombre canónico asociado.

**Mail server aliasing**: Hace posible que las direcciones de correo sean simples y fáciles de usar (ej: *[bob@yahoo.com](mailto:bob@yahoo.com)*

**Distribución de carga**: DNS reparte tráfico entre varios servidores replicados (como los de sitios grandes tipo *cnn.com*) rotando el orden de las direcciones IP asociadas a un mismo nombre. Esto también se aplica a servidores de correo y a sistemas de distribución de contenido como Akamai.

**Aparte de los servicios:**

- Se menciona que el **DNS está definido en los RFC 1034 y 1035**, con actualizaciones posteriores. Es un sistema complejo y existen textos y artículos que explican su diseño y evolución.



**Como funciona DNS **

En muchos so unix like la funcion que llama a la aplicacion para obtener el host name es gethostbyname(). 

Todas las consultas dns que salen a través de la red desde el user agent que envía lo hace con el protocolo UDP por el puerto 53. 

Para el user agent que solicita la traduccion de host a ip, el servicio de dns que realiza la traslasión es una simple caja negra que le resuelve la consulta.  Pero en realidad el dns es un servicio que se involucra una colaboración entre servidores dns interconectados que estar al rededor del mundo. 

Una idea sencilla sería un servidor centralizado que sea consultado para obtener cualquier ip, pero la realidad es que es imposible por diferentes motivos:

- Una sola falla, no hay servicio
- Tráfico de volúmen enorme
- Distancia central a una base de datos lo cual se traducirá en delays significativos
- Matenimiento: Sería costoso en mantenimiento porque surgen host e ips nuevas todo el tiempo y el servidor necesitaría actualizaciones constantes

Basicamente un servidor centralizado no sería escalable.

**DNS es una base de datos jerárquica y distribuida**

En función de la escalabilidad dns funciona en una jerarquía de servidores distribuidos por el mundo.

Ningun servidor dns tiene todos los registros de host-ip del mundo. En cambio el mapeo está distribuido dependiendo de diferentes aspectos. 

Tres clases de servidores dns:

- servidores root dns: Son más de 1000 sevidores  copias al rededor del mundo de los 13 servidores root  que mantienen 12 organizaciones, estos servidores son los que proveen las ip de los TLD servers. 
- servidores top-level domain: Básicamente son los que guardan las ip de los host con top level domain como .com, .net, o también los dominios de país. Básicamente hay un TLD por top level domain, es decir, hay un servidor dedicado al .com, luego al .net, luego al .ar, .edu, etc. Estos servidores contienen las ips de los servidores autoritativos. 
- servidores autoritativos: Basicamente son los servidores que alojan los datos de una organización. Cada organización que posee hosts que desea que esten publicos en el internet como servidores web o servidores de mail, deben proveer registros dns publicos accesibles que mapeen los nombres de esos hosts a su ip particular. Las organizaciones además de tener su server autoritativo donde está toda la información actualizada, pueden replicar en servidores no autoritativos para tener un background y  copias funcionales. 

Hay un tipo más de servidor dns que no entra de manera formal en la jerarquía pero es importante para entender el funcionamiento real de dns

Estos son los servidores dns locales. Son los que están en los ISP, que sirven cómo proxys que realizan la solicitud dns a los diferentes servidores. Todos los useragent se conectan o hacen la solicitud dns al servidor dns local. 

En general las consultas del user agent al servidor dns local son recursivas (ya que solicita y recibe la ip del hostname) y las consultas del servidor dns local para obtener la ip son iterativas (siempre recibe una respuesta intermedia para poder llegar efectivamente a la ip del hostname solicitado).

**Cáche DNS**

Es una funcionalidad escencial para evitar delay constantes en las conexiones a través de la internet

Evita muchos mensajes que diambulan por el intenet.

En una cadena de consultas un servidor puede guardar una copia temporal de registro de ip de un hostname que se resolvió. Lo guarda en su memoria local. 

Cada que venga una nueva consulta por un hostname que está en caché, se podrá responder sin hacer consultas, sin necesidad de ser un servidor autoritativo para el dominio solicitado y que se mantiene registrado. 

Claramente por un período de tiempo se mantienen y luego se descartan, ya que las ip y los hotsname no son permanentes como tal, pueden cambiar, y lo hacen, a lo largo del tiempo. 

Los servidores locales casi siempre guardan las ip de los TLD, por ende las consultas a servidores root no son muy frecuentes. 

**Registro y mensajes DNS**





![image-20250922184409497](/home/ignaciomariano/.var/app/io.typora.Typora/config/Typora/typora-user-images/image-20250922184409497.png)

Todos los servidores dns almacena registros de recursos, que incluyen mapeo de hostname a ip. Todos los mensajes de respuesta de dns tienen mas de un registro dns. 

Campos que son parte de los registros de recursos:

- Name: Depende del campo Type, indica que tipo de registro es.
- value: Depende del campo Type, indica el valor del registro
- type:
  - A: Indica que Name es un hostname y valor es la dirección IP para el hostname.
  - NS: Name es un dominio (like foo.com) y el valor indica el hostname del servidor dns autoritativo que conoce como obtener la dirección IP de los hosts del dominio. Son registros usados para hacer la cadena de consultas dns que resuelvan la direccion ip. 
  - CNAME: el valor es un hostname canonico para el alias de hostname que está en Name. 
  - MX: El Value es el nombre canónico del servidor de mail que tiene el alias que hostname que figura en Name. 
- ttl: tiempo de vida de registro del recurso. Determina cuando un registro debe ser removido de cache. 

Si el servidor DNS es el servidor autoritativo para un hostname particular, entonces el servidor dns contendrá el registro tipo A para el hostname. TAmbien es posible que contenga el registro A sino es autoritativo, porque lo tiene en cache por ejemplo. SI no es un server autoritativo lo mas probable es que tenga el registro NS de un hostname, indicando cual es el server autoritativo y que tenga el registro A de ese NS. 

**Mensaje DNS**

Sólo hay request y respond

- Primeros doce bytes son la sección de headers, que contienen campos. El primero campo identifica con un número a la query. Permitiendo saber cuando se recibió la respuesta a la query porque se identifica. Luego siguen una serie de bits que son flags: el primero indica con 0 si es query y 1 si es reply. Una fladg de autoritativo indica que la respuesta fue autoritativa. Una flag de rq indica que se quiere que la respuesta sea recursiva cuando el servidor no tiene el registro para el hostname. El flag ra indica que el servidor acepta la recursión para realizar consultas sobre un hostname. 
- La sección de **question ** contiene información sobre la query que se realizó. Incluye el campo de nombre que tiene el nombre por el cual se hizó query, y un campo de tipo que indica el tipo de query que se hizó por el name. Es decir dirá si el name se hizó por un tipo MX o A.
- La sección de respuesta contiene los registros del recurso para el nombre que fue originalmente solicitado. Cómo básico contendra el tipo, el valor y el ttl para el name solicitado. Pero una respuesta puede devolver múltiples registros de recurso, ya que un hostname puede tener múltiples ips. 
- La sección de autoridad contiene registros de otros servidores autoritativos.
- La sección adicional contiene registros útiles o de ayuda. Por ejemplo una query puede devolver el nombre canónico de un hostname de servidor de correo y en la sección adicional deja la ip para el servidor de correo específicado con el CNAME. 

Es posible hacer consultas dns directamente desde el host con nslookup program.

**Inserción de registros en la Base de datos de DNS**

Básicamente el registrar un dominio nuevo conlleva registrar una serie de elementos que deben ser conocidos para que en la jerarquía de dns se pueda identificar los servidores de tu dominio. 

Entonces cuando registro un dominio, debo indicar la ip de los servidores  dns autoritativos primarios y secundarios del dominio. 

El registrador de dominio con el cual tramitamos para que registre el dominio querrá saber esto para poder tener registros de tipo NS (servidores autoritativos dns) y registros tipo A que se serán insertados en servidores dns TLD.  Ejemplo si nuestro dominio es networkutopia.com, entonces el TLP com será el que registre. 

- tendrá los siguientes registros el TLD com sobre nuestro servidor autoritativo primario como mínimo.
  - networkutopia.com           dns1.networkutopia.com    NS
  - dns1.networkutopia.com       212.212.212.1                A

Luego debemos tener claro que en el servidor autoritativo de nuestro dominio se deberá tener registro tipo A del servidor web del dominio www.networkutopia.com y el registro type MX del servidor de correo del dominio que sería mail.networkutopia.com.

- tendrá algo como 
  - www.networkutopia.com          212.212.32.2    A
  - www.networkutopia.com       mail.networkutopia.com     MX
  - mail.networkutopia.com       12.2.12.2                A

Una vez que queden estos registros cargados en los servidores, las personas podrán visitar la web y enviar correo a los empleados que responden al sitio. 