# Newbie documentation - What you should know
## OCS Inventory NG in 5 lines, what is it ?

OCS Inventory NG or **Open Computer and Software Inventory Next Generation** is free software that enables
users to automatically inventory their IT assets. OCS-NG collects information about the hardware and software
of networked machines running the OCS client program ("OCS Inventory Agent"). OCS can be used to visualize
the inventory through a web interface. Furthermore, OCS allows the possibility of deploying applications
on the computers according to search criteria. Agent-side IpDiscover and SNMP scan make it possible to identify
the entire network of computers and devices.

## Operating principle

The OCS server receives inventories sent by agents in XML format and stores the data in a MySQL database.
The agents contact the server while the server only listens during this process.
Exchanges between agents and server are realised via HTTP and/or HTTPS requests. Software deployments and SNMP scans are HTTPS only.

Schema of data transmission:

`Raw `**`data`**` -> `**`XML`**` formatting -> sending in HTTP or HTTPS in `**`/ocsinventory`**` -> handling with `**`mod_perl`**` on the fly -> DB `**`mysql`**

Management server is made up of 4 main components:

1. **Database server**, which stores inventory information (MySQL)
2. **Communication server**, which handles HTTP/HTTPS communication between database server and agents
(Apache, perl and mod_perl)
3. **Administration console**, which allows administrators to query the database server using their
favorite browser (Apache, PHP)
4. **Deployment server**, which stores all package deployment configuration (Apache, SSL)

![OCS Inventory Structure Diagram](../../img/server/schema/architecture_ocs.png)

## Deployment tools of the solution

### **Simplified installation of server**
* .deb package with automatic install script for Debian 9 Stretch
* .rpm package with automatic install script for Fedora/Redhat/Centos 7
* [setup.sh](https://github.com/OCSInventory-NG/OCSInventory-Server/blob/master/setup.sh) script included in the package (either download it from the official website or clone the [GitHub repository](https://github.com/OCSInventory-NG/OCSInventory-Server))

### **Deployment tools of agents by the network**
* [OCS Deployment Tool](../07.OCS-Tools/OCS-Inventory-NG-Agent-Deployement-Tool.md) based on psexec
* [OCSPackager](../07.OCS-Tools/OCS-Windows-Packager.md) and
[OCSLogon](../03.Basic-documentation/Setting-up-the-Windows-Agent-2.x-on-client-computers.md#deploying-agent-using-launcher-ocslogonexe-through-login-script-or-active-directory-gpo)
based on GPO and logon scripts

## Interfacing with many softwares

### **Classics**
* GLPI http://glpi-project.org/
* LDAP https://ldap.com/
* ITOP https://www.combodo.com/itop-193
* OTRS https://otrs.com/

### **Others**
OCS provides a **REST API**, which allows it to interface with many applications, such as Nagios for example.

## Major technical information

### **Windows agent**
Agent configuration directory:
* Windows 2000, XP and Server 2003
  * `C:\Documents and Settings\All Users\Application Data\OCS Inventory NG\Agent`
* Windows Vista, 7, 8, 8.1, 10, Server 2008, Server 2008 R2, etc.
* `C:\ProgramData\OCS Inventory NG\Agent`
Agent configuration file is **ocsinventory.ini**

### **Server**
Do not mix up the directories **/ocsinventory** and **/ocsreports**.
* `ocsreports` : Directory containing all the PHP files that providing the **web administration console**.
* `ocsinventory` : **Virtual** directory used by `mod_perl` to handle XML inventories sent by agents
and to store data in database.

### **Debugging**
* Using agents logs:
    * Windows : Use **Debug** parameter in **ocsinventory.ini** file to have more verbose logs (`Debug=2`).
    This config file is located in the same folder as the agent configuration (see above).
If OCS is installed as a service you have to stop it first. Then set `Debug=2` to obtain the higher
 log level and save the file. Finally restart OCS service and send a new inventory.
    * Unix/Linux : Use `--debug` and `--logfile` parameters to obtain a detailed log.
Launch inventory with these options : `ocsinventory-agent --debug --logfile=/mon/path/log.txt`
* Using server logs (to find them you can use the commands `locate` or `find`):
    * Apache logs : **access.log** and **error.log** (normally located in `/var/log/apache2`)
    * OCS log : **activity.log** (normally located in `/var/log/ocsinventory-server`)
To use this functionality, you have to activate log function from administration console (`LOGLEVEL`),
and modify the server configuration file **z-ocsinventory-server.conf** to set
`OCS_OPT_DBI_PRINT_ERROR` option to **1**. Remember to **restart Apache** to reflect this change.
