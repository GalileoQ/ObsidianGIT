`Tarea:`
Para resolver el laboratorio, utilice su servidor de exploits para alojar una página HTML que utilice un ataque CSRF para cambiar la dirección de correo electrónico del espectador.

Tienes dos cuentas en la aplicación que puedes usar para diseñar tu ataque. Las credenciales son las siguientes:

- `wiener:peter`
- `carlos:montoya`

iniciamos sesión en el laboratorio con las credenciales de `wiener` intentamos actualizar el correo y capturamos la petición. 

podemos ver un parámetro llamado `csrf` que es nuestro token y un segundo parámetro llamado `csrfKey` enviamos esta información al repeater y vamos a iniciar con el usuario `carlos` pare ver cuales son las diferencias
![[Pasted image 20250709132926.png]]

en este caso podemos ver los mismos parámetros. vamos a enviar esta solicitud al repeater para analizarla
![[Pasted image 20250709133409.png]]

analizando las dos solicitudes nos damos cuenta que los parámetros `csrf` y `csrfKey` son diferentes para ambos usuarios. así que lo mas seguro es que el token `csrf` este vinculado al `csrfKey` así que decidí probar los token del usuario `wiener` para la solicitud de actualización de correo de `carlos` y funciona pero aquí tenemos un problema


ya que el token `csrfKey` viene configurado con la `cookie` de sesión del usuario ¡aunque sean parámetros diferentes! no podemos construir el encabezado de la `cookie` que contiene esto valores ya que no tenemos toda la información necesaria. por lo que para continuar necesitamos una segunda vulnerabilidad en la que podamos forzar a la web a devolvernos un segundo encabezado de `cookies`
![[Pasted image 20250709133640.png]]

podemos realizar mas pruebas en la web. en este caso en el apartado de `Home` podemos realizar una búsqueda de un producto y interceptar esta solicitud
![[Pasted image 20250709144124.png]]

esta solicitud hace un `GET` sobre el parámetro `/search=` y si analizamos la respuesta podemos ver una nueva `cookie` llamada `LastSearchTerm` que es igual al valor de nuestra búsqueda

ahora bien. dependiendo de como se este validando esto podemos reconstruir nuestra propia `cookie` partiendo de aquí. 
![[Pasted image 20250709144043.png]]

## Descripción

El término CRLF se refiere a Retorno **de** **Carro** (ASCII 13 ) y Avance de **Línea** **(** ASCII 10 ). Se utilizan para indicar el final de una línea; sin embargo, su manejo es diferente en los sistemas operativos actuales. Por ejemplo, en Windows se requieren tanto CR como LF para indicar el final de una línea, mientras que en Linux/UNIX solo se requiere LF. En el protocolo HTTP, la secuencia CR-LF siempre se utiliza para terminar una línea.`\r``\n`

Un ataque de inyección CRLF ocurre cuando un usuario logra enviar un CRLF a una aplicación. Esto se realiza habitualmente modificando un parámetro HTTP o una URL. [OWASP-Top10](https://owasp.org/www-community/vulnerabilities/CRLF_Injection) 

partiendo de esta información sobre las vulnerabilidades web podemos agregar un retorno de carro `%0d` a nuestra búsqueda y efectivamente funciona
![[Pasted image 20250709150430.png]]

ahora bien. la finalidad de este ataque es lograr una respuesta de estatus `200 OK` como lo vimos en la imagen de arriba para poder seguir construyendo el payload de manera que podamos hacer una solicitud completa y estructurada de una nueva cookie 

```python
GET /?search=test%0d%0aSet-Cookie:%20csrfKey=KdqFrXzWr2aWPkpedy4R1HPHJIHdvMza HTTP/2
```

- `%0d` → `\r` (Carriage Return o retorno del carro)
- `%0a` → `\n` (Line Feed o salto de linea)
- `%20` → espacio ( el espacio en blanco entre un parámetro y otro)  

Entonces, lo que se está enviando realmente es:

```python
GET /?search=test Set-Cookie: csrfKey=KdqFrXzWr2aWPkpedy4R1HPHJIHdvMza HTTP/2
```

![[Pasted image 20250709150853.png]]

### ¿Cómo funciona internamente?

1. El atacante inyecta `%0d%0a` (CRLF) dentro del parámetro `search` de la URL.
2. Esto **simula el final de una cabecera HTTP y el comienzo de otra**, rompiendo la estructura normal de las cabeceras de respuesta del servidor.
3. Luego inyecta manualmente una cabecera nueva:

```python

```
   
4. Si el servidor no valida ni escapa correctamente los datos del parámetro `search`, el navegador interpretará que la respuesta del servidor incluye **una cabecera adicional**:
    
    javascript
    
    CopiarEditar
    
    `Set-Cookie: csrfKey=KdqFrXzWr2aWPkpedy4R1HPHJIHdvMza`
    
5. El navegador aceptará esa cabecera falsa, creando una cookie en el navegador del usuario.