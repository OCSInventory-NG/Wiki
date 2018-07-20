# Management server tuning

OCS Inventory NG management server needs some tuning to support the load of a large number of inventoried
computers. Performances are just limited by the hardware configuration (especially the amount of RAM,
processor is not very loaded) of computer hosting the 3 main components:

* MySQL database server
* Communication server
* Administration server
* Deployment server

For example, our production server manages more than 70 000 clients. For this, we have 3 servers running
Linux Debian Sarge, one for the database server and the Communication server, another one for
Administration console and a replica of database server (we choose to replicate database on
Administration server to avoid Administration console SQL queries using CPU and MySQL connexions of
database used by Communication server) and the last one for deployment server. Hardware configuration
for servers is the following:

* 1 Xeon E3 2,8 GHz.
* 4 GB RAM

Because of the amount of available RAM, we have to limit the number of simultaneous HTTP connection
to Communication and Administration server to 500.

**You must keep an eye on Apache web server logs for Communication server to detect any problems.
Also, check Communication server log file in directory “/var/log/ocsinventory-NG”.**

If you want to upgrade the number of simultaneous connections, you must update the “MaxClients”
directive in Apache configuration file, usually “/etc/httpd/conf/httpd.conf”.

**Refer to Apache tuning guidelines on Apavhe web site ([http://httpd.apache.org](http://httpd.apache.org/))
for more information.**

Also, MySQL database server is limited by default to 100 simultaneous connections. So, if the
Communication server handles more than 100 simultaneous requests for inventory, it will not be able
to answer all. You can upgrade this value by updating the “max_connections” MySQL variable for mysqld daemon.

Here is sample lso, MySQL database server is limited by default to 100 simultaneous connections. So, if the
Communication server handles more than 100 simultaneous requests for inventory, it will not be able
to answer all. You can upgrade this value by updating the “max_connections” MySQL variable for mysqld daemon.

Here is sample recommendations found on MySQL web site, using server with different amount of physical memory.

**For further informations on MySQL tuning, see their guidelines on MySQL / MariaDB web site
([http://dev.mysql.com/tech-resources/articles/](http://dev.mysql.com/tech-resources/articles/))
for more information.**