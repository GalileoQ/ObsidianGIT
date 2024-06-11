El parámetro "--script" de Nmap se utiliza para especificar un conjunto de scripts que se ejecutarán durante un escaneo. 

El parámetro "--script" puede tomar un solo script como argumento, o puede especificar varios scripts separados por comas. También se pueden utilizar comodines para seleccionar múltiples scripts a la vez. Por ejemplo, --script=http-* ejecutará todos los scripts relacionados con el protocolo HTTP. El uso de scripts puede aumentar significativamente la eficacia de un escaneo de Nmap, ya que permite a los usuarios detectar vulnerabilidades y recopilar información que de otra manera sería difícil de obtener.

-----------------------------

### SCRIPT PARA HACER FUZZING

Podemos hacer fuzzing con un script de nmap, donde vamos a hacerlo sobre el puerto 80 de la [[MAQUINA OVERPASS (Obtener id_rsa modificando cookies en web, ssh2john para cracking de ir_rsa y escalada privilegios permisos suid con exploit de pkexec)]] de tryhackme para detectar directorios web:
```bash
nmap --script http-enum -p80 10.10.186.249 -vvv
```
![[Pasted image 20230501191549.png]]

### SCRIPT VULN PARA DETECTAR VULNERABILIDADES

Podemos mirar si esta máquina [[MAQUINA BLUE]] de tryhackme es vulnerable a alguna vulnerabilidad; y vemos que sí lo es a eternalblue:
![[Pasted image 20230427115145.png]]
Por tanto vamos a usar metasploit para ganar acceso al sistema explotando esta vulnerabilidad utilizando el siguiente exploit:
![[Pasted image 20230427120812.png]]
Realizamos las configuraciones básicas:
![[Pasted image 20230427120834.png]]
Configuramos el payload correspondiente, ya que el que viene por defecto da error:
![[Pasted image 20230427120914.png]]
Y si ejecutamos el comando run ya estaríamos dentro:
![[Pasted image 20230427120933.png]]
### SCRIPT DE NMAP PARA ATAQUES DE FUERZA BRUTA SSH
Para hacer un ataque de fuerza bruta al puerto ssh, podemos hacerlo con el siguiente parámetro de nmap:
```bash
nmap --script ssh-brute 192.168.0.25
```
Y nos realiza el ataque probando usuarios y contraseñas:
![[Pasted image 20230501193242.png]]
Pero en caso de conocer el nombre de usuario, que por ejemplo es mario, y querer encontrar sólo la contraseña, lo haríamos de la siguiente forma:
```bash
nmap -p 22 --script ssh-brute --script-args userdb='usuario.txt',passdb='rockyou.txt' 192.168.0.25
```
Y vemos que ahora comprueba las distintas contraseñas para el usuario mario:
![[Pasted image 20230501194124.png]]
# SCRIPT ENUMERAR SMB
Podemos usar este script para ver vulnerabilidades de smb:
```bash
nmap -p445 --script smb-protocols 192.168.0.48
```
![[Pasted image 20230727142007.png]]
También podemos saber la seguridad si permite iniciar sesión como invitado o no con este otro script, de tal forma que podríamos entrar sin proporcionar contraseña:
```bash
nmap -p445 --script smb-security-mode 192.168.0.48
```
![[Pasted image 20230727142202.png]]
# LISTAR RECURSOS COMPARTIDOS SMB
Podemos también listar recursos compartidos con nmap usando el siguiente script:
```bash
nmap -p445 --script smb-enum-shares 192.168.0.48
```
![[Pasted image 20230727150238.png]]
