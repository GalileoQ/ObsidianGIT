Por ejemplo vamos a hacer un ataque donde ya sepamos cual es el usuario (el usuario
se llama pi), pero no sabemos cual es la contraseña, por tanto para la contraseña
pondremos -P, entonces lo haremos de esta manera. Y ahora irá probando uno a uno
con el diccionario que le pasemos hasta haber acertado, donde la contraseña es
raspberry:
![[Pasted image 20240613010037.png]]

También puede darse el caso que de a medida que vaya probando intentos que nos lo vaya reportando, por tanto para esto utilizaremos la opción -vV:
![[Pasted image 20240613010106.png]]

![[Pasted image 20240613010110.png]]

También puede darse el caso de que no conozcamos el usuario pero sí la contraseña; y en estos casos pondremos la opción -L en el usuario y en la contraseña le ponemos la p minúscula; y le pasamos el diccionario:
![[Pasted image 20240613010115.png]]

Y ahora nos encontró el usuario:
![[Pasted image 20240613010144.png]]

También podemos quitar la opción de que se nos vaya reportando todo el rato cada intento quitando la opción -vV:
![[Pasted image 20240613010151.png]]

También podemos usar estos otros parámetros:
```bash
hydra -t 64 -l axel -P rockyou.txt ssh://192.168.0.39 -F -I
```

![[Pasted image 20240613010202.png]]
