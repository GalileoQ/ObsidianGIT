### Buscar subdominios con ffuz
#fuzzing
```python
ffuf -u http://direccion.di/ -H "Host:FUZZ.direccion.di" -w wordlists -fs numero de error
```
### Subdominios con Wfuzz

```python
wfuzz -c --hc 404,400,302 -t 200 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -u http://devvortex.htb/ -H "Host: FUZZ.devvortex.htb"

❯ wfuzz -c -w /usr/share/wordlists/seclists/Discovery/Web-Content/raft-medium-files.txt --hc 404,403 http://shop.trickster.htb/FUZZ
```
### Enumerar plugins Wp

```python
curl -s -X GET "http://localhost:80" | grep plugins
```

### fuerza bruta web con hydra

```python
hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.10.41.104 http-post-form "/rutaDelLoginDesdeLaRaiz:user=^USER^&pass=^PASS^:F= mensaje de error" -V
```
