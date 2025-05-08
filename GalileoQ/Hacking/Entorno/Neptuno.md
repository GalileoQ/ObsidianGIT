### Paso 1: Mover el ejecutable a un directorio más permanente
```python
mkdir -p ~/.local/bin mv ~/Downloads/Neptune-Cli ~/.local/bin/ chmod +x ~/.local/bin/Neptune-Cli
```

Esto asegura que el binario esté en un lugar estándar para programas de usuario y sea ejecutable.

---

### ✅ Paso 2: Crear el archivo de servicio `systemd`

```python
mkdir -p ~/.config/systemd/user nano ~/.config/systemd/user/neptune.service
```

Pega este contenido en `nano`:
```python
[Unit]
Description=Neptune Audio Visualizer
After=default.target

[Service]
ExecStart=/home/gleoq/.local/bin/Neptune-Cli -cli -soundkey nk-cream -volume 1.0
Restart=on-failure

[Install]
WantedBy=default.target
```


Guarda con `Ctrl+O`, luego `Enter`, y sal con `Ctrl+X`.

---

### ✅ Paso 3: Recargar los servicios de usuario
```python
systemctl --user daemon-reexec systemctl --user daemon-reload
```

---

### ✅ Paso 4: Habilitar el servicio para que se inicie con la sesión
```python
systemctl --user enable neptune.service
```
---

### ✅ Paso 5: Iniciar el servicio manualmente (para probar ahora)

```python
systemctl --user start neptune.service
```
