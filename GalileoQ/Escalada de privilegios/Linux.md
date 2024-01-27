#### SUID

```python
	find / -perm -u=s -type f 2>/dev/null
```
#### Buscar binarios para escalada de privilegios
#### Capabilities

```python
	getcap -r / 2>/dev/null
```
#### Tareas programadas

```python
	cat /etc/crontab
```
