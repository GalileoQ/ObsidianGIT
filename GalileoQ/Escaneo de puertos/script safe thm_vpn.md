```css
#!/bin/bash

# Verificar si el usuario es root
if [[ $EUID -ne 0 ]]; then
    # Si no es root, solicitar contraseña y ejecutar en la terminal
    read -s -p "Introduce tu contraseña: " password
    (echo $password | sudo -S openvpn /media/sf_VM/galileoquevedo.ovpn) &
    sleep 3
else
    # Si es root, simplemente ejecutar el comando en la terminal
    sudo openvpn /media/sf_VM/galileoquevedo.ovpn &
    sleep 3
fi

# Esperar un momento para permitir la inicialización completa
sleep 1

# Limpiar la terminal
clear

# Solicitar entrada de la IP
read -p "Ingresa la IP: " ip_address
# Ejecutar el comando settarget con la IP ingresada usando Zsh
zsh -i -c "settarget $ip_address"

# Aplicar reglas de iptables con la IP ingresada
# Author: Nisrin Ahmed aka Wh1teDrvg0n

# IPv4 flush
iptables -P INPUT ACCEPT
iptables -P FORWARD ACCEPT
iptables -P OUTPUT ACCEPT
iptables -t nat -F
iptables -t mangle -F
iptables -F
iptables -X
iptables -Z

# IPv6 flush
ip6tables -P INPUT DROP
ip6tables -P FORWARD DROP
ip6tables -P OUTPUT DROP
ip6tables -t nat -F
ip6tables -t mangle -F
ip6tables -F
ip6tables -X
ip6tables -Z

# Ping machine
iptables -A INPUT -p icmp -i tun0 -s $ip_address --icmp-type echo-request -j ACCEPT
iptables -A INPUT -p icmp -i tun0 -s $ip_address --icmp-type echo-reply -j ACCEPT
iptables -A INPUT -p icmp -i tun0 --icmp-type echo-request -j DROP  
iptables -A INPUT -p icmp -i tun0 --icmp-type echo-reply -j DROP
iptables -A OUTPUT -p icmp -o tun0 -d $ip_address --icmp-type echo-reply -j ACCEPT
iptables -A OUTPUT -p icmp -o tun0 -d $ip_address --icmp-type echo-request -j ACCEPT
iptables -A OUTPUT -p icmp -o tun0 --icmp-type echo-request -j DROP
iptables -A OUTPUT -p icmp -o tun0 --icmp-type echo-reply -j DROP

# Allow VPN connection only from machine
iptables -A INPUT -i tun0 -p tcp -s $ip_address -j ACCEPT
iptables -A OUTPUT -o tun0 -p tcp -d $ip_address -j ACCEPT
iptables -A INPUT -i tun0 -p udp -s $ip_address -j ACCEPT
iptables -A OUTPUT -o tun0 -p udp -d $ip_address -j ACCEPT
iptables -A INPUT -i tun0 -j DROP
iptables -A OUTPUT -o tun0 -j DROP

# Para el Resto del script para usar la variable $ip_address según sea necesario

```