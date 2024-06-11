#windows 
### nmap
```python
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
135/tcp   open  msrpc         Microsoft Windows RPC
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
6379/tcp  open  redis         Redis key-value store 2.8.2402
49665/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49670/tcp open  msrpc         Microsoft Windows RPC
49685/tcp open  msrpc         Microsoft Windows RPC
49698/tcp open  msrpc         Microsoft Windows RPC
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
|_clock-skew: -47s
| smb2-time: 
|   date: 2024-03-01T16:51:55
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 128.05 seconds
```

### Ports
| PORT      | STATE | SERVICE     | VERSION                               |
|-----------|-------|-------------|---------------------------------------|
| 53/tcp    | open  | domain      | Simple DNS Plus                       |
| 135/tcp   | open  | msrpc       | Microsoft Windows RPC                 |
| 445/tcp   | open  | microsoft-ds|                                       |
| 464/tcp   | open  | kpasswd5    |                                       |
| 6379/tcp  | open  | redis       | Redis key-value store 2.8.2402        |
| 49665/tcp | open  | msrpc       | Microsoft Windows RPC                 |
| 49668/tcp | open  | msrpc       | Microsoft Windows RPC                 |
| 49669/tcp | open  | ncacn_http  | Microsoft Windows RPC over HTTP 1.0   |
| 49670/tcp | open  | msrpc       | Microsoft Windows RPC                 |
| 49685/tcp | open  | msrpc       | Microsoft Windows RPC                 |
| 49698/tcp | open  | msrpc       | Microsoft Windows RPC                 |

### Enumeracion con smbclient

![[Pasted image 20240302172336.png]]

### Enumeracion con smbmap
![[Pasted image 20240302172043.png]]

### Enumeracion del puerto 6379/ redis
