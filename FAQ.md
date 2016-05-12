* Ipdiscover works fine but I have no SNMP data up in my OCS Inventory NG server ?

**Solution**: SNMP is not currently implemented in Windows agent. It will work in future version. You can use the unix agent instead to retrieve SNMP data.

* Can I upgrade my server from version X to the last stable version ?

**Solution**: Yes of course! All old versions can be upgrade to the latest version without problem.

* After installing the server, I go to the graphical admin console (GUI) and it asks me to install the database ?

**Solution**: The database is installed on the first access to the graphical admin console (GUI).

* What URL must I put in the agent to contact the server ?

**Solution**: The URL must be in this form http://dns_or_ip/ocsinventory. It is recommended that you use DNS instead of IP.

* ocsinventory directory does not exist on my server

**Solution**: ocsinventory is a virtual directory call by mod_perl in apache. ocsinventory directory must not exist.

* Fatal error: main() [<a href='function.main'>function.main</a>]: The script tried to execute a method or access a property of an incomplete object. Please ensure that the class definition "language" of the object you are trying to operate on was loaded _before_ unserialize() gets called or provide a __autoload() function to load the class definition in /usr/share/ocsinventory-reports/ocsreports/install.php on line 29

**Solution**: Edit the php.ini file and change the value of session_auto_start from 1 to 0. Save and restart apache.

* Error 500 :

**Solution**: OCS engine can't comunicate with mysql server. Probably due to a wrong mysql account. You have to check z-ocsinventory-server.conf (ocsinventory-server.conf on Windows Server), exactly theses few lines

    # Master Database settings
      # Replace localhost by hostname or ip of MySQL server for WRITE
      PerlSetEnv OCS_DB_HOST localhost
      # Replace 3306 by port where running MySQL server, generally 3306
      PerlSetEnv OCS_DB_PORT 3306
      # Name of database
      PerlSetEnv OCS_DB_NAME ocsweb
      PerlSetEnv OCS_DB_LOCAL ocsweb
      # User allowed to connect to database
      PerlSetEnv OCS_DB_USER ocs
      # Password for user
      PerlSetVar OCS_DB_PWD ocs

Modify **OCS_DB_USER** and **OCS_DB_PWD** with your own account, restart apache, and finaly launch an inventory.

* Error 417 :

**Solution** : insert ignore_expect_100 on into your squid.conf file and then run squid -k reconfigure.