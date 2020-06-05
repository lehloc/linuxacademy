## Install EPEL repos, Nmap, sendemail, python36, python36-devel, and git
```
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
sudo yum install nmap python36 python36-devel
sudo yum install git
sudo yum install sendemail
```

## Get vulnersCom and Perform a Vulnerability Scan of httpd, mariadb, and sshd
```
git clone https://github.com/vulnersCom/nmap-vulners.git
sudo mv /home/cloud_user/nmap-vulners/ /usr/share/nmap/scripts/
nmap -sV --script nmap-vulners <IP> -p22,80,3306
```

## Write a Python Script to Incorporate the Scans and Send Results via Email
```
vim /home/cloud_user/scan.py
```
```python
#!/bin/python3.6

import subprocess

p = subprocess.Popen(["nmap", "-sV", "--script", "nmap-vulners", "[IP_ADDRESS]", "-p22,80,3306"], stdout=subprocess.PIPE)
(output, err) = p.communicate()
msg = output.decode('utf-8').strip()

subprocess.check_output(['sendemail', '-f', '[FROM_EMAIL]', '-u', 'AUTH_NOTIFICATION', '-t', '[TO_EMAIL]', '-s', 'smtp.gmail.com:587', '-o', 'tls=yes', '-xu', '[USER_NAME]', '-xp', '[PASSWORD]', '-m', msg], stdin=None, stderr=None, shell=False, universal_newlines=False)
```
```
chmod +x /home/cloud_user/scan.py
```
## Configure the Script to Run Once a Week
```
sudo crontab -e
```
```
@weekly /home/cloud_user/scan.py
```
