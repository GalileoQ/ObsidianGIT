crear un perfil para iniciar el programa cada vez que se inicie la maquia 

```python
# crear una carpeta para guardar el script de inicio
mkdir -p ~/.config/autostart

# crear un archivo para guardar el codigo
nano ~/.config/autostart/neptune.desktop

# codigo del inicio del software

[Desktop Entry]
Type=Application
Exec=Neptune -cli -soundkey nk-cream -volume 0.5
Hidden=false
NoDisplay=false
X-GNOME-Autostart-enabled=true
Name=Neptune
Comment=Startup sound
```

```python
## Método: Crear un servicio `systemd --user` para Neptune

### 1. Crea el archivo del servicio

bash

CopiarEditar

`mkdir -p ~/.config/systemd/user nano ~/.config/systemd/user/neptune.service`

### 2. Pega este contenido:

ini

CopiarEditar

`[Unit] Description=Neptune Audio Visualizer After=default.target  [Service] ExecStart=/usr/bin/Neptune -cli -soundkey nk-cream -volume 1.0 Restart=on-failure  [Install] WantedBy=default.target`

> Asegúrate de que la ruta a `Neptune` sea correcta (`which Neptune` para verificar). Si está en otro lugar, cámbiala en `ExecStart`.

---

### 3. Recarga los servicios de usuario

bash

CopiarEditar

`systemctl --user daemon-reexec systemctl --user daemon-reload`

### 4. Habilita el servicio para que inicie en cada sesión

bash

CopiarEditar

`systemctl --user enable neptune.service`

### 5. Inicia el servicio (para probar sin reiniciar):

bash

CopiarEditar

`systemctl --user start neptune.service`
```