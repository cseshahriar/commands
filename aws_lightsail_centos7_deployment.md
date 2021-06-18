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
# for server 1
<pre>hostnamectl set-hostname domain.com</pre>
<pre>hostnamectl set-hostname "app name" --pretty</pre>

# for server 2
<pre>hostnamectl set-hostname pmdb.domain.com</pre>
<pre>hostnamectl set-hostname "app name db" --pretty</pre>

Third, we edit the /etc/hosts file to include the hostname:
<pre>nano /etc/hosts</pre>

Add the hostname to the end of the two lines:

# for server
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

# method 2
sudo yum install wget
cd /tmp
wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
ls *.rpm
To install epel-release-7-11.noarch.rpm, type:
sudo yum install epel-release-latest-7.noarch.rpm


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
or
<pre>sudo yum install redhat-rpm-config python-devel python3-devel python-pip python3-pip python-setuptools python3-setuptools python-wheel python3-wheel python-cffi python36-cffi libffi-devel cairo pango gdk-pixbuf2</pre>
Reference:
https://computingforgeeks.com/how-to-install-python-3-on-centos/


## 5 Install Postgresql-12
1.
<pre>
sudo yum install https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm
sudo yum list postgresql*
sudo yum install postgresql12-server
sudo /usr/pgsql-12/bin/postgresql-12-setup initdb
sudo systemctl start postgresql-12.service
sudo systemctl enable postgresql-12.service
sudo systemctl status postgresql-12.service</pre>

2. Configure Firewall
If the firewall is configured on CentOS, we need to open ports so remote users can connect to PostgreSQL:
<pre>
sudo yum install firewalld
sudo systemctl start firewalld
sudo systemctl enable firewalld
sudo systemctl status firewalld

sudo firewall-cmd --add-service=postgresql --permanent
sudo firewall-cmd --reload</pre>

3. Enable Remote Access
To allow PostgreSQL to accept remote connections, first, we need to change the listen address to * in the configuration file /var/lib/pgsql/12/data/postgresql.conf:
<pre>sudo nano /var/lib/pgsql/12/data/postgresql.conf</pre>

CHANGE TO THIS (192.168.61.135 or *)
<pre>listen_addresses = '192.168.61.135'</pre>

4. Also, we need to let PostgreSQL know to accept remote connections in file /var/lib/pgsql/12/data/pg_hba.conf:
<pre>sudo nano /var/lib/pgsql/12/data/pg_hba.conf</pre>

APPEND/EDIT THE FOLLOWING (choose anywhere or within trusted subnet)
ALSO CHANGE ANY ident METHOD TO md5

5. Accept from anywhere
<pre>host all all 0.0.0.0/0 md5</pre>
    
6. Accept from trusted subnet
<pre>host all all 192.168.61.0/24 md5</pre>
    
7. Enable and Start PostgreSQL Service
<pre>
sudo systemctl enable postgresql-12.service
sudo systemctl start postgresql-12.service</pre>

8. Set PostgreSQL Admin Password
Set admin user and password for PostgreSQL:

<pre>sudo su - postgres
psql -c "alter user postgres with password 'Shosen123#'"</pre>

9. Create User and Database: project_user and project_db
<pre>psql
CREATE USER project_user WITH ENCRYPTED PASSWORD 'Shosen123#';
CREATE DATABASE project_db;
GRANT ALL PRIVILEGES ON DATABASE project_db TO project_user;</pre>

# LIST USERS OR DATABASE USING THE FOLLOWING COMMANDS WITHIN PSQL
<pre>\du   # list users
\l    # list databases
\q    # to quit psql</pre>
exit;

You can drop a database within psql using the following command:
<pre>
psql
DROP DATABASE project_db;</pre>

10 a. Access Remote Database Remotely

From server 192.168.61.140 (app server):
<pre>psql -U project_db_user -h 192.168.61.139 -d project_db</pre>

From server 192.168.61.139 (db server):
<pre>psql -U project_db_user -h 192.168.61.140 -d project_db</pre>

systemctl restart postgresql-12.service

## 6 Django Deploy
1. Install python dependency 
<pre>sudo yum install redhat-rpm-config python-devel python3-devel python-pip python3-pip python-setuptools python3-setuptools python-wheel python3-wheel python-cffi python36-cffi libffi-devel cairo pango gdk-pixbuf2</pre>

2. Create and Activate Virtual Environment
<pre>
cd /opt
sudo python3 -m venv venv
sudo chown -R superuser:superuser venv
source venv/bin/activate</pre>

