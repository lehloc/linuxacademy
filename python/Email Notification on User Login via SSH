### Install EPEL Repos, sendemail, python36, python36-devel
```
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
sudo yum install python36 python36-devel
sudo yum install sendemail
```
### Test Sending out an Email
```
sendemail -f [FROM_EMAIL] -u 'AUTH_NOTIFICATION' -t [TO_EMAIL] -s smtp.gmail.com:587 -o tls=yes -xu [USER_NAME] -xp [PASSWORD] -m "[YOUR MESSAGE]"
```

### Write a Script to Send an Email with the Time Stamp and Username upon SSH Login
```
vim /home/cloud_user/onLogin.py
```
```
#!/bin/python3.6

import subprocess
from datetime import datetime
import time
import getpass

msg = "#########################\nTIME: " + datetime.now().strftime('%Y-%m-%d %H:%M:%S') + "\nUSER: " + getpass.getuser() + "\nWAS AUTHENTICATED\n#########################\n"

subprocess.check_output(['sendemail', '-f', '[FROM_EMAIL]', '-u', 'SCAN_NOTIFICATION', '-t', '[TO_EMAIL]', '-s', 'smtp.gmail.com:587', '-o', 'tls=yes', '-xu', '[USER_NAME]', '-xp', '[PASSWORD]', '-m', msg], stdin=None, stderr=None, shell=False, universal_newlines=False)
```
```
chmod +x /home/cloud_user/onLogin.py
sudo cp /home/cloud_user/onLogin.py /bin/
```

### Configure SSH to Make Use of the Script
```
sudo vim /etc/pam.d/sshd
session optional pam_exec.so seteuid /bin/onSSHLoginHook.py
```
