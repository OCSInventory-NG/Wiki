# Deploying database server

OCS Inventory server need a database to store information from inventory

## Install database server requirements

**Database** server need to match database version indicated in the [`prerequisites page`](../01.Prerequisites/Libraries-version.md)

**`Note : On Fedora/Redhat/Centos 7 mariadb is avalaible on`[`EPEL`](https://fedoraproject.org/wiki/EPEL/FAQ#howtouse)`. you need to install this repo for install database server.`**

**On Fedora/Redhat/Centos 7** you can use "yum" to install mariadb

    yum install mariadb-server

**On Debian 9 Stretch** you can use "apt" to install mariadb

    apt install mariadb-server mariadb-common mariadb-client

## Launch MariaDB

First step we need to launch MariaDB. **On Fedora/Redhat/Centos 7** you need to enable and start mariadb first

    systemctl enable mariadb
    systemctl start mariadb

Launch MariaDB

    mysql -u root

If you doesn't have set password for root you can set it up by the following command:

    SET PASSWORD FOR 'root'@'localhost' = PASSWORD('yourpassword');

## Configure database server in only one server

This part is only if you want to install database server, communication server and administration console in only one server.

First step create database:

    CREATE DATABASE ocsweb;

OCS need a user to use the database "ocsweb":

    CREATE USER 'ocs'@'localhost' IDENTIFIED BY 'ocs';

This user need all privileges on the database "ocsweb"

    GRANT ALL PRIVILEGES ON ocsweb.* TO 'ocs'@'localhost' WITH GRANT OPTION;

Don't forget to apply parameters:

    FLUSH PRIVILEGES;

**`Note: This user will be used by Administration server and Communication server to connect to the database.
If you do not wish to use default MySQL user ocs with ocs password, you must update in the file
/etc/apache2/conf-avaible/z-ocsinventory-server.conf.
Don’t forget to also enable your conf and restart apache.
Refer to `[`Secure your OCS Inventory NG Server`](../09.Extras/Secure-your-OCS-Inventory-NG-Server.md)`
for all information about modifications of configuration files.`**


## Configuring database server for separated OCS Server

This part is only if you want to install database server, communication server and administration console in separated server

First step create database:

    CREATE DATABASE ocsweb;

For separated server you need to have two user if you use different server for database, communication server and administration console

    CREATE USER 'ocs'@'CommunicationServerIP' IDENTIFIED BY 'ocs';
    CREATE USER 'ocs'@'AdministrationConsoleIP' IDENTIFIED BY 'ocs';

**`Note : If you want to deploy OCS in only two server you only need to create one user with the communication server / admin console IP if you install a database server and another server for communication server / administration console`**

Then user need all privileges on the database "ocsweb"

    GRANT ALL PRIVILEGES ON ocsweb.* TO 'ocs'@'CommunicationServerIP' WITH GRANT OPTION;
    GRANT ALL PRIVILEGES ON ocsweb.* TO 'ocs'@'AdministrationConsoleIP' WITH GRANT OPTION;

Don't forget to apply parameters:

    FLUSH PRIVILEGES;

**On your Communication server/Administration console server** : You need to change the host of the database in the file /etc/apache2/conf-avaible/z-ocsinventory-server.conf

    PerlSetEnv OCS_DB_HOST YourDatabaseServerIP

Don't forget to activate your conf with the following command

    a2enmod z-ocsinventory-server.conf

Restart your apache service to activate the conf

    systemctl restart apache2

**On your Communication server/Administration console server** : You need to change the host of the database in the file /usr/share/ocsinventory-reports/ocsreports/dbconfig.inc.php

    $_SESSION["SERVEUR_SQL"]="YourDatabaseServerIP";

**`Note: This user will be used by Administration server and Communication server to connect to the database.
If you do not wish to use default MySQL user ocs with ocs password, you must update in the file
/etc/apache2/conf-avaible/z-ocsinventory-server.conf.
Don’t forget to also enable your conf and restart apache.
Refer to `[`Secure your OCS Inventory NG Server`](../09.Extras/Secure-your-OCS-Inventory-NG-Server.md)`
for all information about modifications of configuration files.`**
