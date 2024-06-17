## Impedir Ejecucion de scripts
![[Pasted image 20240612161549.png]]

De tal forma que ahora si intentamos ejecutar cualquier script no vamos a poder:
![[Pasted image 20240612161554.png]]

![[Pasted image 20240612161557.png]]
Si queremos bypassear esta protección, podemos usar iex de la siguiente forma con el mismo script de antes:
```powershell
cat .\script.ps1|iex
```

![[Pasted image 20240612161603.png]]
También podemos bypassear esto cambiando el modo de ejecución de powershell de la siguiente forma:
```powershell
powershell -ExecutionPolicy Bypass
```

![[Pasted image 20240612161609.png]]
## Habilitar modo restringido de lenguaje
Creamos dentro del registro el siguiente archivo:

![[Pasted image 20240612161614.png]]
Y desde powershell comprobamos que esté activado con el siguiente comando:
```powershell
$ExecutionContext.SessionState.LanguageMode
```
