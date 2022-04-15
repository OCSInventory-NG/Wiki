# OCS Inventory Docker image

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
* run the image along with a mysql server using docker compose

### OCS Inventory image (without MySQL)

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

### OCS Inventory image with Mariadb (using docker-compose)

We have a docker-compose example in every folder for each tag of our image.

To get these examples / templates, clone our git repository :
```
git clone https://github.com/OCSInventory-NG/OCSInventory-Docker-Image
```

Browse to tag directory (i.e) :
```
cd 2.6/
``` 

An then run : 
```
docker-compose up -d
```

### A note on OCS Mysql SSL

Since 2.7 and nightly after 1st january of 2020, our product support MySQL SSL connection.
All env variables are usable in our docker image but we don't provide default volume for this type of configuration.
You will have to add the volume by yourself. See *List of all environments variables available* for more informations 

### A note on OCS HTTPS Configuration

OCS Inventory support HTTPS inventory and webconsole access. 
You will have to configure ocsinventory-reports.conf and z-ocsinventory-server.conf file which are (by default) saved in volumes.
Although there is no volume created to store the SSL certificates.

## List of all environments variables available 

You will find below the list of all available environments variables available for our docker image.

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

## List of all image tags 

| Tag | Description | Usage |
| :--- | :---: | :--- |
| **2.6** | Stable version of OCS Inventory | Production |
| **nightly** | Rolling releases | Production / Testing |
| **dev** | Apache run directory | Developpment / Testing |
| **latest** | Use the last stable version of OCS Inventory | Production |

## Use older image tags

To use the older image tags (2.5 and below), please check the following link : 
[OLD Documentation](../14.Archive/OLD-Docker-documentation.md)

Obviously, we don't recommend to use these old tags since they are related to older versions of OCS Inventory product.
