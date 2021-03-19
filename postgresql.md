# Postgresql

<pre>sudo -u postgres psql / sudo su - postgres</pre>

# Alter User
<pre>
sudo -u postgres psql
sudo -u postgres psql template1
ALTER USER postgres PASSWORD 'newPassword';
</pre>
# Restart: 
  
<pre>sudo systemctl restart postgresql</pre>

list:
<pre>\l</pre>

list table:
<pre>\dt</pre>

colums names:
<pre>\d table_name</pre>

create:
<pre>CREATE DATABASE name;</pre>

<pre>
dropdb dbname
createdb dbname
</pre>

select this name:
<pre>\c birthdays</pre>

restore remote Database:
<pre>psql -U postgres -h localhost -f db.sql -d dbname</pre>
or 
<pre>psql test < dbname.bak</pre>

There are several options for the backup format:
*.bak: compressed binary format
*.sql: plaintext dump
*.tar: tarball

backup:
<pre>pg_dump dbname > dbname.bak</pre>


Automate Backups with a Cron Task:
You may want to set up a cron job so that your database will be backed up automatically at regular intervals.
The steps in this section will set up a cron task that will run pg_dump once every week.

Make sure you are logged in as the postgres user:
<pre>su - postgres</pre>

Create a directory to store the automatic backups:
<pre>mkdir -p ~/postgres/backups</pre>
Edit the crontab to create the new cron task:

<pre>crontab -e</pre>
Add the following line to the end of the crontab:

File: crontab
<pre>0 0 * * 0 pg_dump -U postgres dbname > ~/postgres/backups/dbname.bak</pre>
Save and exit from the editor. Your database will be backed up at midnight every Sunday. 
To change the time or frequency of the updates, see our Schedule Tasks with Cron guide.


useful links:
https://www.linode.com/docs/guides/how-to-back-up-your-postgresql-database/
