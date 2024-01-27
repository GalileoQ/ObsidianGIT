### Buscar subdominios con ffuz

```python
ffuf -u http://direccion.di/ -H "Host:FUZZ.direccion.di" -w wordlists -fs numero de error
```
### Subdominios con Wfuzz

```python
wfuzz -c --hc 404,400,302 -t 200 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -u http://devvortex.htb/ -H "Host: FUZZ.devvortex.htb"
```
### Enumerar plugins Wp

```python
curl -s -X GET "http://localhost:80" | grep plugins
```

### fuerza bruta web con hydra
