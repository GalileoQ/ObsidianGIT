`TAREA:` La función de cambio de correo electrónico de este laboratorio es vulnerable a CSRF. Para solucionar este problema, realice un ataque CSRF que cambie la dirección de correo electrónico de la víctima. Debe usar el servidor de exploits proporcionado para alojar el ataque.

El laboratorio admite el inicio de sesión con OAuth. Puede iniciar sesión a través de su cuenta de redes sociales con las siguientes credenciales: `wiener:peter`

`NOTA:` - Los navegadores impiden que se abran las ventanas emergentes a menos que se activen mediante una interacción manual del usuario, como un clic. El usuario víctima hará clic en cualquier página a la que lo redirija, por lo que puede crear ventanas emergentes mediante un controlador de eventos global como se indica a continuación

```python
<script> 
	window.onclick = () => { 
		window.open('about:blank') 
	} 
</script>
```

iniciamos sesión en la web y de inmediato nos damos cuenta que nos esta haciendo una redirección hacia endpoints diferentes tenemos un botón de continuar que probablemente necesite una cookie. en este caso la de sesión
![[Pasted image 20250722103219.png]]

luego tenemos otro botón de continuar que probablemente contenga otra cookie. aquí lo esencial seria analizar ambas solicitudes e identificar si la cookie es diferente
![[Pasted image 20250722103359.png]]

realizamos una solicitud de cambio de correo y la interceptamos para enviarla al repeater 
![[Pasted image 20250722103605.png]]