# OCS Inventory NG 2.x Documentation

To make it easier to read, the OCS Inventory NG documentation has been divided into 11 sections.

## Prerequisites

* [Libraries and Modules versions](01.Prerequisites/Libraries-version.md)

## Newbie documentation

* [OCS Inventory NG - Basics](02.Newbie-documentation/OCS-Inventory-NG-Basics.md)

## Basic documentation

* [Setting up a OCS Inventory server](03.Basic-documentation/Setting-up-a-OCS-Inventory-Server.md)
* [Setting up the UNIX agent on client computers](03.Basic-documentation/Setting-up-the-UNIX-agent-on-client-computers.md)
* [Setting up the Windows Agent 2.X on client computers](03.Basic-documentation/Setting-up-the-Windows-Agent-2.x-on-client-computers.md)
* [Setting up the MacOSX agent on client computers](03.Basic-documentation/Setting-up-the-MacOSX-agent-on-client-computers.md)
* [Setting up the Android Agent](03.Basic-documentation/Setting-up-the-Android-Agent.md)
* [Administration of OCS Inventory NG](03.Basic-documentation/Administration-of-OCS-Inventory-NG.md)
* [Deploying database server](03.Basic-documentation/Deploying-database-server.md)
* [Updating the server](03.Basic-documentation/Updating-the-server.md)

## Management console and its advanced features

* [Querying inventory results](04.Management-console-and-its-advanced-features/Querying-inventory-results.md)
* [Using computers groups](04.Management-console-and-its-advanced-features/Using-computers-groups.md)
* [Using assets categorization](04.Management-console-and-its-advanced-features/Using-Assets-Categorization.md)
* [Using softwares categorization](04.Management-console-and-its-advanced-features/Using-Software-Categorization.md)
* [Managing users profiles of the web interface](04.Management-console-and-its-advanced-features/Managing-users-profiles-of-the-web-interface.md)
* [Managing administrative data](04.Management-console-and-its-advanced-features/Managing-administrative-data.md)
* [Managing CVE-search reporting](04.Management-console-and-its-advanced-features/CVE-Search-management.md)
* [Synchronization between OCS and LDAP](04.Management-console-and-its-advanced-features/Synchronization-between-OCS-and-LDAP.md)
* [Export a computer](04.Management-console-and-its-advanced-features/Export-a-computer.md)
* [Configure mail notification](04.Management-console-and-its-advanced-features/Configure-mail-notification.md)
* [Configure OCS news](04.Management-console-and-its-advanced-features/Configure-OCS-news.md)

## Deployment

* [Deploying packages or executing commands on client hosts](05.Deployment/Deploying-packages-or-executing-commands-on-client-hosts.md)

## Network Discovery with OCS Inventory NG

* [Using IP discovery feature](06.Network-Discovery-with-OCS-Inventory-NG/Using-IP-discovery-feature.md)
* [Using SNMP scan feature](06.Network-Discovery-with-OCS-Inventory-NG/Using-SNMP-scan-feature.md)

## OCS Tools

* [OCS Inventory NG Agent Deployement Tool](07.OCS-Tools/OCS-Inventory-NG-Agent-Deployement-Tool.md)
* [OCS Packager for Windows](07.OCS-Tools/OCS-Windows-Packager.md)
* [OCS Packager for MacOSX](07.OCS-Tools/OCS-MacOSX-Packager.md)
* [OCS Packager for Unix](07.OCS-Tools/OCS-Unix-Packager.md)
* [Ansible role for Unix Packager](07.OCS-Tools/OCS-Ansible-Role-for-Unix-Packager.md)

## Multi-site network architecture

* [Using local proxy cache to deploy](08.Multi-site-network-architecture/Using-local-proxy-cache-to-deploy.md)
* [Synchronisation between OCS server master/slaves](08.Multi-site-network-architecture/Synchronisation-between-OCS-server-master-slaves.md)

## Extras

* [Secure your OCS Inventory NG Server](09.Extras/Secure-your-OCS-Inventory-NG-Server.md)
* [Management server tuning](09.Extras/Management-server-tuning.md)
* [Enable Powershell Support on Windows Agent](09.Extras/Enable-Powershell-Support-on-Windows-Agent.md)
* [Backup/restore of OCS Inventory NG database ](09.Extras/Backup-restore-of-OCS-Inventory-NG-database.md)
* [Common errors](09.Extras/Common-errors.md)

## Plugin Engine

* [Plugin installer](10.Plugin-engine/Using-plugins-installer.md)

## REST API
* [Introduction](11.Rest-API/Introduction.md)
* [Configuration](11.Rest-API/Configuration.md)
* [Autentification](11.Rest-API/Authentification.md)
* [GET Routes](11.Rest-API/GET-Routes.md)
* [POST Routes](11.Rest-API/POST-Routes.md)
* [DELETE Routes](11.Rest-API/DELETE-Routes.md)
* [PUT Routes](11.Rest-API/PUT-Routes.md)

## Developers

* [XML-Format](12.Developers/XML-Format.md)
* [HOWTO create theme](12.Developers/HOWTO-create-theme.md)
* [HOWTO package OCS Unix server releases](12.Developers/HOWTO-package-OCS-Unix-server-releases.md)
* [HOWTO package OCS Unix agent releases](12.Developers/HOWTO-package-OCS-Unix-agent-releases.md)
* [HOWTO package OCS MacOSX agent releases](12.Developers/HOWTO-package-OCS-MacOSX-agent-releases.md)
* [HOWTO compile OCS Windows Agent](12.Developers/HOWTO-compile-OCS-Windows-Agent.md)


# FAQ


* Can I upgrade my server from version X to the last stable version ?

    **Solution**: Since we made a lot of changes from 2.0 to 2.5 in the database schema. We recommend you to not upgrade your OCS from 1.0 to 2.5 and newer directly. You may need to upgrade to 2.0 before.

* After installing the server, I go to the graphical admin console (GUI) and it asks me to install the database ?

    **Solution**: The database is installed on the first access to the graphical admin console (GUI).

* What URL must I put in the agent to contact the server ?

    **Solution**: The URL must be in this form http://dns_or_ip/ocsinventory. It is recommended that you use DNS instead of IP.

* ocsinventory directory does not exist on my server

    **Solution**: ocsinventory is a virtual directory call by mod_perl in apache. ocsinventory directory must not exist.

* Error 500:

    **Solution**: OCS engine can't comunicate with mysql server. Probably due to a wrong mysql account. You have to check z-ocsinventory-server.conf exactly theses few lines

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

* Error 417:

    **Solution** : insert ignore_expect_100 on into your squid.conf file and then run squid -k reconfigure.


# Contribution

If you think something is missing feel free to open an issue or fork it and document it by yourself.
Same for mistakes in the documentation.

The Github repository below is directly linked to our documentation.
See [our GitHub repository](https://github.com/OCSInventory-NG/Wiki)