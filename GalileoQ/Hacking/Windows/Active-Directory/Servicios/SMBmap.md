Smbmap es una herramienta de línea de comandos que se utiliza para enumerar y explorar recursos compartidos en una red SMB.
### Enumerar recursos compartidos en un servidor SMB

```python
smbmap -H <IP address or hostname>
```
Por ejemplo, para enumerar los recursos compartidos en un servidor con la dirección IP 192.168.1.100, puede ejecutar el siguiente comando, el cual mostrará una lista de los recursos compartidos en el servidor, junto con información sobre los permisos de acceso.

```python
smbmap -H 192.168.1.100
```

Y este sería el resultado:
![[Pasted image 20240615210739.png]]

### Enumerar archivos y directorios en un recurso compartido
Podemos enumerar recursos compartidos sin proporcionar credenciales haciendo uso de una null session simplemente usando el parámetro -H:

![[Pasted image 20240615210757.png]]

También podríamos conectarnos haciendo uso de una null session de esta forma:

```python
smbmap -u "null" -H 10.10.182.108
```

![[Pasted image 20240615210815.png]]

Si quisiéramos proporcionar credenciales, lo haríamos de esta forma:

```python
smbmap -H <IP address or hostname> -u <username> -p <password> -s <sharename> -R
```

Por ejemplo, para enumerar los archivos y directorios en el recurso compartido "compartido1" en un servidor con la dirección IP 192.168.1.100, puede ejecutar el siguiente comando, el cual mostrará una lista de los archivos y directorios en el recurso compartido "compartido1", junto con información sobre los permisos de acceso.

```python
smbmap -H 192.168.1.100 -u usuario -p contrasena -r compartido1
```

![[Pasted image 20240615210841.png]]

--- 
### EJEMPLO FUNCIONAMIENTO SMBMAP

Así sería el funcionamiento de smbmap
![[Pasted image 20240615210852.png]]

### DESCARGAR UN ARCHIVO CON SMBMAP
Lo haríamos con el parámetro --download:

![[Pasted image 20240615210904.png]]
