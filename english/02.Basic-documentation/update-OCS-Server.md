## Update an existing OCS Server installation
The procces is mainly backup your configuration download the new version of OCS-Server use the setup.sh and restore the backed up configuration.


### Backup existing configuration
You need to backup the following configuration files:
1. Apache files:    
* ~/apache2/conf-available/z-ocsinventory-serve.conf
*  ~/apache2/conf-available/ocsinventory-resports.conf
2. OCS configuration:
* /usr/share/ocsinventory-reports/ocsreports/dbconfig.inc.php

If your scared about losing data then better backup your database, but theoretical this is not necessary.
```
mysqldump -u ocs -p --all-databases > ocsdbbackup.sql
```

### update the existing installation
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


### restore backed up configuration and start
restore the backed up files from above 

restart apache webserver
```
service apache2 restart
```

sometimes is it required to update the database via the webconsole 


## That´s all!
