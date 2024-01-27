
```python
import hashlib

num = range(101)
hashes = []

for x in num:
    hashes.append(hashlib.md5(str(x).encode('utf-8')).hexdigest())
with open('hashdump.txt', 'w') as fname:
    for i in hashes:
        fname.write(i + '\n')
print('hashing completed')
```
