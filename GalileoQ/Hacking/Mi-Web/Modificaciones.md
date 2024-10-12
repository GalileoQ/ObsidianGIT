modificar el parámetro de las imágenes 
```python
!\[\[Pasted image ([0-9]+)\.png\]\]
```

bucle `for` para eliminar el espacio del nombre de las imagenes
```python
for file in *'Pasted image'*; do mv "$file" "${file//'Pasted image '/}"; done
```
