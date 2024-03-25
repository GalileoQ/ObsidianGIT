[[MAQUINA VULNED ROASTED (enum4linux, enumeración de usuarios con impacket-lookupsid, asreproasting y kerberoasting, john the ripper y acceso con evil-winrm)]]
Podemos enumerar usuarios de un dominio con una herramienta que se llama lookupsid y encontrando como invitado de esta forma:
```bash
impacket-lookupsid guest@10.10.182.108 -no-pass
```
![[Pasted image 20230701110242.png]]
