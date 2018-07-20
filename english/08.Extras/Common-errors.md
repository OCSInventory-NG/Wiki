# Common errors

## Troubleshooting the agents execution

### **Agent HTTP errors**

**I see in agent logs http errors. What do they mean ?**

* **500**: server encountered an internal error. You must take a look at apache log files,
especially file "error.log", generally located under "/var/log/httpd".
* **404**: URL "/ocsinventory" cannot be found on server. You have made a mistake in Apache configuration.
Do you have included in apache configuration content of apache_config file ? Do you have updated this
content to match your need ?
* **301**: This error means that you already have a directory named "/ocsinventory" in your apache server,
and this name conflicts with apache \<location> directive of apache_config file. You must change name of
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


Then, restart MySQL server.

    systemctl restart mysql

### **PHP Requested content-length**

**If you see in apache error log** (file error_log or ssl_error.log) **message like following:**

\[Mon Sep 05 18:30:03 2005] \[error] \[client XXX.XXX.XXX.XXX] Requested content-length of 831148
is larger than the configured limit of 524288, referer:
[http://administration_server/ocsreports/?multi=8](http://administration_server/ocsreports/?multi=8)

That’s because Apache directive “LimitRequestBody” is used to limit size of HTTP requests.

To fix this, open Apache configuration file “httpd.conf”, usually in directory “/etc/httpd/conf”
(under some distributions, Apache configuration for PHP may also resides in include directory,
usually “/etc/httpd/conf.d”).

`Note : for debian, default directory is /etc/apache2/`

Find the directive “LimitRequestBody” and ensure that the size is at least 4 MB (4194304 bytes).

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

**`Note: On some Linux distributions (for example Debian), you do not need to modify php.ini,
but just edit the ocsinventory-reports.conf which is in the conf.d directory
(in Debian /etc/apache2/conf-available/ocsinventory-reports.conf).
All the settings needed regarding to PHP are located in this file.`**

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

## Getting help in ASK

If you are unable to diagnose yourself the problem, you can get help using OCS Inventory NG web site

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
