Vamos a configurar un entorno de active directory donde habrá un domain controller y otro equipo windows vinculado al dominio. Por tanto lo primero será tener instalado un Windows Server:

![[Pasted image 20240615211005.png]]

Lo primero podemos ponerle un nombre al domain controller:
![[Pasted image 20240615211011.png]]

Ahora debemos abrir la consola de administración:
![[Pasted image 20240615211019.png]]

Y debemos ir al apartado de agregar roles y características:
![[Pasted image 20240615211025.png]]

Elegimos la primera opción:
![[Pasted image 20240615211031.png]]

Aquí dejamos todo igual:
![[Pasted image 20240615211039.png]]

Y aquí dejaremos activada la opción para instalar el Active Directory:
![[Pasted image 20240615211048.png]]

Damos en siguiente dentro de los siguientes menús y lo instalamos:
![[Pasted image 20240615211104.png]]

Y unavez que esté esto instalado, debemos activarlo, ya que veremos que nos salta un triángulo de advertencia en la parte de arriba a la derecha:
![[Pasted image 20240615211112.png]]

Se nos abrirá esta ventana:
![[Pasted image 20240615211118.png]]

Y en este punto vamos a seleccionar la opción de agregar un nuevo bosque y ponemos el nombre de dominio raiz que queramos:
![[Pasted image 20240615211125.png]]

Y ahora nos pide una contraseña, yo pondré la misma que tiene mi DC que es 124816mario:
![[Pasted image 20240615211133.png]]

Daremos todo a siguiente e instalamos lo que falte. Ahora desde otro equipo Windows ya podremos conectarnos al dominio, tal y como podemos ver tras aplicar el reinicio:
![[Pasted image 20240615211140.png]]

Por tanto ahora para no tener problemas, reiniciamos el DC y vamos a deshabilitar el antivirus con este comando:
![[Pasted image 20240615211148.png]]

Ahora vamos a reiniciar el sistema:
![[Pasted image 20240615211155.png]]

Ahora desde otra máquina Windows tenemos que establecer una IP manual e indicar que en la dirección DNS esté apuntando a la IP de nuestro servidor que configuramos antes, por lo que abrimos otra máquina cliente:
![[Pasted image 20240615211203.png]]

![[Pasted image 20240615211209.png]]

Y ahora desde la máquina cliente ya podemos hacer un ping al dominio:
![[Pasted image 20240615211216.png]]

Recordemos que el nombre del dominio podemos sacarlo dentro de este menú del DC:
![[Pasted image 20240615211224.png]]

Ahora a nivel de dominio vamos a crear un nuevo usuario para que se conecte desde la máquina cliente, por lo que vamos a abrir la ventana de Usuarios y equipos de Active Directory:
![[Pasted image 20240615211232.png]]

Vamos a la parte de Users:
![[Pasted image 20240615211238.png]]

Y creamos un nuevo usuario llamada pinguhack:
![[Pasted image 20240615211245.png]]

Pondré esta configuración en la contraseña y haré que nunca expire (contraseña Password1, con la p en mayúscula):
![[Pasted image 20240615211251.png]]

![[Pasted image 20240615211257.png]]

Ahora desde la máquina cliente, para que termine de estar totalmente conectado al dominio, vamos a abrir el menú de obtener acceso a trabajo o escuela:
![[Pasted image 20240615211305.png]]

Hacemos clic en conectar y se nos abre esta ventana, donde haremos clic en Unir este dispositivo a un dominio local de Active DIrectory:
![[Pasted image 20240615211312.png]]

Y aquí ponemos el nombre de nuestro dominio:
![[Pasted image 20240615211319.png]]

Y ahora aquí entramos con el usuario que hemos creado anteriormente desde el DC:
![[Pasted image 20240615211325.png]]

Y vemos que se nos abre esta ventana de que funcionó correctamente y le podemos dar a omitir:
![[Pasted image 20240615211331.png]]

Y ahora por aquí iniciamos sesión:
![[Pasted image 20240615211338.png]]


