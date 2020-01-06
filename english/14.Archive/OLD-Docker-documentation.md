# Docker documentation (DEPRECATED)

## Installation
#### Start your OCSInventory container

Starting a **OCSInventory container** is simple:
Clone this repository :

```bash
sudo git clone https://github.com/OCSInventory-NG/OCSInventory-Docker-Image.git
cd OCSInventory-Docker-Image
```

*The following command uses the **default values**.*

```bash
sudo docker run \
-p 80:80 \
--name myocsinventory \
-e OCS_DBNAME=ocsweb \
-e OCS_DBSERVER_READ=localhost \
-e OCS_DBSERVER_WRITE=localhost \
-e OCS_DBUSER=ocs \
-e OCS_DBPASS=ocs \
-itd \
ocsinventory/ocsinventory-docker-image:latest \
bash
```
----------
### Use setup.sh to start you OCSInventory container

Enter the directory you just clone, and run the setup.sh script

```bash
cd OCSInventory-Docker-Image
sudo bash setup.sh
```
Follow the steps, the script will do the work for you

### Environmental variables options

Use the following environmental variables to connect your MySQL Server.

```bash
OCS_DBNAME= *(Name of your database)*
OCS_DBSERVER_READ= *(Database Server)*
OCS_DBSERVER_WRITE=*(Database Server)*
OCS_DBSERVER_READ_PORT=	*(Database Server Port)*
OCS_DBSERVER_WRITE_PORT=*(Database Server Port)*
OCS_DBUSER= *(User database)*
OCS_DBPASS= *(User password)*
TZ= *(TIMEZONE)*
```
----------

### Using Docker container

If you want to run your OCSInventory within a MYSQL docker container, simply start your database server before starting your OCSInventory container. More information here for MYSQL container or use our **[Stack OCSInventory](https://github.com/OCSInventory-NG/OCSInventory-Docker-Stack.git)**.

----------

### Container shell access and viewing container logs

The docker exec command allows you to run commands inside a Docker container. The following command line will give you a bash shell inside your OCSInventory container:

```bash
sudo docker exec -it yourcontainerOCSInventory bash
```
You can access to the container logs through the following Docker command:

```bash
sudo docker logs yourcontainerOCSInventory
```
----------

### Create a volume back up from the docker's host

The Docker documentation is a good starting point for understanding the different storage options and give advice in this area. We will simply show the basic procedure here:

Create a data directory on your host system:

```bash
mkdir /my/own/datadir
```
Start your OCSInventory container like this:

```bash
sudo docker run \
-p 80:80 \
--name myocsinventory \
-v /my/own/datadir:/data/save/ocsinventory \
-e OCS_DBNAME=ocsweb \
-e OCS_DBSERVER_READ=localhost \
-e OCS_DBSERVER_WRITE=localhost \
-e OCS_DBUSER=ocs \
-e OCS_DBPASS=ocs \
-itd \
ocsinventory/ocsinventory-docker-image:latest \
bash
```

The  option -v /my/own/datadir:/data/save/ocsinventory mounts the /my/own/datadir directory from the host system as /data/save/ocsinventory inside the container.

----------

### Mount a volume as a data volume

Create a volume:

```bash
docker volume create ocsdata
```
Run your container:

```bash
sudo docker run \
 -p 80:80 \
--name name-container \
-v ocsdata:/usr/share/ocsinventory-reports/ \
-v ocsdata:/etc/ocsinventory-reports/ \
-v ocsdata:/var/lib/ocsinventory-reports/ \
-e OCS_DBNAME=ocsweb \
-e OCS_DBSERVER_READ=localhost \
-e OCS_DBSERVER_WRITE=localhost \
-e OCS_DBUSER=ocs \
-e OCS_DBPASS=ocs \
-itd \
ocsinventory/ocsinventory-docker-image:latest \
bash
```

It is advisable to keep the directories mentioned in the example:

```
/var/lib/ocsinventory-reports/
/etc/ocsinventory-reports/
/usr/share/ocsinventory-reports/ocsreports/
```
They contain the necessary information for a proper server and web interface functioning

## Contributing

1. Fork it!
2. Create your feature branch: git checkout -b my-new-feature
3. Add your changes: git add folder/file1.php
4. Commit your changes: git commit -m 'Add some feature'
5. Push to the branch: git push origin my-new-feature
6. Submit a pull request !


## License

OCS Inventory Docker Image is GPLv3 licensed