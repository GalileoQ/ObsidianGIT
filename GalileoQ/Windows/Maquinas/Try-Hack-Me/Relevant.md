### Ping
```python
ping -c 1 10.10.235.94
PING 10.10.235.94 (10.10.235.94) 56(84) bytes of data.
64 bytes from 10.10.235.94: icmp_seq=1 ttl=127 time=198 ms

--- 10.10.235.94 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 197.975/197.975/197.975/0.000 ms
```

### TTL = 127 > Window

### nmap
```python
PORT      STATE SERVICE        VERSION
80/tcp    open  http           Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: IIS Windows Server
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|_  Potentially risky methods: TRACE
135/tcp   open  msrpc          Microsoft Windows RPC
139/tcp   open  netbios-ssn    Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds   Windows Server 2016 Standard Evaluation 14393 microsoft-ds
3389/tcp  open  ms-wbt-server?
| ssl-cert: Subject: commonName=Relevant
| Not valid before: 2024-02-23T00:00:13
|_Not valid after:  2024-08-24T00:00:13
|_ssl-date: 2024-02-24T00:30:18+00:00; -44s from scanner time.
| rdp-ntlm-info: 
|   Target_Name: RELEVANT
|   NetBIOS_Domain_Name: RELEVANT
|   NetBIOS_Computer_Name: RELEVANT
|   DNS_Domain_Name: Relevant
|   DNS_Computer_Name: Relevant
|   Product_Version: 10.0.14393
|_  System_Time: 2024-02-24T00:29:40+00:00
49663/tcp open  http           Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: IIS Windows Server
49667/tcp open  msrpc          Microsoft Windows RPC
49669/tcp open  msrpc          Microsoft Windows RPC
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 1h35m17s, deviation: 3h34m42s, median: -44s
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb-os-discovery: 
|   OS: Windows Server 2016 Standard Evaluation 14393 (Windows Server 2016 Standard Evaluation 6.3)
|   Computer name: Relevant
|   NetBIOS computer name: RELEVANT\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2024-02-23T16:29:42-08:00
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2024-02-24T00:29:40
|_  start_date: 2024-02-24T00:01:10

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 170.95 seconds
```

### Ports

| PORT      | STATE | SERVICE        | VERSION                                                    |
| --------- | ----- | -------------- | ---------------------------------------------------------- |
| 80/tcp    | open  | http           | Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)                    |
| 135/tcp   | open  | msrpc          | Microsoft Windows RPC                                      |
| 139/tcp   | open  | netbios-ssn    | Microsoft Windows netbios-ssn                              |
| 445/tcp   | open  | microsoft-ds   | Windows Server 2016 Standard Evaluation 14393 microsoft-ds |
| 3389/tcp  | open  | ms-wbt-server? |                                                            |
| 49663/tcp | open  | http           | Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)                    |
| 49667/tcp | open  | msrpc          | Microsoft Windows RPC                                      |
| 49669/tcp | open  | msrpc          | Microsoft Windows RPC                                      |
### Port 80

![[Pasted image 20240223205759.png]]

### Enumeracion del puerto 445

![[Pasted image 20240223215640.png]]
hemos intentado con todos los recursos, tenemos conexión con el nt4wrksv 

