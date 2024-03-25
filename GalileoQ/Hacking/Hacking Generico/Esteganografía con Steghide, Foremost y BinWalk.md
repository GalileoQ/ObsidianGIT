La esteganografía sirve para ocultar información en algún archivo; y para ello vamos a utilizar una herramienta que se llama steghide, la cual podemos ejecutar el comando steghide --help para ver las distintas opciones que tiene:
![[Pasted image 20230224124914.png]]
### OCULTAR MENSAJE OCULTO EN IMAGEN CON STEGHIDE
Por tanto yo tengo esta imagen que será donde voy a ocultar la información:
![[Pasted image 20230224125053.png]]
Y también este mensaje que será el que vamos a guardar en la imagen:
![[Pasted image 20230224125127.png]]
Y este sería el comando que vamos a utilizar, donde nos pedirá que proporcionemos una contraseña, que en mi caso será 123123:
![[Pasted image 20230224125311.png]]
Si vamos a la carpeta, vemos que aparentemente las dos fotos son idénticas:
![[Pasted image 20230224125405.png]]
## EXTRAER MENSAJE OCULTO DE IMAGEN CON STEGHIDE
Para extraer el mensaje oculto de una imagen con steghide, debemos utilizar este comando:
![[Pasted image 20230224125623.png]]
Y tras proporcionar la contraseña que habíamos introducido en el momento de la ocultación, ya accedemos al fichero resultante:
![[Pasted image 20230224125707.png]]
# FOREMOST
Foremost es otra herramienta alternativa para realizar esteganografía; y se utiliza de esta forma [[MAQUINA AGENT SUDO (User agent burpsuite, esteganografía con binwalk, steghide y foremost, cracking con zip2john y escalada de privilegios linux exploit suggester versión vulnerable de sudo)]]:
![[Pasted image 20230601214703.png]]
# BINWALK
Binwalk es una herramienta de análisis de archivos binarios utilizada para identificar y extraer información incrustada dentro de ellos. Está diseñada principalmente para analizar imágenes de firmware y archivos empaquetados, como archivos ejecutables, imágenes de disco, firmware de dispositivos, archivos comprimidos, entre otros. Por ejemplo analizamos esta imagen y vemos que tiene varios archivos dentro:
![[Pasted image 20230601214121.png]]
