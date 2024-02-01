### nmap 

```python
sudo nmap -sS --min-rate 5000 -p- 10.10.10.10,IP,,IP,IP -oN tcp_scan.txt
```
### grep 
```python
	grep '[0-9]$' archivo.txt | cut -d '/' -fi | sort 
```
