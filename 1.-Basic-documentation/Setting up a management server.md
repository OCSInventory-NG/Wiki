# Setting up management server

Management server is made up of 4 main components:
1. **Database server**, which stores inventory information
2. **Communication server**, which handles HTTP communications between database server and agents.
3. **Administration console**, which allows administrators to query the database server using their favorite browser.
4. **Deployment server**, which stores all package deployment configuration (requires HTTPS!)

These 4 components can be hosted on a single computer or on different computers to allow load balancing. Above 10000 inventoried computers, we recommend using at least 2 physical servers, one hosting database server + Communication server and the other one hosting a database replica + Administration server + Deployement server.

![OCS Inventory Structure Diagram](img/Architecture_OCS.jpg)

**Figure 1 : OCS Inventory NG communication architecture.**

**`Note`**`: If you want to use multiple computers to host OCS inventory NG management server,
we recommend that you set it up on Linux servers. OCS Inventory NG server for Windows comes as an integrated package
including all required components (apache, perl, php, mod_perl, mysql…).`

**Database** server currently can only be MySQL 4.1 or higher with InnoDB engine active.
**Communication server** needs Apache Web Server 1.3.X/2.X and is written in PERL as an Apache module. Why? Because PERL scripts are compiled when Apache starts, and not at each request. This is better performance-wise. Communication server may require some additional PERL modules, according to your distribution.
**Deployment server** needs any Web Server with SSL enabled.
**Administration console** is written in PHP 4.1 (or higher) and runs under Apache Web Server 1.3.X/2.X. Administration console requires ZIP and GD support enabled in PHP in order to use package deployment.

# Under Linux Operating System

We assume that you have:

1. MySQL database server running somewhere and listening on default port 3306 with TCP/IP communication enabled.
2. Apache Web server installed and running for Communication server and Administration server.
3. PHP and Perl installed and usable by Apache Web server for the Administration console.
4. Perl and mod_perl installed and usable by Apache Web server for the Communication server.

## Requirements


* Apache version 1.3.33 or higher / Apache version 2.0.46 or higher.
    * Mod_perl version 1.29 or higher.
    * Mod_php version 4.3.2 or higher.
* PHP 4.3.2 or higher, with ZIP and GD support enabled.
* PERL 5.6 or higher.
    * Perl module XML::Simple version 2.12 or higher.
    * Perl module Compress::Zlib version 1.33 or higher.
    * Perl module DBI version 1.40 or higher.
    * Perl module DBD::Mysql version 2.9004 or higher.
    * Perl module Apache::DBI version 0.93 or higher.
    * Perl module Net::IP version 1.21 or higher.
    * Perl module SOAP::Lite version 0.66 or higher (optional)
* MySQL version 4.1.0 or higher with InnoDB engine active.
* Make utility such as GNU make.

      Note: OCS Inventory NG Server Setup will check for all these components and will exit if any are missing.

## Installing Communication server required PERL modules.

The Web communication server requires Apache web server and Perl 5 scripting language and some additional modules for Perl 5
(see [Requirements](http://wiki.ocsinventory-ng.org/index.php/Documentation:Server#Requirements.)).
It acts as an Apache module which handles HTTP OCS Inventory agents' requests to a virtual directory /ocsinventory.


    Warning: You must have root privileges to set required perl modules up.
    It is better for system integrity to use your distribution's precompiled packages when they are available. Some of these packages are only avalaible in [EPEL](https://fedoraproject.org/wiki/EPEL/FAQ#howtouse).
