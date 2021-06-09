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

# for APP-SERVER
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

Host server 1
    <pre>
    HostName 192.168.61.141
    User superuser
    Port 22</pre>
    
Host server 2
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


## 4 Install Python 3.8
1. Install yum-utils and Development Tools:
<pre>sudo yum install yum-utils
sudo yum groups install "Development Tools"</pre>

2. Install EPEL:
<pre>
sudo yum install epel-release
sudo yum install epel-release-7-11.noarch.rpm
sudo yum check-update
sudo yum update</pre>

3. Install Python 3.8:
Step 1: Install Python Dependencies
<pre>
sudo yum -y update
sudo yum -y groupinstall "Development Tools"
sudo yum -y install openssl-devel bzip2-devel libffi-devel
</pre>

Step 2: Download latest Python 3.8 Archive
<pre>
sudo yum -y install wget
wget https://www.python.org/ftp/python/3.8.3/Python-3.8.3.tgz
</pre>

Extract the package.
<pre>
tar xvf Python-3.8.3.tgz</pre>

Change the created directory:
<pre>cd Python-3.8*/</pre>

Step 2: Install Python 3.8 on CentOS 7 / CentOS 8
Setup installation by running the configure script.
<pre>./configure --enable-optimizations</pre>

Initiate compilation of Python 3.8 on CentOS 7.
<pre>sudo make altinstall</pre>

If this was successful, you should get a message like below:

Confirm that the installation of Python 3.8 on CentOS 8 / CentOS 7 was successful.
<pre>python3.8 --version</pre>
Python 3.8.3

Pip is also installed.
<pre>pip3.8 --version</pre>

<pre>sudo yum install python3 python3-libs python3-devel python3-pip </pre>
Reference:
https://computingforgeeks.com/how-to-install-python-3-on-centos/


## 5 Install Postgresql-12
<pre>
sudo yum install https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm
sudo yum list postgresql*
sudo yum install postgresql12-server
sudo /usr/pgsql-12/bin/postgresql-12-setup initdb
sudo systemctl start postgresql-12.service
sudo systemctl enable postgresql-12.service
sudo systemctl status postgresql-12.service</pre>

## 6 Install Nginx
<pre>
sudo yum install epel-release
sudo yum install nginx
sudo systemctl start nginx
sudo systemctl status nginx</pre>

## install uwsgi
<pre>pip install uwsgi</pre>
