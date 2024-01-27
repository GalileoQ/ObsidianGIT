#### SUID

```python
	find / -perm -u=s -type f 2>/dev/null
```
#### Capabilities

```python
	getcap -r / 2>/dev/null
```
#### Tareas programadas[](#tareas-programadas)

cat /etc/crontab