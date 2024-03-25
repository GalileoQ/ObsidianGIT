### DEFINICIÓN PROTOCOLO KERBEROS

Kerberos es un protocolo de autenticación que se utiliza en sistemas Windows para proporcionar seguridad en la autenticación de usuarios y servicios. Cuando un usuario se autentica en un sistema Windows, se solicita un ticket de servicio para el servicio al que está intentando acceder. Este ticket de servicio contiene una clave secreta que se utiliza para cifrar las comunicaciones entre el usuario y el servicio.

![[Pasted image 20230218134749.png]]

---

Este ataque consiste en capturar un Ticket Granting Service (TGS), el cual es un hash con las credenciales del administrador en caso de que el usuario SVC_TGS sea kerberoasteable. Para hacer este ataque debemos tener credenciales válidas del usuario kerberoasteable (las tenemos del SVC_TGS).

Lo primero será sincronizar el reloj con la misma hora que el domain controller, y para ello tenemos una herramienta que se llama ntpdate:
![[Pasted image 20230126103119.png]]
Para poder comprobar si podemos lanzar este ataque podemos solicitar un TGS con este comando y vemos que el usuario administrador es kerberoasteable:
![[Pasted image 20230126104404.png]]
Entonces viendo que podemos solicitar un TGS con las credenciales de este usuario, podemos ejecutar el mismo comando pero pasando la opción -request:
![[Pasted image 20230126104553.png]]

Por tanto una vez obtenido este hash, podemos tratar de crackearlo para que nos proporcione la contraseña del usuario administrador. Por lo que vamos a copiar todo este hash y lo vamos a guardar en un documento .txt:

![[Pasted image 20230126104909.png]]
Lanzamos el ataque con john para crackear este hash y obtenemos la contraseña:
![[Pasted image 20230126105336.png]]
Ahora vamos a validar con crackmapexec que esta contraseña sirva para el usuario administrador; y vemos que sí:
![[Pasted image 20230126105512.png]]
Por tanto ahora vamos a conectarnos de manera remota con estas credenciales utilizando psexec.py y conseguimos una cmd como el usuario Administrator:
![[Pasted image 20230126105848.png]]

----------------------------

[[MAQUINA ACTIVE (Recursos Compartidos de SMB y listar recursos compartidos con smbmap y desencriptar contraseñas con gpp-decrypt - fichero groups.xml, smbclient, smbmap y kerberoasting con TGS)]]

## KERBRUTE

Kerbrute es una herramienta para realizar un ataque de fuerza bruta sobre Kerberos en una máquina víctima. Por lo que este es su repositorio:
![[Pasted image 20230425150757.png]]
Y procedemos con su instalación, por lo que en primer lugar lo clonamos y lo instalamos de esta forma, simplemente ejecutando el fichero requirements para que queden instaladas todas las dependencias:
![[Pasted image 20230425150924.png]]
Y con el comando kerbrute ya vemos las instrucciones de cómo utilizarlo:
![[Pasted image 20230425151011.png]]
Ahora tendremos que tener un diccionario de usuarios y otro de contraseñas (que podemos usar los que nos proporcionan en la propia web de tryhackme):
![[Pasted image 20230425154502.png]]
![[Pasted image 20230425154603.png]]
## BÚSQUEDA DE USUARIOS DOMINIO
Y este es el parámetro que debemos usar para hacer una búsqueda de usuarios válidos del dominio:
```bash
python3 kerbrute.py -users userlist.txt -passwords passwordlist.txt -domain spookysec.local -t 100
```
Y en el resultado vemos varios usuarios, aunque hay uno de ellos que no requiere autenticación:
![[Pasted image 20230425154919.png]]
