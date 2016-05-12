# Common errors

Check [FAQ](http://www.ocsinventory-ng.org/index.php?page=faq) on OCS Inventory web site for updates.

## Troubleshooting the agents execution

### **Windows launcher OcsLogon.exe does not download Agent**

#### **When I launch launcher OcsLogon.exe, agent installation files are not downloaded**

Launcher "OcsLogon.exe" must be renamed with communication server IP address or DNS name
(ex : 192.168.1.12.exe or ocs_com.domain.tld.exe). If this is already done, you may have configured a
proxy in Internet Explorer, and OCS is using this configuration. Try to disable use of proxy by
lauching OcsLogon with "/NP" command line switch. In any case, launch OcsLogon with "/DEBUG" command
line switch and take a look at log file "C:\ocs-ng\ocslogon.log" and "C:\ocs-ng\computer_name.log".

Here is a typical OcsLogon.log content of faulting launcher:

    OCS server port number : Default (80)
    Install folder : C:\Ocs-ng
    OCSserver is set to: a.b.c.d
    Internal Ocslogon version: 4.0.1.4
    Testing: C:\ocs-ng\BIOSINFO.EXE
    Ocs Inventory NG () was not previously installed.
    Start deploying OCS
    http://a.b.c.d/ocsinventory/deploy/ocsagent.exe : HTTP/1.1 500 Internal Server Error
    http://a.b.c.d/ocsinventory/deploy/label : HTTP/1.1 500 Internal Server Error
    End Deploying
    Testing ocsagent.exe version:0000
    Proxy use.
    Launching : C:\ocs-ng\OCSInventory.exe /debug /server:a.b.c.d
    Cmdline option is :\\server_share\a.b.c.d.exe /debug

As you can see, launcher is using proxy settings from IE, and there is "HTTP/1.1 500 Internal Server Error"
error when downloading file "ocsagent.exe". This means that launcher is not able to download agent
installation file. So try disabling use of proxy by adding « /NP » to agent’s command line launch,
and then take a look at § 11.1.4 Agent HTTP errors., and then at § 11.3 Communication server errors.

**`Note`**`: same error for file "label" is not blocking one. This means you aren't using TAG.`

### **Windows agent does not send inventory to server**

**I have set OCS Inventory NG server up as per the guide, but when I launch agent, nothing
appears in Administration console.**

On Windows client computer, launch "INSTALL_FOLDER\ocsinventory.exe /server:communication_server_ip /debug".
A log file "computer_name.log" is created in directory "C:\ocs-ng", which will help you finding problem.
Generally, you will see something like: "...is not a well configured ocs server" and an http error
(see FAQ Windows agent HTTP errors).

Here is a typical "computer_name.log" content of faulting agent:

    OCS INVENTORY ver. 4014 Starting session for Device <COMPUTER_NAME> on Friday, February 24, 2006 15:34:27...
    Command line parameters: </np /debug /server:a.b.c.d>
    WMI Connect: Trying to connect to WMI namespace root\cimv2 on device <Localhost>...OK.
    Registry Connect: Trying to connect to HKEY_LOCAL_MACHINE on device <Localhost>...OK.
    SetupAPI Connect: Trying to connect to SetupAPI on device <Localhost>...OK.
    CHECKINGS: No ocsinventory.dat file found !
    IpHlpAPI GetNetworkAdapters...
    IpHlpAPI GetNetworkAdapters: Calling GetIfTable to determine network adapter properties...OK
    IpHlpAPI GetNetworkAdapters: Calling GetAdapterInfo to determine IP Infos...OK
    IpHlpAPI GetNetworkAdapters: OK (1 objects).
    DID_CHECK: Mac changed new:<00:40:63:D8:BC:61> old:<>, hname changed new:<COMPUTER_NAME> old:<>
    Generating Unique ID for device <COMPUTER_NAME>...OK (COMPUTER_NAME-2006-02-24-15-34-27)
    CHECKINGS: write <COMPUTER_NAME-2006-02-24-15-34-27> and <00:40:63:D8:BC:61> in ocsinventory.dat
    HTTP SERVER: Connection WITHOUT proxy
    HTTP SERVER: Creating CInternetSession to get inventory parameters...OK.
    HTTP SERVER: Connecting to server a.b.c.d 80...OK.
    HTTP SERVER: Sending prolog query...
    HTTP SERVER: The server <a.b.c.d> is not a well configured OCS server
    HTTP ERROR:
    <...>
    Server error!

    The server encountered an internal error and was unable to complete your request.
    Either the server is overloaded or there was an error in a CGI script.
    If you think this is a server error, please contact the <a href="mailto:admin@localhost">webmaster</a>.

    Error 500
    <address>
    a.b.c.d
    24.02.2006 15:40:10
    Apache/2.2.0 (Win32) DAV/2 mod_ssl/2.2.0 OpenSSL/0.9.8a mod_autoindex_color PHP/5.1.1 mod_perl/2.0.2 Perl/v5.8.7
    </address>

    HTTP SERVER: Closing HTTP connection
    WMI Disconnect: Disconnected from WMI namespace.
    SetupAPI Disconnect: Disconnected from SetupAPI.
    Execution duration: 00:00:00.

As you can see, agent is not using IE proxy settings ("HTTP SERVER: Connection WITHOUT proxy "),
and there is error "HTTP SERVER: The server is not a well configured OCS server" followed by "Error 500".
So take a look at § 11.1.4 Agent HTTP errors., and then at § 11.3 Communication server errors.

### **Linux agent does not send inventory to server**

**I have set OCS Inventory NG server up as per the guide, but when i launch Linux agent, nothing appears
in Administration console.**

On Linux client computer, you can use "ocsinv –debug" or "ocsinv –info" to obtain a trace.

Generally, you will see something like: « ...is not a well configured ocs server » and an http error.
So take a look at § 11.1.4 Agent HTTP errors., and then at § 11.3 Communication server errors.

### **Agent HTTP errors**

**I see in agent logs http errors. What do they mean ?**

* **500**: server encountered an internal error. You must take a look at apache log files,
especially file "error.log", generally located under "/var/log/httpd".
* **404**: URL "/ocsinventory" cannot be found on server. You have made a mistake in Apache configuration.
Do you have included in apache configuration content of apache_config file ? Do you have updated this
content to match your need ?
* **301**: This error means that you already have a directory named "/ocsinventory" in your apache server,
and this name conflicts with apache <location> directive of apache_config file. You must change name of
"/ocsinventory" directory.

In all case, you must also take a look at Communication Server log files and check § 11.3
Communication server errors.

## Administration console errors

### **MySQL Max_allowed_packet error**

If you encounter an error message with “max_allowed_packet” MySQL error, you must update your
MySQL configuration to increase the maximum size of packet accepted by MySQL. We recommend setting
the value to 4 MB.

Open the file “my.ini” (usually available in “/etc” directory under Linux,
in “C:\OCSinventoryNG\xampp\mysql\bin” under Windows) and add the line “max_allowed_packet=4M”
in “[mysqld]”, “[mysql.server]” or “[safe_mysqld]” section.

    [mysqld]
    datadir=/var/lib/mysql
    socket=/var/lib/mysql/mysql.sock'
    max_allowed_packet=4M

    [mysql.server]
    user=mysql
    basedir=/var/lib
    max_allowed_packet=4M

    [safe_mysqld]
    err-log=/var/log/mysqld.log
    pid-file=/var/run/mysqld/mysqld.pid
    max_allowed_packet=4M

**Figure 15 : Sample my.cnf MySQL configuration file**

Then, restart MySQL server.

    /etc/rc.d/init.d/mysql restart

### **MySQL Client does not support authentication protocol**

**If you encounter an error message with “Client does not support authentication protocol requested by server;
consider upgrading MySQL client” MySQL error, you must enable support for old password storage method
in your MySQL configuration.**

There are 2 ways to do this.

1. Add directive “old-passwords” to the file “my.cnf” (usually in directory “/etc” under Linux and
in “C:\OCSinventoryNG\xampp\mysql\bin” under Windows), in the section corresponding to your MySQL server.

        [mysqld]
        old-passwords
        port=3306
        socket=mysql

    **Figure 16 : Sample my.cnf MySQL configuration file.**

2. Add switch “--old-password” to the command line launching MySQL server.

Then, restart MySQL server.

    /etc/rc.d/init.d/mysql restart

Next, you may have to update ‘root’ password with the following commands:

* Connect to MySQL database “mysql –u root –p mysql” as root to update his password.
* Then, run the update statement “update user set password=OLD_PASSWORD(‘root_password’) where user=’root’;”
* Once terminated, exit mysql command interpreter by entering “exit” command.


    [root@linux root]# mysql -u root -p mysql
    Enter password:
    Reading table information for completion of table and column names
    You can turn off this feature to get a quicker startup with -A'
    Welcome to the MySQL monitor. Commands end with ; or \g.
    Your MySQL connection id is 19 to server version: 4.1.7-standard
    Type 'help;' or '\h' for help. Type '\c' to clear the buffer.

    mysql> update user set password=OLD_PASSWORD('admin123') where user='root';
    Query OK, 1 row affected (0.00 sec)
    Rows matched: 1 Changed: 1 Warnings: 0

    mysql> exit
    Bye
    [root@linux root]#

**Figure 17 : Sample MySQL root password update**

### **PHP Requested content-length**

**If you see in apache error log** (file error_log or ssl_error.log) **message like following:**

\[Mon Sep 05 18:30:03 2005] \[error] \[client XXX.XXX.XXX.XXX] Requested content-length of 831148
is larger than the configured limit of 524288, referer:
[http://administration_server/ocsreports/?multi=8](http://administration_server/ocsreports/?multi=8)

That’s because Apache directive “LimitRequestBody” is used to limit size of HTTP requests.

To fix this, open Apache configuration file “httpd.conf”, usually in directory “/etc/httpd/conf”
(under some distributions, Apache configuration for PHP may also resides in include directory,
usually “/etc/httpd/conf.d”).

Find the directive “LimitRequestBody” and ensure that the size is at least 4 MB (4194304 bytes)
and not the default 512 KB (524288 bytes).

    #
    # PHP is an HTML-embedded scripting language which attempts to make it
    # easy for developers to write dynamically generated webpages.
    #

    LoadModule php4_module modules/libphp4.so

    #
    # Cause the PHP interpreter handle files with a .php extension.
    #
    <Files *.php>
    SetOutputFilter PHP
    SetInputFilter PHP
    LimitRequestBody 4194304
    </Files>

**Figure 18 : Sample Apache configuration for PHP.**

Update this value if needed and restart Apache daemon.

    /etc/rc.d/init.d/httpd restart

### **Uploads size for package deployment**

All the configuration settings for your installation are contained in the “php.ini” file or Apache
configuration files. Sometimes these setting might be overridden by directives in apache .htaccess
files or even with in the scripts themselves. However you cannot over ride the settings that effect
file uploads with .htaccess directives in this way. So let's just concentrate on the ini file.

You can call the phpinfo() function to find the location of your “php.ini” file, it will also tell you
the current values for the following settings that we need to modify:

* file_uploads
* upload_max_filesize
* max_input_time
* memory_limit
* max_execution_time
* post_max_size

The first one is fairly obvious if you set this off, uploading is disabled for your installation, so
you will not be able to upload software for deployment. We will cover the rest of the configuration
settings in detail below.

**`Note`**`: On some Linux distributions (for example Debian), you do not need to modify php.ini,
but just edit the ocsinventory-reports.conf which is in the conf.d directory
(in Debian /etc/apache2/conf.d/ocsinventory-reports.conf).
All the settings needed regarding to PHP are located in this file.`

Remenber to restart Apache web server for changes to take effect.

#### **upload_max_filesize and post_max_size**

Files to deploy are *POST*ed to the webserver in a format known as 'multipart/form-data'. The post_max_size
sets the upper limit on the amount of data that a script can accept in this manner. Ideally this value
should be larger than the value that you set for upload_max_filesize.

It's important to realize that upload_max_filesize is the sum of the sizes of all the files that you
are uploading. post_max_size is the upload_max_filesize plus the sum of the lengths of all the other
fields in the form plus any mime headers that the encoder might include. Since these fields are
typically small you can often approximate the upload max size to the post max size.

If you want to deploy files up to 200 MB, you must set in “php.ini” file

    upload_max_filesize = 200M
     post_max_size = 201M

#### **memory_limit**

When the PHP engine is handling an incoming POST, it needs to keep some of the incoming data in memory.
This directive has any effect only if you have used the _--enable-memory-limit_ option at configuration
time. Setting too high a value can be very dangerous because if several uploads are being handled
concurrently all available memory will be used up and other unrelated scripts that consume a lot
of memory might effect the whole server as well.

So we recommend using the following value in “php.ini” file:

    memory_limit = 16M

#### **max_execution_time and max_input_time**

These settings define the maximum life time of the script and the time that the script should spend in
accepting input. If several mega bytes of data are being transfered max_input_time should be reasonably high.

So we recommend disabling these limits using the following value in “php.ini” file:

    max_execution_time = -1
    max_input_time = -1

#### **Additonal Comments**

The apache webserver has a _LimitRequestBody_ configuration directive that restricts the size of all
POST data regardless of the web scripting language in use. Some RPM installations sets limit request
body to 512Kb. You will need to change this to a larger value or remove the entry altogether.

If you want to deploy files up to 200 MB, you must comment in apache main configuration file.

    #LimitRequestBody

## Communication server errors

### **I see "Unknown directive PerlRequire...." in Apache log files.**

This means that mod_perl for Apache is not installed, or loaded at startup by Apache. Install mod_perl
and then update apache configuration file "httpd.conf" to load mod_perl.so extension.

You can check which version of mod_perl is installed by launching following command:

* On RPM based Linux: rpm -q mod_perl
* On Debian package based Linux: dpkg –l libapache*-mod-perl*


    [root@linux conf.d]# rpm -q mod_perl

    mod_perl-1.99_16-4.centos4

    [root@linux conf.d]#

If mod_perl is installed, check Apache configuration files to ensure mod_perl is enabled.
You may found something like this:

    #
    # Mod_perl incorporates a Perl interpreter into the Apache web server,
    # so that the Apache web server can directly execute Perl code.
    # Mod_perl links the Perl runtime library into the Apache web server
    # and provides an object-oriented Perl interface for Apache's C
    # language API. </nowiki>The end result is a quicker CGI script turnaround''
    # process, since no external Perl interpreter has to be started.
    #
    LoadModule perl_module modules/mod_perl.so

If needed, uncomment line (remove # at line begin) and restart Apache.

Please, refer to Apache manual for more details.

### **I see "Can't locate [Perl module name], cannot resolve handler Ocsinventory.pm..." in Apache log files.**

Perl module [Perl module name] is not installed on your server. Please, refer to OCS Inventory NG
guide or to Perl manual to install missing module.

If this error concerns Perl module "compat.pm" like below

    Thu Mar 02 11:43:56 2006] [error] [client client_ip] failed to resolve handler
    Ocsinventory': Can't locate Apache/compat.pm in @INC (@INC contains:...)

You probably have installed Communication server for use with Apache mod_perl version 1.999_21 or previous. Open OCS Inventory NG apache configuration file « ocsinventory.conf » and set variable « OCS_PERL_VERSION » to 2 enable use of mod_perl version 1.999_22 or higher.

    # Which version of mod_perl we are using
    # For mod_perl <= 1.999_21, replace VERSION_MP by 1
    # For mod_perl > 1.999_21, replace VERSION_MP by 2

    PerlSetEnv OCS_MODPERL_VERSION 2

Then restart Apache web server.

**If this error concerns Perl module "Apache2/connection.pm"** like below

[Sun Mar 05 12:46:09 2006] [error] [client client_ip] failed to resolve handle
Ocsinventory: Can't locate Apache2/Connection.pm in @INC (@INC contains:...)

You probably have installed Communication server for use with Apache mod_perl 1.999_22 and newer. Open OCS Inventory NG apache configuration file « ocsinventory.conf » and set variable « OCS_PERL_VERSION » to 1 to enable use of mod_perl version 1.999_21 or previous.

# Which version of mod_perl we are using
# For mod_perl <= 1.999_21, replace VERSION_MP by 1
# For mod_perl > 1.999_21, replace VERSION_MP by 2

PerlSetEnv OCS_MODPERL_VERSION 1

Then restart Apache web server.

### **I see "Cannot open log file: ..." in Apache logs. Communication server is not able to write his logs.**

* Does directory « /var/log/ocsinventory-NG » (or any other path you’ve fill in at setup) exists ?
* Can Apache web server write in this directory ?

## Files and directories permissions under Linux

We assume that Apache web server is running under account “apache” and group “apache”, and that
you’ve set OCS Inventory NG management server up as described previously:

* Administration console files are in directory “/var/www/html/ocsreports”
* Deployment server files are in directory “/var/www/html/download”
* Log for Communication server are in directory “/var/log/ocsinventory-NG”

Directory | File | Owner | Group | Permissions
------|------|------|------|------
/var/www/html/download |   | root | apache | -rwxrwxr-x
 | All directories | apache | apache | -rwxrwxr-x
 | All files | apache | apache | -rw-rw-r--
/var/www/html/ocsreports |   | root | apache | -rwxrwxr-x
 | dbconfig.inc.php | apache | apache | -rw-rw-r--
 | All others | root | root | -rw-r--r--
/var/www/html/ocsreports/ipd |   | root | apache | -rwxrwxr-x
 | All files | apache | apache | -rw-rw-r--
/var/www/html/ocsreports/css |   | root | root | -rwxr-xr-x
 | All files | root | root | -rw-r--r--
/var/www/html/ocsreports/files |   | root | root | -rw-r--r--
 | All files | root | root | -rw-r--r--
/var/www/html/ocsreports/image |   | root | root | -rwxr-xr-x
 | All files | root | root | -rw-r--r--
/var/www/html/ocsreports/js |   | root | root | -rwxr-xr-x
 | All files | root | root | -rw-r--r--
/var/www/html/ocsreports/languages |   | root | root | -rwxr-xr-x
 | All files | root | root | -rw-r--r--
/var/log/ocsinventory-NG |   | root | apache | -rwxrwx-r-x
 | All files | apache | apache | -rw-rw-r--

## Getting help in forums

If you are unable to diagnose yourself the problem, you can get help using OCS Inventory NG web site
forums ([http://forums.ocsinventory-ng.org](http://forums.ocsinventory-ng.org)).

If you do so, please provide us:

* Server operating system
* OCS Inventory NG server version and patch level
* Agent’s operating system
* OCS Inventory NG agent version
* Agent execution logs
    * Run “AGENT_INSTALL_FOLDER\ocsinventory.exe /NP /DEBUG /SERVER:you_server_address” under Windows.
    Log file is created on folder “AGENT_INSTALL_FOLDER” under name “your_computer_name.log”.
    * Run “ocsinv –debug > ocsinv.log” under Linux. Log file is “ocsinv.log”.
* Apache server error.log file, located for Windows under “SERVER_INSTALL_FOLDER\xampp\apache\logs\error.log”
and for Linux under “/var/log/httpd/*error.log”.
* OCS Inventory NG Server log file, located for Windows under
“SERVER_INSTALL_FOLDER\xampp\apache\logs\ocsinventory-NG.log” and for Linux under
“/var/log/ocsinventory-NG/ocsinventory-NG.log” under Linux.

Thanks in advance.
