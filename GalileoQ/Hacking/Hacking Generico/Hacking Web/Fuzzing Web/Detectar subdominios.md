Podemos detectar subdominios con distintas herramientas como wfuzz o gobuster, aunque también incluso desde la web de phonebook.

Los subdominios son parte de un dominio más grande y a menudo están configurados para apuntar a diferentes recursos de la red.
## DETECTAR SUBDOMINIOS CON PHONEBOOK
En esta web podemos hacer una búsqueda de los subdominios de una web:
![[Pasted image 20230416074807.png]]
Y vemos todo lo que nos encuentra:
![[Pasted image 20230416074819.png]]
# HERRAMIENTA CTRF.PY
Esta herramienta está hecha en python y sirve también para detectar subdominios. Se trata de una herramienta de recolección de información de forma pasiva (no hace ningún ataque). Aquí tenemos el repositorio de github:
![[Pasted image 20230416075423.png]]
Y aquí tenemos la herramienta funcionando:
![[Pasted image 20230416075527.png]]
Y si seguimos con el ejemplo de ikea.com, vemos todos los subdominios:
![[Pasted image 20230416075630.png]]
# DETECTAR SUBDOMINIOS CON GOBUSTER
Podemos también enumerar subdominios con gobuster (ataque de forma activa), pero utilizando el parámetro de vhost:
![[Pasted image 20230416080053.png]]
Y ahora tenemos que usar los parámetros -u para indicar la URL y -w para indicar un diccionario, donde podemos usar un diccionario específico de subdominios del repositorio de github de seclist:
![[Pasted image 20230416080416.png]]
Nos bajamoos el diccionario de subdomains, el que queramos, y podemos hacer la búsqueda:
```bash
gobuster vhost -u https://ikea.com -w subdomains-top1million-5000.txt
```
Pero nos encontramos con todos los intentos que fueron fallidos:
![[Pasted image 20230416080640.png]]
Si quisiéramos filtrar y quitar estos códigos de error, usar´ñiamos el comando grep -v dentro de una tubería y vemos como sí encuentra:
![[Pasted image 20230416080827.png]]
# DETECTAR SUBDOMINIOS CON WFUZZ
[[WFUZZ]]
Podemos también usar WFUZZ para comprobar si hay subdominios. Donde pone –hl=83 es el parámetro hide lines de wfuzz para ocultar todas las respuestas que son iguales, ya que en esta web salían muchas de 83 que no nos interesaban:
```bash
wfuzz -c --hc=404 -t 200 --hl=83 -w directory-list-2.3-medium.txt -H "Host: preprod-FUZZ.trick.htb" -u 10.10.11.166
```
Y nos encuentra un subdominio que se llama marketing. Aunque en condiciones normales, el comando genérico sería el siguiente:
```bash
wfuzz -c -w /usr/share/wordlists/dirb/common.txt -H "Host: FUZZ.example.com" http://example.com/
```
Por ejemplo si usamos un dominio real, nos encuentra lo siguiente (quitando los códigos de errores 403 y con 200 hilos):
![[Pasted image 20230416081617.png]]
# DETECTAR SUBDOMINIOS CON SUBLIST3R
Esta es una herramienta de recolección pasiva; y podemos instalarla desde los repositorios de Kali Linux:
![[Pasted image 20230416082146.png]]
Le podemos pasar dominios:
![[Pasted image 20230416082432.png]]
