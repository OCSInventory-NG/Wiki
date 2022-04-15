# OCS Inventory Docker image

`Since 2.9.2 version, the OCS Inventory Docker image is based on the Ubuntu image (CentOS was used in the older images) and a Nginx container has been added to act as reverse proxy, SSL and Api restricted access. (see "Update from an old OCS Inventory Image" chapter to see how to migrate from older tags)`

## List of all image tags 

| Tag | Description | Images | Usage |
| :--- | :---: | :--- | :--- |
| **2.9.2** | Stable version of OCS Inventory | Ubuntu 20.04 / MySQL 8.0 / Nginx latest | Production |
| **2.9** | Stable version of OCS Inventory | CentOS 7 / MySQL 5.7 | Production |
| **2.8.1** | Stable version of OCS Inventory | CentOS 7 / MySQL 5.7 | Production |
| **2.8** | Stable version of OCS Inventory | CentOS 7 / MySQL 5.7 | Production |
| **2.7** | Stable version of OCS Inventory | CentOS 7 / MySQL 5.7 | Production |
| **2.6** | Stable version of OCS Inventory | CentOS 7 / MySQL 5.7 | Production |
| **nightly** | Rolling releases | Ubuntu 20.04 / MySQL 8.0 / Nginx latest | Production / Testing |
| **dev** | Apache run directory | Ubuntu 20.04 / MySQL 8.0 / Nginx latest | Developpment / Testing |
| **latest** | Use the last stable version of OCS Inventory | Ubuntu 20.04 / MySQL 8.0 / Nginx latest | Production |

Note : Nginx and MySQL are reveleant only if you use docker compose. The ocs image alone doesn't provide these components.

## Pull image from docker hub

Just launch the following command : 
```
docker pull ocsinventory/ocsinventory-docker-image:latest
```

Note : latest will pull the latest stable version

To pull nightly environment run :
```
docker pull ocsinventory/ocsinventory-docker-image:nightly
```

## Pull image from sources and build it locally

To build the image locally pull the git repository : 
```
git clone https://github.com/OCSInventory-NG/OCSInventory-Docker-Image
```

Docker build :
```
docker build --rm -f "MY_TAG/Dockerfile" -t ocsinventory/ocsinventory-docker-image:MY_TAG "MY_TAG"
```

## Run OCS Inventory using docker

You currently have two options :
* run the image alone using docker run
* run the image along with a mysql and proxy (nginx) server using docker compose

### OCS Inventory image (without MySQL and Nginx Proxy)

OCS Inventory image doesn't come with MySQL instance, if you want one, please check the documentation below (docker-compose)

To run a OCS Inventory instance with the most basics settings, you can use the following command :

```
docker run \
-p [HOST_HTTP_PORT]:80 \
-p [HOST_HTTPS_PORT]:443 \
--name [PUT_A_NAME_HERE] \
-e OCS_DB_NAME=[DB_NAME] \
-e OCS_DB_SERVER=[DB_HOST] \
-e OCS_DB_PORT=[DB_PORT] \
-e OCS_DB_USER=[DB_USER] \
-e OCS_DB_PASS=[DB_PASS] \
-itd \
ocsinventory/ocsinventory-docker-image:MY_TAG
```

See *List of all image tags* for more informations.

### A note on OCS Mysql over SSL

Since 2.7 and nightly after 1st january of 2020, our product support MySQL SSL connection.
All env variables are usable in our docker image but we don't provide default volume for this type of configuration.
You will have to add the volume by yourself. See *List of all environments variables available* for more informations 

## List of all environments variables available 

You will find below the list of all available environments variables for our docker image.

