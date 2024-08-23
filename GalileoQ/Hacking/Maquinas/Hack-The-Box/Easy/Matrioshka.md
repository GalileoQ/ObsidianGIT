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

```python

```

### Enumeración 


### Vulnerabilidades

| CVE-XXXX-XXXXX | Nombre de la vulnerabilidad | Tipo | Nivel | Link |
| -------------- | --------------------------- | ---- | ----- | ---- |
|                |                             |      |       |      |
|                |                             |      |       |      |
|                |                             |      |       |      |
