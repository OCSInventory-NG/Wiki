# Secure your OCS Inventory NG Server

We recommend you to secure you OCS Inventory NG Server.
Since OCS Inventory NG 2.0, warning message prevents you about a security risk in management console.

![Error message](../../img/server/reports/secure_ocs_1.png)

## Remove installation file

You have to remove install.php in ocsreports directory. Generally under linux in

    /usr/share/ocsinventory-reports/ocsreports/

## Secure your management console

By default, an account is created by installation script : admin / admin You have to create your own
account with Super Administrator profile, and after that remove default account. An other solution is
to change the admin account password.

## Secure mysql access

By default, installation script create a mysql user **ocs** with password **ocs**. We recommend you to change
minimum the password, but better thing is to create a new mysql user.

### **Create a new mysql user**

Connect to your mysql server

    mysql -u root -p

Create a new user: **user** with password:**password** with all privileges for ocsweb database.

    GRANT ALL PRIVILEGES ON ocsweb.* TO 'user'@'localhost' IDENTIFIED BY 'password' WITH GRANT OPTION;

![Error message](../../img/server/linux/secure_ocs_database.png)

### **Modify configuration files**

#### **dbconfig.inc.php file**

Under Linux, generally in

    /usr/share/ocsinventory-reports/ocsreports/
<br/>

    define("DB_NAME", "ocsweb");
    define("SERVER_READ", "localhost");
    define("SERVER_WRITE", "localhost");
    define("COMPTE_BASE", "ocs");
    define("PSWD_BASE", "ocs");

#### **z-ocsinventory-server.conf file**

Under Linux, generally in

    /etc/apache2/conf-available/
<br/>

    # User allowed to connect to database
    PerlSetEnv OCS_DB_USER user
    # Password for user
    PerlSetVar OCS_DB_PWD password

**`Warning: Don't forget to restart apache. Else, OCS Inventory NG server will return an ERROR 500 to agents
which contact it.`**

# Desactivate warning message in management console

**`Warning: We recommend you to secure your server following preceding paragraphs, but if you want,
you can desactivate warning message in GUI`**

You have to connect to management console, click on **User** icon, then on **Administer profiles** tab,
and choose the profile to modify.

Click on **Rights to manage** tab, an set **See warning messages of the GUI** to **NO**.

Finally, register modification.

![GUI message](../../img/server/reports/secure_ocs_3.png)

For more informations about users and profile, click [here](../04.Management-console-and-its-advanced-features/Managing-users-profiles-of-the-web-interface.md).

# Apache and PHP informations leakage

By default, the apache2 and php version can be recovered in the HTTP header.

### Apache2 :
You can hide the apache version from the header by modify the `/etc/apache2/conf-enabled/security.conf` on Debian/Ubuntu or `/etc/httpd/conf/httpd.conf` on RHEL/CentOS : 
```
ServerTokens Prod
ServerSignature Off
```

### PHP : 
To hide the PHP version, you need to modify the php.ini. (`/etc/php/[php_version]/apache2/php.ini on Debian/Ubuntu` or `/etc/php.ini` on RHEL/CentOS).
```
expose_php = Off
```

You need to restart apache2 after these to apply modifications.