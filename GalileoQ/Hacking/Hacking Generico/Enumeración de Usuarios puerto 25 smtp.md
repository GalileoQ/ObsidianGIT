Para enumerar este puerto, podemos usar una herramienta que posiblemente nos devuelva usuarios válidos, llamada smtp-user-enum con la opción VRFY para verificar una lista de usuarios:
```bash
smtp-user-enum -M VRFY -U /usr/share/wordlists/metasploit/unix_users.txt -t 192.168.0.55
```
![[Pasted image 20230730152236.png]]
