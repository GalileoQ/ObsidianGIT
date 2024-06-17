El primer paso será entrar en alguna de las imágenes que se nos muestran:
![[Pasted image 20240613005811.png]]

Y vamos a recargar esta página e interceptamos la petición y nos saldrán 2. En la primera nada interesante:
![[Pasted image 20240613005815.png]]

Damos a forward y nos fijamos que en la segunda petición se realiza una búsqueda donde selecciona cada una de las imágenes, tal y como se puede ver en el  filename=6.png donde se busca dentro del directorio /image:
![[Pasted image 20240613005822.png]]

Viendo esto, es posible que si encontramos algún punto donde insertar algún archivo, podamos llamar al mismo desde esta parte. Por tanto vamos a mirar en el apartado de submit feedback y desde ahí interceptamos la petición:
![[Pasted image 20240613005825.png]]

![[Pasted image 20240613005828.png]]

Una vez interceptada la petición, podemos colar información dentro del campo de correo:
![[Pasted image 20240613005833.png]]

Vamos a concatenar un comando en el parámetro del correo donde vamos a crear un nuevo archivo que tenga en su interior la palabra whoami en un fichero llamado pwned.txt dentro de la ruta images:
![[Pasted image 20240613005837.png]]

Ahora que tenemos este archivo creado, podemos llamarlo desde cualquiera de los artículos que hay en la web, tal y como vimos en el principio, ya que las imágenes de cada artículo también se guardan en el directorio /images. Por tanto enviamos esta petición y vemos que se procesó correctamente:
![[Pasted image 20240613005844.png]]

Ahora vamos a entrar en alguna de los artículos:
![[Pasted image 20240613005847.png]]

Y al interceptar la petición nos encontramos con que podemos llamar al archivo pwned.txt que hemos llamado anteriormente:
![[Pasted image 20240613005852.png]]

![[Pasted image 20240613005855.png]]
Enviamos esta petición y ya estaría completado el laboratorio.

