Por tanto lo que podemos hacer es extraer la ssh e iniciar sesión para que se ejecute el script por parte de root y podamos recibir la conexión:
![[Pasted image 20240612162353.png]]

![[Pasted image 20240612162356.png]]
Nos copiamos esta clave pública a un fichero y la utilizaremos para iniciar sesión, pero primero le tenemos que dar el permiso 600 para poder utilizarla:
![[Pasted image 20240612162401.png]]

Iniciamos la sesión con esta id_rsa y ya estamos dentro por SSH:
![[Pasted image 20240612162407.png]]

----------------------------------------------------------

[[MAQUINA WALDO (Path traversal y Local File Inclusion LFI para obtener clave ssh privada id_rsa para entrar con ssh)]]

[[MAQUINA TRICK]]