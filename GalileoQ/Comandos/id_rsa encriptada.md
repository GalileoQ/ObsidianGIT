
```python

	ssh2john id_rsa > id_rsa_hash
	
	john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa_hash
```
