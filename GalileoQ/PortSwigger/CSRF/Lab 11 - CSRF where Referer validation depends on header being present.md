`TAREA:` La función de cambio de correo electrónico de este laboratorio es vulnerable a CSRF. Intenta bloquear solicitudes entre dominios, pero cuenta con una alternativa insegura.
Para resolver el laboratorio, utilice su servidor de exploits para alojar una página HTML que utilice un ataque CSRF para cambiar la dirección de correo electrónico del espectador.

Puede iniciar sesión en su propia cuenta utilizando las siguientes credenciales: `wiener:peter`

iniciamos sesión en el laboratorio y luego interceptamos una solicitud de inicio de sesión. como sabemos que la validación de esta solicitud depende depende de la referencia del encabezado directamente ya podemos ver que el valor referer identifica al usuario wiener
![[Pasted image 20250722114214.png]]

podemos eliminar el referer y la solicitud sigue siendo un `200 OK` en este caso `302 Found` porque es valido solo debemos seguir el redireccionamiento. ok vamos a construir nuestro CSRF
![[Pasted image 20250722114703.png]]

creamos nuestro CSRF-POC y una parte muy importante es que debemos agregar una linea para que podamos suprimir la validación del referer.

```python
<head><meta name="referrer" content="no-referrer"></head>
```

![[Pasted image 20250722115040.png]]

enviamos nuestra CSRF