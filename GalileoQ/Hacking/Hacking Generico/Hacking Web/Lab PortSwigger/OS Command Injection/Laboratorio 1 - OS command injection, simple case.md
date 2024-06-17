En algunas interceptaciones HTTP que hagamos con burp suite, es posible que tengamos algún campo para insertar comandos, sobre todo en aquellas webs donde podemos elegir varias opciones y se nos mostrará un dato diferente en función de lo que hagamos, como en este laboratorio de portswigger:
![[Pasted image 20240613005721.png]]

Si interceptamos con burp suite y damos en check stock, nos encontramos con una petición donde vemos que se hace una búsqueda en función del comando que vayamos a utilizar:
![[Pasted image 20240613005727.png]]

En este punto podemos concatenar un comando poniendo un punto y comando después del número que se nos muestra; y lo haríamos de la siguiente forma, donde ya habremos ejecutado un comando en la máquina remota:
![[Pasted image 20240613005731.png]]
