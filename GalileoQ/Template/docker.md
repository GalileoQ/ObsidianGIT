```python
docker rm -f $(docker ps -a -q) && docker rmi -f $(docker images -q) && docker volume rm $(docker volume ls -q) && docker network prune -f
```


```python
!\[\[Pasted image ([0-9]+)\.png\]\]
```

```python
aqui

para eliminar el `paste imagen` de las imagenes
for file in *'Pasted image'*; do mv "$file" "${file//'Pasted image '/}"; done
```

nuevo post