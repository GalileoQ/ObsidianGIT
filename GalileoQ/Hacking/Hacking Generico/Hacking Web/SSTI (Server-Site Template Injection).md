## EJEMPLO 1
Vamos al menú de ajustes para ver si es vulnerable a server site template injection, ya que en el nombre se llega a multiplicar el 7 por 7:
![[Pasted image 20240613002231.png]]

Ahora el siguiente objetivo es ejecutar comandos dentro de este recuadro de full name, para ello
vamos a utilizar un payload que podemos encontrarlo dentro de un repositorio de github donde se ubican los payloads más utilizados:
![[Pasted image 20240613002240.png]]

Filtramos dentro de go to file por server side template injection:
![[Pasted image 20240613002246.png]]

Y buscamos esto para localizar payloads que nos sirvan para ejecutar código remoto:
![[Pasted image 20240613002253.png]]

Y aquí podemos elegir el que queramos:
![[Pasted image 20240613002259.png]]

Y habremos comprobado que tenemos la opción de ejecutar comandos remotamente, en este caso ejecutamos el comando para conocer el id:
![[Pasted image 20240613002305.png]]

Pero yo puedo ejecutar cualquier comando en vez del id, entonces voy a poner hostname -i para
conocer la IP:
![[Pasted image 20240613002316.png]]

## EJEMPLO 2
También puede darse el caso de una web donde a pesar de no ver el output vulnerable, es posible que se esté aconteciendo por detrás, dentro del código fuente de la página:
![[Pasted image 20240613002323.png]]

Y vemos que aparentemente no se ejecuta, pero podemos mirar en el código html por detrás y veremos cómo si aparece el 49:
![[Pasted image 20240613002328.png]]

Y nos aparece este comentario diciendo que hay un directorio llamado archive:
![[Pasted image 20240613002332.png]]

Y en este directorio es donde si podemos ver el 49 y que por tanto es vulnerable a SSTI:
![[Pasted image 20240613002337.png]]

Ahora que vemos que es vulnerable a SSTI, vamos a ir al repositorio de payloadallthethings y encontramos este código de SSTI:
![[Pasted image 20240613002341.png]]

Por tanto vamos a ejecutar esto pero en vez de con un id, mejor ponemos por ejemplo un ls -l para listar los archivos:
![[Pasted image 20240613002346.png]]

Y vemos que por detrás, en el html sí funciona:
![[Pasted image 20240613002353.png]]

## EJEMPLO 3
Ahora vamos a comprobar si al escribir un correo estamos ante un server side template injection, y efectivamente lo es, por lo que podremos ejecutar comandos:
![[Pasted image 20240613002359.png]]

Ahora para explotar esta vulnerabilidad, debemos conocer el lenguaje que está hecha la web, el cual es Nose.js:
![[Pasted image 20240613002405.png]]

Ahora buscaremos por internet una inyección de código para insertarlo en webs que tengan esta vulnerabilidad y sean Node.js, donde encontramos esta web que explica el SSTI en webs de node.js:
![[Pasted image 20240613002413.png]]

![[Pasted image 20240613002416.png]]

Copiaremos esa línea y vamos a ejecutarla en la web, pero a través de burpsuite, por tanto escribimos un correo aleatorio para que burpsuite lo intercepte (debemos configurar el certificado SSL para que burpsuite intercepte la petición):
![[Pasted image 20240613002422.png]]

Y esta petición será interceptada por burpsuite:
![[Pasted image 20240613002427.png]]

Y pegaremos todo esto:
![[Pasted image 20240613002433.png]]

Aunque tenemos que escapar las comillas para que no haya problemas con ellas, quedando de la siguiente manera:
![[Pasted image 20240613002439.png]]

Y si enviamos todo esto, vemos cómo recibimos una respuesta donde se ejecuta el código del /etc/passwd:
![[Pasted image 20240613002444.png]]

Llegados a este punto yo puedo ejecutar cualquier código, por ejemplo un whoami en lugar de un /etc/passwd; y vemos como nos da un email:
![[Pasted image 20240613002448.png]]

