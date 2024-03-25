
El protocolo NFS utiliza el puerto 111 para la administración de servicios RPC, lo que permite la comunicación entre sistemas para compartir y acceder a archivos de manera remota a través de una red.

# EJEMPLO 1 - MÁQUINA KENOBI
En este primer ejemplo, para enumerar el puerto 111 usamos las herramientas de showmount o nmap en la máquina [[MAQUINA KENOBI (Recursos compartidos SMBMAP, monturas con showmount, vulnerabilidad proFTPd para copiar el id_rsa a la montura en máquina loca, escalada privilegios manipular path)]]:

Podemos usar el comando showmount para inspeccionar monturas:
![[Pasted image 20230617174447.png]]
Aunque también podríamos comprobar esto mismo con nmap de la siguiente forma:
![[Pasted image 20230617174749.png]]
Entonces vamos a montarnos todo lo del directorio /var de la máquina víctima a nuestro equipo:
![[Pasted image 20230617175407.png]]
![[Pasted image 20230617175416.png]]
Si vamos al directorio /tmp vemos que no tenemos la clave id_rsa:
![[Pasted image 20230617175651.png]]
Pero ahora podemos ver que gracias a que esta versión de proFTP es vulnerable, podemos copiar y pegar archivos dentro de la máquina, de tal forma que podemos obtener el id_rsa y copiarlo al directorio /var/tmp/id_rsa:
![[Pasted image 20230617180817.png]]
Por tanto ahora tenemos que crear una montura dentro del directorio /var/tmp/id_rsa, donde habíamos copiado anteriormente el archivo id_rsa y también donde teníamos permisos de montura, y ahora todo esto lo montamos en nuestra máquina local y podemos acceder al id_rsa:
![[Pasted image 20230617181229.png]]
```bash
-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEA4PeD0e0522UEj7xlrLmN68R6iSG3HMK/aTI812CTtzM9gnXs
qpweZL+GJBB59bSG3RTPtirC3M9YNTDsuTvxw9Y/+NuUGJIq5laQZS5e2RaqI1nv
U7fXEQlJrrlWfCy9VDTlgB/KRxKerqc42aU+/BrSyYqImpN6AgoNm/s/753DEPJt
dwsr45KFJOhtaIPA4EoZAq8pKovdSFteeUHikosUQzgqvSCv1RH8ZYBTwslxSorW
y3fXs5GwjitvRnQEVTO/GZomGV8UhjrT3TKbPhiwOy5YA484Lp3ES0uxKJEnKdSt
otHFT4i1hXq6T0CvYoaEpL7zCq7udl7KcZ0zfwIDAQABAoIBAEDl5nc28kviVnCI
ruQnG1P6eEb7HPIFFGbqgTa4u6RL+eCa2E1XgEUcIzxgLG6/R3CbwlgQ+entPssJ
dCDztAkE06uc3JpCAHI2Yq1ttRr3ONm95hbGoBpgDYuEF/j2hx+1qsdNZHMgYfqM
bxAKZaMgsdJGTqYZCUdxUv++eXFMDTTw/h2SCAuPE2Nb1f1537w/UQbB5HwZfVry
tRHknh1hfcjh4ZD5x5Bta/THjjsZo1kb/UuX41TKDFE/6+Eq+G9AvWNC2LJ6My36
YfeRs89A1Pc2XD08LoglPxzR7Hox36VOGD+95STWsBViMlk2lJ5IzU9XVIt3EnCl
bUI7DNECgYEA8ZymxvRV7yvDHHLjw5Vj/puVIQnKtadmE9H9UtfGV8gI/NddE66e
t8uIhiydcxE/u8DZd+mPt1RMU9GeUT5WxZ8MpO0UPVPIRiSBHnyu+0tolZSLqVul
rwT/nMDCJGQNaSOb2kq+Y3DJBHhlOeTsxAi2YEwrK9hPFQ5btlQichMCgYEA7l0c
dd1mwrjZ51lWWXvQzOH0PZH/diqXiTgwD6F1sUYPAc4qZ79blloeIhrVIj+isvtq
mgG2GD0TWueNnddGafwIp3USIxZOcw+e5hHmxy0KHpqstbPZc99IUQ5UBQHZYCvl
SR+ANdNuWpRTD6gWeVqNVni9wXjKhiKM17p3RmUCgYEAp6dwAvZg+wl+5irC6WCs
dmw3WymUQ+DY8D/ybJ3Vv+vKcMhwicvNzvOo1JH433PEqd/0B0VGuIwCOtdl6DI9
u/vVpkvsk3Gjsyh5gFI8iZuWAtWE5Av4OC5bwMXw8ZeLxr0y1JKw8ge9NSDl/Pph
YNY61y+DdXUvywifkzFmhYkCgYB6TeZbh9XBVg3gyhMnaQNzDQFAUlhM7n/Alcb7
TjJQWo06tOlHQIWi+Ox7PV9c6l/2DFDfYr9nYnc67pLYiWwE16AtJEHBJSHtofc7
P7Y1PqPxnhW+SeDqtoepp3tu8kryMLO+OF6Vv73g1jhkUS/u5oqc8ukSi4MHHlU8
H94xjQKBgExhzreYXCjK9FswXhUU9avijJkoAsSbIybRzq1YnX0gSewY/SB2xPjF
S40wzYviRHr/h0TOOzXzX8VMAQx5XnhZ5C/WMhb0cMErK8z+jvDavEpkMUlR+dWf
Py/CLlDCU4e+49XBAPKEmY4DuN+J2Em/tCz7dzfCNS/mpsSEn0jo
-----END RSA PRIVATE KEY-----
```
Y ahora tan solo tenemos que copiar el contenido de este id_rsa, otorgarle permisos chmod 600 y acceder a la máquina de esta forma:
![[Pasted image 20230617181447.png]]
# EJEMPLO 2 - MÁQUINA REMOTE

Para este ejemplo, usaremos la máquina [[conocimiento/Hacking/CTF/HackTheBox/MAQUINA REMOTE|MAQUINA REMOTE]], por lo que continuamos enumerando el puerto 111 donde corre el protocolo de NFS, por lo que tenemos tanto las herramientas de nmap como showmount, donde nos encontramos con la montura de /site-backups:
![[Pasted image 20231202130439.png]]
En este punto, podemos montar una carpeta en el directorio /mnt y así accedemos al directorio /site-backups:
```python
mount -t nfs 10.10.10.180:/site_backups /mnt/
```
![[Pasted image 20231202131013.png]]
E identificamos el directorio /umbraco, donde posiblemente podamos encontrar algunas credenciales, pero ello estará dentro del directorio /App_Data y analizando el fichero Umbraco.sdf:
![[Pasted image 20231202131317.png]]
Lo abrimos con el comando strings y vemos información de un usuario administrador:
![[Pasted image 20231202131336.png]]
Y nos fijamos en su hash:
![[Pasted image 20231202131408.png]]
Lo intentamos crackear en crackstation, donde vemos que la contraseña es baconandcheese:
![[Pasted image 20231202131455.png]]
```bash
Admin@htb.local:baconandcheese
```
