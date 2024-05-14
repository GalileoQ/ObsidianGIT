#ActiveDirectory #medium #tecnicas #windows
### Ping
```python
ping -c 1 10.10.11.16
PING 10.10.11.16 (10.10.11.16) 56(84) bytes of data.
64 bytes from 10.10.11.16: icmp_seq=1 ttl=127 time=151 ms

--- 10.10.11.16 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 150.560/150.560/150.560/0.000 ms
```

### TTL 127= windows

### nmap
```python
Nmap scan report for 10.10.11.16
Host is up (0.17s latency).
Not shown: 65530 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT     STATE SERVICE       VERSION
80/tcp   open  http          nginx 1.24.0
|_http-server-header: nginx/1.24.0
|_http-title: Did not follow redirect to http://solarlab.htb/
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds?
6791/tcp open  http          nginx 1.24.0
|_http-server-header: nginx/1.24.0
|_http-title: Did not follow redirect to http://report.solarlab.htb:6791/
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: -1m27s
| smb2-time: 
|   date: 2024-05-14T18:58:02
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 124.66 seconds
```

### Enumeración del puerto 80

![[Pasted image 20240514151758.png]]

### Fuzzing con wfuzz




### SMB
logramos establecer una coneccion por `SMB` y en el recurso compartido Documents obtenemos un archivo llamado `details-file.xlsx`
![[Pasted image 20240514152944.png]]

