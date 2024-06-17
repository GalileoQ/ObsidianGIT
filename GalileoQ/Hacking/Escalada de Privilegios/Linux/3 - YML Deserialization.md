Para mostrar esta técnica de escalar privilegios, vamos a utilizar la máquina precious de hackthebox. 

Este ataque consiste en una técnica en la que un atacante manipula los datos de entrada en un archivo YAML para ejecutar código malicioso en un sistema vulnerable. Por tanto tenemos que buscar qué lenguaje de programación se está utilizando para llamar al tipo de fichero yml que estamos intentando explotar (que en este caso es ruby).

Por tanto vamos a ejecutar el comando sudo -l para ver los permisos de sudo asignados al usuario que somos; y podemos ver este script, donde vemos que el script hecho en ruby es el que llama al fichero yml: 
![[Pasted image 20240612155614.png]]
 Vamos a ver el contenido de este script y vemos que está llamando a un fichero que se llama dependencies.yml:  
 ![[Pasted image 20240612155623.png]]
 Vemos que este fichero ya se encuentra en nuestro actual directorio, por lo que vamos a inspeccionarlo:
 ![[Pasted image 20240612155628.png]]
  por lo que ponemos en google yaml deserialization ruby: 
  ![[Pasted image 20240612155636.png]]
  
 Y en esta web tenemos un tutorial, donde nos dice que en la parte de git set podemos insertar un comando: 
 ![[Pasted image 20240612155642.png]]
 Por lo que vamos a poner este código tal cual dentro del fichero dependencies.yml:
 ![[Pasted image 20240612155646.png]]
 
 ![[Pasted image 20240612155649.png]]
 Y ahora por ejemplo como se puede ver hemos dado la orden de crear un fichero llamado pwned, por tanto vamos a ejecutar el script y vamos a ver si llama y ejecuta correctamente este código dentro del fichero yml:
 ![[Pasted image 20240612155657.png]]
 
 Y ahora vemos que efectivamente ha creado el archivo:
 ![[Pasted image 20240612155701.png]]
 
 Vamos a dar todos los permisos a la bash:
 ![[Pasted image 20240612155715.png]]
 
 Ahora probamos en ejecutar el script: 
 ![[Pasted image 20240612155727.png]]
 
 Y ahora si ejecutamos el comando bash -p nos debería lanzar una bash como root:
 
 ![[Pasted image 20240612155732.png]]