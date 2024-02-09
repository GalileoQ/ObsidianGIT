![[Pasted image 20240208124843.png]]


![[Pasted image 20240208125612.png]]


![[Pasted image 20240208125442.png]]


### SQLI (XSS)

```python
<script>alert("XSS")</script>
```

```python
<script>
var email= prompt("introduce tu correo");

if (email==null | email == ""){
	alert("Es necesario introducir un correo valido para visualizar el post"); 
} else {
	
}
</script>
```
