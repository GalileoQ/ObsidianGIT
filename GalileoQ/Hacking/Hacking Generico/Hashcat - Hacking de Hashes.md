## PREPARATIVOS
Lo primero será cifrar una contraseña de prueba en el formato que queramos, por ejemplo en nuestro caso utilizaremos md5 en esta web:
```
https://emn178.github.io/online-tools/md5.html
```
![[Pasted image 20230123135013.png]]
Pondremos el hash que queramos descifrar en un documento txt:
![[Pasted image 20230123135328.png]]
Y ahora el siguiente paso será conocer los números que corresponden a cada uno de los tipos de hashes, ya que en función del hash que vamos a crackear debemos de proporcionarle un número, por tanto pondremos man hashcat:
![[Pasted image 20230123140723.png]]
![[Pasted image 20230123135225.png]]
## CRACKEANDO MD5
Y ahora ya podemos lanzar el ataque, donde tendremos que tener un diccionario (por ejemplo rockyou.txt) y el fichero txt donde tengamos el hash que queramos crackear; y para ello usaremos este comando (con -m indicamos el 0, que era el que corresponde a md5 como vimos en la captura anterior):
![[Pasted image 20230123135953.png]]
Y ahora este comando nos habrá generado un fichero llamado resultado.txt con la contraseña deshasheada:
![[Pasted image 20230123140047.png]]
## CRACKEANDO SHA1
Para ello la única diferencia es en vez de poner el -m 0, ahora pondremos -m 100, que es el número que corresponde para indicar el sha1; por tanto vamos a generar la misma contraseña pero como sha1:
![[Pasted image 20230123140346.png]]
Y ahora la pegamos en un nuevo fichero:
![[Pasted image 20230123140413.png]]
Y lanzamos el ataque de esta otra forma:
![[Pasted image 20230123140449.png]]
Y tenemos el resultado generado en este otro fichero:
![[Pasted image 20230123140520.png]]
## CONOCER EL TIPO DE HASH CON HASH-IDENTIFIER
Para utilizar la herramienta hashcat, debemos de indicarle el tipo de hash a descifrar, por tanto hay una herramienta que se llama hash-identifier en kali linux que si le proporcionamos un hash nos dirá cual es:
![[Pasted image 20230124161807.png]]
Por ejemplo vamos a pasarle un hash en md5:
![[Pasted image 20230124161847.png]]
Por tanto, sabiendo esto ya podemos utilizarlo en la herramienta hashcat.