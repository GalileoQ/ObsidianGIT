El "**bypass**" del login mediante el GRUB implica acceder a un sistema Linux sin pasar por el proceso habitual de inicio de sesión. Esto se logra mediante la manipulación del cargador de arranque GRUB, que es responsable de iniciar el sistema operativo. Al realizar ciertos cambios en la configuración del GRUB, se puede ingresar directamente al sistema sin requerir credenciales de inicio de sesión, lo cual puede representar un riesgo de seguridad si no se protege adecuadamente.

Podemos seguir el tutorial desde la siguiente web:
```bash
https://www.curiosidadesdehackers.com/2023/12/bypass-maestro-saltando-el-login-desde.html
```

----------------

 Para ello, lo que haremos será modificar unas líneas en grub para que en lugar de realizar un arranque convencional, se ejecute una Shell con los privilegios de un super usuario. Primero encienda la máquina y espere hasta que muestre el cargador de arranque grub pantalla:
 
 ![[Pasted image 20240612163252.png]]
 
 En este punto, para abrir la terminal de grub, debemos pulsar la tecla e en la pantalla de selección de OS a arrancar:
 ![[Pasted image 20240612163259.png]]
 
 En este punto, tenemos que hacer algunos cambios primero tenemos que hacer cambia la "**ro**" significa leer solo para "**rw**" significa "**read&write/lectura y escritura**" y en la última de la línea nosotros necesitamos agregar **"init=/bin/bash**":
![[Pasted image 20240612163318.png]]


 Cuando se realizan todos los cambios presione **CTRL+X** para guardar y salir:
 ![[Pasted image 20240612163325.png]]
 
 Podemos acceder a todos los archivos y vamos a cambiar la contraseña del usuario que necesitemos:
 ![[Pasted image 20240612163329.png]]
 
 Una vez cambiada la contraseña reiniciaremos mediante "reboot -f" e iniciaremos sesión con la contraseña que hayamos modificado y habremos entrado correctamente.
 ## SOLUCIÓN
 Para proteger el grub tendremos que hacer lo siguiente. Donde primero tendremos que hashear nuestra contraseña con grub-mkpasswd-pbkdf2:
  ![[Pasted image 20240612163425.png]]
  
  Y dentro del archivo /etc/grub.d/10_linux  añadimos la siguiente línea:
```bash
cat << EOF
set superusers="kali,root"
password_pbkdf2 kali grub.pbkdf2.sha512.10000.931382FD1FC4FE705B34DF0D38C84D448F64554F82386725E17B4CE30BC15D62F09672B1A4F31BD9B737F2045D46501678905A1114EB14692603B6BE13823EF7.75B35648CAA78BFC428477DBADA8DE0EC>
password_pbkdf2 root grub.pbkdf2.sha512.10000.931382FD1FC4FE705B34DF0D38C84D448F64554F82386725E17B4CE30BC15D62F09672B1A4F31BD9B737F2045D46501678905A1114EB14692603B6BE13823EF7.75B35648CAA78BFC428477DBADA8DE0EC>

```


Una vez hecho esto, guardamos y ejecutamos los siguientes comandos:
```bash
sudo update-grub
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

![[Pasted image 20240612163442.png]]

![[Pasted image 20240612163459.png]]

Ahora si intentamos acceder al grub, nos pedirá contraseña:

![[Pasted image 20240612163456.png]]