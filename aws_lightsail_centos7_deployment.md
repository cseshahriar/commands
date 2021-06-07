# Django application deploy in aws lightsain with centos, nginx, uWSGI

## 1.Add Sudo User

1. Add sudo user for app and db:
sudo useradd superuser
Verify superuser after creation:
id superuser

Set password for superuser:
passwd superuser

2. Add superuser to the wheel group for sudo rights:
usermod -aG wheel superuser
Switch to superuser:

su superuser
Test sudo rights:

sudo ls -al /root
Reference:

https://www.cyberciti.biz/faq/create-a-new-user-account-in-centos-7-8-linux/
