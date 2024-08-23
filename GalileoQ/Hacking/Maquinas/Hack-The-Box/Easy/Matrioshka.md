![[banner.png]]
### Machine Name: Matrioshka/Mamushka

`​DDth:22 Month:08 YYYY:2024`

Machine Author(s): [GalileoQ](https://app.hackthebox.com/profile/overview) - [b0ySie7e](https://app.hackthebox.com/users/417609) 

### Description:
This machine combines various vulnerabilities in a Docker container environment. The machine includes two containers, each with its own set of challenges.

The first container runs a WordPress website that uses a plugin vulnerable to SQL injection. By exploiting this vulnerability, it is possible to gain access to the database and eventually compromise the system.

The second container runs an HTTP server vulnerable to remote code execution. This vulnerability allows attackers to gain access to the container. Furthermore, this container is configured with a Docker breakout vulnerability, allowing attackers to escalate privileges and compromise the host.

The Matrioshka challenge tests players' ability to identify and chain multiple attack vectors, from exploiting web vulnerabilities to escalating privileges using advanced Docker breakout techniques.

### Difficulty:

`easy`

### Flags:

User: `<md5>`

Root: `<md5>`

### Automation / Crons
Due to a problem when exporting the machine in `.ova` format we decided to create a service which is responsible for ensuring that the containers are always running.

1) Create the service file
```python
sudo nano /etc/systemd/system/docker-compose-restart.service

This command opens the `nano` text editor with superuser privileges (`sudo`) to create a `systemd` service file called `docker-compose-restart.service`.

```

2) Contents of docker-compose-restart.service service
```python
[Unit]
Description=Restarts Docker Compose at a specified path with a delay after the full startup
After=network.target

[Service]
Type=oneshot
WorkingDirectory=/root/docker-hfs/
ExecStartPre=/bin/sleep 3
ExecStart=/usr/bin/docker-compose down
ExecStartPost=/bin/sleep 5
ExecStartPost=/usr/bin/docker-compose up -d

[Install]
WantedBy=default.target

This file defines a `systemd` service that, once activated, will do the following:

1. Wait 3 seconds.
2. Stop all containers defined in the `docker-compose.yml` file in `/root/.docker-hfs/`.
3. It will wait for 5 seconds.
4. It will restart the containers in the background.

The service is a oneshot service, meaning it will run these actions once and then terminate.


3) Reload systemd configuration
python
sudo systemctl daemon-reload

This command reloads the systemd configuration to recognize the new service file.

4) Enable the service
python
sudo systemctl enable docker-compose-restart.service

This command enables the service to run automatically at system startup.
```

### Firewall Rules
`NONE`

---------------------------------------------------
### Ping

```python
ping -c 1 10.0.2.24
PING 10.0.2.24 (10.0.2.24) 56(84) bytes of data.
64 bytes from 10.0.2.24: icmp_seq=1 ttl=64 time=0.524 ms

--- 10.0.2.24 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.524/0.524/0.524/0.000 ms
```

### TTL = 64 Linux

### nmap
sudo nmap -p- -open -sCV --min-rate 5000 -n -Pn 10.0.2.24

```python
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u3 (protocol 2.0)
| ssh-hostkey: 
|   256 b5:a4:7c:65:5c:1f:d7:89:42:bd:76:df:2c:8e:93:4e (ECDSA)
|_  256 5d:3d:2b:43:fc:89:fa:24:a3:f4:73:5f:7b:89:6c:e3 (ED25519)
80/tcp open  http    Apache httpd 2.4.61 ((Debian))
|_http-title: mamushka
|_http-server-header: Apache/2.4.61 (Debian)
MAC Address: 08:00:27:05:CB:78 (Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 19.19 seconds
```

### Enumeración del puerto 80 (http)
en la primera parte de la web podemos observar que la web contiene 8 apartados ( `Account,Login,Logout,Members,Password Reset,Register,Simple Page,User` ) también observamos una barra de búsqueda y al final de la web un panel de registro de usuarios. 
todos estos apartados aunque interesantes no nos proporcionan un camino ni tampoco son la vía intencionada para esta primera incursión 
![[Pasted image 20240822205025.png]]

![[Pasted image 20240822205106.png]]

![[Pasted image 20240822205132.png]]

![[Pasted image 20240822205146.png]]

### Fuzzing con gobuster
en nuestra enumeración con gobuster encontramos tres directorios los cuales nos indican que nos estamos enfrentando a una web diseñada en WordPress
![[Pasted image 20240822210221.png]]

### nuclei
realizando un escaneo con nuclei podemos conseguir una CVE en este caso es la CVE-2024-27956 la cual esta albergada en un plugin de wordpress. 
![[Pasted image 20240822223658.png]]

### wfuff
podemos verificar esto usando una wordlist de wordpress actualizada que podemos conseguir en este repositorio [Wordlist-Wordpress](https://github.com/kongsec/Wordpress-BruteForce-List/blob/main/Fuzz)
![[Pasted image 20240822224231.png]]

### Vulnerabilidades

| CVE-XXXX-XXXXX | Nombre de la vulnerabilidad                                        | Tipo | Nivel | Link                                                    |
| -------------- | ------------------------------------------------------------------ | ---- | ----- | ------------------------------------------------------- |
| CVE-2024-27956 | SQL Injection Vulnerability in ValvePress Automatic (WP-Automatic) | RCE  | 9.9   | [link](https://nvd.nist.gov/vuln/detail/CVE-2024-27956) |
|                |                                                                    |      |       |                                                         |
|                |                                                                    |      |       |                                                         |

### explotación De La CVE
para la explotación de esta CVE usaremos el siguiente repositorio [github](https://github.com/diego-tella/CVE-2024-27956-RCE) en este punto solo debemos ejecutar el exploit con el siguiente comando

```python
python3 exploit.py http
```

![[Pasted image 20240822225120.png]]