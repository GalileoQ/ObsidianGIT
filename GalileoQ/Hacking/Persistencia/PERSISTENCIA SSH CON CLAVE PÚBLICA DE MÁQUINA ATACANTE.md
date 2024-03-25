Podemos pasarle el id_rsa de mi máquina de atacante a la máquina remota si le pasamos el id_rsa dentro de su directorio .ssh. Por lo que primero en mi máquina atacante creamos el id_rsa:
![[Pasted image 20230731213220.png]]
Le cambiamos el nombre a authorized_keys:
![[Pasted image 20230731213251.png]]
Y dentro de la máquina víctima eliminamos la clave pública que ya tenía:
![[Pasted image 20230731213346.png]]
A continuación nos compartimos este fichero de authorized_keys con la máquina víctima:
![[Pasted image 20230731213422.png]]
Y si iniciamos sesión, ya no debería de pedirnos la contraseña:
![[Pasted image 20230731213453.png]]
# PERSISTENCIA USANDO LA CLAVE PRIVADA ID_RSA DE LA MÁQUINA VÍCTIMA
También podemos capturar el id_rsa para entrar dentro de la máquina víctima sin proporcionar credenciales:
![[Pasted image 20230801110146.png]]
Tenemos la clave privada de la máquina víctima, donde tendremos que darle los permisos:
![[Pasted image 20230801110236.png]]
Y ahora si la usamos para iniciar sesión, no debería pedirnos credenciales:
![[Pasted image 20230801110311.png]]

# PERSISTENCIA SSH COMO ROOT

Para realizar pivoting o conseguir una mejor persistencia puede ser muy útil obtener un acceso como root vía ssh una vez hayamos escalado privilegios, por tanto primero debemos ser usuario root:
![[Pasted image 20230830084514.png]]
Y abrimos el fichero de configuración de ssh, donde debemos ver que el acceso por ssh como root esté habilitado:
```bash
sudo nano /etc/ssh/sshd_config
```
![[Pasted image 20230830084706.png]]
Ahora desde la máquina atacante, dentro del directorio de /root/.ssh debemos crear una clave pública que enviaremos a la máquina víctima:
![[Pasted image 20230830084904.png]]
![[Pasted image 20230830084945.png]]
Este id_rsa.pub lo llamaremos authorized_keys:
![[Pasted image 20230830085020.png]]
Y nos compartimos el authorized_keys con la máquina víctima:
![[Pasted image 20230830085113.png]]
![[Pasted image 20230830085258.png]]
Y ahora si entramos vía ssh como root a la máquina víctima, ya hemos conseguido persistencia como root:
![[Pasted image 20230830085344.png]]
