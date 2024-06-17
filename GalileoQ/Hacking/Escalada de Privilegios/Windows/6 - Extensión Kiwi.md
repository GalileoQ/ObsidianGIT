Kiwi es una extensión de Metasploit que permite realizar operaciones orientadas a credenciales, como volcado de contraseñas y hashes, volcado de contraseñas en memoria, generación de golden tickets y mucho más.

------------------------------

Una vez dentro de una sesión de meterpreter y dentro del proceso lsass.exe con metasploit, podemos cargar la extensión Kiwi:
![[Pasted image 20240612161339.png]]

Y con el comando creds_all podemos cargar las credenciales del sistema:
![[Pasted image 20240612161343.png]]

Y con el comando lsa_dump_sam podremos obtener el hash del resto de usuarios:
![[Pasted image 20240612161348.png]]

Y con este otro comando obtenemos credenciales del sistema secretas:
![[Pasted image 20240612161352.png]]
