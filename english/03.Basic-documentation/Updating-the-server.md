## Update an existing OCS Server installation
The process is mainly backup your configuration download the new version of OCS-Server use the setup.sh and restore the backed up configuration.

### Delete existing plugins
If you have plugins installed delete this plugins prior the update otherwise you might have problems with or after re-install them.
Go to the plugin manager and delete them you may also need delete the configuration in:    
_/etc/ocsinventory-server/plugins_

### Backup existing configuration
Before you start it´s better to stop the web server that nobody could do changes after you backed up the files.

```
service apache2 stop
```
You need to backup the following configuration files:
1. Apache files:    
* ~/apache2/conf-available/z-ocsinventory-server.conf
* ~/apache2/conf-available/zz-ocsinventory-restapi.conf
* ~/apache2/conf-available/ocsinventory-reports.conf
2. OCS configuration:
* /usr/share/ocsinventory-reports/ocsreports/dbconfig.inc.php

it´s recommended to create a folder where you store all backed up files
```
mkdir /home/backup_ocs
```
then copy the files to the backup folder
```
cp /etc/apache2/conf-available/z-ocsinventory-server.conf
/etc/apache2/conf-available/zz-ocsinventory-restapi.conf /etc/apache2/conf-available/ocsinventory-reports.conf /home/backup_ocs/ && cp /usr/share/ocsinventory-reports/ocsreports/dbconfig.inc.php /home/backup_ocs/
```
If your scared about losing data then better backup your database, but theoretical this is not necessary.
```
mysqldump -u ocs -p --all-databases > /home/backup_ocs/ocsdbbackup.sql
```

### Update the existing installation
Download the last release of OCS from the [Website](https://www.ocsinventory-ng.org/en/download-en/) or here from github.    
Unpack it
```
tar –xvzf OCSNG_UNIX_SERVER-2.x.x.tar.gz
cd OCSNG_UNIX_SERVER-2.x.x
```
Run “setup.sh” installer. During the installer, default choice is presented between []. For example, [y]/n means that “y” (yes) is the default choice, and “n” (no) is the other choice.
```
sh setup.sh
```
*Note: Installer writes a log file “ocs_server_setup.log” in the same directory. If you encounter any error, please refer to this log for detailed error message.*


### Restore backed up configuration and start
restore the backed up files from above 
```
cd /home/backup_ocs
cp ocsinventory-reports.conf z-ocsinventory-server.conf zz-ocsinventory-restapi.conf /etc/apache2/conf-available/ && cp dbconfig.inc.php /usr/share/ocsinventory-reports/ocsreports/
```
don´t forget to delete the install.php in the install dir
```
rm /usr/share/ocsinventory-reports/ocsreports/install.php
```
start apache webserver
```
service apache2 start
```

Sometimes it's required to update the database via the webconsole, simply click update on the webconsole.

