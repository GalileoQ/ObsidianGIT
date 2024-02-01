### nmap 

```python
sudo nmap -sS --min-rate 5000 -p- -Pn 10.10.10.10,IP,,IP,IP -oN tcp_scan.txt
```
### grep 
```python
	grep '[0-9]$' archivo.txt
	
	grep '[0-9]$' archivo.txt | cut -d '/' -f1 | sort -u
```
