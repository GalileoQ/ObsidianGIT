Gobuster es una herramienta de enumeración y fuzzing web que permite encontrar archivos y directorios dentro de un servidor web. Se utiliza de la siguiente forma:
```python
gobuster dir -u http://example.com -w rockyou.txt
```
### FUZZZING DE EXTENSIONES DE ARCHIVOS
Si quieremos hacer fuzzing por extensiones de archivos, lo haremos de esta forma:
```python
gobuster dir -u http://example.com -w rockyou.txt -x xml,py
```
## FUZZING DE EXTENSIONES DE ARCHIVOS OCULTOS
Podemos también hacer fuzzing de la misma forma pero buscando archivos ocultos:
```python
gobuster dir -u http://example.com -w rockyou.txt -x .xml,.py,.txt
```
### FUZZING DE SUBDOMINIOS
Para encontrar subdominios con gobuster, lo haremos de la siguiente forma:
```python
gobuster dns -d example.com -w /ruta_diccionario/subdomains-top1million-110000.txt
```
### FUZZING DE SUBDOMINIOS CON HTTPS
En caso de que estemos haciendo una búsqueda de subdominios mediante HTTPS, debemos usar el parámetro -k y además un --append-domain:
```bash
gobuster vhost --append-domain -u https://nunchucks.htb/ -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt -k
```
