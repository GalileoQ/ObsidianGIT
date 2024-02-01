### nmap 

```python
sudo nmap -sS --min-rate 5000 -p- -Pn 10.10.10.10,IP,,IP,IP -oN tcp_scan.txt
```
### grep 

```python
	grep '[0-9]$' archivo.txt
	
	grep '[0-9]$' archivo.txt | cut -d '/' -f1 | sort -u | xargs | tr ' ' ','
```

### smbclient

```python
smbclint -L IP -N 
```

### smbmap

```python
smbmap -H IP
```

### crakmapexec

```python
crakmapexec smb IP -u '' -p '' --shares
```

### enun4linux

```python
enun4linux IP
```
