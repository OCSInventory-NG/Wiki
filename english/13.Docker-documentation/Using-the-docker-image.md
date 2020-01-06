# OCS Inventory Docker image

## Pull image from docker hub

Just launch the following command : 
```
docker pull ocsinventory/ocsinventory-docker-image:latest
```

Note : latest will pull the latest stable version

To pull nightly environement run :
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

To run a OCS Inventory instance with the most settings, you can use the following command :

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

### List of all environements variables available 

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
| **TZ** | Default timezone | Europe/Paris |

*`NOTE : Default volumes are created for OCS_LOG_DIR, OCS_VARLIB_DIR, OCS_WEBCONSOLE_DIR, OCS_PERLEXT_DIR, OCS_PLUGINSEXT_DIR. If you edit these variables you will need to create your own volumes.`*

## Use older image tags

To use the older image tags (2.5 and below), please check the following link : 
[OLD Documentation](../14.Archive/OLD-Docker-documentation.md)

Obviously, we don't recommend to use these old tags since they are related to older versions of OCS Inventory product.