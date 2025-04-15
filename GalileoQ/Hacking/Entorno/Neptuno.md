crear un perfil para iniciar el programa cada vez que se inicie la maquia 

````python
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