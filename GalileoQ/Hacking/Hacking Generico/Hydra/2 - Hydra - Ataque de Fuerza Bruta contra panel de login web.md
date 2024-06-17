Vamos a suponer que estamos ante un panel de login normal como este:
![[Pasted image 20240613010235.png]]

Por tanto lo que debemos hacer es insertar unas credenciales normales e interceptar la petición con burp suite:
![[Pasted image 20240613010247.png]]

E interceptamos la petición:
![[Pasted image 20240613010301.png]]

Entonces, basándonos en los datos que nos proporciona burp suite al interceptar la
petición, debemos poner el siguiente comando con hydra (pondremos http-post-form
porque se tramita con POST, tal y como vemos en burp suite. Podremos
dvwa/loign.php porque lo podemos ver en burp suite en la primera línea. Podremos
también el username=^USER^&password=^PASS^ porque en la petición vemos la
palabra username=canario&password=canario:
![[Pasted image 20240613010313.png]]

Luego en el Login=Login failed lo ponemos para que Hydra sepa cual es la respuesta
del servidor cada vez que una password no es correcta y esto lo podemos ver cuando
introducimos una contraseña incorrecta manualmente:
![[Pasted image 20240613010321.png]]

Y nos habrá encontrado la contraseña, que en este caso es admin:
![[Pasted image 20240613010326.png]]

## ATAQUE A PANEL DE LOGIN WEB QUE CORRA POR OTRO PUERTO
Si nos encontramos con que lel login a atacar corre por un puerto diferente, tenemos que utilizar estos parámetros:
```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt 127.0.0.1 -s 443 -f http-post-form '/j_acegi_security_check:j_username=admin&j_password=^PASS^&from=%2F&Submit=Sign+in:Invalid username or password'
```

![[Pasted image 20240613010334.png]]

[[MAQUINA INTERNAL (wordpress wpscan, hydra http, portforwarding chisel y hacking jenkins en docker)]]
