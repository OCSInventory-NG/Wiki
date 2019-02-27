# Configuration

Our REST API need to be configured during the initial OCS Inventory setup AND afterwards depending on the database configuration.

## Configuration During OCS Setup

During the setup (2.4 and newer). OCS Inventory will ask you if you want to install rest API on your server. Awnser yes or let the default choice.

```
Do you wish to setup Rest API server on this computer ([y]/n)?
```

A rapid dependencies check will be performed in order to prevent problem when using the API.

```
+----------------------------------------------------------+
| Checking for REST API Dependencies ...              	   |
+----------------------------------------------------------+

Found that PERL module Mojolicious::Lite is available.
Found that PERL module Switch is available.
Found that PERL module Plack::Handler is available.

```

The last step will be to set the directory where the API code will be stored. 

WARNING : If you set a directory outside the @INC of perl you will need to add the directory manually by exporting path variables.

```
+----------------------------------------------------------+
| Configuring REST API Server files ...               	   |
+----------------------------------------------------------+

Where do you want the API code to be store [/usr/local/share/perl/5.20.2] ?
Copying files to /usr/local/share/perl/5.20.2

+----------------------------------------------------------+
| Configuring REST API Server configuration files ...  	   |
+----------------------------------------------------------+

```
You can now proceed to the post setup configuration step.

## Post Setup configuraiton

Once, your OCS Installation and database configuration is complete. You need to setup the Apache2 REST API configuration file.

By default the file is located in:
``` 
/etc/apache2/conf-availabe/zz-ocsinventory-restapi.conf
```

The configuration look like this:
```
PerlOptions +Parent

<Perl>
  $ENV{PLACK_ENV} = 'production';
  $ENV{MOJO_HOME} = '/usr/local/share/perl/5.20.2';
  $ENV{MOJO_MODE} = 'deployment';
  $ENV{OCS_DB_HOST} = 'localhost';
  $ENV{OCS_DB_PORT} = '3306';
  $ENV{OCS_DB_LOCAL} = 'ocsweb';
  $ENV{OCS_DB_USER} = 'ocs';
  $ENV{OCS_DB_PWD} = 'ocs';
</Perl>

<Location /ocsapi>
  SetHandler perl-script
  PerlResponseHandler Plack::Handler::Apache2
  PerlSetVar psgi_app '/usr/local/share/perl/5.20.2/Api/Ocsinventory/Restapi/Loader.pm'
</Location>
```

You need to change the following variables in the `<perl></perl>` indent:
1. OCS_DB_HOST => Database host 
2. OCS_DB_PORT => Database port
3. OCS_DB_LOCAL => OCS Database name
4. OCS_DB_USER => Database username 
5. OCS_DB_PWD => Database password

Restart apache service after editing this file.
