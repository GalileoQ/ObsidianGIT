
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

primero instalamos kitty pero la version actualizada desde los repositorios
```python
# abrimos firefox
https://github.com/kovidgoyal/kitty/releases

# descargamos el release

Linux amd64 binary bundle
```

vamos al directorio opt
```python
sudo su

cd /opt

# movemos el archivo de kitty a la ruta /opt

# creamos un directorio llamado kitty y descomprimimos el archivo kitty

tar -xf archivo kitty
```

dentro del archivo sxhkd vamos a cambiar la terminal para que abre una kitty

```python
# terminal emulator

super + Return
	/opt/kitty/bin/kitty
```

cambiamos el atajo de teclado para restaurar el bspwm

```python
# quit/restar bspwm
super + shift
```

cambiamos el focus de las ventanas al moverlas
```python
# cambiamos hjkl por las flechas
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

dentro del sxhkd
```python
# program launcher
super + d
	/user/bin/rofi -show run
```

ya podemos ir al bspwm

ahora vamos con las fuentes
```python
sudo root

cd /user/local/share/fonts
```

abrimos firefox y descargamos las fuentes hack nerd fonts
```python
https://www.nerdfonts.com/font-downloads
```

una ves descargado el archivo lo movemos a la ruta de las fuentes 
```python
mv archivodefuentes.zip /user/local/share/fonts
```

descomprimimos el archivo
```python
7z x archivo.zip 
```

instalamos la zsh
```python
sudo apt install zsh
```

vamos a activar la clipboard
```python
nano ./bspwm/bspwmrc

#agregamos

vmware-user-suid-wrapper &
```

vamos a configurar la kitty
```python
# vamos al directorio ./config

cd kitty

# pagamos esta configuracion dentro de un archivo que se llame kitty.conf

enable_audio_bell no

include color.ini

font_family HackNerdFont
font_size 13

disable_ligatures never

url_color #61afef

url_style curly

map ctrl+left neighboring_window left
map ctrl+right neighboring_window right
map ctrl+up neighboring_window up
map ctrl+down neighboring_window down

map f1 copy_to_buffer a
map f2 paste_from_buffer a
map f3 copy_to_buffer b
map f4 paste_from_buffer b

cursor_shape beam
cursor_beam_thickness 1.8

mouse_hide_wait 3.0
detect_urls yes

repaint_delay 10
input_delay 3
sync_to_monitor yes

map ctrl+shift+z toggle_layout stack
tab_bar_style powerline

inactive_tab_background #e06c75
active_tab_background #98c379
inactive_tab_foreground #000000
tab_bar_margin_color black

map ctrl+shift+enter new_window_with_cwd
map ctrl+shift+t new_tab_with_cwd

background_opacity 0.95

shell zsh
```

instalamos feh
```python
apt install feh
```

