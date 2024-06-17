### Como enumerar procesos de windows
```bash
ps
```

![[Pasted image 20240615205514.png]]

O también podemos enumerar procesos con el comando net start:
![[Pasted image 20240615205520.png]]
### Conocer PID de un proceso
```bash
pgrep explorer.exe
```

![[Pasted image 20240615205529.png]]

O también podemos ver los PID de todos los procesos con el siguiente comando:
```bash
wmic ervice list brief
```

![[Pasted image 20240615205537.png]]
### Migrar a un proceso
```bash
migrate 2176
```

![[Pasted image 20240615205608.png]]
### Enumerar tareas programadas

```bash
tasklist /SVC
```

![[Pasted image 20240615205623.png]]

O también tenemos este otro comando:
```bash
schtasks /query /fo LIST
```

![[Pasted image 20240615205628.png]]