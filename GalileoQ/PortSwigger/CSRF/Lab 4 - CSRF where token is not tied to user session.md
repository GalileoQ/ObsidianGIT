Tarea: La función de cambio de correo electrónico de este laboratorio es vulnerable a ataques CSRF. Utiliza tokens para intentar prevenirlos, pero no están integrados en el sistema de gestión de sesiones del sitio.

Para resolver el laboratorio, utilice su servidor de exploits para alojar una página HTML que utilice un ataque CSRF para cambiar la dirección de correo electrónico del espectador.

Tienes dos cuentas en la aplicación que puedes usar para diseñar tu ataque. Las credenciales son las siguientes:

- `wiener:peter`
- `carlos:montoya`

ingresamos en el laboratorio como el usuario `wiener` y vamos a actualizar el correo electronico
![[Pasted image 20250704160613.png]]

vamos a enviar la solicitud al Repeater para almacenarla y hacemos `Drop` de la petición para que esta no se complete
![[Pasted image 20250704160718.png]]

ahora vamos a iniciar sesión como el usuario `carlos` y vamos a actualizar el correo interceptando la petición de nuevo
![[Pasted image 20250704161023.png]]

ahora vamos a cambiar el token `CSRF` del usuario`carlos` y usaremos el token de la primera solicitud. del usuario `wiener` 
![[Pasted image 20250704161234.png]]

hacemos clic en `Fordward` para poder enviar esta solicitud. y todas las relacionadas con esta acción. podemos notar que efectivamente el token del usuario `wiener` ha sido valido para poder actualizar el correo del usuario `carlos` 
![[Pasted image 20250704161523.png]]

ahora vamos a ejecutar nuestro ataque. vamos a repetir los pasos anteriores. necesitamos capturar el token `csrf` de alguno de los dos usuarios. en este caso el usuario `wiener` (recuerda que tiene que ser el token proporcionado para actualizar el correo. no el token de inicio de sesión) y luego vamos a hacer hacer lo mismo para el segundo usuario. `carlos` luego vamos a reemplazar el token de `carlos` y usaremos el token de `wiener` y después clic derecho en la solicitud y seleccione Herramientas de interacción ó (Engagement tools) después  > Generar CSRF PoC. como lo hemos hecho anteriormente
![[Pasted image 20250704164120.png]]

esto nos genera un `CSRF POC` en el cual estamos usando el token de `wiener` sobre el usuario `carlos` luego vamos a nuestro servidor de exploit y lo usamos. 

En una variación de la vulnerabilidad anterior, algunas aplicaciones vinculan el token CSRF a una cookie, pero no a la misma cookie que se utiliza para rastrear las sesiones. Esto puede ocurrir fácilmente cuando una aplicación utiliza dos entornos diferentes, uno para la gestión de sesiones y otro para la protección CSRF, que no están integrados.

```python
POST /email/change HTTP/1.1 Host: vulnerable-website.com Content-Type: 
application/x-www-form-urlencoded 
Content-Length: 68 Cookie: 
session=pSJYSScWKpmC60LpFOAHKixuFuM4uXWF; csrfKey=rZHCnSzEp8dbI6atzagGoSYyqJqTz5dv 

csrf=RhV7yQDO0xcq9gLEah2WVbmuFqyOq7tY&email=wiener@normal-user.com
```

![[Pasted image 20250704165216.png]]