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