3. Download project_name repository
<pre>
sudo git clone -b release --single-branch https://github.com/username/project_name.git
sudo chown -R superuser:superuser project_name</pre>

4. Install project requirements.txt
<pre>
cd project_name
pip install -r requirements.txt</pre>

5. Make Migrations (if any)
<pre>
python manage.py makemigrations
python manage.py migrate</pre>

7. Test application
<pre>python manage.py runserver 0.0.0.0:80</pre>

# if require admin privileges
<pre>sudo /opt/venv/bin/python manage.py runserver 0.0.0.0:80</pre>


## 7 Install Nginx
Nginx packages are available in the EPEL repositories. If you don’t have EPEL repository already installed you can do it by typing:
<pre>sudo yum install epel-release</pre>

Install Nginx by typing the following yum command:
<pre>sudo yum install nginx</pre>


Once the installation is complete, enable and start the Nginx service with:
<pre>sudo systemctl enable nginx
sudo systemctl start nginx</pre>


Check the status of the Nginx service with the following command:
<pre>sudo systemctl status nginx</pre>

Use the following commands to open the necessary ports:
<pre>sudo firewall-cmd --permanent --zone=public --add-service=http
sudo firewall-cmd --permanent --zone=public --add-service=https
sudo firewall-cmd --reload</pre>

To verify your Nginx installation, open http://YOUR_IP in your browser of choice, and you will see the default Nginx welcome page as shown in the image below:

Manage the Nginx Service with systemctl

To stop the Nginx service, run:
<pre>sudo systemctl stop nginx</pre>

To start it again, type:
<pre>sudo systemctl start nginx</pre>

To restart the Nginx service :
<pre>sudo systemctl restart nginx</pre>

Reload the Nginx service after you have made some configuration changes:
<pre>sudo systemctl reload nginx</pre>

If you want to disable the Nginx service to start at boot:
<pre>sudo systemctl disable nginx</pre>

And to re-enable it again:
<pre>sudo systemctl enable nginx</pre>

references:
https://linuxize.com/post/how-to-install-nginx-on-centos-7/


3. Install uWSGI into project environment
<pre>pip install uwsgi</pre>

4. Configure uWSGI
<pre>
# change directory to project root directory
<pre>cd /opt/project_dir</pre>

# make directory for uwsgi
<pre> mkdir uwsgi</pre>

# create uwsgi config file
<pre>nano uwsgi/uwsgi.ini</pre>

Copy-paste the configuration into uwsgi.ini:
<pre>
[uwsgi]

# Django-related settings
# the base directory (full path)
chdir           = /opt/PM_Fellowship
# Django's wsgi file (this will be the folder name where wsgi.py and settings.py is)
module          = PM_Fellowship.wsgi
# the virtualenv (full path)
home            = /opt/PM_venv
# process-related settings
# master
master          = true
# maximum number of worker processes
processes       = 10
# the socket (use the full path to be safe
socket          = /opt/PM_Fellowship/uwsgi/uwsgi.sock
# ... with appropriate permissions - may be needed
chmod-socket    = 666
# clear environment on exit
vacuum          = true
</pre>

Create log file:
<pre>
touch uwsgi/uwsgi.log
chmod 664 uwsgi/uwsgi.log</pre>

Test uWSGI:
<pre>uwsgi --ini uwsgi/uwsgi.ini</pre>
If there are any errors please cat or tail the log file uwsgi.log


5. Create SystemD unit file for uWSGI
<pre>
sudo nano /etc/systemd/system/uwsgi.service</pre>

Copy-paste the following configuration:
<pre>
[Unit]
Description=PM Fellowship uWSGI instance
After=network.target postgresql-11.service

[Service]
User=superuser
Group=nginx
WorkingDirectory=/opt/PM_Fellowship
Environment="PATH=/opt/PM_venv/bin"
ExecStart=/opt/PM_venv/bin/uwsgi --ini /opt/PM_Fellowship/uwsgi/uwsgi.ini
Restart=always
KillSignal=SIGQUIT
Type=notify
NotifyAccess=all

[Install]
WantedBy=multi-user.target
</pre>

File and directory ownership

# add superuser to nginx group
<pre>sudo usermod -a -G nginx superuser</pre>

# change ownership to include nginx group
<pre>sudo chown -R superuser:nginx /opt/project_dir
sudo chown -R superuser:nginx /opt/venv</pre>

