Se trata de robar una credencial de usuario en forma de hash; y luego sin descifrar, la reutiliza para engañar a un sistema de autenticación para que cree una nueva sesión autenticada en la misma red. Es decir, se trata de una técnica que nos permite loguearnos con un usuario simplemente proporcionando su hash; y para ello podemos usar psexec.
![[Pasted image 20230425163121.png]]
[[MAQUINA ATTACKTIVE DIRECTORY (enum4linux, kerbrute enumerar usuarios, ASREPRoasting obtener TGT, john the ripper, smbclient para listar recursos compartidos, secredump dumpear hashes y pass the hash)]]

-----------------------------

## impacket wmiexec

También podemos hacer esto mismo con wmiexec de impacket de la siguiente forma, donde hacemos un pass the hash y nos autenticamos: 
![[Pasted image 20230712113833.png]]
Y aquí otro ejemplo de como iniciar sesión pero con un usuario y contraseña:
```bash
a-whitehat:bNdKVkjv3RR9ht
```
Por tanto, con estas credenciales, podemos probar en acceder con wmiexec:
```php
evil-winrm -i vulnnet-rst.local -u a-whitehat -p "bNdKVkjv3RR9ht"
```
Y con esto deberíamos estar dentro y poder obtener la flag de user, aunque es posible que la máquina tenga errores y no funcione el acceso:
![[Pasted image 20230712114934.png]]
[[MAQUINA VULNED ROASTED (enum4linux, enumeración de usuarios con impacket-lookupsid, asreproasting y kerberoasting, john the ripper y acceso con evil-winrm)]]
