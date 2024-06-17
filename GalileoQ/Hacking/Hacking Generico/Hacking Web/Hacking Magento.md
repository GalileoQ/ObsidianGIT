Vamos a utilizar este repositorio de github para montar el laboratorio de pruebas:
![[Pasted image 20240613000530.png]]

Lo ejecutamos con docker compose:
![[Pasted image 20240613000535.png]]

Y ya tenemos el magento corriendo por el puerto 8080:
![[Pasted image 20240613000547.png]]

Y aquí ponemos esta configuración (en password ponemos root):
![[Pasted image 20240613000549.png]]
Y aquí dentro del directorio /admin se encontrará el panel de administración:
![[Pasted image 20240613000555.png]]

Y llegamos al panel de login:
![[Pasted image 20240613000602.png]]

Y vamos a explotar una SQL injection, donde tendremos este script que nos automatiza todo este proceso:
![[Pasted image 20240613000609.png]]

Iniciamos sesión dentro del magento:
![[Pasted image 20240613000615.png]]

Y ahora si lanzamos este script, podrá hacer un robo de cookies de sesión:
![[Pasted image 20240613000624.png]]

Y desde la extensión cookie editor tenemos este campo donde podemos poner una cookie de sesión:
![[Pasted image 20240613000629.png]]

Le ponemos la nueva cookie que hayamos capturado:
![[Pasted image 20240613000633.png]]

Recargamos la página y ya estamos dentro:
![[Pasted image 20240613000637.png]]

## HERRAMIENTA PARA ENUMERAR MAGENTO
Tenemos una herramienta llamada magscan:
![[Pasted image 20240613000642.png]]

Nos clonamos el proyecto:
![[Pasted image 20240613000647.png]]

Y nos pide también que descarguemos un archivo con extensión .phar:
![[Pasted image 20240613000652.png]]

Y ahora con php le pasamos este archivo y ya lo tenemos todo listo:
![[Pasted image 20240613000656.png]]

Y así le lanzamos un escaneo:

![[Pasted image 20240613000700.png]]