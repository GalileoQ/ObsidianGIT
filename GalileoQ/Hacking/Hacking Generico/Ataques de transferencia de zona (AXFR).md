Los ataques de transferencia de zona, también conocidos como AXFR (siglas de "Authoritative eXtensible File Transfer Protocol") son un tipo de ataque dirigido a los servidores de nombres de dominio para obtener información sensible sobre los dominios de una organización.

----------------------------

Para esto usaremos el siguiente laboratorio:
```php
https://github.com/vulhub/vulhub/tree/master/dns/dns-zone-transfer
```

![[Pasted image 20240612162850.png]]

![[Pasted image 20240612162854.png]]
Abrimos el fichero named.conf.local y ponemos el nombre de transferencia de zona que queramos:
![[Pasted image 20240612162900.png]]

Montamos el laboratorio con docker-compose up -d:
![[Pasted image 20240612162905.png]]

Una vez desplegado, puedo hacer un ataque de transferencia de zona de tal forma que con el comando dig pueda conocer los subdominios existentes y más información:
```bash
dig axfr @127.0.0.0 pingucorp.local
```

![[Pasted image 20240612162913.png]]
#### OTRO EJEMPLO
Otro ejemplo de este ataque puede ocurrir cuando estamos ante un dominio y queremos obtener subdominios; de tal forma que podemos hacer lo siguiente:
[[MAQUINA HUNTER]]
```bash
dig axfr hunterzone.nyx @192.168.0.40
```

![[Pasted image 20240612162917.png]]