colores para los shapes de la polybar
```python 
	/home/kali/.config/polybar/shapes
```

elinimar bordes 
```python
	picom --experimental-backedns &
	bspc config border_width 0
```

agregamos bordes 

pycon.conf
```python
corner-radius = 10;
round-borders = 1;

# bspwmrc
bspc config border_width 0
```

p10k
```python
typeset -g POWERLEVEL9K_OS_ICON_FOREGROUND=1  
typeset -g POWERLEVEL9K_OS_ICON_BACKGROUND=0
```

## ermatologic



### Diseño

realizar un script automatizado que ejecute un themes colors para modificar en fondo de escritorio y los colores en un intervalo de tiempo "X"

-------------------------------------------------------
### ✅ Objetivo

Supongamos:

- Imagen real:  
    `/usr/share/backgrounds/kali/kali-ascii.png`
    
- Quieres que en:  
    `/usr/share/backgrounds/kali-16x9/kali-ascii-16x9.png`  
    haya un **symlink** apuntando a la imagen real.
    

---

### ✅ Paso 1: Asegúrate de que no exista un archivo con ese nombre

bash

CopiarEditar

`sudo rm -f /usr/share/backgrounds/kali-16x9/kali-ascii-16x9.png`

---

### ✅ Paso 2: Crear el symlink correctamente

bash

CopiarEditar

`sudo ln -s ../kali/kali-ascii.png /usr/share/backgrounds/kali-16x9/kali-ascii-16x9.png`

