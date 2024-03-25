Con esta herramienta podremos capturar paquetes en red mediante una técnica conocida como sniffing. Por tanto, si queremos ver todo el tráfico que entra a través de una interfaz determinada, lo haríamos de esta forma:
![[Pasted image 20230216220011.png]]
Y capturamos todo el tráfico:
![[Pasted image 20230216220037.png]]
También podremos enviar un ping de una máquina y recibirlo con la otra, de tal forma que podremos comprobar la conectividad y comprobar que el ping ha llegado con tcpdump. Por tanto, nos pondremos en escucha de esta forma:
![[Pasted image 20230215210826.png]]
Y ahora desde otra máquina enviamos una traza ICMP:
![[Pasted image 20230215210858.png]]
Y lo recibimos en la máquina Kali:
![[Pasted image 20230215210913.png]]
También podemos estar en escucha para cualquier tipo de tráfico y esperando conexión de una IP en concreto. Por ejemplo, en este caso voy a recibir una conexión desde mi teléfono de cualquier tipo, por lo que ejecuto este comando con la IP del device al que estoy esperando la conexión:
![[Pasted image 20230216220740.png]]
Y ahora levantamos un servidor web con Python para comprobar si recibimos conexiones:
![[Pasted image 20230216220811.png]]
Hacemos una petición HTTP desde la IP del teléfono y vemos que en tcpdump nos notifica de este tráfico:
![[Pasted image 20230216221004.png]]
También podemos guardar tráfico de red en un determinado fichero .cap con tcpdump [[Nmap#GUARDAR TRÁFICO DE NMAP EN FICHERO .CAP CON TCPDUMP]]