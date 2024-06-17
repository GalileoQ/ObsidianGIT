John the Ripper es una herramienta para recuperación de contraseñas. 

### EXTRAER CONTRASEÑA FICHERO ZIP

Lo primero será ver todos los scripts que tiene esta herramienta; los verwemos con el comando locate:
![[Pasted image 20240612232943.png]]

Ahora vamos a hacer la prueba de crear un fichero .zip que tenga una contraseña:
![[Pasted image 20240612232950.png]]

![[Pasted image 20240612232953.png]]
Y esto ya nos genera el archivo .zip con contraseña, pero si queremos ahora romper esa password con john the ripper, tenemos primero que extraer el hash de dicho fichero:
![[Pasted image 20240612233000.png]]

Ahora ya podemos crackear el fichero hash.txt con el fin de desencriptar la password; lo cual lo haremos estos parámetros seleccionando un diccionario y el fichero .txt con el hash extraido:
![[Pasted image 20240612233006.png]]

### OBTENER CONTRASEÑA FICHERO RAR

Lo primero será crear el fichero .rar con la contraseña:
![[Pasted image 20240612233012.png]]

Y hacemos igual que antes, extraemos el hash de este fichero con john:
![[Pasted image 20240612233016.png]]

Y de la misma forma que hicimos antes, hacemos el ataque de fuerza bruta contra este otro hash:
![[Pasted image 20240612233020.png]]

### CRACKEAR HASHES CON JOHN THE RIPPER

Podemos también romper un hash con john the ripper, por ejemplo voy a romper este hash que contiene la password 123123 en md5 y sha1:
![[Pasted image 20240612233027.png]]

Y vamos a ver que no hace falta proporcionar un diccionario porque John ya está utilizando uno por defecto:
![[Pasted image 20240612233032.png]]

Haríamos lo mismo si quisiéramos hacerlo con un hash SHA:
![[Pasted image 20240612233035.png]]

También podemos hacer esto mismo pero especificando un diccionario, por ejemplo el rockyou:
![[Pasted image 20240612233042.png]]

![[Pasted image 20240612233046.png]]
### CRACKING HASHES BCRYPT

Bcrypt es un algoritmo de hashing de contraseñas diseñado para ser seguro y resistente a los ataques de fuerza bruta y diccionario. Se utiliza comúnmente para almacenar contraseñas de usuarios en bases de datos, protegiéndolas contra el acceso no autorizado.

----------------------------------------------------

Si queremos desencriptar un hash en formato bcrypt, lo haremos con el siguiente comando:
```bash
john --format=bcrypt --wordlist=rockyou.txt hash
```
O también podemos usar hashcat:
```bash
hashcat -m 1800 hash.txt rockyou.txt
```
# CRACKEAR KEEPAS CON JOHN THE RIPPER
También podemos crackear una base de datos de keepas con keepass2john, donde podemos ejecutar este comando:
```bash
keepass2john Database.kdbx > keepas.txt
```
Y a continuación, ya podemos crackear la base de datos con normalidad:
```bash
john --wordlist=/usr/share/wordlists/rockyou.txt keepas.txt
```
![[Pasted image 20240612233102.png]]

Y efectivamente esta es la contraseña:

![[Pasted image 20240612233105.png]]