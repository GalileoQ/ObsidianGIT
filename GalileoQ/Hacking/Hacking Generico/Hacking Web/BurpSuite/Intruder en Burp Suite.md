Suponemos que tenemos una petición web interceptada con burpsuite; y hay un campo donde queremos hacer fuzzing, como es en este caso dentro de donde pone loaaa.php:
![[Pasted image 20230330200119.png]]
Quiero hacer fuzzing para ver cual es la URL correcta, por lo que debo hacer control + I para ir al intruder:
![[Pasted image 20230330200213.png]]
Por tanto seleccionamos la parte exacta que queramos fuzzear, que en mi caso será el aaa y hacemos click en add:
![[Pasted image 20230330200303.png]]
Ahora vamos a setting:
![[Pasted image 20230330200323.png]]
Vamos a la parte de grep extract:
![[Pasted image 20230330200353.png]]
Una vez hecho clic en Add, se nos abrirá esta ventana donde haremos clic en refetch response; y se nos muestra un ejemplo de la actual petición, ya que tenemos que fijarnos en el mensaje de error de la web, paea que burpsuite sepa cuando una petición es exitosa y cuando no (previamentre en caso de haber redirecciones, marcamos la opción de que las siga):
![[Pasted image 20230731131753.png]]
![[Pasted image 20230330200546.png]]
Seleccionamos dicho mensaje y haremos clic en OK:
![[Pasted image 20230330200655.png]]
Ahora vamos a la pestaña payloads:
![[Pasted image 20230330200845.png]]
Y en esta parte vamos añadiendo cada palabra que queramos fuzzear en ese aaa dentro de la ruta del principio:
![[Pasted image 20230330200943.png]]
Ahora hacemos clic en start attack:
![[Pasted image 20230330201000.png]]
Y vemos que con la palabra gin ha funcionado, porque la ruta del principio era login, cuando antes habia loaaa:
![[Pasted image 20230330201040.png]]
