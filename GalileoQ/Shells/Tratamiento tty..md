```css
		script /dev/null -c bash 
		ctrl + z 
		stty raw -echo; fg
		reset 
	--especifica la terminal --xterm 
		export TERM=xterm 
		export SHELL=bash
```

```css
	python3 -c 'import pty; pty.spawn("/bin/bash")'
```

```python
	script -qc /bin/bash /dev/null
	ctrl + z 
		stty raw -echo; fg
		reset 
	--especifica la terminal --xterm 
		export TERM=xterm 
		export SHELL=bash
```
