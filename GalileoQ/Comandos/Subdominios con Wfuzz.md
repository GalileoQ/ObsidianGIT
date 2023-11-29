```python
wfuzz -c --hc 404,400,302 -t 200 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -u http://devvortex.htb/ -H "Host: FUZZ.devvortex.htb"
```
