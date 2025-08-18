`TAREA:` Este laboratorio tiene una función de verificación de existencias que obtiene datos de un sistema interno.

Para resolver el laboratorio, cambie la URL de verificación de existencias para acceder a la interfaz de administración en `http://localhost/admin`y eliminar el usuario `carlos`.

El desarrollador ha implementado dos defensas anti-SSRF débiles que deberás evitar.

nuevamente iniciamos sesión y vamos a interceptar la solicitud de `Check Stock`
![[Pasted image 20250818111225.png]]

tenemos el parámetro `StockApi` que ya sabemos que es vulnerable así que vamos a enviar esto al repeater para ver como se comporta
![[Pasted image 20250818111213.png]]

sabemos que existen algunas defensas anti SSRF que debemos baypasear para esto debemos hacer muchas pruebas e ir analizando la respuesta del servidor. 

```python
- http://127.0.0.1/
- http://127.1/
- http://127.1/admin
- Ofuscamos la  "a" con doble-URL encoding que seria esto `%2561`
```

![[Pasted image 20250818111830.png]]

con la doble ofuscación tenemos un estatus `200` 
![[Pasted image 20250818111514.png]]

vamos a ver esta solicitud en el navegador y tenemos acceso y podemos eliminar al usuario carlos
![[Pasted image 20250818111917.png]]

para ello debemos hacer una solicitud directamente al endpoint ya que si intentamos eliminarlo a mano no nos dejara
![[Pasted image 20250818112038.png]]

nos da in estado `401` Unauthorized pero la solicitud se ha echo al servidor
![[Pasted image 20250818112136.png]]

y de esta manera resolvemos el laboratorio
![[Pasted image 20250818112219.png]]