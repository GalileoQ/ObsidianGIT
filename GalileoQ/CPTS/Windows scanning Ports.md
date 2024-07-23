# Enumeración de Puertos Comunes en Windows

## Puerto 21: FTP

```python
# Herramienta: ftp
ftp <IP>

# Herramienta: Nmap
nmap -p21 <IP> --script=ftp-anon,ftp-bounce,ftp-syst
```

## Puerto 22: SSH

```python
# Herramienta: ssh
ssh <username>@<IP>

# Herramienta: Nmap
nmap -p22 <IP> --script=ssh-hostkey,ssh-auth-methods
```

## Puerto 23: Telnet

```python
# Herramienta: telnet
telnet <IP>

# Herramienta: Nmap
nmap -p23 <IP> --script=telnet-ntlm-info
```

## Puerto 25: SMTP

```python
# Herramienta: telnet
telnet <IP> 25

# Herramienta: Nmap
nmap -p25 <IP> --script=smtp-enum-users

# Herramienta: swaks
swaks --to user@example.com --server <IP> --port 25
```

## Puerto 53: DNS

```python
# Herramienta: dig
dig @<IP> <domain>

# Herramienta: nslookup
nslookup <domain> <IP>

# Herramienta: Nmap
nmap -p53 <IP> --script=dns-brute
```

## Puerto 80 y 443: HTTP y HTTPS

```python
# Herramienta: Gobuster
gobuster dir -u http://<IP> -w /path/to/wordlist
gobuster dir -u https://<IP> -w /path/to/wordlist
gobuster vhost -u http://<IP> -w /path/to/wordlist

# Herramienta: Wfuzz
wfuzz -c -w /path/to/wordlist --hc 404 http://<IP>/FUZZ
wfuzz -c --hc 404, -t 200 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -u http://devvortex.htb/ -H "Host: FUZZ.devvortex.htb"

# Herramienta: Nmap
nmap -p80,443 <IP> -A
```

## Puerto 110: POP3

```python
# Herramienta: telnet
telnet <IP> 110

# Herramienta: Nmap
nmap -p110 <IP> --script=pop3-capabilities
```

## Puerto 135: RPC

```python
# Herramienta: Nmap
nmap -p135 <IP> --script=msrpc-enum

## Puerto 139: NetBIOS

# Herramienta: Nmap
nmap -p139 <IP> --script=nbstat
```

## Puerto 143: IMAP

```python

```
# Herramienta: telnet
telnet <IP> 143

# Herramienta: Nmap
nmap -p143 <IP> --script=imap-capabilities

## Puerto 389: LDAP

# Herramienta: ldapsearch (OpenLDAP)
ldapsearch -x -h <IP> -p 389 -b "dc=example,dc=com"

# Herramienta: Impacket (ldapsearch.py)
ldapsearch.py <domain>/<username>:<password>@<IP>

# Herramienta: CrackMapExec
crackmapexec ldap <IP>
crackmapexec ldap <IP> -u <username> -p <password>

## Puerto 445: SMB

# Herramienta: smbmap
smbmap -H <IP>
smbmap -u <username> -p <password> -H <IP>

# Herramienta: smbclient
smbclient -L <IP> -U <username>%<password>
smbclient //IP/share -U <username>%<password>

# Herramienta: CrackMapExec
crackmapexec smb <IP>
crackmapexec smb <IP> -u <username> -p <password>
crackmapexec smb <IP> --shares

# Herramienta: Enum4linux
enum4linux -a <IP>

# Herramienta: Impacket (smbclient.py)
smbclient.py <domain>/<username>:<password>@<IP>

## Puerto 636: LDAP over SSL (LDAPS)

# Herramienta: ldapsearch (OpenLDAP)
ldapsearch -x -H ldaps://<IP> -b "dc=example,dc=com"

# Herramienta: CrackMapExec
crackmapexec ldap <IP> -u <username> -p <password> --ssl

## Puerto 1433: Microsoft SQL Server

# Herramienta: sqsh
sqsh -S <IP> -U <username> -P <password>

# Herramienta: CrackMapExec
crackmapexec mssql <IP>
crackmapexec mssql <IP> -u <username> -p <password>

## Puerto 3306: MySQL

# Herramienta: mysql
mysql -h <IP> -u <username> -p

# Herramienta: Nmap
nmap -p3306 <IP> --script=mysql-enum

## Puerto 3389: RDP

# Herramienta: Nmap
nmap -p3389 <IP> -sV --script=rdp-enum-encryption

# Herramienta: rdesktop
rdesktop <IP>

# Herramienta: xfreerdp
xfreerdp /v:<IP>

## Puerto 1521: Oracle Database

# Herramienta: Nmap
nmap -p1521 <IP> --script=oracle-sid-brute

# Herramienta: sqlplus
sqlplus <username>/<password>@//<IP>:1521/<SID>

## Puerto 5985: WinRM (HTTP)

# Herramienta: CrackMapExec
crackmapexec winrm <IP> -u <username> -p <password>

## Puerto 5986: WinRM (HTTPS)

# Herramienta: CrackMapExec
crackmapexec winrm <IP> -u <username> -p <password> --ssl
