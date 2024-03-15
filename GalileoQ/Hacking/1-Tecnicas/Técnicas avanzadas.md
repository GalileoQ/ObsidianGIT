### Descifrar claves PuTTY key a id_rsa
#
```python
	puttygen NombreDelArchivo.ppk -O private-openssh -o id_rsa
```
### Generar claves hash con openssl

```python
openssl passwd -6 -salt 'salt' 'password'
```