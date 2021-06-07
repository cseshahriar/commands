# Django application deploy in aws lightsain with centos, nginx, uWSGI

## 1.Add Sudo User

1. Add sudo user for app and db:
<pre>sudo useradd superuser</pre>

Verify superuser after creation:
<pre>id superuser</pre>

Set password for superuser:
<pre>passwd superuser</pre>

2. Add superuser to the wheel group for sudo rights:
<pre>usermod -aG wheel superuser</pre>

Switch to superuser:
<pre>su superuser</pre>

Test sudo rights:
<pre>sudo ls -al /root</pre>

Reference:

https://www.cyberciti.biz/faq/create-a-new-user-account-in-centos-7-8-linux/


## 2. Edit-Hostname

First, we can check what the current hostname is: <pre>hostnamectl</pre>
# output of hostnamectl

Secondly, we set a new hostname using a Fully Qualified Domain Name (FQDN):
# for PM-APP-SERVER
<pre>hostnamectl set-hostname pmfellowship.pmo.gov.bd</pre>
<pre>hostnamectl set-hostname "PM Fellowship APP" --pretty</pre>

# for PM-DB-SERVER
<pre>hostnamectl set-hostname pmdb.pmo.gov.bd<pre>
<pre>hostnamectl set-hostname "PM Fellowship DB" --pretty<pre>

Third, we edit the /etc/hosts file to include the hostname:
<pre>nano /etc/hosts</pre>

Add the hostname to the end of the two lines:

# for PM-APP-SERVER
<pre>127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4 pmfellowship.pmo.gov.bd</pre>
<pre>::1         localhost localhost.localdomain localhost6 localhost6.localdomain6 pmfellowship.pmo.gov.bd<pre>

Finally, reboot the server
<pre>reboot</pre>

Reference:
https://phoenixnap.com/kb/how-to-set-or-change-a-hostname-in-centos-7
