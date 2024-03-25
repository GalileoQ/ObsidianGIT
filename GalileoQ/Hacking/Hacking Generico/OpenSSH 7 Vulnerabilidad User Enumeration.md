[[MAQUINA SOURCE (Servidor Webmin con SSL, exploit para atacar webmin y obtener ejecución remota de comandos)]]
Si vemos que la versión de OpenSSH es igual o inerior a la 7.7, es vulnerable a poder enumerar usuarios válidos, por lo que podremos hacer un ataque de fuerza bruta para conocer qué usuarios existen por ssh en la siguiente máquina metasploitable que tiene una versión desactualizada de OpenSSH:
![[Pasted image 20230523151158.png]]
Por lo que podemos buscar en github un exploit, donde encontramos este:
https://github.com/Sait-Nuri/CVE-2018-15473
![[Pasted image 20230403111457.png]]
No obstante, también nos hará falta un diccionario de posibles usuarios, por lo que podemos o bien crear uno o descargar uno ya hecho (en mi caso crearé uno):
![[Pasted image 20230523151315.png]]
Ejecutamos el exploit contra la IP de la máquina y nos hará el ataque:
![[Pasted image 20230523151402.png]]
