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
# for APP-SERVER
<pre>hostnamectl set-hostname domain.com</pre>
<pre>hostnamectl set-hostname "app name" --pretty</pre>

# for DB-SERVER
<pre>hostnamectl set-hostname pmdb.domain.com</pre>
<pre>hostnamectl set-hostname "app name db" --pretty</pre>

Third, we edit the /etc/hosts file to include the hostname:
<pre>nano /etc/hosts</pre>

Add the hostname to the end of the two lines:

# for PM-APP-SERVER
<pre>127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4 domain.com</pre>
<pre>::1         localhost localhost.localdomain localhost6 localhost6.localdomain6 domain.com</pre>

Finally, reboot the server
<pre>reboot</pre>

Reference:
https://phoenixnap.com/kb/how-to-set-or-change-a-hostname-in-centos-7


Add a new user to the aws Linux instance
<pre> sudo adduser new_user</pre>

Note: If you add the new_user to an Ubuntu instance, include the --disabled-password option to avoid adding a password to the new account:
<pre>sudo adduser new_user --disabled-password</pre>

3. Change the security context to the new_user account so that folders and files you create have the correct permissions:
<pre>sudo su - new_user</pre>

Note: When you run the sudo su - new_user command, the name at the top of the command shell prompt changes to reflect the new user account context of your shell session.

4. Create a .ssh directory in the new_user home directory:

<pre>mkdir .ssh</pre>
5. Use the chmod command to change the .ssh directory's permissions to 700. Changing the permissions restricts access so that only the new_user can read, write, or open the .ssh directory.
<pre>chmod 700 .ssh</pre>

6. Use the touch command to create the authorized_keys file in the .ssh directory:
<pre>touch .ssh/authorized_keys</pre>

7. Use the chmod command to change the .ssh/authorized_keys file permissions to 600. Changing the file permissions restricts read or write access to the new_user.
<pre>chmod 600 .ssh/authorized_keys</pre>

# nano
<pre>sudo yum install -y nano</pre>

## 3. Edit SSH Config
For quick access to servers internally, SSH config file created or edited.
Following file needs to be created or edited: ~/.ssh/config:

<pre>sudo nano ~/.ssh/config
~/.ssh/config:</pre>

Host APP-SERVER
    <pre>
    HostName 192.168.61.141
    User superuser
    Port 22</pre>
    
Host DB-SERVER
    <pre>HostName 192.168.61.132
    User superuser
    Port 22</pre>
We can now use the following command to access the servers using HOST name:

ssh APP-SERVER

References:
https://linuxize.com/post/using-the-ssh-config-file/

## Set SELinux
1. Check the SELinux Status
To view the current SELinux status and the SELinux policy that is being used on your system, use the sestatus command:

<pre>sestatus</pre>
You can temporarily change the SELinux mode from targeted to permissive with the following command:

<pre>sudo setenforce 0</pre>
2a. Set SELinux Status to Permissive
To set SELinux to Permissive on your CentOS 7 system, follow the steps. Open the /etc/selinux/config file and set the SELINUX mod to permissive:

<pre>sudo nano /etc/selinux/config</pre>
Save the file and reboot your CentOS system with:

<pre>sudo shutdown -r now</pre>
Once the system boots up, verify the change with the sestatus command:

<pre>sestatus</pre>
2b. Disable SELinux
To permanently disable SELinux on your CentOS 7 system, follow the steps. Open the /etc/selinux/config file and set the SELINUX mod to disabled:

<pre>sudo nano /etc/selinux/config</pre>
Save the file and reboot your CentOS system with:

<pre>sudo shutdown -r now</pre>
Once the system boots up, verify the change with the sestatus command:

<pre>sestatus</pre>
References:
https://linuxize.com/post/how-to-disable-selinux-on-centos-7/
