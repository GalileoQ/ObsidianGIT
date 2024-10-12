modificar el parametro de 
```python
!\[\[Pasted image ([0-9]+)\.png\]\]
```

```python
para eliminar el `paste imagen` de las imagenes
for file in *'Pasted image'*; do mv "$file" "${file//'Pasted image '/}"; done
```