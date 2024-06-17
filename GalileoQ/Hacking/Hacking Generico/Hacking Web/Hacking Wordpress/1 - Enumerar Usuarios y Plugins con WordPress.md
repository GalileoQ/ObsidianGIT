Si queremos únicamente enumerar usuarios válidos en wordpress con wpscan, lo haríamos de la siguiente forma:
```bash
wpscan --url http://10.10.123.20/wordpress/ -e u
```

![[Pasted image 20240613004929.png]]
Y con los parámetros vp, u podemos enumerar plugins vulnerables y usuarios existentes:
![[Pasted image 20240613004941.png]]

Y nos encuentra el usuario mario e incluso otro más llamado Editor:
![[Pasted image 20240613005029.png]]
## ENUMERAR VULNERABILIDADES PLUGINS WORDPRESS
Es posible que en wordpress exista vulnerabilidades incluso si se encuentra actualizado en su última versión debido a los plugins. Por tanto, de forma introductoria, podemos listar los plugins instalados en el sistema haciendo un petición con curl y filtrando en donde ponga plugins:
![[Pasted image 20240613005038.png]]

Aunque también con la herramienta wpscan podemos hacer un análisis de vulnerabilidades de los plugins de wordpress, pero necesitamos la API de esta herramienta:
![[Pasted image 20240613005044.png]]

Una vez con esta API, la podemos poner en el parámetro oportuno de la herramienta y ahora ya sí va a mostrarnos los plugins vulnerables:
![[Pasted image 20240613005116.png]]

Y nos las encuentra:
![[Pasted image 20240613005122.png]]

E incluso si filtramos ya tenemos un RCE sin estar autenticado:
![[Pasted image 20240613005126.png]]

Ahora ya con esta información podemos buscarlo en searchsploit:
![[Pasted image 20240613005131.png]]
# ENUMERACIÓN AGRESIVA DE PLUGINS WPSCAN
Podemos enumerar plugins de forma agresiva con wpscan de la siguiente forma:
```bash
wpscan --url http://192.168.0.37/wordpress -e p --plugins-detection aggressive
```

![[Pasted image 20240613005137.png]]

## ENUMERAR PLUGINS DE WORDPRESS CON NMAP
También podemos enumerar plugins de wordpress directamente con nmap:
```bash
nmap -p80 --script http-wordpress-enum --script-args http-wordpress-enum.root='/wordpress',search-limit=1000 ejemplo.com
```
