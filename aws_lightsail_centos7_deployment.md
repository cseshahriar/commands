# Django application deploy in aws lightsain with centos, nginx, uWSGI

## 1.Add Sudo User

1. Add sudo user for app and db:
'sudo useradd superuser'

Verify superuser after creation:
'id superuser'

Set password for superuser:
'passwd superuser'

2. Add superuser to the wheel group for sudo rights:
'usermod -aG wheel superuser'

Switch to superuser:
'su superuser'

Test sudo rights:
'sudo ls -al /root'

Reference:

https://www.cyberciti.biz/faq/create-a-new-user-account-in-centos-7-8-linux/


## 2. Edit-Hostname

First, we can check what the current hostname is: 'hostnamectl'
# output of hostnamectl

Secondly, we set a new hostname using a Fully Qualified Domain Name (FQDN):
# for PM-APP-SERVER
'hostnamectl set-hostname pmfellowship.pmo.gov.bd'
'hostnamectl set-hostname "PM Fellowship APP" --pretty'

# for PM-DB-SERVER
'hostnamectl set-hostname pmdb.pmo.gov.bd
'hostnamectl set-hostname "PM Fellowship DB" --pretty

Third, we edit the /etc/hosts file to include the hostname:
'nano /etc/hosts'

Add the hostname to the end of the two lines:

# for PM-APP-SERVER
'127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4 pmfellowship.pmo.gov.bd'
'::1         localhost localhost.localdomain localhost6 localhost6.localdomain6 pmfellowship.pmo.gov.bd'

Finally, reboot the server
'reboot'

Reference:
https://phoenixnap.com/kb/how-to-set-or-change-a-hostname-in-centos-7
