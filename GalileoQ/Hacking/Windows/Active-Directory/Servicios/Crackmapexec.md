CrackMapExec es una herramienta de seguridad de red que se utiliza para escanear redes, identificar hosts y realizar ataques de credenciales.
## Listar equipos disponibles de Windows con Crakmapexec

![[Pasted image 20240615205835.png]]
## Detectar dominio y hostname con crackmapexec
Vemos que tiene abierto el puerto 445, que es el puerto smb, por tanto vamos a lanzar un comando de para ver dominios (sirve para hacer pentesting a Windows):

![[Pasted image 20240615205841.png]]

## Comprobar y validar credenciales con crackmapexec
En caso de obtener unas credenciales en texto claro, con crackmapexec podemos comprobar si estas credenciales son correctas; y vemos que sí:

![[Pasted image 20240615205855.png]]

## Listar recursos compartidos con crackmapexec
Entonces con estas credenciales válidas podemos listar recursos compartidos con crackmapexec; y podemos listar archivos dentro de varias ubicaciones, entre las que tenemos la de users:

![[Pasted image 20240615205905.png]]

## VALIDAR SI PODEMOS EJECUTAR EVIL-WINRM
Vamos a conectarnos a través de winrm (administración remota de windows), ya que con crackmapexec vemos que también podemos entrar:

![[Pasted image 20240615205915.png]]

Pues ahora vamos a explotar la conexión winrm con una herramienta que se llama evil-winrm, donde la instalaré de esta manera:

![[Pasted image 20240615205923.png]]

Y ahora ejecutamos este comando para conectarnos por winrm:
![[Pasted image 20240615205929.png]]

![[Pasted image 20240615205933.png]]
