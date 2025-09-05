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
2. Si dos procesos deben comunicarse:
a. ¿Cómo podrían hacerlo si están en diferentes máquinas?
b. Y si están en la misma máquina, ¿qué alternativas existen?
3. Explique brevemente cómo es el modelo Cliente/Servidor. Dé un ejemplo de un sistema
Cliente/Servidor en la “vida cotidiana” y un ejemplo de un sistema informático que siga el
modelo Cliente/Servidor. ¿Conoce algún otro modelo de comunicación?
4. Describa la funcionalidad de la entidad genérica “Agente de usuario” o “User agent”.
5. ¿Qué son y en qué se diferencian HTML y HTTP?
6. HTTP tiene definido un formato de mensaje para los requerimientos y las respuestas.
(Ayuda: apartado “Formato de mensaje HTTP”, Kurose).
a. ¿Qué información de la capa de aplicación nos indica si un mensaje es de
requerimiento o de respuesta para HTTP? ¿Cómo está compuesta dicha
información?¿Para qué sirven las cabeceras?

Redes y Comunicaciones LINTI - UNLP
b. ¿Cuál es su formato? (Ayuda:
https://developer.mozilla.org/es/docs/Web/HTTP/Headers)
c. Suponga que desea enviar un requerimiento con la versión de HTTP 1.1 desde
curl/7.74.0 a un sitio de ejemplo como www.misitio.com para obtener el recurso
/index.html. En base a lo indicado, ¿qué información debería enviarse mediante
encabezados? Indique cómo quedaría el requerimiento.
7. Utilizando la VM, abra una terminal e investigue sobre el comando curl. Analice para qué
sirven los siguientes parámetros (-I, -H, -X, -s).
8. Ejecute el comando curl sin ningún parámetro adicional y acceda a
www.redes.unlp.edu.ar. Luego responda:
a. ¿Cuántos requerimientos realizó y qué recibió? Pruebe redirigiendo la salida
(>) del comando curl a un archivo con extensión html y abrirlo con un
navegador.
b. ¿Cómo funcionan los atributos href de los tags link e img en html?
c. Para visualizar la página completa con imágenes como en un navegador,
¿alcanza con realizar un único requerimiento?
d. ¿Cuántos requerimientos serían necesarios para obtener una página que
tiene dos CSS, dos Javascript y tres imágenes? Diferencie cómo funcionaría
un navegador respecto al comando curl ejecutado previamente.
9. Ejecute a continuación los siguientes comandos:
curl -v -s www.redes.unlp.edu.ar > /dev/null
curl -I -v -s www.redes.unlp.edu.ar
a. ¿Qué diferencias nota entre cada uno?
b. ¿Qué ocurre si en el primer comando se quita la redirección a /dev/null? ¿Por
qué no es necesaria en el segundo comando?
c. ¿Cuántas cabeceras viajaron en el requerimiento? ¿Y en la respuesta?
10. ¿Qué indica la cabecera Date?
11. En HTTP/1.0, ¿cómo sabe el cliente que ya recibió todo el objeto solicitado de manera
completa? ¿Y en HTTP/1.1?
12. Investigue los distintos tipos de códigos de retorno de un servidor web y su significado.
Considere que los mismos se clasifican en categorías (2XX, 3XX, 4XX, 5XX).
13. Utilizando curl, realice un requerimiento con el método HEAD al sitio
www.redes.unlp.edu.ar e indique:
a. ¿Qué información brinda la primera línea de la respuesta?
b. ¿Cuántos encabezados muestra la respuesta?
c. ¿Qué servidor web está sirviendo la página?
2
Redes y Comunicaciones LINTI - UNLP
d. ¿El acceso a la página solicitada fue exitoso o no?
e. ¿Cuándo fue la última vez que se modificó la página?
f. Solicite la página nuevamente con curl usando GET, pero esta vez indique que
quiere obtenerla sólo si la misma fue modificada en una fecha posterior a la que
efectivamente fue modificada. ¿Cómo lo hace? ¿Qué resultado obtuvo? ¿Puede
explicar para qué sirve?
14. Utilizando curl, acceda al sitio www.redes.unlp.edu.ar/restringido/index.php y siga las
instrucciones y las pistas que vaya recibiendo hasta obtener la respuesta final. Será de
utilidad para resolver este ejercicio poder analizar tanto el contenido de cada página como
los encabezados.
15. Utilizando la VM, realice las siguientes pruebas:
a. Ejecute el comando ’curl www.redes.unlp.edu.ar/extras/prueba-http-1-0.txt’ y
copie la salida completa (incluyendo los dos saltos de línea del final).
b. Desde la consola ejecute el comando telnet www.redes.unlp.edu.ar 80 y
luego pegue el contenido que tiene almacenado en el portapapeles. ¿Qué
ocurre luego de hacerlo?
c. Repita el proceso anterior, pero copiando la salida del recurso
/extras/prueba-http-1-1.txt. Verifique que debería poder pegar varias veces el
mismo contenido sin tener que ejecutar el comando telnet nuevamente.
16. En base a lo obtenido en el ejercicio anterior, responda:
a. ¿Qué está haciendo al ejecutar el comando telnet?
b. ¿Qué método HTTP utilizó? ¿Qué recurso solicitó?
c. ¿Qué diferencias notó entre los dos casos? ¿Puede explicar por qué?
d. ¿Cuál de los dos casos le parece más eficiente? Piense en el ejercicio donde
analizó la cantidad de requerimientos necesarios para obtener una página
con estilos, javascripts e imágenes. El caso elegido, ¿puede traer asociado
algún problema?
17. En el siguiente ejercicio veremos la diferencia entre los métodos POST y GET. Para ello,
será necesario utilizar la VM y la herramienta Wireshark. Antes de iniciar considere:
■ Capture los paquetes utilizando la interfaz con IP 172.28.0.1. (Menú “Capture
->Options”. Luego seleccione la interfaz correspondiente y presione Start).
■ Para que el analizador de red sólo nos muestre los mensajes del protocolo http
introduciremos la cadena ‘http’ (sin las comillas) en la ventana de especificación de
filtros de visualización (display-filter). Si no hiciéramos esto veríamos todo el tráfico
que es capaz de capturar nuestra placa de red. De los paquetes que son
capturados, aquel que esté seleccionado será mostrado en forma detallada en la

Redes y Comunicaciones LINTI - UNLP
sección que está justo debajo. Como sólo estamos interesados en http ocultaremos
toda la información que no es relevante para esta práctica (Información de trama,
Ethernet, IP y TCP). Desplegar la información correspondiente al protocolo HTTP
bajo la leyenda “Hypertext Transfer Protocol”.
■ Para borrar la cache del navegador, deberá ir al menú “Herramientas->Borrar
historial reciente”. Alternativamente puede utilizar Ctrl+F5 en el navegador para
forzar la petición HTTP evitando el uso de caché del navegador.
■ En caso de querer ver de forma simplificada el contenido de una comunicación http,
utilice el botón derecho sobre un paquete HTTP perteneciente al flujo capturado y
seleccione la opción Follow TCP Stream.
a. Abra un navegador e ingrese a la URL: www.redes.unlp.edu.ar e ingrese al
link en la sección “Capa de Aplicación” llamado “Métodos HTTP”. En la
página mostrada se visualizan dos nuevos links llamados: Método GET y
Método POST. Ambos muestran un formulario como el siguiente:
b. Analice el código HTML
c. Utilizando el analizador de paquetes Wireshark capture los paquetes
enviados y recibidos al presionar el botón Enviar.
d. ¿Qué diferencias detectó en los mensajes enviados por el cliente?
e. ¿Observó alguna diferencia en el browser si se utiliza un mensaje u otro?
18. Investigue cuál es el principal uso que se le da a las cabeceras Set-Cookie y Cookie en
HTTP y qué relación tienen con el funcionamiento del protocolo HTTP.
19. ¿Cuál es la diferencia entre un protocolo binario y uno basado en texto? ¿De qué tipo
de protocolo se trata HTTP/1.0, HTTP/1.1 y HTTP/2?
20. Responder las siguientes preguntas:
a. ¿Qué función cumple la cabecera Host en HTTP 1.1? ¿Existía en HTTP 1.0?
¿Qué sucede en HTTP/2? (Ayuda:
4
Redes y Comunicaciones LINTI - UNLP
https://undertow.io/blog/2015/04/27/An-in-depth-overview-of-HTTP2.html para
HTTP/2)
b. En HTTP/1.1, ¿es correcto el siguiente requerimiento?
GET /index.php HTTP/1.1
User-Agent: curl/7.54.0
c. ¿Cómo quedaría en HTTP/2 el siguiente pedido realizado en HTTP/1.1 si se
está usando https?
GET /index.php HTTP/1.1
Host: www.info.unlp.edu.ar
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
> b. ¿Qué método está utilizando? Dicho método, ¿retorna el recurso completo
> solicitado?
> c. ¿Cuál es el recurso solicitado?
> d. ¿El método funcionó correctamente?
> e. Si la solicitud hubiera llevado un encabezado que diga:
> If-Modified-Since: Sat, 20 Jan 2018 13:02:41 GMT
> ¿Cuál habría sido la respuesta del servidor web? ¿Qué habría hecho el
> navegador en este caso?
> 5