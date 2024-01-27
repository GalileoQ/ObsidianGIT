
```python
#!/bin/bash

# Códigos de color ANSI
GREEN='\033[1;32m' # Verde brillante
RED='\033[1;32m'   # verde brillante
CYAN='\033[1;32m'  # verde brillante
NC='\033[1;32m'    # verde color

# Mensaje del autor con efecto visual
echo -e "${CYAN}╔══════════════════════════════════════════════════╗${NC}"
echo -e "${CYAN}║           Script by PwnTheOvni                   ║${NC}"
echo -e "${CYAN}║      Unlocking the Secrets of the Universe!      ║${NC}"
echo -e "${CYAN}╚══════════════════════════════════════════════════╝${NC}"

while true; do
    echo -e "${GREEN}Selecciona una opción:${NC}"
    echo -e "${GREEN}1. Conectar VPN${NC}"
    echo -e "${GREEN}2. Desconectar VPN${NC}"
    echo -e "${GREEN}3. Salir${NC}"
    read -p "Ingresa el número de la opción: " choice

    case $choice in
        1)
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
            read -p "Ingresa la IP: " ip_address && zsh -i -c "settarget $ip_address"

            # Aplicar reglas de iptables con la IP ingresada
            iptables -P INPUT ACCEPT
            iptables -P FORWARD ACCEPT
            iptables -P OUTPUT ACCEPT
            iptables -t nat -F
            iptables -t mangle -F
            iptables -F
            iptables -X
            iptables -Z

            ip6tables -P INPUT DROP
            ip6tables -P FORWARD DROP
            ip6tables -P OUTPUT DROP
            ip6tables -t nat -F
            ip6tables -t mangle -F
            ip6tables -F
            ip6tables -X
            ip6tables -Z
            iptables -A INPUT -p icmp -i tun0 -s $ip_address --icmp-type echo-request -j ACCEPT
            iptables -A INPUT -p icmp -i tun0 -s $ip_address --icmp-type echo-reply -j ACCEPT
            iptables -A INPUT -p icmp -i tun0 --icmp-type echo-request -j DROP  
            iptables -A INPUT -p icmp -i tun0 --icmp-type echo-reply -j DROP
            iptables -A OUTPUT -p icmp -o tun0 -d $ip_address --icmp-type echo-reply -j ACCEPT
            iptables -A OUTPUT -p icmp -o tun0 -d $ip_address --icmp-type echo-request -j ACCEPT
            iptables -A OUTPUT -p icmp -o tun0 --icmp-type echo-request -j DROP
            iptables -A OUTPUT -p icmp -o tun0 --icmp-type echo-reply -j DROP

            iptables -A INPUT -i tun0 -p tcp -s $ip_address -j ACCEPT
            iptables -A OUTPUT -o tun0 -p tcp -d $ip_address -j ACCEPT
            iptables -A INPUT -i tun0 -p udp -s $ip_address -j ACCEPT
            iptables -A OUTPUT -o tun0 -p udp -d $ip_address -j ACCEPT
            iptables -A INPUT -i tun0 -j DROP
            iptables -A OUTPUT -o tun0 -j DROP

            echo "VPN conectada: Reglas de ip estables aplicadas para la IP: $ip_address"
            exit 0
            ;;
        2)
            # Desconectar VPN usando pkill
            sudo killall openvpn && sudo pkill openvpn && sudo iptables -F && sudo ip6tables -F
            zsh -i -c "settarget"

            echo "VPN desconectada."
            exit 0
            ;;
        3)
            echo "Saliendo del script."
            exit 0
            ;;
        *)
            echo "Opción inválida. Por favor, selecciona 1, 2 o 3."
            ;;
    esac
done
```