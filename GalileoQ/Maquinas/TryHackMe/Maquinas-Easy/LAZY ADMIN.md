Hacemos el escaneo de nmap:
![[Pasted image 20230705123730.png]]
Viendo el puerto 80 nos encontramos con una plantilla por defecto de apache:
![[Pasted image 20230705123815.png]]
Probamos en hacer fuzzing y nos encontramos con los directorios /content:
![[Pasted image 20230705123932.png]]
Nos encontramos con un aviso de que esta web está en mantenimiento, además del nombre de un CMS.
![[Pasted image 20230705124006.png]]
Podemos hacer fuzzing nuevamente pero ahora sobre este otro directorio de content, donde encontramos más directorios:
![[Pasted image 20230705125909.png]]
Pero dentro de inc, vemos una carpeta de mysql:
![[Pasted image 20230705130038.png]]
Por lo que entramos en ella y nos encontramos con un fichero de .sql:
![[Pasted image 20230705130058.png]]
Y ahora dentro de /as tenemos un panel de login:
![[Pasted image 20230705130208.png]]
Pero si seguimos mirando el archivo .sql, podemos hacer un grep para ver si encontramos algo de password o pass; y vemos que sí:
```bash
manager:42f749ade7f9e195bf475f37a44cafcb
```
![[Pasted image 20230705130344.png]]
No obstante, esta contraseña viene en formato de hash, por lo que tenemos que deshasearla usando por ejemplo crackstation:
![[Pasted image 20230705130515.png]]
Por lo que ahora las credenciales son:
```bash
manager:Password123
```
Dentro de esta plataforma, debemos de buscar algún lugar para subir archivos maliciosos, por lo que vamos dentro de post y create:
![[Pasted image 20230705130701.png]]
Y nos encontramos con un botón de add file:
![[Pasted image 20230705130718.png]]
Por tanto vamos a subir un .php malicioso:
