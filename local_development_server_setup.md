# Development Environment Setup with Ubuntu 20.4

## Vscode Install
  <pre>
  https://code.visualstudio.com/sha/download?build=stable&os=linux-deb-x64 
  Extensions: Python, Tabnine Autocomplete AI, Djaneiro,Pylance,Pylint, Material Icon Theme, Fira code, Code Spell Checker, Django, Git History
              PyCharm's Darcula Theme, VSCode Great Icons, Bracket Pair Colorizer 2 (CoenraadS) 
  https://github.com/tonsky/FiraCode
  
  settings.json : 
  // my custom settings
    "editor.formatOnSave": true,
    "editor.rulers": [79],
    "files.exclude": {
        "**/.git": true,
        "**/.svn": true,
        "**/.hg": true,
        "**/CVS": true,
        "**/.DS_Store": true,
        ".vscode": true,
        "**/*.pyc": true,
    },
    "workbench.editor.enablePreview": false,
    "editor.minimap.enabled": false,
    "python.linting.pylintArgs": [
        "--load-plugins",
        "pylint_django"
    ]
    </pre>
  
## Python 
<pre>
  # pyhon2
  sudo apt-get update
  sudo apt-get install python-pip python-dev libpq-dev postgresql postgresql-contrib

  #python3
  sudo apt-get update
  sudo apt-get install python3-pip python3-dev libpq-dev postgresql postgresql-contrib
  sudo apt-get install python3-venv
  python -m pip install --upgrade pip
  pip install autopep8
  pip install pylint
  pip install pylint
  sudo apt install flake8
  pip install psycopg2-binary or pip install psycopg2
  sudo apt-get install libpq-dev
  </pre>
  
## Nodejs
  <pre>
  Installing Node.js via package manager
  # Using Ubuntu
  sudo apt install curl
  curl -fsSL https://deb.nodesource.com/setup_14.x | sudo -E bash -
  sudo apt-get install -y nodejs
</pre>

## Apache
  <pre>
  sudo apt-get install apache2
  sudo apt-get install php libapache2-mod-php
  sudo apt-get install vim
  
  cd /var/www/html
  sudo vim index.php
  i // insert mode
  :wq // save and exit
  
  sudo gedit /etc/apache2/mods-available/dir.conf
  <IfModule mod_dir.c>
	  DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
  </IfModule>
  sudo systemctl status apache2	
  sudo service apache2 restart
  sudo service apache2 stop
  sudo service apache2 start

  ls -la
  sudo chown pcusrname:pcusername -R ./
  
  sudo gedit /etc/apache2/envvars
  16 export APACHE_RUN_USER=shosen
  17 export APACHE_RUN_GROUP=shosen
  
  sudo service apache2 restart
  
  sudo apt-get install mysql-server
  sudo service mysql status
  sudo apt-get install phpmyadmin
  
  configuring phpmyadmin
  apache2 - *(spacebar)
  ok
  yes
  password: shosen123#
  
  sudo mysql
  ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';
  exit
   
  </pre>

## Git

	sudo apt-get install git

	
## Postgresql 
	sudo apt update
	sudo apt -y upgrade
	sudo apt install postgresql postgresql-client
	
	sudo systemctl stop postgresql.service
	sudo systemctl start postgresql.service
	sudo systemctl enable postgresql.service
	or
	
	sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
	wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
	sudo apt-get update
	sudo apt-get -y install postgresql
	
	sudo passwd postgres
	ttttttt123#
	
	sudo su -l postgres
	psql
	psql -c "alter user postgres with password 'my00pass'"
	
	alter user dbuser with password 'tpl123'
	
	createuser dbusersam
	createdb samdb -O dbusersam
	psql samdb
	
	sudo nano /etc/postgresql/12/main/postgresql.conf
	#listen_addresses= ‘+’ // remote access
	
	sudo service postgresql restart
	
	# Pgadmin: Install the public key for the repository (if not done previously):
	curl https://www.pgadmin.org/static/packages_pgadmin_org.pub | sudo apt-key add
	
	# Create the repository configuration file:
	sudo sh -c 'echo "deb https://ftp.postgresql.org/pub/pgadmin/pgadmin4/apt/$(lsb_release -cs) pgadmin4 main" > /etc/apt/sources.list.d/pgadmin4.list && apt 	   update'
	
	# Install for both desktop and web modes:
	sudo apt install pgadmin4
	
	# Install for desktop mode only:
	sudo apt install pgadmin4-desktop
	
# Feferences 
https://djangocentral.com/visual-studio-code-setup-for-django-developers/
https://medium.com/dev-genius/best-visual-studio-code-extensions-for-python-django-af2fdbf7198a
https://github.com/nodesource/distributions/blob/master/README.md#deb
https://ubuntu.com/server/docs/web-servers-apache
https://linuxhint.com/postgresql_installation_guide_ubuntu_20-04/
https://www.pgadmin.org/download/pgadmin-4-apt/
https://www.digitalocean.com/community/tutorials/how-to-use-postgresql-with-your-django-application-on-ubuntu-14-04
  
  
