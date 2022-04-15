# OCS Inventory Docker image

`Note : since version 2.9.2 of OCS Inventory Docker Image, important changes have been made. Please read the following part of the documentation : OCS Inventory Docker Image - Update 2.9.2.` 

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
cd 2.9.2/
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

## OCS Inventory Docker Image - Update 2.9.2

Since version 2.9.2, the OCS Inventory Docker image is based on the Ubuntu image (CentOS before) and a Nginx container has been added to manage proxy, SSL and Api restricted access.

### Update from an old OCS Inventory Image

Si vous souhaitez mettre à jour votre application docker OCS Inventory déjà en place vers la 2.9.2, plusieurs précautions sont à prendre.

Il est tout d'abord très important de sauvegarder vos volumes existants :

* perlcomdata
* ocsreportsdata
* varlibdata
* sqldata

Seul httpdconfdata n'est pas à sauvegarder car la structure du système ayant changé, les chemins d'accès aux fichiers de configuration ne seront pas compatibles avec la nouvelle structure.

### NGINX configuration

Avec l'ajout de l'image Nginx, plusieurs fichiers de bases ont été ajoutés. Afin de configurer au mieux votre proxy, voici un descriptif des différents fichiers et leur emplacement :

* nginx/conf/ocsinventory.conf.template : fichier de configuration de l'application OCS. Ce fichier peut-être remplacé par votre propre fichier de configuration. Attention : le nom de votre fichier de configuration doit impérativement se terminer par .template.
* nginx/certs/ocs-dummy.crt : certificat SSL par défaut. Ce fichier peut-être remplacé par votre propre certificat. Dans ce cas là, mettre à jour la variable d'environnement SSL_CERT dans le docker-compose.yml.
* nginx/certs/ocs-dummy.key : clé de certificat SSL par défaut. Ce fichier peut-être remplacé par votre propre clé de certificat. Dans ce cas là, mettre à jour la variable d'environnement SSL_KEY dans le docker-compose.yml.
* nginx/auth/ocsapi.htpasswd : fichier htpasswd contenant les identifiants des utilisateurs autorisés à se connecter à l'API Rest OCS. Ce fichier peut-être remplacé par votre propre fichier htpasswd. Dans ce cas là, mettre à jour la variable d'environnement API_AUTH_FILE dans le docker-compose.yml.



You will find below the list of all available environments variables for our nginx docker image.

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

## Use older image tags

To use the older image tags (2.5 and below), please check the following link : 
[OLD Documentation](../14.Archive/OLD-Docker-documentation.md)

Obviously, we don't recommend to use these old tags since they are related to older versions of OCS Inventory product.
