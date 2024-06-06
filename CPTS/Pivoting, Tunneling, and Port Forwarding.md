![[Pasted image 20240605151052.png]]
#### Ejecución del reenvío de puerto local por ssh
```python
ssh -L 1234:localhost:3306 ubuntu@10.129.202.64
```

#### Reenvío de múltiples puertos

Reenvío dinámico de puertos con túnel SSH y SOCKS

```python
ssh -L 1234:localhost:3306 -L 8080:localhost:80 ubuntu@10.129.202.64
```

#### Habilitación del reenvío dinámico de puertos con SSH

Reenvío dinámico de puertos con túnel SSH y SOCKS

```python
G41i130Q@htb[/htb]$ ssh -D 9050 ubuntu@10.129.202.64

	# nota: de esta manera podemos enviar todo el trafico de la maquina numero 3 (maquina final) hacia nuestra maquina por el puerto 9050 que esta configurado usando el proxychains
```

#### Enumeración del destino de Windows a través de Proxychains

Reenvío dinámico de puertos con túnel SSH y SOCKS

```python
proxychains nmap -v -Pn -sT 172.16.5.19
```

#### Usando xfreerdp con Proxychains

Reenvío dinámico de puertos con túnel SSH y SOCKS

```python
proxychains xfreerdp /v:172.16.5.19 /u:victor /p:pass@123
```

