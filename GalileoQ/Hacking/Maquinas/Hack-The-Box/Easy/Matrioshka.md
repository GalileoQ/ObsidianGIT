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

debido a un problema al momento de exportar la maquina en formato `.ova` hemos decidido crear un servicio el cual se encarga de que los con
### Firewall Rules
`NONE`

# Enumeration

[](https://github.com/hackthebox/writeup-templates/blob/master/machine/Machine_Name.md#enumeration)

# Foothold

[](https://github.com/hackthebox/writeup-templates/blob/master/machine/Machine_Name.md#foothold)

# Lateral Movement

[](https://github.com/hackthebox/writeup-templates/blob/master/machine/Machine_Name.md#lateral-movement)

# Privilege Escalation