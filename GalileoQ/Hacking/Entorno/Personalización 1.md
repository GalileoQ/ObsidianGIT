
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