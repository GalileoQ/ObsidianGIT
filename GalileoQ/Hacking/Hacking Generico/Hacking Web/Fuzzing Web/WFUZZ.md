Esta herramienta sirve para fuzzear todo tipo de parámetro, incluso para encontrar subdominios [[Detectar subdominios]].

Para buscar directorios ocultos, lo haremos con este comando y utilizando uno de los diccionarios que se encuentran en la ruta /usr/share/wordlists/dirbuster:
```python
wfuzz -c --hc 404 -t 200 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt https://google.es/FUZZ
```
Y nos encuentra dos directorios ocultos:
![[Pasted image 20240613003133.png]]

## FUERZA BRUTA PANEL DE LOGIN CON WFUZZ
También podemos hacer un ataque de diccionario con wfuzz para probar muchos posibles usuarios dentro de un panel de login, por ejemplo en una web donde si pongo un usuario, me dice si existe o no en el sistema como en este caso:
![[Pasted image 20240613003147.png]]

Por tanto yo puedo probar con muchos usuarios a ver si en alguno me dice que sí existe este usuario. Utilizando este diccionario, con WFUZZ podemos hacer estas comprobación de esta forma:
![[Pasted image 20240613003420.png]]

Pero así lo que ocurre es que me muestra cada uno de los intentos:
![[Pasted image 20240613003423.png]]

Pero con wfuzz, podemos decir que siempre y cuando el mensaje de error de la web sea el mismo, que no lo muestre; y que sólo nos muestre el usuario que genera un mensaje diferente en la web; usando el parámetro --hs:
![[Pasted image 20240613003430.png]]

Y nos encuentra el usuario Tyler:
![[Pasted image 20240613003436.png]]

[[MAQUINA SECNOTES (SQL Injection, CSRF (Cross-site request forgery), detectar usuarios válidos con wfuzz, netcat en windows, php malicioso y escalar privilegios en máquina Windows con subsistema de Linux instalado a través de ejecutar un bash.exe)]]

### FUZZEAR POR EXTENSIONES DE ARCHIVOS CON WFUZZZ
Si queremos fuzzear por extensiones de archivos, lo haremos de la siguiente forma:
```python
wfuzz -c --hc 404 -w diccionario.txt http://example.com/FUZZ.php
```
### DETECTAR SUBDOMINIOS CON WFUZZ
Podemos detectar subdominios con wfuzz de la siguiente forma:
```bash
wfuzz -c --hc=404,200 -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -H "Host: FUZZ.hunterzone.nyx" -u 192.168.0.40
```