| ENV Variable name | Description | Default value |
| :--- | :---: | :--- |
| **APACHE_RUN_USER** | Apache's user | apache |
| **APACHE_RUN_GROUP** | Apache's group | apache |
| **APACHE_RUN_DIR** | Apache run directory | /var/run/httpd |
| **APACHE_LOCK_DIR** | Apache lock directory | /var/lock/httpd |
| **APACHE_PID_FILE** | Apache PID file | /var/run/httpd.pid |
| **APACHE_LOG_DIR** | Apache log directory | /var/log/httpd |
| **OCS_DB_SERVER** | Database hostname | ocsdb |
| **OCS_DB_PORT** | Database port | 3306 |
| **OCS_DB_USER** | OCS Mysql user | ocs |
| **OCS_DB_PASS** | OCS Mysql pass | ocs |
| **OCS_DB_NAME** | OCS Database name | ocsweb |
| **OCS_LOG_DIR** | OCS Inventory log directory | /var/log/ocsinventory-server/ |
| **OCS_VARLIB_DIR** | OCS var lib directory (used to store deployment packages)  | /var/lib/ocsinventory-reports/ |
| **OCS_WEBCONSOLE_DIR** | OCS webconsole directory  | /usr/share/ocsinventory-reports/ocsreports/ |
| **OCS_PERLEXT_DIR** | OCS communication perl extensions directory | /etc/ocsinventory-server/perl/ |
| **OCS_PLUGINSEXT_DIR** | OCS communication plugins extensions directory | /etc/ocsinventory-server/plugins/ |
| **OCS_SSL_ENABLED** | OCS MySQL SSL Enabled | 0 (NO) |
| **OCS_SSL_WEB_MODE** | OCS MySQL mode for webconsole | NULL : Can be either MYSQLI_CLIENT_SSL or MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT (see mysql documentation for more information)  |
| **OCS_SSL_COM_MODE** | OCS MySQL mode for communication server  | NULL : Can be either SSL_MODE_PREFERRED (SSL enabled but optional) / SSL_MODE_REQUIRED (SSL enabled, mandatory but don't verify server certificate. Ex self signed cert) / SSL_MODE_STRICT (SSL enabled, mandatory and server cert must be trusted) |
| **OCS_SSL_KEY** | SSL Key file path |  |
| **OCS_SSL_CERT** | SSL Cert file path |  |
| **OCS_SSL_CA** | SSL CA file path |  |
| **TZ** | Default timezone | Europe/Paris |

*`NOTE : Default volumes are created for OCS_LOG_DIR, OCS_VARLIB_DIR, OCS_WEBCONSOLE_DIR, OCS_PERLEXT_DIR, OCS_PLUGINSEXT_DIR. If you edit these variables you will need to create your own volumes.`*

## OCS Inventory image with Mariadb and Proxy (using docker-compose)

We have a docker-compose example in every folder for each tag of our image.

To get these examples / templates, clone our git repository :
```
git clone https://github.com/OCSInventory-NG/OCSInventory-Docker-Image
```

Browse to tag directory (i.e) :
```
cd 2.9.2/
``` 

Then run : 
```
docker-compose up -d
```

### NGINX configuration

From the 2.9.2 tag, we added a Nginx service inside the docker compose we provide by default. Nginx will manage the SSL certificate of the application and secure the rest API.
A few files has been added to the docker repository in order to manage this configuration : 

* nginx/conf/ocsinventory.conf.template : Nginx configuration of the OCS Inventory application, it can be replaced with an other Nginx configuration that will suit you needs. As documented on the NGINX docker image, the file name need to end with ".template" 
* nginx/certs/ocs-dummy.crt : Dummy ssl certificate, can be replaced by a self-signed one or one from a certificate authority. In the case you change the file name, you'll have to edit the SSL_CERT variable in the docker-compose file.
* nginx/certs/ocs-dummy.key : Dummy ssl certificate key, can be replaced by a self-signed one or one from a certificate authority. In the case you change the file name, you'll have to edit the SSL_KEY variable in the docker-compose file.
* nginx/auth/ocsapi.htpasswd : By default, OCS' REST API is not secured in any way. This file will contain the username / password that can be used to access it. You can generate your own file and replace it. If you change the file's name you need to change the API_AUTH_FILE environement variable.

You will find below the list of all available environments variables for our nginx docker implementation.

| ENV Variable name | Description | Default value |
| :--- | :---: | :--- |
| **LISTEN_PORT** | Proxy listen port (80 or 443) | 80 |
| **PORT_TYPE** | OCS SSL port type precision (empty or ssl) | |
| **SSL_CERT** | OCS SSL certificate name | ocs-dummy.crt |
| **SSL_KEY** | OCS SSL certificate key name | ocs-dummy.key |
| **API_AUTH_FILE** | OCS Api htpasswd file name | ocsapi.htpasswd |
| **READ_TIMEOUT** | OCS donwload read timeout | 300 |
| **CONNECT_TIMEOUT** | OCS download connect request timeout | 300 |
| **SEND_TIMEOUT** | OCS download sending request timeout | 300 |
| **MAX_BODY_SIZE** | OCS download max body size | 1G |

## Update from an old OCS Inventory Image

In the case you need to update your OCS Inventory instance from an version older than the 2.9.2, you'll have to make a sure of a few things.

You'll be able to keep your old volumes and use them on the ubuntu implementation of our docker image :
* perlcomdata
* ocsreportsdata
* varlibdata
* sqldata

Please note that the httpdconfdata volume cannot be saved and thus the apache configuration will need to be reconfigured.
In fact, there is too much changes between the file structure in centos and ubuntu based images.

## Use older image tags

To use the older image tags (2.5 and below), please check the following link : 
[OLD Documentation](../14.Archive/OLD-Docker-documentation.md)

Obviously, we don't recommend to use these old tags since they are related to older versions of OCS Inventory product.
