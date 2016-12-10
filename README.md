# **Linux Server Configuration**
###### Udacity's Full Stack Web Developer Nanodegree
---
#### Project Overview
---
Take a baseline installation of a Linux distribution on a virtual machine and prepare it to host your web applications, to include installing updates, securing it from a number of attack vectors and installing/configuring web and database servers.

#### Important Project Information
---
Public IP Address: 35.165.249.98

SSH port: 2200

Application URL: [http://ec2-35-165-249-98.us-west-2.compute.amazonaws.com/](http://ec2-35-165-249-98.us-west-2.compute.amazonaws.com/)


#### 1. Launch your Virtual Machine with your Udacity account and log in. You can manage your virtual server [here](https://www.udacity.com/account#!/development_environment)
---

* 1.1 Download Private Key
* 1.2 Move the private key file into the folder ~/.ssh (where ~ is your environment's home directory). So if you downloaded the file to the Downloads folder, just execute the following command in your terminal.

        $ mv ~/Downloads/udacity_key.rsa ~/.ssh/
* 1.3 Open your terminal and type in

        $ chmod 600 ~/.ssh/udacity_key.rsa
* 1.4 In your terminal, type in

        $ ssh -i ~/.ssh/udacity_key.rsa root@INSERT_YOUR_PUBLIC_IP_ADDRESS
* 1.5 A message will appear:

        Are you sure you want to continue connecting (yes/no)?

* 1.6 Type `yes`

* 1.7 You will now be logged in as the root user in the Virtual Machine. For example:

        root@ip-10-20-43-32:~#

#### 2. Create a new user named **grader**.
---

Sources: [**Digitalocean**](https://www.digitalocean.com/community/tutorials/how-to-create-a-sudo-user-on-ubuntu-quickstart),

* 2.1 Add a new user:

        # sudo adduser grader
* 2.2 Set and confirm the new user's password at the prompt.
    Enter a password.

        Enter new UNIX password:
        new UNIX password:
        passwd: password updated successfully
* 2.3 Follow the prompts to set the new user's information. It is fine to accept the defaults to leave all of this information blank.

        Enter the new value, or press ENTER for the default
        Full Name []:
        Room Number []:
        Work Phone []:
        Home Phone []:
        Other []:
* 2.4  Type `y` and then enter if the information is correct.

        Is the information correct? [Y/n]

#### 3. Grant **grader** user sudo permissions.
---

Sources: [**askubuntu**](http://askubuntu.com/questions/7477/how-can-i-add-a-new-user-as-sudoer-using-the-command-line), [**ubuntu**](https://help.ubuntu.com/community/RootSudo#Allowing_other_users_to_run_sudo)

* 3.1 Add user to the sudo group

        # sudo adduser grader sudo
        Adding user `grader' to group `sudo' ...
        Adding user grader to group sudo
        Done.

#### 4. Update all currently installed packages.
---

Sources: [**askubuntu**](http://askubuntu.com/questions/196768/how-to-install-updates-via-command-line), [**unix.stackexchange**](http://unix.stackexchange.com/questions/113732/a-new-version-of-configuration-file-etc-default-grub-is-available-but-the-vers)

* 4.1 Fetch the list of current available updates.

        # sudo apt-get update
* 4.1 Upgrade the current packages.

        # sudo apt-get upgrade
* 4.2 You will be prompted to continue, type `y` and then enter.

        Do you want to continue? [Y/n]

* 4.3 (optional) Skip this step if you didn't receive this message:

    Source: [**stackexchange**](http://unix.stackexchange.com/questions/113732/a-new-version-of-configuration-file-etc-default-grub-is-available-but-the-vers)

        A new version of /boot/grub/menu.lst is available, but the version installed currently has been locally modified. What would you like to do about menu.lst?

    Select: `install the package maintainer's version`

#### 5. Configure the local timezone to UTC.
---

Sources: [**askubuntu**](http://askubuntu.com/questions/138423/how-do-i-change-my-timezone-to-utc-gmt)

* 5.1 Execute this command:

        # sudo dpkg-reconfigure tzdata

* 5.2 A menu will appear, select `none of the above`.

* 5.3 Select time zone `UTC`.

#### 6. Add public key authentication.
---

Sources: [**Digitalocean**](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-14-04)

* 6.1 Generate a new key pair by entering the following command at the terminal of your **local machine**.

        $ ssh-keygen
        Generating public/private rsa key pair.
        Enter file in which to save the key (/Users/localuser/.ssh/id_rsa):

* 6.2 Hit enter to accept this file name and path(or enter a new name).

* 6.3 You will be prompted for a passphrase to secure the key with. You may either enter a passphrase or leave the passphrase blank.

        Enter passphrase (empty for no passphrase):
        Enter same passphrase again:

* 6.4 Print the public key.

        $ cat ~/.ssh/id_rsa.pub

* 6.5 Select the public key and copy it. Should look similar to below:

        ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDBGTO0tsVejssuaYR5R3Y/i73SppJAhme1dH7W2c47d4gOqB4izP0+fRLfvbz/tnXFz4iOP/H6eCV05hqUhF+KYRxt9Y8tVMrpDZR2l75o6+xSbUOMu6xN+uVF0T9XzKcxmzTmnV7Na5up3QM3DoSRYX/EP3utr2+zAqpJIfKPLdA74w7g56oYWI9blpnpzxkEd3edVJOivUkpZ4JoenWManvIaSdMTJXMy3MtlQhva+j9CgguyVbUkdzK9KKEuah+pFZvaugtebsU+bllPTB0nlXGIJk98Ie9ZtxuY3nCKneB+KjKiXrAvXUPCI9mWkYS/1rggpFmu3HbXBnWSUdf localuser@machine.local

* 6.6 Add public key to remote user on your **virtual machine**.

        # su - grader

    >**Note**: You will now be logged in as **grader**

* 6.7 Create a new directory called .ssh.

        $ mkdir .ssh

* 6.8 Restrict its' permissions with the following command:

        $ chmod 700 .ssh

* 6.9 Edit the file called authorized_keys in .ssh.

        $ nano .ssh/authorized_keys

* 6.10 Insert your public key from step 6.5. `CTRL-O` to save changes, confirm the file name, and `CTRL-X` to exit.

* 6.11 Restrict the permissions of the authorized_keys file.

        $ chmod 600 .ssh/authorized_keys

#### 7. Change the SSH port from 22 to 2200 and disable root access.
---

Sources: [**2daygeek**](http://www.2daygeek.com/how-to-change-the-ssh-port-number/), [**superuser**](http://superuser.com/questions/161609/can-someone-explain-the-passwordauthentication-in-the-etc-ssh-sshd-config-fil), [**linuxlookup**](http://www.linuxlookup.com/howto/change_default_ssh_port), [**digitalocean**](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-14-04)

* 7.1 Open the configuration file.

        $ sudo nano /etc/ssh/sshd_config

    >**Note**: May have to enter password for **grader**.

* 7.2 Change port 22 to 2200.

        # What ports, IPs and protocols we listen for
        Port 2200

* 7.3 Disable root access by changing `without-password` to `no`.

        PermitRootLogin no

* 7.4 `CTRL-X` to exit, save changes, and confirm file name.

* 7.5 Restart the SSH service so that it will use our new configuration.

        $ sudo service ssh restart

* 7.6 (Optional) Verify from **Local Machine** that new connections can be established successfully.

        $ ssh grader@YOUR_PUBLIC_IP_ADDRESS


    >**Note**: You should be logged in as **grader**, try various commands, and exit when satisfied.

#### 8. Configure the Uncomplicated Firewall (UFW)
---

Sources: [**ubuntu**](https://help.ubuntu.com/community/UFW), [**digitalocean**](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-14-04)

* 8.1 Allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123).

        $ sudo ufw allow ssh
        $ sudo ufw allow 2200/tcp
        $ sudo ufw allow 80/tcp
        $ sudo ufw allow 123/tcp

* 8.2 Turn UFW on with the default set of rules:

        $ sudo ufw enable

* 8.3 Respond to the prompt `y`.

        Command may disrupt existing ssh connections. Proceed with operation (y|n)?


#### 9. (Optional) Received **sudo: unable to resolve host ip-10-20-43-32** error?
---

Sources: [**aws**](https://forums.aws.amazon.com/message.jspa?messageID=495274)

* 9.1 Open /etc/hosts file.

        $ sudo nano /etc/hosts

* 9.2 Add `127.0.1.1 ip-10-20-54-68` to the file, should look similar to below:

        127.0.0.1 localhost
        127.0.1.1 ip-10-20-54-68  #Add this line

        # The following lines are desirable for IPv6 capable hosts
        ::1 ip6-localhost ip6-loopback
        fe00::0 ip6-localnet
        ff00::0 ip6-mcastprefix
        ff02::1 ip6-allnodes
        ff02::2 ip6-allrouters
        ff02::3 ip6-allhosts

    >**Note**: `ip-10-20-54-68` your machine name may be different.

* 9.3 `CTRL-X` to exit, save changes, and confirm file name.

#### 10. Install and configure Apache to serve a Python mod_wsgi application.
---

Sources: [**digitalocean**](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu), [**digitalocean**](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps)

* 10.1 Install Apache, a web server software.

        $ sudo apt-get install apache2

* 10.2 You will be prompted to continue, type `y` and then enter.

        Do you want to continue? [Y/n] y

* 10.3 (Optional) To check if Apache is installed, direct your browser to your server’s IP address (eg. http://35.163.49.226/). The page should display the Apache2 Ubuntu Default Page.


* 10.4 Install mod_wsgi.

        $ sudo apt-get install libapache2-mod-wsgi python-dev

    >**Note 1**: WSGI (Web Server Gateway Interface) is an interface between web servers and web apps for python. Mod_wsgi is an Apache HTTP server mod that enables Apache to serve Flask applications.

* 10.5 You will be prompted to continue, type `y` and then enter.

        Do you want to continue? [Y/n] y

* 10.6 (Optional) If you get this error: **apache2: Could not determine the server's fully qualified domain name, using 127.0.0.1 for ServerName**.

    Source: [**ubuntu**](https://help.ubuntu.com/community/ApacheMySQLPHP#Troubleshooting_Apache), [**askubuntu**](http://askubuntu.com/questions/256013/could-not-reliably-determine-the-servers-fully-qualified-domain-name)

    Run this command:

        $ echo "ServerName localhost" | sudo tee /etc/apache2/conf-available/fqdn.conf && sudo a2enconf fqdn


    To activate the new configuration, you need to run:

        $ service apache2 reload

    >**Note**: This is just a friendly warning and not really a problem (as in that something does not work)

* 10.7 Enable mod_wsgi.

        $ sudo a2enmod wsgi

    >**Note**: Module wsgi may already be enabled.

* 10.8 Restart Apache with the following command to apply the changes:

        $ sudo service apache2 restart

#### 11. Install and configure PostgreSQL.
---

Sources: [**digitalocean**](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-16-04), [**digitalocean**](https://www.digitalocean.com/community/tutorials/how-to-secure-postgresql-on-an-ubuntu-vps) [**postgresql**](https://www.postgresql.org/docs/9.2/static/auth-methods.html#AUTH-IDENT), [**postgresql**](https://www.postgresql.org/docs/9.2/static/app-createuser.html), [**postgresql**](https://www.postgresql.org/docs/9.0/static/sql-createdatabase.html), [**postgresql**](https://www.postgresql.org/docs/9.1/static/manage-ag-createdb.html), [**stackoverflow**](http://stackoverflow.com/questions/10861260/how-to-create-user-for-a-db-in-postgresql)

* 11.1 Install the the Postgres package.

        $ sudo apt-get install postgresql postgresql-contrib

    >**Note**: `-contrib` package adds some additional utilities and functionality.

* 11.2 You will be prompted to continue, type `y` and then enter.

        Do you want to continue? [Y/n] y

* 11.3 Postgres creates a Linux user called *postgres* which can be used to access the system. We can change to this user by typing:

        $ sudo su - postgres

* 11.4 Connect to the PostgreSQL shell.

        $ psql

* 11.5 Create user *catalog* with a password.

        postgres-# CREATE USER catalog WITH PASSWORD 'password';

* 11.6 Create database named *catalog* where user *catalog* can configure and manage.

        postgres-# CREATE DATABASE catalog OWNER catalog;

* 11.7 Exit the PostgreSQL shell.

        postgres-# \q

* 11.8 Exit from *postgres* user.

        $ exit

    >**Note**: Back to **grader** user

* 11.9 Create a Linux user with the same name as the Postgres role and database in order to log in with ident based authentication. Follow steps 2.2 - 2.4.

        $ sudo adduser catalog


#### 12. (Optional) Check remote connections and log in with ident authentication.
---

Sources: [**digitalocean**](https://www.digitalocean.com/community/tutorials/how-to-secure-postgresql-on-an-ubuntu-vps), [**digitalocean**](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-16-04)

* 12.1 Double check that no remote connections are allowed(Default Setting).

        $ sudo nano /etc/postgresql/9.3/main/pg_hba.conf

    >**Note**: [**stackoverflow**](http://stackoverflow.com/questions/11919275/can-not-enter-and-change-postgresql-pg-hba-conf-file) If you cant find the pg_hba.conf file, enter `$ sudo find / -name pg_hba.conf`

* 12.2 Edit the uncommented part if it doesn't look like this. Save changes if necessary and exit.

        local   all             postgres                                peer
        local   all             all                                     peer
        host    all             all             127.0.0.1/32            md5
        host    all             all             ::1/128                 md5

* 12.3 log in with ident authentication.

        $ sudo -u catalog psql

    >**Note**: [**postgresql**](https://www.postgresql.org/docs/9.2/static/auth-methods.html#AUTH-IDENT) You will be logged in automatically if everything is configured properly. `\q` to exit.

#### 13. Install git, clone and set up FlaskApp App project.
---

Sources: [**digitalocean**](https://www.digitalocean.com/community/tutorials/how-to-install-git-on-ubuntu-14-04), [**digitalocean**](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps)

* 13.1 Install Git.

        $ sudo apt-get install git

* 13.2 You will be prompted to continue, type `y` and then enter.

        Do you want to continue? [Y/n] y

* 13.3 Navigate to the /var/www directory.

        $ cd /var/www

* 13.5 Create the application directory.

        $ sudo mkdir Catalog

* 13.6 Change ownership (both user and group) of all files and directories inside of *Catalog*.

        $ sudo chown -R grader:grader Catalog

    >**Note**: [**askubuntu**](http://askubuntu.com/questions/6723/change-folder-permissions-and-ownership) Now the **grader** user can git clone into this folder.

* 13.7 Move inside the *Catalog* directory.

        $ cd Catalog

* 13.8 Clone the Catalog App to the Virtual Machine.

        $ git clone https://github.com/egarcia410/catalog.git Catalog

    >**Note**: Creates another **Catalog** folder with the cloned project.

* 13.9 Move inside the inner *Catalog* directory.

        $ cd Catalog

* 13.10 Rename `project.py` to `__init__.py`

        $ sudo mv project.py __init__.py

#### 14. Set up virtual environment, install Flask, and Item Catalog App dependencies.
---

Sources: [**digitalocean**](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps)

* 14.1 Install pip.

        $ sudo apt-get install python-pip

* 14.2 You will be prompted to continue, type `y` and then enter.

        Do you want to continue? [Y/n] y

* 14.3 Install virtualvenv.

        $ sudo pip install virtualenv

* 14.4 Set virtual environment to name *venv*.

        $ sudo virtualenv venv

* 14.5 Activate the virtual environment.

        $ source venv/bin/activate

* 14.6 Change permissions to the virtual environment.

        (venv) sudo chmod -R 777 venv

* 14.7 Install Flask and dependencies.

        (venv) sudo pip install Flask
        (venv) pip install requests
        (venv) pip install httplib2
        (venv) sudo pip install sqlalchemy
        (venv) pip install oauth2
        (venv) sudo pip install --upgrade oauth2client

* 14.8 Install dependencies for psycopg2.

        (venv) sudo apt-get install python-psycopg2
        (venv) sudo apt-get install libpq-dev
        (venv) pip install psycopg2

    >**Note**: [**stackoverflow**](http://stackoverflow.com/questions/12906351/importerror-no-module-named-psycopg2)

* 14.9 Test if the installation is successful and the app is running.

        (venv) python __init__.py

    If you see a similar message, you have successfully configured the app:

        * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
        * Restarting with stat
        * Debugger is active!
        * Debugger pin code: 138-920-380

* 14.10 Add items to the database.

        (venv) python lotsofmenus.py

* 14.11 Exit the virtual environment.

        (venv) deactivate

#### 15 Configure and enable a new virtual host.
---

Sources: [**digitalocean**](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps), [**digitalocean**](https://www.digitalocean.com/community/tutorials/how-to-set-up-apache-virtual-hosts-on-centos-6)

* 15.1 Open this file:

        $ sudo nano /etc/apache2/sites-available/Catalog.conf

* 15.2 Add the following lines of code to the file to configure the virtual host.

        <VirtualHost *:80>
                ServerName 35.160.40.188
                ServerAdmin admin@example.com
                ServerAlias ec2-35-160-40-188.us-west-2.compute.amazonaws.com
                WSGIScriptAlias / /var/www/Catalog/catalog.wsgi
                <Directory /var/www/Catalog/Catalog/>
                    Order allow,deny
                    Allow from all
                </Directory>
                Alias /static /var/www/Catalog/Catalog/static
                <Directory /var/www/Catalog/Catalog/static/>
                    Order allow,deny
                    Allow from all
                </Directory>
                ErrorLog ${APACHE_LOG_DIR}/error.log
                LogLevel warn
                CustomLog ${APACHE_LOG_DIR}/access.log combined
        </VirtualHost>

    Save and exit the file.

    >**Note**: Update `ServerName`, `ServerAdmin`, `ServerAlias`, and file directory names for your purposes.

* 15.3 Enable the virtual host.

        $ sudo a2ensite Catalog

#### 16. Create the .wsgi file.
---

Sources: [**digitalocean**](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps)

* 16.1 Move to the /var/www/Catalog directory.

        $ cd /var/www/Catalog

* 16.2 Create a file named *catalog.wsgi*

        $ sudo nano catalog.wsgi

* 16.3 Add the following lines of code to the *catalog.wsgi* file:

        #!/usr/bin/python
        import sys
        import logging
        logging.basicConfig(stream=sys.stderr)
        sys.path.insert(0,"/var/www/Catalog/")

        from Catalog import app as application
        application.secret_key = 'Add_your_secret_key'

    Save and exit the file.

* 16.4 Restart Apache.

        $ sudo service apache2 restart

>**Note**: Check for errors `sudo tail -20 /var/log/apache2/error.log`
