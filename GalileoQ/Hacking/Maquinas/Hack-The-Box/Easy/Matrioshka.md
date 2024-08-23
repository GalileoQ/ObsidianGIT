#Easy #Linux #tecnicas 

## Introducción


[Incluya por qué creó este cuadro, qué habilidades y vulnerabilidades desea resaltar, etc.]

## Información para HTB
Machine Name

​ DDth Month YYYY

​ Machine Author(s):

​

### Description:

[](https://github.com/hackthebox/writeup-templates/blob/master/machine/Machine_Name.md#description)

This machine...

### Difficulty:

[](https://github.com/hackthebox/writeup-templates/blob/master/machine/Machine_Name.md#difficulty)

`easy`

### Flags:

[](https://github.com/hackthebox/writeup-templates/blob/master/machine/Machine_Name.md#flags)

User: `<md5>`

Root: `<md5>`
### Acceso

[](https://github.com/hackthebox/public-templates/blob/master/Machine-Writeup-Template.md#access)

Contraseñas:

| Usuario  | Contraseña                                              |
| -------- | ------------------------------------------------------- |
| usuario1 | [frase de contraseña, no demasiado difícil de escribir] |
| usuario2 | [frase de contraseña, no demasiado difícil de escribir] |
| raíz     | [frase de contraseña, no demasiado difícil de escribir] |

### Procesos clave

[](https://github.com/hackthebox/public-templates/blob/master/Machine-Writeup-Template.md#key-processes)

[Describa los procesos que se están ejecutando para proporcionar servicios básicos en la caja, como servidor web, FTP, etc. **Para cualquier binario personalizado, incluya el código fuente (en un archivo separado a menos que sea muy corto)** . Además, incluya si alguno de los servicios o programas ejecuta versiones intencionalmente vulnerables.]

### Automatización / Crons

[](https://github.com/hackthebox/public-templates/blob/master/Machine-Writeup-Template.md#automation--crons)

[Describa cualquier automatización en el cuadro:

- ¿Qué hace?
- ¿Por qué? (necesario para el paso de explotación, limpieza, etc.)
- ¿Cómo lo hace? Proporcione el código fuente (cualquier cosa más larga que unas pocas líneas en un archivo adjunto separado)
- ¿Cómo funciona?

]

### Reglas del cortafuegos

[](https://github.com/hackthebox/public-templates/blob/master/Machine-Writeup-Template.md#firewall-rules)

[Describa aquí cualquier regla de firewall no predeterminada]

### Estibador

[](https://github.com/hackthebox/public-templates/blob/master/Machine-Writeup-Template.md#docker)

[Describa cómo se usa Docker, si es que se usa. Adjuntar archivos Docker]

### Otro

[](https://github.com/hackthebox/public-templates/blob/master/Machine-Writeup-Template.md#other)

[Incluya cualquier otra decisión de diseño que haya tomado y que el personal de HTB deba conocer]

# redacción

[](https://github.com/hackthebox/public-templates/blob/master/Machine-Writeup-Template.md#writeup)

[

Proporcione una explicación detallada de los pasos necesarios para completar el cuadro de principio a fin. Divida su tutorial en las siguientes secciones y subsecciones e incluya imágenes para guiar al usuario a través de la explotación.

Incluya también capturas de pantalla de cualquier elemento visual (como sitios web) que formen parte del envío. Nuestro equipo de revisión no sólo está evaluando la ruta técnica, sino también el realismo y la historia de la caja.

Mostrar **todos** los comandos específicos usando las triples comillas invertidas de Markdown ( ` ```bash `) de modo que el lector pueda copiarlos/pegarlos, y también mostrar la salida de los comandos a través de imágenes o bloques de código de rebajas ( ` ``` `).

**Un lector debería poder resolver el cuadro por completo copiando y pegando los comandos que usted proporciona.**

]

# Enumeración

[](https://github.com/hackthebox/public-templates/blob/master/Machine-Writeup-Template.md#enumeration)

[Describa los pasos que describen la enumeración del cuadro. Normalmente, esto incluye un subtítulo para el escaneo de Nmap, enumeración HTTP/web, etc.]

# Asidero para el pie

[](https://github.com/hackthebox/public-templates/blob/master/Machine-Writeup-Template.md#foothold)

[Describa los pasos para obtener un punto de apoyo inicial (ejecución de shell/comando) en el objetivo.]

# Movimiento lateral (opcional)

[](https://github.com/hackthebox/public-templates/blob/master/Machine-Writeup-Template.md#lateral-movement-optional)

[Describa los pasos para el movimiento lateral. Esto puede incluir rupturas de Docker/escape al host, etc.]

# Escalada de privilegios

[](https://github.com/hackthebox/public-templates/blob/master/Machine-Writeup-Template.md#privilege-escalation)

[Describa los pasos para obtener privilegios de root/administrador en el cuadro.]