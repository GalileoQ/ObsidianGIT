Heartbleed es una vulnerabilidad en la biblioteca de código abierto OpenSSL. Este bug permite a un atacante leer la memoria de un servidor o un cliente, permitiéndole por ejemplo, conseguir las claves privadas SSL de un servidor. Por tanto para practicar con esta vulnerabilidad del certificado SSL vamos a montar un contenedor de docker para ello desde este repositorio:
[vulhub/openssl/CVE-2014-0160 at master · vulhub/vulhub (github.com)](https://github.com/vulhub/vulhub/tree/master/openssl/CVE-2014-0160)
![[Pasted image 20240613002457.png]]

Lo descargamos:
![[Pasted image 20240613002501.png]]

Ahora lo montamos:
![[Pasted image 20240613002523.png]]

Y vemos que lo tenemos corriendo por el puerto 8443 de nuestra máquina que a su vez redirige al 443 del contenedor:
![[Pasted image 20240613002532.png]]

Y vemos que ya tenemos la web operativa:
![[Pasted image 20240613002537.png]]

Y ahora si le pasamos un escaneo con sslcan vemos que es vulnerable [[SSLSCAN - Analizar Certificado SSL en webs HTTPS]]:
![[Pasted image 20240613002543.png]]

Dentro de este mismo directorio, en el repositorio que nos hemos descargado, tenemos un exploit de python para lanzar este ataque llamado ssltest.py; por tanto vamos a ejecutarlo contra la máquina:
![[Pasted image 20240613002549.png]]

Y vemos que nos devuelve información de la memoria interna de la máquina:
![[Pasted image 20240613002552.png]]

Vemos que nos sale siempre lo mismo, pero podemos filtrar esta salida con el comando grep -v:
```bash
python3 ssltest.py 127.0.0.1 -p 8443 | grep -v '00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00'
```

![[Pasted image 20240613002558.png]]