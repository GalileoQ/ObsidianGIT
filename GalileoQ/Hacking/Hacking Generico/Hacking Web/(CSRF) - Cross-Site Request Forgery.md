Lanzamos el laboratorio con docker-compose up -d:
![[Pasted image 20230705154253.png]]
Y ahora configuramos el /etc/hosts de esta forma:
![[Pasted image 20230705154459.png]]
Por tanto entramos a www.seed-server.com
![[Pasted image 20230705154623.png]]
Hay que tener en cuenta también que tenemos dos usuarios, los cuales son:
```bash
alice:seedalice
samy:seedsamy
```
Por lo que empezamos entrando como alice:
![[Pasted image 20230705154751.png]]
Ahora una vez dentro, vamos a los ajustes del usuario:
![[Pasted image 20230705155007.png]]
Y hacemos algún cambio con el fin de ver cómo se tramita esta petición por detrás:
![[Pasted image 20230705155030.png]]
Y esta es la petición que se está tramitando por detrás cada vez que queremos hacer algún cambio en la configuración del usuario:
![[Pasted image 20230705155317.png]]
Pero ahora podemos cambiar esta petición para que se tramite por get:
![[Pasted image 20230705155436.png]]
Y vemos que ahora tenemos la configuración del usuario mucho más manipulable:
![[Pasted image 20230705155455.png]]
Intentamos manipular la información y ahora en vez de usuario mario, desde el burpsuite ponemos el usuario pepino:
![[Pasted image 20230705155652.png]]
Y vemos que efectivamente se cambio:
![[Pasted image 20230705155708.png]]
Y ahora nos falta conseguir algún dato identificativo de algún usuario, donde si vamos a la parte members:
![[Pasted image 20230705155802.png]]
Y entramos en alguno de estos usuarios, si ponemos el cursor encima de la pestaña add friend, veremos un número identificativo para cada uno:
![[Pasted image 20230705160419.png]]
Por tanto ahora vamos a volver a la misma ventana de antes para interceptar la misma petición por GET:
![[Pasted image 20230705160034.png]]
Y ponemos por tanto en esta parte el número 57, que era el número que veíamos antes como el usuario Samy:
![[Pasted image 20230705160509.png]]
Ahora le podemos enviar esto al usuario Samy de tal forma que si hace clic en ese enlace se le habrá cambiado el nombre de usuario:
![[Pasted image 20230705160946.png]]
Ahora entramos como Samy:
![[Pasted image 20230705161029.png]]
Y nos encontramos con el mensaje que se envió anteriormente:
![[Pasted image 20230705161047.png]]
Hacemos clic en este enlace y se me habrá cambiado el nombre de usuario por pepino:
![[Pasted image 20230705161253.png]]
Por ejemplo también podemos hacer esto mismo para añadir amigos:
![[Pasted image 20230705161905.png]]
Interceptamos la petición:
![[Pasted image 20230705162037.png]]
Y enviamos el link de arriba al usuario que queramos, de tal forma que si hace clic en dicho enlace, ese usuario habrá agregado como amigo a Boby sin ningún tipo de validación:
![[Pasted image 20230705162141.png]]
Y ahora como Alice, nos llega el mensaje:
![[Pasted image 20230705162226.png]]
Hacemos clic en dicho enlace y tenemos al usuario Boby añadido sin ningún tipo de validación:
![[Pasted image 20230705162255.png]]
