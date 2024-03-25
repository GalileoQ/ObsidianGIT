- [ ] CrackMapExec es una herramienta de seguridad de red que se utiliza para escanear redes, identificar hosts y realizar ataques de credenciales.
## Listar equipos disponibles de Windows con Crakmapexec
![[Pasted image 20230129172226.png]]
## Detectar dominio y hostname con crackmapexec
Vemos que tiene abierto el puerto 445, que es el puerto smb, por tanto vamos a lanzar un comando de para ver dominios (sirve para hacer pentesting a Windows):
![[Pasted image 20230116154627.png]]
## Comprobar y validar credenciales con crackmapexec
En caso de obtener unas credenciales en texto claro, con crackmapexec podemos comprobar si estas credenciales son correctas; y vemos que sí:
![[Pasted image 20230116160210.png]]
## Listar recursos compartidos con crackmapexec
Entonces con estas credenciales válidas podemos listar recursos compartidos con crackmapexec; y podemos listar archivos dentro de varias ubicaciones, entre las que tenemos la de users:
![[Pasted image 20230116160349.png]]
## VALIDAR SI PODEMOS EJECUTAR EVIL-WINRM
Vamos a conectarnos a través de winrm (administración remota de windows), ya que con crackmapexec vemos que también podemos entrar:
![[Pasted image 20230509120454.png]]
Pues ahora vamos a explotar la conexión winrm con una herramienta que se llama evil-winrm, donde la instalaré de esta manera:
![[Pasted image 20230509120501.png]]
Y ahora ejecutamos este comando para conectarnos por winrm:
![[Pasted image 20230509120516.png]]
![[Pasted image 20230509120520.png]]
