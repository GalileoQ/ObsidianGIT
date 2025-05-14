
instalacion de paquetes y dependencias necesarias

```python
apt install build-essential git vim xcb libxcb-util0-dev libxcb-ewmh-dev libxcb-randr0-dev libxcb-icccm4-dev libxcb-keysyms1-dev libxcb-xinerama0-dev libasound2-dev libxcb-xtest0-dev libxcb-shape0-dev
```

ahora hacemos un update

```python
sudo apt update
```

despues clonamos los repos 

```python
git clone https://github.com/baskerville/bspwm.git

git clone https://github.com/baskerville/sxhkd.git
```

despues dentro del repo de bspwm

```python
make

sudo make install
```

hacemos lo mismo con el repo de sxhkd

```python
make

sudo make install
```

después vamos a crear los directorios de configuración para ambos archivos

```python
mkdir ~/.config/{bspwm,sxhkd}
```

ahora vamos a copiar los archivos necesarios que estan dentro de los repositorios que hemos descargado:

```python
cd (ruta del repo de bspwm)

cd examples

cp bspwmrc ~/.config/bspwm/

cd sxhkd ~/.config/sxhkd/
```

ahora configuramos un poco el archivo sxhkd

primero instalamos kitty
```python
sudo apt install kitty
```

dentro del archivo sxhkd vamos a cambiar la terminal para que abre una kitty

```python
# terminal emulator

super + Return
	/user/bin/kitty
```

cambiamos el atajo de teclado para restaurar el bspwm

```python
# quit/restar bspwm
super + shift
```

cambiamos el focus de las ventanas al moverlas
```python
# cambiamos hjkl poer las flechas
```

cambiamos el tamaño de la ventala antes de abrirla (preselectores)
```python
# cambiamos hjkl por las flechas
```

guardaremos el bspwm rezice

```python
#!/usr/bin/env dash

if bspc query -N -n focused.floating > /dev/null; then
	step=20
else
	step=100
fi

case "$1" in
	west) dir=right; falldir=left; x="-$step"; y=0;;
	east) dir=right; falldir=left; x="$step"; y=0;;
	north) dir=top; falldir=bottom; x=0; y="-$step";;
	south) dir=top; falldir=bottom; x=0; y="$step";;
esac

bspc node -z "$dir" "$x" "$y" || bspc node -z "$falldir" "$x" "$y"
```

instalamos la polibar 
```python
sudo apt install polybar
```

instalamos picom
```python
apt install libconfig-dev libdbus-1-dev libegl-dev libev-dev libgl-dev libepoxy-dev libpcre2-dev libpixman-1-dev libx11-xcb-dev libxcb1-dev libxcb-composite0-dev libxcb-damage0-dev libxcb-glx0-dev libxcb-image0-dev libxcb-present-dev libxcb-randr0-dev libxcb-render0-dev libxcb-render-util0-dev libxcb-shape0-dev libxcb-util-dev libxcb-xfixes0-dev meson ninja-build uthash-dev -y
```

actualizamos de nuevo
```python
apt update
```

descargarmos el repositorio de de picom

```python
git clone https://github.com/yshui/picom.git
```

despues dentro del directorio de picom

```python
meson setup --buildtype=release build

ninja -C build

ninja -C build install
```

instalamos rofi
```python
sudo apt install rofi
```

dentro del 
