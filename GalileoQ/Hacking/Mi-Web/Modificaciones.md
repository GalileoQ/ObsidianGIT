modificar el parámetro de las imágenes 
```python
!\[\[Pasted image ([0-9]+)\.png\]\]
```

bucle `for` para eliminar el espacio del nombre de las imagenes
```python
for file in *'Pasted image'*; do mv "$file" "${file//'Pasted image '/}"; done
```

```python
pnpm dev # iniciar el server

pnpm build # crear el proyecto

	git add punto

	git commit "v#"

	git push

```
