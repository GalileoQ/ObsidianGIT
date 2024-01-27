
```python
sudo nmap -sS -sV -A -p- -T4 -O --script=default,vuln,exploit,auth,discovery --stats-every 10s 10.10.70.81
```