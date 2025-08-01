Tarea: La funcionalidad de cambio de correo electrónico de este laboratorio es vulnerable a CSRF. Para resolver el laboratorio, utilice su servidor de exploits para alojar una página HTML que utilice un ataque CSRF para cambiar la dirección de correo electrónico de la victima. Puede iniciar sesión en su propia cuenta utilizando las siguientes credenciales: `wiener:peter`

interceptamos la solicitud y la enviamos al repeater. de esta manera podemos analizar la solicitud  
![[Pasted image 20250703174201.png]]

si eliminamos el token la respuesta nos indica que falta el parámetro `csrf` 
![[Pasted image 20250703174558.png]]

si eliminamos los parámetros `email` y `csrf` la respuesta nos indica que falta el parámetro email 
![[Pasted image 20250703174652.png]]

ok. parece que el parámetro `csrf` solo tiene una validación si este se encuentra presente. por lo que podemos simplemente eliminar este parámetro y crear nuestra etiqueta `csrf` para poder realizar el ataque
![[Pasted image 20250703175706.png]]

### OPCIÓN 2 

```python
`<form method="POST" action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email"> 
	<input type="hidden" name="$param1name" value="$param1value"> 
</form> 
<script> 
	document.forms[0].submit(); 
	</script>`
```

usando la platilla proporcionada o usando uno de los métodos que podemos encontrar en internet y que les proporcione anteriormente podemos crear una solicitud `CSRF` y hacer las modificaciones necesarias

en este caso la plantilla que nos entregan tiene los valore `"$param1name"` y `"$param1value"` podemos modificar estos valores para inyectar los parámetros `email` `value` 
![[Pasted image 20250703180113.png]]

esto sucede porque algunas aplicaciones no validan que el token pertenezca a la misma sesión que el usuario que realiza la solicitud. En su lugar, la aplicación mantiene un conjunto global de tokens emitidos por ella misma y acepta cualquier token que aparezca en este conjunto.

En esta situación, el atacante puede iniciar sesión en la aplicación usando su propia cuenta, obtener un token válido y luego proporcionar ese token al usuario víctima en su ataque CSRF. también podemos eliminar este token ya que la web no valida si existe o no y simplemente obtiene uno de los que tiene almacenados