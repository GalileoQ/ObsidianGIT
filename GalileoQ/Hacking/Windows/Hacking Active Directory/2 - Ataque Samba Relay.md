Para hacer este ataque, tenemos que tener el DC y la máquina cliente conectadas y bien configuradas dentro del dominio; por tanto una vez hecho esto vamos a abrir el Kali Linux y nos ubicamos a la ubicación de una herramienta que se llama responder:
![[Pasted image 20230129165605.png]]
Lo ejecutamos con estos parámetros:
![[Pasted image 20230129170551.png]]
Y se pone a envenenar el tráfico:
![[Pasted image 20230129170613.png]]
Y ahora en este punto, si desde la máquina cliente se intenta acceder a un recurso compartido que no existe, vamos a obtener el hash con las credenciales de dicha máquina.
![[Pasted image 20230129170707.png]]
Y una vez hecho este intento, en la máquina Kali Linux desde el responder habremos interceptado el hash con las credenciales de este usuario:
![[Pasted image 20230129171109.png]]
Y también podemos interceptar de misma forma el hash del usuario administrador si esta petición se hace desde el DC:
![[Pasted image 20230129171432.png]]
Y ahora con John the Ripper podemos tratar de hacer un ataque de fuerza bruta a cualquiera de estos hashes; y si la password es débil, va a funcionar correctamente como en este caso:[[John The Ripper]]
![[Pasted image 20230129171752.png]]