Start and Enable on startup
<pre>
sudo systemctl daemon-reload
sudo systemctl start uwsgi
sudo systemctl enable uwsgi
</pre>

6. Configure NginX
<pre>sudo nano /etc/nginx/conf.d/project_dir.conf</pre>

Copy-paste the following configuration:
<pre>
upstream uwsgi {
        server unix:/opt/advance-ticket-system/uwsgi/uwsgi.sock;
}

server {
        listen 80;
        server_name 3.6.38.103;
        charset utf-8;

        client_max_body_size 16M;

        location /static {
                alias /opt/advance-ticket-system/static;
        }

	location /media {
                alias /opt/advance-ticket-system/media;
        }

	location / {
                include uwsgi_params;
                uwsgi_pass uwsgi;
                uwsgi_read_timeout 300s;
                uwsgi_send_timeout 300s;
        }
}

</pre>
Test the NginX configuration:

sudo nginx -t
Restart NginX to apply config:

sudo systemctl restart nginx


## opt permissions
<pre>sudo chmod +x file.sh
sudo chmod +x /opt/*
sudo chmod +xw /opt/*
sudo chmod +x /opt/bin/*
sudo chmod -R 777 /opt
PATH=$PATH:/opt/bin
</pre>
--------------------------------------------------------------------------------------------------------------------------------------------------------
21  sudo mkdir /etc/nginx/conf.d
   22  sudo nano /etc/nginx/conf.d/ticket_sytem.conf

	upstream ticket {
		server unix:/opt/advance_ticket_system/uwsgi/uwsgi.sock;
	}

	server {
		listen 80;
		server_name 3.6.38.103;

		access_log /opt/advance_ticket_system/logs/access.log;
		error_log /opt/advance_ticket_system/logs/error.log;

		charset utf-8;
		client_max_body_size 16M;

		location /static {
			alias /opt/advance_ticket_system/staticfiles;
		}

		location /media {
			alias /opt/advance_ticket_system/media;
		}

		location / {
			uwsgi_pass ticket;
			include uwsgi_params;
			uwsgi_read_timeout 300s;
			uwsgi_send_timeout 300s;
		}
	}

   23  sudo sestatus
   24  sudo cat /etc/systemd/system/uwsgi.service 
   	[Unit]
	Description=Advance Ticket System uWSGI instance
	After=network.target postgresql-12.service

	[Service]
	User=superuser
	Group=nginx
	WorkingDirectory=/opt/advance_ticket_system
	Environment="PATH=/opt/venv/bin"
	ExecStart=/opt/venv/bin/uwsgi --ini /opt/advance_ticket_system/uwsgi/uwsgi.ini
	Restart=always
	KillSignal=SIGQUIT
	Type=notify
	NotifyAccess=all

	[Install]
	WantedBy=multi-user.target

   25  sudo systemctl status uwsgi
   26  cd /opt/advance_ticket_system
   27  ll
   28  sudo nano /etc/nginx/conf.d/ticket_sytem.conf
   	upstream ticket {
        	server unix:/opt/advance_ticket_system/uwsgi/uwsgi.sock;
	}

	server {
		listen 80;
		server_name 3.6.38.103;

		access_log /opt/advance_ticket_system/logs/access.log;
		error_log /opt/advance_ticket_system/logs/error.log;

		charset utf-8;
		client_max_body_size 16M;

		location /static {
		        alias /opt/advance_ticket_system/staticfiles;
		}

		location /media {
		        alias /opt/advance_ticket_system/media;
		}

		location / {
		        uwsgi_pass ticket;
		        include uwsgi_params;
		        uwsgi_read_timeout 300s;
		        uwsgi_send_timeout 300s;
		}
	}
   29  sudo nginx -t
   30  sudo nano /etc/nginx/conf.d/ticket_sytem.conf
   	upstream ticket {
        	server unix:/opt/advance_ticket_system/uwsgi/uwsgi.sock;
	}

	server {
		listen 80;
		server_name 3.6.38.103;

		access_log /opt/advance_ticket_system/logs/access.log;
		error_log /opt/advance_ticket_system/logs/error.log;

		charset utf-8;
		client_max_body_size 16M;

		location /static {
		        alias /opt/advance_ticket_system/staticfiles;
		}

		location /media {
		        alias /opt/advance_ticket_system/media;
		}

		location / {
		        uwsgi_pass ticket;
		        include uwsgi_params;
		        uwsgi_read_timeout 300s;
		        uwsgi_send_timeout 300s;
		}
	}
   31  sudo nginx -t
   32  sudo ls /etc/sellinux
   33  sudo nano /etc/selinux/config 
   	# This file controls the state of SELinux on the system.
	# SELINUX= can take one of these three values:
	#     enforcing - SELinux security policy is enforced.
	#     permissive - SELinux prints warnings instead of enforcing.
	#     disabled - No SELinux policy is loaded.
	SELINUX=permissive
	# SELINUXTYPE= can take one of three values:
	#     targeted - Targeted processes are protected,
	#     minimum - Modification of targeted policy. Only selected processes are protected.
	#     mls - Multi Level Security protection.
	SELINUXTYPE=targeted
	sudo shutdown -r now

   34  sudo reboot
   35  sudo nginx -t
   36  sudo systemctl status nginx
   37  sudo nginx -t
   38  sudo systemctl restart nginx
   39  sudo systemctl status nginx
   40  sudo systemctl status uwsgi
   41  cd /opt/
   42  ll
   43  cd advance_ticket_system
   44  ll
   45  ls logs
   46  sudo cat error.log
   47  sudo cat logs/error.log
   48  sudo nginx -t
   49  sudo tail -f /var/log/nginx/error.log
   50  rpm -qa "httpd"
   51  yum remove "httpd*" -y
   52  sudo yum remove "httpd*" -y
   53  sudo netstat -tulpn
   54  sudo rm -rf /var/www
   55  rm -rf /etc/httpd
   56  rm -rf /usr/lib64/httpd
   57  userdel -r apache
   58  sudo userdel -r apache
   59  sudo grep "apache" /etc/passwd
   60  sudo systemctl status httpd
   61  sudo nginx -t
   62  sudo systemctl restart nginx
   63  sudo systemctl status nginx
   64  sudo tail -f /var/log/nginx/error.log
   65  sudo systemctl restart nginx
   66  sudo systemctl daemon-reload
   67  ll
   68  sudo nano  ticket_system/settings.py 
   69  sudo systemctl restart uwsgi
   70  sudo systemctl status uwsgi
   71  ls - al
   72  ls -al
   73  cd ../
   74  ll
   75  cd advance_ticket_system/
   76  ll
   77  cat logs/access.log 
   78  sudo sestatus
   79  cat logs/error.log 
   80  ls -al uwsgi/
   81  cat uwsgi/uwsgi.log 
   82  sudo cat uwsgi/uwsgi.log 
   83  sudo nano /etc/nginx/conf.d/ticket_sytem.conf 
   
   	upstream ticket {
        	server unix:/opt/advance_ticket_system/uwsgi/uwsgi.sock;
	}

	server {
		listen 80;
		server_name 3.6.38.103;

		access_log /opt/advance_ticket_system/logs/access.log;
		error_log /opt/advance_ticket_system/logs/error.log;

		charset utf-8;
		client_max_body_size 16M;

		location /static {
		        alias /opt/advance_ticket_system/staticfiles;
		}

		location /media {
		        alias /opt/advance_ticket_system/media;
		}

		location / {
		        uwsgi_pass ticket;
		        include uwsgi_params;
		        uwsgi_read_timeout 300s;
		        uwsgi_send_timeout 300s;
		}
	}


   84  sudo systemctl restart nginx
   85  cat logs/error.log 
   86  sudo nano /etc/nginx/conf.d/ticket_sytem.conf
   	upstream ticket {
        	server unix:/opt/advance_ticket_system/uwsgi/uwsgi.sock;
	}

	server {
		listen 80;
		server_name 3.6.38.103;

		access_log /opt/advance_ticket_system/logs/access.log;
		error_log /opt/advance_ticket_system/logs/error.log;

		charset utf-8;
		client_max_body_size 16M;

		location /static {
		        alias /opt/advance_ticket_system/staticfiles;
		}

		location /media {
		        alias /opt/advance_ticket_system/media;
		}

		location / {
		        uwsgi_pass ticket;
		        include uwsgi_params;
		        uwsgi_read_timeout 300s;
		        uwsgi_send_timeout 300s;
		}
	}

   
   87  sudo nginx -t
   88  sudo systemctl restart nginx
   89  sudo systemctl status nginx
   90  sudo nano uwsgi/uwsgi.ini 
   91  ll
   92  sudo systemctl restart uwsgi
   93  history
   94  exit
   95  history
