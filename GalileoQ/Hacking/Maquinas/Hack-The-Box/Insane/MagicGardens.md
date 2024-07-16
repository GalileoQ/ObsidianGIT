#windows #tecnicas #Linux #Insane
### Ping

```python
ping -c 1 10.10.11.9
PING 10.10.11.9 (10.10.11.9) 56(84) bytes of data.
64 bytes from 10.10.11.9: icmp_seq=1 ttl=63 time=151 ms

--- 10.10.11.9 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 151.348/151.348/151.348/0.000 ms
```

### TTL = 63 Linuz

### nmap

```python
# Nmap 7.94SVN scan initiated Tue Jul 16 13:04:33 2024 as: nmap -p- -open -sCV --min-rate 5000 -n -Pn -oN Scan 10.10.11.9
Nmap scan report for 10.10.11.9
Host is up (0.15s latency).
Not shown: 65530 closed tcp ports (reset), 1 filtered tcp port (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT     STATE SERVICE  VERSION
22/tcp   open  ssh      OpenSSH 9.2p1 Debian 2+deb12u2 (protocol 2.0)
| ssh-hostkey: 
|   256 e0:72:62:48:99:33:4f:fc:59:f8:6c:05:59:db:a7:7b (ECDSA)
|_  256 62:c6:35:7e:82:3e:b1:0f:9b:6f:5b:ea:fe:c5:85:9a (ED25519)
80/tcp   open  http     nginx 1.22.1
|_http-title: Did not follow redirect to http://magicgardens.htb/
|_http-server-header: nginx/1.22.1
1337/tcp open  waste?
| fingerprint-strings: 
|   DNSStatusRequestTCP, DNSVersionBindReqTCP, FourOhFourRequest, GenericLines, GetRequest, HTTPOptions, Help, JavaRMI, LANDesk-RC, LDAPBindReq, LDAPSearchReq, LPDString, NCP, NotesRPC, RPCCheck, RTSPRequest, TerminalServer, TerminalServerCookie, X11Probe, afp, giop, ms-sql-s: 
|_    [x] Handshake error
5000/tcp open  ssl/http Docker Registry (API: 2.0)
|_http-title: Site doesn't have a title.
| ssl-cert: Subject: organizationName=Internet Widgits Pty Ltd/stateOrProvinceName=Some-State/countryName=AU
| Not valid before: 2023-05-23T11:57:43
|_Not valid after:  2024-05-22T11:57:43
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port1337-TCP:V=7.94SVN%I=7%D=7/16%Time=6696A837%P=x86_64-pc-linux-gnu%r
SF:(GenericLines,15,"\[x\]\x20Handshake\x20error\n\0")%r(GetRequest,15,"\[
SF:x\]\x20Handshake\x20error\n\0")%r(HTTPOptions,15,"\[x\]\x20Handshake\x2
SF:0error\n\0")%r(RTSPRequest,15,"\[x\]\x20Handshake\x20error\n\0")%r(RPCC
SF:heck,15,"\[x\]\x20Handshake\x20error\n\0")%r(DNSVersionBindReqTCP,15,"\
SF:[x\]\x20Handshake\x20error\n\0")%r(DNSStatusRequestTCP,15,"\[x\]\x20Han
SF:dshake\x20error\n\0")%r(Help,15,"\[x\]\x20Handshake\x20error\n\0")%r(Te
SF:rminalServerCookie,15,"\[x\]\x20Handshake\x20error\n\0")%r(X11Probe,15,
SF:"\[x\]\x20Handshake\x20error\n\0")%r(FourOhFourRequest,15,"\[x\]\x20Han
SF:dshake\x20error\n\0")%r(LPDString,15,"\[x\]\x20Handshake\x20error\n\0")
SF:%r(LDAPSearchReq,15,"\[x\]\x20Handshake\x20error\n\0")%r(LDAPBindReq,15
SF:,"\[x\]\x20Handshake\x20error\n\0")%r(LANDesk-RC,15,"\[x\]\x20Handshake
SF:\x20error\n\0")%r(TerminalServer,15,"\[x\]\x20Handshake\x20error\n\0")%
SF:r(NCP,15,"\[x\]\x20Handshake\x20error\n\0")%r(NotesRPC,15,"\[x\]\x20Han
SF:dshake\x20error\n\0")%r(JavaRMI,15,"\[x\]\x20Handshake\x20error\n\0")%r
SF:(ms-sql-s,15,"\[x\]\x20Handshake\x20error\n\0")%r(afp,15,"\[x\]\x20Hand
SF:shake\x20error\n\0")%r(giop,15,"\[x\]\x20Handshake\x20error\n\0");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Tue Jul 16 13:05:49 2024 -- 1 IP address (1 host up) scanned in 75.85 seconds
```

### Enumeración delpuerto 80 (http)



### Vulnerabilidades

| CVE-XXXX-XXXXX | Nombre de la vulnerabilidad | Tipo | Nivel | Link |
| -------------- | --------------------------- | ---- | ----- | ---- |
|                |                             |      |       |      |
|                |                             |      |       |      |
|                |                             |      |       |      |
